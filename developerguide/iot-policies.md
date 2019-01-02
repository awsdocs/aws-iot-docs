# AWS IoT Policies<a name="iot-policies"></a>

AWS IoT policies are JSON documents\. They follow the same conventions as IAM policies\. AWS IoT supports named policies so many identities can reference the same policy document\. Named policies are versioned so they can be easily rolled back\.

AWS IoT defines a set of policy actions that describe the operations and resources to which you can grant or deny access\. For example:

+ `iot:Connect` represents permission to connect to the AWS IoT message broker\.

+ `iot:Subscribe` represents permission to subscribe to an MQTT topic or topic filter\.

+ `iot:GetThingShadow` represents permission to get a device's shadow\.

AWS IoT policies allow you to control access to the AWS IoT data plane\. The AWS IoT data plane consists of operations that allow you to connect to the AWS IoT message broker, send and receive MQTT messages, and get or update a device's shadow\. For more information, see [AWS IoT Policy Actions](policy-actions.md)\.

An AWS IoT policy is a JSON document that contains one or more policy statements\. Each statement contains an `Effect`, an `Action`, and a `Resource`\. The `Effect` specifies whether the action is allowed or denied\. The `Action` specifies the action the policy is allowing or denying\. The `Resource` specifies the resource or resources on which the action is allowed or denied\. The following policy grants all devices permission to connect to the AWS IoT message broker, but restricts the device to publishing on a specific MQTT topic:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action":["iot:Publish"],
        "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/foo/bar"]
    },
    {
        "Effect": "Allow",
        "Action": ["iot:Connect"],
        "Resource": ["*"]
        }]
}
```