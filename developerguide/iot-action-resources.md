# AWS IoT Core action resources<a name="iot-action-resources"></a>

To specify a resource for an AWS IoT Core policy action, you must use the ARN of the resource\. All resource ARNs are of the following form:

```
arn:aws:iot:region:AWS-account-ID:Resource-type/Resource-name
```

The following table shows the resource to specify for each action type:


****  

| Action | Resource type | Resource name | ARN example | 
| --- | --- | --- | --- | 
| iot:Connect | client | The client's client ID | arn:aws:iot:us\-east\-1:123456789012:client/myClientId | 
| iot:DeleteThingShadow | thing | The thing's name |  `arn:aws:iot:us-east-1:123456789012:thing/thingOne`  | 
| iot:DescribeJobExecution | thing |  The thing's name  |  ``arn:aws:iot:us-east-1:123456789012:thing/thingOne``  | 
| iot:GetPendingJobExecutions | thing |  The thing's name  |  ``arn:aws:iot:us-east-1:123456789012:thing/thingOne``  | 
| iot:GetRetainedMessage | topic |  A retained message topic\.  |  `arn:aws:iot:us-east-1:123456789012:topic/myTopicName`  | 
| iot:GetThingShadow | thing |  The thing's name  |  `arn:aws:iot:us-east-1:123456789012:thing/thingOne`  | 
| iot:ListRetainedMessages | All | All |  `*`  | 
| iot:Publish | `topic` | A topic string | arn:aws:iot:us\-east\-1:123456789012:topic/myTopicName | 
| iot:Receive |  `topic`  |  A topic string  | arn:aws:iot:us\-east\-1:123456789012:topic/myTopicName | 
| iot:RetainPublish | topic |  A topic to publish with the RETAIN flag set\.  |  `arn:aws:iot:us-east-1:123456789012:topic/myTopicName`  | 
| iot:StartNextPendingJobExecution | thing |  The thing's name  |  `arn:aws:iot:us-east-1:123456789012:thing/thingOne`  | 
| iot:Subscribe | `topicfilter` | A topic filter string | arn:aws:iot:us\-east\-1:123456789012:topicfilter/myTopicFilter | 
| iot:UpdateJobExecution | thing |  The thing's name  |  `arn:aws:iot:us-east-1:123456789012:thing/thingOne`  | 
| iot:UpdateThingShadow | thing |  The thing's name, and the shadow's name, if applicable  |  `arn:aws:iot:us-east-1:123456789012:thing/thingOne` `arn:aws:iot:us-east-1:123456789012:thing/thingOne/shadowOne`  | 
| iot:AssumeRoleWithCertificate | rolealias |  A role alias that points to a role ARN  |  `arn:aws:iot:us-east-1:123456789012:rolealias/CredentialProviderRole_alias`  | 