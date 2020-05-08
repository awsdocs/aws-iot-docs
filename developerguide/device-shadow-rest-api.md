# Device shadow RESTful API<a name="device-shadow-rest-api"></a>

A shadow exposes the following URI for updating state information:

```
https://endpoint/things/thingName/shadow
```

The endpoint is specific to your AWS account\. To retrieve your endpoint, use the [describe\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/iot/describe-endpoint.html) command\. The format of the endpoint is as follows:

```
identifier.iot.region.amazonaws.com
```

The shadow RESTful API follows the same HTTPS protocols/port mappings as described in [Protocols](protocols.md)\.

**Topics**
+ [GetThingShadow](API_GetThingShadow.md)
+ [UpdateThingShadow](API_UpdateThingShadow.md)
+ [DeleteThingShadow](API_DeleteThingShadow.md)

**Note**  
When using these API, make sure you use the 8443 port as described in [Protocols, port mappings, and authentication](protocols.md#protocol-port-mapping)\.