# AWS IoT Core action resources<a name="iot-action-resources"></a>

To specify a resource for an AWS IoT Core policy action, you must use the ARN of the resource\. All resource ARNs are of the following form:

```
arn:aws:iot:region:AWS-account-ID:Resource-type/Resource-name
```

The following table shows the resource to specify for each action type:


****  

| Action | Resource type | Resource name | ARN example | 
| --- | --- | --- | --- | 
| iot:DeleteThingShadow | thing | The thing's name |  `arn:aws:iot:us-east-1:123456789012:thing/thingOne`  | 
| iot:Connect | client | The client's client ID | arn:aws:iot:us\-east1:123456789012:client/myClientId | 
| iot:Publish | `topic` | A topic string | arn:aws:iot:us\-east\-1:123456789012:topic/myTopicName | 
| iot:Subscribe | `topicfilter` | A topic filter string | arn:aws:iot:us\-east\-1:123456789012:topicfilter/myTopicFilter | 
| iot:Receive |  `topic`  |  A topic string  | arn:aws:iot:us\-east\-1:123456789012:topic/myTopicName | 
| iot:UpdateThingShadow | thing |  The thing's name, and the shadow's name, if applicable  |  `arn:aws:iot:us-east-1:123456789012:thing/thingOne` `arn:aws:iot:us-east-1:123456789012:thing/thingOne/shadowOne`  | 
| iot:GetThingShadow | thing |  The thing's name, and the shadow's name, if applicable  |  `arn:aws:iot:us-east-1:123456789012:thing/thingOne` `arn:aws:iot:us-east-1:123456789012:thing/thingOne/shadowOne`  | 
| iot:DescribeJobExecution | thing |  The thing's name  |  ``arn:aws:iot:us-east-1:123456789012:thing/thingOne``  | 
| iot:GetPendingJobExecutions | thing |  The thing's name  |  ``arn:aws:iot:us-east-1:123456789012:thing/thingOne``  | 
| iot:UpdateJobExecution | thing |  The thing's name  |  ``arn:aws:iot:us-east-1:123456789012:thing/thingOne``  | 
| iot:StartNextPendingJobExecution | thing |  The thing's name  |  ``arn:aws:iot:us-east-1:123456789012:thing/thingOne``  | 