# Thing Policy Variables<a name="thing-policy-variables"></a>

Thing policy variables allow you to write AWS IoT policies that grant or deny permissions based on thing properties like thing names, thing types, and thing attribute values\. The thing name is obtained from the client ID in the MQTT `Connect` message sent when a thing connects to AWS IoT\. The thing policy variables are replaced when a thing connects to AWS IoT over MQTT using TLS mutual authentication or MQTT over the WebSocket protocol using authenticated Amazon Cognito identities\. Thing policy variables are also replaced when a certificate or authenticated Amazon Cognito identity is attached to a thing\. You can use the [AttachThingPrincipal](https://docs.aws.amazon.com/iot/latest/apireference/API_AttachThingPrincipal.html) API to attach certificates and authenticated Amazon Cognito identities to a thing\.

The following thing policy variables are available:
+ `iot:Connection.Thing.ThingName`
+ `iot:Connection.Thing.ThingTypeName`
+ `iot:Connection.Thing.Attributes[attributeName]`
+ `iot:Connection.Thing.IsAttached`

## iot:Connection\.Thing\.ThingName<a name="thing-policy-variables-thingName"></a>

This resolves to the name of the thing in the AWS IoT registry for which the policy is being evaluated\. AWS IoT uses the certificate the device presents when it authenticates to determine which thing to use to verify the connection\. This policy variable is only available when a device connects over MQTT or MQTT over the WebSocket protocol\.

## iot:Connection\.Thing\.ThingTypeName<a name="thing-policy-variables-thingTypeName"></a>

This resolves to the thing type associated with the thing for which the policy is being evaluated\. The thing name is set to the client ID of the MQTT/WebSocket connection\. The thing type name is obtained by a call to the `DescribeThing` API\. This policy variable is available only when connecting over MQTT or MQTT over the WebSocket protocol\.

## iot:Connection\.Thing\.Attributes\[*attributeName*\]<a name="thing-policy-variables-thing-attributes"></a>

This resolves to the value of the specified attribute associated with the thing for which the policy is being evaluated\. A thing can have up to 50 attributes\. Each attribute is available as a policy variable: `iot:Connection.Thing.Attributes[attributeName]` where *attributeName* is the name of the attribute\. The thing name is set to the client ID of the MQTT/WebSocket connection\. This policy variable is only available when connecting over MQTT or MQTT over the WebSocket protocol\.

## iot:Connection\.Thing\.IsAttached<a name="thing-policy-variables-thing-attached"></a>

This resolves to `true` if the certificate or Amazon Cognito identity for which the policy is being evaluated is attached to an IoT thing\. You can use this variable to prevent a device from connecting to AWS IoT if it presents a certificate that is not attached to an IoT thing in the AWS IoT registry\.