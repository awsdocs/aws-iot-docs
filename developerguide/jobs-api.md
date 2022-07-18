# AWS IoT jobs APIs<a name="jobs-api"></a>

AWS IoT Jobs API can be used for either of the following categories:
+ API operations used for administrative tasks such as management and control of jobs\. This is the *control plane*\.
+ API operations used for devices executing those jobs\. This is the *data plane*, which allows you to send and receive data\.

Job management and control uses an HTTPS protocol API\. Devices can use either an MQTT or an HTTPS protocol API\. The control plane API is designed for a low volume of calls typical when creating and tracking jobs\. It usually opens a connection for a single request, and then closes the connection after the response is received\. The data plane HTTPS and MQTT API allow for long polling\. These API operations are designed for large amounts of traffic that can scale to millions of devices\.

Each AWS IoT Jobs HTTPS API has a corresponding command that allows you to call the API from the AWS CLI\. The commands are lowercase, with hyphens between the words that make up the name of the API\. For example, you can invoke the `CreateJob` API on the CLI by typing:

```
aws iot create-job ...
```

If an error occurs during an operation, you get an error response that contains information about the error\.

## ErrorResponse<a name="jobs-mqtt-error-response"></a>

Contains information about an error that occurred during an AWS IoT Jobs service operation\.

The following example shows the syntax of this operation:

```
{
    "code": "ErrorCode",
    "message": "string",
    "clientToken": "string",
    "timestamp": timestamp,
    "executionState": JobExecutionState
}
```

The following is a description of this `ErrorResponse`:

`code`  
ErrorCode can be set to:    
InvalidTopic  
The request was sent to a topic in the AWS IoT Jobs namespace that does not map to any API\.  
InvalidJson  
The contents of the request could not be interpreted as valid UTF\-8\-encoded JSON\.  
InvalidRequest  
The contents of the request were invalid\. For example, this code is returned when an `UpdateJobExecution` request contains invalid status details\. The message contains details about the error\.  
InvalidStateTransition  
An update attempted to change the job execution to a state that is invalid because of the job execution's current state \(for example, an attempt to change a request in state SUCCEEDED to state IN\_PROGRESS\)\. In this case, the body of the error message also contains the `executionState` field\.  
ResourceNotFound  
The `JobExecution` specified by the request topic does not exist\.   
VersionMismatch  
The expected version specified in the request does not match the version of the job execution in the AWS IoT Jobs service\. In this case, the body of the error message also contains the `executionState` field\.  
InternalError  
There was an internal error during the processing of the request\.  
RequestThrottled  
The request was throttled\.  
TerminalStateReached  
Occurs when a command to describe a job is performed on a job that is in a terminal state\.

`message`  
An error message string\.

`clientToken`  
An arbitrary string used to correlate a request with its reply\.

`timestamp`  
The time, in seconds since the epoch\.

`executionState`  
A [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_JobExecutionState.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_JobExecutionState.html) object\. This field is included only when the `code` field has the value `InvalidStateTransition` or `VersionMismatch`\. This makes it unnecessary in these cases to perform a separate `DescribeJobExecution` request to obtain the current job execution status data\.

**Topics**
+ [Jobs management and control API and data types](jobs-management-control-api.md)
+ [Jobs device MQTT and HTTPS APIs and data types](jobs-mqtt-https-api.md)