# Security best practices in AWS IoT Core<a name="security-best-practices"></a>

This section contains information about security best practices for AWS IoT Core\. For more information, see [ Ten security golden rules for IoT solutions](https://aws.amazon.com/blogs/iot/ten-security-golden-rules-for-iot-solutions/)\.

## Protecting MQTT connections in AWS IoT<a name="secure-mqtt"></a>

[AWS IoT Core](https://aws.amazon.com/iot-core/) is a managed cloud service that makes it possible for connected devices to interact with cloud applications and other devices easily and securely\. AWS IoT Core supports HTTP, [WebSocket](https://en.wikipedia.org/wiki/WebSocket), and [MQTT](https://en.wikipedia.org/wiki/MQTT), a lightweight communication protocol specifically designed to tolerate intermittent connections\. If you are connecting to the [Message broker for AWS IoT](iot-message-broker.md) using MQTT, each of your connections must be associated with an identifier known as a client ID\. MQTT client IDs uniquely identify MQTT connections\. If a new connection is established using a client ID that is already claimed for another connection, the AWS IoT message broker drops the old connection to allow the new connection\. Client IDs must be unique within each AWS account and each AWS Region\. This means that you don't need to enforce global uniqueness of client IDs outside of your AWS account or across Regions within your AWS account\.

The impact and severity of dropping MQTT connections on your device fleet depends on many factors\. These include:
+ Your use case \(for example, the data your devices send to AWS IoT, how much data, and the frequency that the data is sent\)\.
+ Your MQTT client configuration \(for example, auto reconnect settings, associated back\-off timings, and use of [MQTT persistent sessions](mqtt-persistent-sessions.md)\)\.
+ Device resource constraints\.
+ The root cause of the disconnections, its aggressiveness, and persistence\.

To avoid client ID conflicts and their potential negative impacts, make sure that each device or mobile application has an AWS IoT or IAM policy that restricts which client IDs can be used for MQTT connections to the AWS IoT message broker\. For example, you can use an IAM policy to prevent a device from unintentionally closing another device's connection by using a client ID that is already in use\. For more information, see [Authorization](iot-authorization.md)\.

All devices in your fleet must have credentials with privileges that authorize intended actions only, which include \(but not limited to\) AWS IoT MQTT actions such as publishing messages or subscribing to topics with specific scope and context\. The specific permission policies can vary for your use cases\. Identify the permission policies that best meet your business and security requirements\.

To simplify creation and management of permission policies, you can use [AWS IoT Core policy variables](iot-policy-variables.md) and [IAM policy variables](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html)\. Policy variables can be placed in a policy and when the policy is evaluated, the variables are replaced by values that come from the device's request\. Using policy variables, you can create a single policy for granting permissions to multiple devices\. You can identify the relevant policy variables for your use case based on your AWS IoT account configuration, authentication mechanism, and network protocol used in connecting to AWS IoT message broker\. However, to write the best permission policies, consider the specifics of your use case and your [threat model](https://en.wikipedia.org/wiki/Threat_model)\.

For example, if you registered your devices in the AWS IoT registry, you can use [thing policy variables](thing-policy-variables.md) in AWS IoT policies to grant or deny permissions based on thing properties like thing names, thing types, and thing attribute values\. The thing name is obtained from the client ID in the MQTT connect message sent when a thing connects to AWS IoT\. The thing policy variables are replaced when a thing connects to AWS IoT over MQTT using TLS mutual authentication or MQTT over the WebSocket protocol using authenticated [Amazon Cognito identities](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-identities.html)\. You can use the [AttachThingPrincipal](https://docs.aws.amazon.com/iot/latest/apireference/API_AttachThingPrincipal.html) API to attach certificates and authenticated Amazon Cognito identities to a thing\. `iot:Connection.Thing.ThingName` is a useful thing policy variable to enforce client ID restrictions\. The following example AWS IoT policy requires a registered thing's name to be used as the client ID for MQTT connections to the AWS IoT message broker:

```
{
    "Version":"2012-10-17",
    "Statement":[
    {
        "Effect":"Allow",
        "Action":"iot:Connect",
        "Resource":[
            "arn:aws:iot:us-east-1:123456789012:client/${iot:Connection.Thing.ThingName}"
        ]
    }
    ]
}
```

If you want to identify ongoing client ID conflicts, you can enable and use [CloudWatch Logs for AWS IoT](cloud-watch-logs.md)\. For every MQTT connection that the AWS IoT message broker disconnects due to client ID conflicts, a log record similar to the following is generated:

```
{
    "timestamp": "2019-04-28 22:05:30.105",
    "logLevel": "ERROR",
    "traceId": "02a04a93-0b3a-b608-a27c-1ae8ebdb032a",
    "accountId": "123456789012",
    "status": "Failure",
    "eventType": "Disconnect",
    "protocol": "MQTT",
    "clientId": "clientId01",
    "principalId": "1670fcf6de55adc1930169142405c4a2493d9eb5487127cd0091ca0193a3d3f6",
    "sourceIp": "203.0.113.1",
    "sourcePort": 21335,
    "reason": "DUPLICATE_CLIENT_ID",
    "details": "A new connection was established with the same client ID"
}
```

You can use a [CloudWatch Logs filter](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/MonitoringLogData.html) such as `{$.reason= "DUPLICATE_CLIENT_ID" }` to search for instances of client ID conflicts or to set up [CloudWatch metric filters](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/MonitoringPolicyExamples.html) and corresponding CloudWatch alarms for continuous monitoring and reporting\.

You can use [AWS IoT Device Defender](https://aws.amazon.com/iot-device-defender/) to identify overly permissive AWS IoT and IAM policies\. AWS IoT Device Defender also provides an audit check that notifies you if multiple devices in your fleet are connecting to the AWS IoT message broker using the same client ID\.

### See also<a name="mqtt-security-see-also"></a>
+ [AWS IoT Core](https://aws.amazon.com/iot-core/)
+ [AWS IoT's Security Features](authentication.md)
+ [AWS IoT Core policy variables](iot-policy-variables.md)
+ [IAM Policy Variables](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html)
+ [Amazon Cognito Identity](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-identities.html)
+ [AWS IoT Device Defender](https://aws.amazon.com/iot-device-defender/)
+ [CloudWatch Logs for AWS IoT](cloud-watch-logs.md)

## Keep your device's clock in sync<a name="device-clock"></a>

It's important to have an accurate time on your device\. X\.509 certificates have an expiry date and time\. The clock on your device is used to verify that a server certificate is still valid\. If you're building commercial IoT devices, remember that your products may be stored for extended periods before being sold\. Real\-time clocks can drift during this time and batteries can get discharged, so setting time in the factory is not sufficient\.

For most systems, this means that the device's software must include a network time protocol \(NTP\) client\. The device should wait until it synchronizes with an NTP server before it tries to connect to AWS IoT Core\. If this isn't possible, the system should provide a way for a user to set the device's time so that subsequent connections succeed\.

After the device synchronizes with an NTP server, it can open a connection with AWS IoT Core\. How much clock skew that is allowed depends on what you're trying to do with the connection\. 

## Validate the server certificate<a name="validate-server-cert"></a>

The first thing a device does to interact with AWS IoT is to open a secure connection\. When you connect your device to AWS IoT, ensure that you're talking to AWS IoT and not another server impersonating AWS IoT\. Each of the AWS IoT servers is provisioned with a certificate issued for the `iot.amazonaws.com` domain\. This certificate was issued to AWS IoT by a trusted certificate authority that verified our identity and ownership of the domain\.

One of the first things AWS IoT Core does when a device connects is send the device a server certificate\. Devices can verify that they were expecting to connect to `iot.amazonaws.com` and that the server on the end of that connection possesses a certificate from a trusted authority for that domain\.

TLS certificates are in X\.509 format and include a variety of information such as the organization's name, location, domain name, and a validity period\. The validity period is specified as a pair of time values called `notBefore` and `notAfter`\. Services like AWS IoT Core use limited validity periods \(for example, one year\) for their server certificates and begin serving new ones before the old ones expire\.

## Use a single identity per device<a name="cert-per-device"></a>

Use a single identity per client\. Devices generally use X\.509 client certificates\. Web and mobile applications use Amazon Cognito Identity\. This enables you to apply fine\-grained permissions to your devices\.

For example, you have an application that consists of a mobile phone device that receives status updates from two different smart home objects – a light bulb and a thermostat\. The light bulb sends the status of its battery level, and a thermostat sends messages that report the temperature\.

AWS IoT authenticates devices individually and treats each connection individually\. You can apply fine\-grained access controls using authorization policies\. You can define a policy for the thermostat that allows it to publish to a topic space\. You can define a separate policy for the light bulb that allows it to publish to a different topic space\. Finally, you can define a policy for the mobile app that only allows it to connect and subscribe to the topics for the thermostat and the light bulb to receive messages from these devices\.

Apply the principle of least privilege and scope down the permissions per device as much as possible\. All devices or users should have an AWS IoT policy in AWS IoT that only allows it to connect with a known client ID, and to publish and subscribe to an identified and fixed set of topics\.

## Use just in time provisioning<a name="use-jitp"></a>

Manually creating and provisioning each device can be time consuming\. AWS IoT provides a way to define a template to provision devices when they first connect to AWS IoT\. For more information, see [Just\-in\-time provisioning](jit-provisioning.md)\.