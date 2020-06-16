# AWS IoT Core action resources<a name="iot-action-resources"></a>

To specify a resource for an AWS IoT Core policy action, you must use the ARN of the resource\. All resource ARNs are of the following form:

```
arn:aws:iot:region:AWS account ID:resource type/resource name
```

The following table shows the resource to specify for each action type:


****  

| Action | Resource | 
| --- | --- | 
| iot:DeleteThingShadow |  A thing ARN: `arn:aws:iot:us-east-1:123456789012:thing/thingOne`  | 
| iot:Connect | A client ID ARN: `arn:aws:iot:us\-east1:123456789012:client/myClientId` | 
| iot:Publish | A topic ARN: `arn:aws:iot:us\-east\-1:123456789012:topic/myTopicName` | 
| iot:Subscribe | A topic filter ARN: `arn:aws:iot:us\-east\-1:123456789012:topicfilter/myTopicFilter` | 
| iot:Receive | A topic ARN: `arn:aws:iot:us\-east\-1:123456789012:topic/myTopicName` | 
| iot:UpdateThingShadow |  A thing ARN: `arn:aws:iot:us-east-1:123456789012:thing/thingOne`  | 
| iot:GetThingShadow |  A thing ARN: `arn:aws:iot:us-east-1:123456789012:thing/thingOne`  | 
| iot:DescribeJobExecution |  A thing ARN: `arn:aws:iot:us-east-1:123456789012:thing/thingOne`  | 
| iot:GetPendingJobExecutions |  A thing ARN: `arn:aws:iot:us-east-1:123456789012:thing/thingOne`  | 
| iot:UpdateJobExecution |  A thing ARN: `arn:aws:iot:us-east-1:123456789012:thing/thingOne`  | 
| iot:StartNextPendingJobExecution |  A thing ARN: `arn:aws:iot:us-east-1:123456789012:thing/thingOne`  | 
