# Invoking a custom authorizer with enhanced custom authentication<a name="enhanced-custom-auth-using"></a>

****  
This feature is currently in public beta and is available only in the US East \(N\. Virginia\) Region\.

This section explains how to use enhanced custom authentication to pass client credentials to your custom authorizer by using HTTP headers and query strings or MQTT user names and passwords\.

Devices and clients that use the Transport Layer Security \(TLS\) protocol to connect to AWS IoT must send the Server Name Indication \(SNI\) TLS extension with a value that matches the domain of the appropriate domain configuration\. You can specify a custom authorizer that isn't the default authorizer if the `allowAuthorizerOverride` value in your domain configuration is set to `true`\. For more information on configuring the SNI extension, see [Transport security in AWS IoT](transport-security.md)\. 

To use enhanced custom authentication in MQTT connections, clients must also send the Application Layer Protocol Negotiation \(ALPN\) TLS extension with a value of `mqtt` and connect on port 443\.

**Note**  
You can find configuration for these TLS extensions in the V2 AWS IoT Device SDKs\. For more information, see the [AWS Labs GitHub repo](https://github.com/awslabs)\.

## HTTPS requests<a name="enhanced-custom-auth-using-https"></a>

Requests to custom authorizers can pass tokens by using one of the following: HTTP headers or query strings in HTTP Publish requests or HTTP Upgrade requests for establishing MQTT over WSS sessions\. The following example uses HTTP headers to send an upgrade request to AWS IoT Device Gateway\.

```
GET /mqtt HTTP/1.1
Host: your-endpoint
Upgrade: WebSocket
Connection: Upgrades
x-amz-customauthorizer-signature: token-signature
token-key-name: some-token
sec-WebSocket-Key: any random base64 value
sec-websocket-protocol: mqtt
sec-WebSocket-Version: websocket version
```

The request doesn't contain a value for `x-amz-customauthorizer-name`, so AWS IoT uses the default authorizer specified in the domain configuration\. The value of `x-amz-customauthorizer-signature` is optional if signing is disabled on the authorizer\.

The following example shows how to make the same request by using query string parameters\.

```
GET /mqtt?x-amz-customauthorizer-signature=${sign}&token-name=${token-value} HTTP/1.1
Host: your-endpoint
Connection: Upgrade
Upgrade: websocket
sec-WebSocket-Key: any random base64 value
sec-websocket-protocol: mqtt
sec-WebSocket-Version: websocket version
```

## MQTT username and password<a name="enhanced-custom-auth-using-mqtt"></a>

You can pass user names and passwords to your custom authorizers by using the `username` and `password` fields of MQTT messages\. Password data is base64\-encoded to support arbitrary binary values\. The `username` value can optionally contain a query string that passes additional values \(including a token, signature, and authorizer name\) to your authorizer\. 

The following example contains a `username` string with extra parameters that specify a token and signature\. You can use this method to authenticate an MQTT connection by using a bearer token\.

```
username?x-amz-customauthorizer-name=${name}&x-amz-customauthorizer-signature=${sign}&token-name=${token-value}                    
```

## Data sent to the custom authorizer<a name="enhanced-custom-auth-sending-data"></a>

The data that AWS IoT sends to your custom authorizer Lambda function depends on which protocols and parameters are present in the connection\. For HTTP requests \(Publishes and WSS Upgrades\), AWS IoT sends all headers and query parameters \(up to 8 KB of data\)\. For WSS upgrades on MQTT Connect, AWS IoT also sends all MQTT data\.

If signing is enabled and the request contains a token and signature, AWS IoT won't trigger your custom authorizer's Lambda function unless the token signature can be decrypted to match the token provided in the request\. You can use this method so that others connecting to your domain don't excessively trigger your Lambda function\.

**Important**  
We strongly recommend that you use the signature verification feature that AWS IoT offers\.

The following is an example payload that AWS IoT sends to the custom authenticator's Lambda function\. AWS IoT populates only the fields that the request contains\.

```
{
    "token" :"aToken",
    "signatureVerified": boolean, // Indicates whether the device gateway has validated the signature.
    "protocols": ["tls", "http", "mqtt"], // Indicates which protocols to expect.
    "protocolData": {
        "tls" : {
            "serverName": "serverName" // The SNI host_name string.
        },
        "http": {
            "headers": {
                "#{name}": "#{value}"
            },
            "queryString": "?#{name}=#{value}"
        },
        "mqtt": {
            "username": "myUserName",
            "password": "myPassword", // base64 encoded.
            "clientId": "myClientId" // Provided only when the device sends it.
        }
    },
    "connectionMetadata": {
        "id": UUID // The connection ID. You can use this for logging.
    },
}
```

**Note**  
All password data is base64 encoded and your Lambda function needs to decode it\.

As with the regular custom authentication, the custom authorizer must return a response that indicates whether it has authorized the request to AWS IoT\. The following is an example of a successful response from a custom authorizer\.

```
{
     "isAuthenticated":true,
     "principalId": "xxxxxxxx",
     "disconnectAfterInSeconds": 86400,
     "refreshAfterInSeconds": 300,
     "policyDocuments": [
      "{ \"Version\": \"2012-10-17\", \"Statement\": [ { \"Action\": \"...\", \"Effect\": \"Allow|Deny\", \"Resource\": \"...\" } ] }"
     ]
    }
```

AWS IoT caches the policies associated with the principal for the duration specified in the value of `refreshAfterInSeconds`\. During this duration, subsequent calls are authorized without having to reauthenticate the device\. Failures that occurs during custom authentication results in authentication failure, which stops the connection\.