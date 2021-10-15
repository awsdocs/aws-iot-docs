# Thing policy variables<a name="thing-policy-variables"></a>

Thing policy variables allow you to write AWS IoT Core policies that grant or deny permissions based on thing properties like thing names, thing types, and thing attribute values\. You can use thing policy variables to apply the same policy to control many AWS IoT Core devices\. For more information about device provisioning, see [Device Provisioning](iot-provision.html)\. The thing name is obtained from the client ID in the MQTT `Connect` message sent when a thing connects to AWS IoT Core\.

Keep the following in mind when using thing policy variables in AWS IoT Core policies\.
+ Use the [AttachThingPrincipal](https://docs.aws.amazon.com/iot/latest/apireference/API_AttachThingPrincipal.html) API to attach certificates or principals \(authenticated Amazon Cognito identities\) to a thing\.
+ When you're replacing thing names with thing policy variables, the value of `clientId` in the MQTT connect message or the TLS connection must exactly match the thing name\.

The following thing policy variables are available:
+ `iot:Connection.Thing.ThingName`

  This resolves to the name of the thing in the AWS IoT Core registry for which the policy is being evaluated\. AWS IoT Core uses the certificate the device presents when it authenticates to determine which thing to use to verify the connection\. This policy variable is only available when a device connects over MQTT or MQTT over the WebSocket protocol\.
+ `iot:Connection.Thing.ThingTypeName`

  This resolves to the thing type associated with the thing for which the policy is being evaluated\. The thing name is set to the client ID of the MQTT/WebSocket connection\. This policy variable is available only when connecting over MQTT or MQTT over the WebSocket protocol\.
+ `iot:Connection.Thing.Attributes[attributeName]`

  This resolves to the value of the specified attribute associated with the thing for which the policy is being evaluated\. A thing can have up to 50 attributes\. Each attribute is available as a policy variable: `iot:Connection.Thing.Attributes[attributeName]` where *attributeName* is the name of the attribute\. The thing name is set to the client ID of the MQTT/WebSocket connection\. This policy variable is only available when connecting over MQTT or MQTT over the WebSocket protocol\.
+ `iot:Connection.Thing.IsAttached`

  `iot:Connection.Thing.IsAttached: ["true"]` enforces that only the devices that are both registered in AWS IoT and attached to principal can access the permissions inside the policy\. You can use this variable to prevent a device from connecting to AWS IoT Core if it presents a certificate that is not attached to an IoT thing in the AWS IoT Core registry\.This variable has values `true` or `false` indicating that the connecting thing is attached to the certificate or Amazon Cognito identity in the registry using [AttachThingPrincipal](https://docs.aws.amazon.com/iot/latest/apireference/API_AttachThingPrincipal.html) API\. Thing name is taken as client Id\. 