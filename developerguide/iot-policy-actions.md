# AWS IoT Core policy actions<a name="iot-policy-actions"></a>

The following policy actions are defined by AWS IoT Core:MQTT Policy Actions

iot:Connect  
Represents the permission to connect to the AWS IoT Core message broker\. The `iot:Connect` permission is checked every time a `CONNECT` request is sent to the broker\. The message broker does not allow two clients with the same client ID to stay connected at the same time\. After the second client connects, the broker closes the existing connection\. The `iot:Connect` permission can be used to ensure only authorized clients using a specific client ID can connect\.

iot:Publish  
Represents the permission to publish on an MQTT topic\. This permission is checked every time a PUBLISH request is sent to the broker\. This can be used to allow clients to publish to specific topic patterns\.  
To grant `iot:Publish` permission, you must also grant `iot:Connect` permission\.

iot:Receive  
Represents the permission to receive a message from AWS IoT Core\. The `iot:Receive` permission is checked every time a message is delivered to a client\. Because this permission is checked on every delivery, it can be used to revoke permissions to clients that are currently subscribed to a topic\.

iot:Subscribe  
Represents the permission to subscribe to a topic filter\. This permission is checked every time a SUBSCRIBE request is sent to the broker\. This can be used to allow clients to subscribe to topics that match specific topic patterns\.  
To grant `iot:Subscribe` permission, you must also grant `iot:Connect` permission\.Shadow Policy Actions

iot:DeleteThingShadow  
Represents the permission to delete a device's shadow\. The `iot:DeleteThingShadow` permission is checked every time a request is made to delete the shadow's contents\.

iot:GetThingShadow  
Represents the permission to retrieve a device's shadow\. The `iot:GetThingShadow` permission is checked every time a request is made to retrieve the shadow's contents\.

iot:UpdateThingShadow  
Represents the permission to update a device's shadow\. The `iot:UpdateThingShadow` permission is checked every time a request is made to update the shadow's contents\.

**Note**  
The job execution policy actions apply only for the HTTP TLS endpoint\. If you use the MQTT endpoint, you must use MQTT policy actions defined in this topic\.Job Executions AWS IoT Core Policy Actions

iot:DescribeJobExecution  
Represents the permission to retrieve a job execution for a given thing\. The `iot:DescribeJobExecution` permission is checked every time a request is made to get a job execution\.

iot:GetPendingJobExecutions  
Represents the permission to retrieve the list of jobs that are not in a terminal status for a thing\. The `iot:GetPendingJobExecutions` permission is checked every time a request is made to retrieve the list\. 

iot:UpdateJobExecution  
Represents the permission to update a job execution\. The `iot:UpdateJobExecution` permission is checked every time a request is made to update the state of a job execution\.

iot:StartNextPendingJobExecution  
Represents the permission to get and start the next pending job execution for a thing\. \(That is, to update a job execution with status QUEUED to IN\_PROGRESS\.\) The `iot:StartNextPendingJobExecution` permission is checked every time a request is made to start the next pending job execution\.