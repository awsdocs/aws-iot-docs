# AWS IoT policy elements<a name="example-iot-policies-elements"></a>

AWS IoT policies are specified in a JSON document\. An AWS IoT policy is composed of the following items:

*Version*  
Must be set to `"2012-10-17"`\.

*Effect*  
Must be set to `"Allow"` or `"Deny"`\.

*Action*  
Must be set to "`iot:operation-name`" where` operation-name` is one of the following:  
`"iot:Connect"`: Connect to AWS IoT\.  
`"iot:Receive"`: Receive messages from AWS IoT\.  
`"iot:Publish"`: MQTT publish\.  
`"iot:Subscribe"`: MQTT subscribe\.  
`"iot:UpdateThingShadow"`: Update a device's shadow\.  
`"iot:GetThingShadow"`: Retrieve a device's shadow\.  
`"iot:DeleteThingShadow"`: Delete a device's shadow\.

*Resource*  
Must be set to one of the following:  
Client: `arn:aws:iot:region:account-id:client/client-id`  
Topic ARN: `arn:aws:iot:region:account-id:topic/topic-name`  
Topic filter ARN: `arn:aws:iot:region:account-id:topicfilter/topic-filter`