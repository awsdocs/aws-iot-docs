# Resolving AWS IoT secure ing connectivity issues by rotating client access tokens<a name="iot-secure-tunneling-troubleshooting"></a>

When you use AWS IoT secure tunneling, you might run into connectivity issues even if the tunnel is open\. The following sections show some possible issues and how you can resolve them by rotating the client access tokens\. To rotate the client access token \(CAT\), use the [RotateTunnelAccessToken](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-secure-tunneling_RotateTunnelAccessToken.html) API or the [rotate\-tunnel\-access\-token](https://docs.aws.amazon.com/cli/latest/reference/iotsecuretunneling/rotate-tunnel-access-token.html) AWS CLI\. Depending on whether you run into an error with using the client in the source or destination mode, you can rotate the CAT either in source or destination mode, or both\.

**Note**  
If you're not sure whether the CAT needs to be rotated on the source or destination, you can rotate the CAT on both the source and destination by setting `ClientMode` to ALL when using the `RotateTunnelAccessToken` API\.
Rotating the CAT doesn't extend the tunnel duration\. For example, say the tunnel duration is 12 hours and the tunnel has already been open for 4 hours\. When you rotate the access tokens, the new tokens that are generated can only be used for the remaining 8 hours\.

**Topics**
+ [Invalid client access token error](#invalid-access-token)
+ [Client token mismatch error](#client-token-mismatch)
+ [Remote device connectivity issues](#tunnel-open-device-error)

## Invalid client access token error<a name="invalid-access-token"></a>

When using AWS IoT secure tunneling, you can run into a connection error when using the same client access token \(CAT\) to reconnect to the same tunnel\. In this case, the local proxy can't connect to the secure tunneling proxy server\. If you're using a client in the source mode, you might see the following error message:

```
Invalid access token: The access token was previously used and cannot be used again
```

The error occurs because the client access token \(CAT\) can only be used once by the local proxy, and it then becomes invalid\. To resolve this error, rotate the client access token in the `SOURCE` mode to generate a new CAT for the source\. For an example that shows how to rotate the source CAT, see [Rotate source CAT example](#rotate-token-source-example)\.

## Client token mismatch error<a name="client-token-mismatch"></a>

**Note**  
Using client tokens to reuse the CAT is not recommended\. We recommend that you use the `RotateTunnelAccessToken` API instead to rotate the client access tokens to reconnect to the tunnel\.

If you're using client tokens, you can reuse the CAT for reconnecting to the tunnel\. To reuse the CAT, you must provide the client token with the CAT the first time you connect to secure tunneling\. Secure tunneling stores the client token so for subsequent connection attempts using the same token, the client token must also be provided\. For more information about using client tokens, see the [ local proxy reference implementation in GitHub](https://github.com/aws-samples/aws-iot-securetunneling-localproxy/blob/master/V2WebSocketProtocolGuide.md)\.

When using client tokens, if you're using a client in the source mode, you might see the following error:

```
Invalid client token: The provided client token does not match the client token 
				that was previously set.
```

The error occurs because the client token provided doesn't match the client token that was provided with the CAT when accessing the tunnel\. To resolve this error, rotate the CAT in the `SOURCE` mode to generate a new CAT for the source\. The following shows an example:

### Rotate source CAT example<a name="rotate-token-source-example"></a>

The following shows an example of how to run the `RotateTunnelAccessToken` API in the `SOURCE` mode to generate a new CAT for the source:

```
aws iotsecuretunneling rotate-tunnel-access-token \ 
    --region <region> \ 
    --tunnel-id <tunnel-id> \ 
    --client-mode SOURCE
```

Running this command generates a new source access token and returns the ARN of your tunnel\.

```
{
    "sourceAccessToken": "<source-access-token>", 
    "tunnelArn": "arn:aws:iot:<region>:<account-id>:tunnel/<tunnel-id>"
}
```

You can now use the new source token to connect the local proxy in source mode\.

```
export AWSIOT_TUNNEL_ACCESS_TOKEN=<source-access-token>
./localproxy -r <region> -s <port>
```

The following shows a sample output of running the local proxy:

```
...

[info]    Starting proxy in source mode
...
[info]    Successfully established websocket connection with proxy server ...
[info]    Listening for new connection on port <port>
...
```

## Remote device connectivity issues<a name="tunnel-open-device-error"></a>

When using AWS IoT secure tunneling, the device might get disconnected unexpectedly even if the tunnel is open\. To identify whether a device is still connected to the tunnel, you can use the [ DescribeTunnel](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-secure-tunneling_DescribeTunnel.html) API or the [describe\-tunnel](https://docs.aws.amazon.com/cli/latest/reference/iotsecuretunneling/describe-tunnel.html) AWS CLI\.

A device can get disconnected for multiple reasons\. To resolve the connectivity issue, you can rotate the CAT on the destination if the device was disconnected due to the following possible reasons:
+ The CAT on the destination became invalid\.
+ The token wasn't delivered to the device over the secure tunneling reserved MQTT topic:

  `$aws/things/<thing-name>/tunnels/notify`

The following example shows how to resolve this issue:

### Rotate destination CAT example<a name="rotate-token-dest-example"></a>

Consider a remote device, `<RemoteThing1>`\. To open a tunnel for that thing, you can use the following command:

```
aws iotsecuretunneling open-tunnel \ 
    --region <region> \ 
    --destination-config thingName=<RemoteThing1>,services=SSH
```

Running this command generates the tunnel details and the CAT for your source and destination\.

```
{
    "sourceAccessToken": "<source-access-token>", 
    "destinationAccessToken": "<destination-access-token>", 
    "tunnelId": "<tunnel-id>", 
    "tunnelArn": "arn:aws:iot:<region>:<account-id>:tunnel/tunnel-id"
}
```

However, when you use the [DescribeTunnel](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-secure-tunneling_DescribeTunnel.html) API, the output indicates that the device has been disconnected, as illustrated below:

```
aws iotsecuretunneling describe-tunnel \ 
    --tunnel-id <tunnel-id> \ 
    --region <region>
```

Running this command displays that the device is still not connected\.

```
{
    "tunnel": {
        ...
        "destinationConnectionState": {
            "status": "DISCONNECTED"
        },
        ...
    }
}
```

To resolve this error, run the `RotateTunnelAccessToken` API with the client in `DESTINATION` mode and the configurations for the destination\. Running this command revokes the old access token, generates a new token, and resends this token to the MQTT topic:

`$aws/things/<thing-name>/tunnels/notify`

```
aws iotsecuretunneling rotate-tunnel-access-token \ 
    --tunnel-id <tunnel-id> \ 
    --client-mode DESTINATION \ 
    --destination-config thingName=<RemoteThing1>,services=SSH \ 
    --region <region>
```

Running this command generates the new access token as shown below\. The token is then delivered to the device to connect to the tunnel, if the device agent is set up correctly\.

```
{
    "destinationAccessToken": "destination-access-token", 
    "tunnelArn": "arn:aws:iot:region:account-id:tunnel/tunnel-id"
}
```