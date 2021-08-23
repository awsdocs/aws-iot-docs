# Jobs device MQTT and HTTPS data types<a name="jobs-data-plane-data-types"></a>

The following data types are used to communicate with the AWS IoT Jobs service over the MQTT and HTTPS protocols\.

## JobExecution<a name="jobs-mqtt-job-execution-data"></a>

------
#### [ JobExecution data type ]

Contains data about a job execution\.

------
#### [ Syntax \(7\) ]

```
{
    "jobId" : "string",
    "thingName" : "string",
    "jobDocument" : "string",
    "status": "QUEUED|IN_PROGRESS|FAILED|SUCCEEDED|CANCELED|TIMED_OUT|REJECTED|REMOVED",
    "statusDetails": {
        "string": "string"
    },
    "queuedAt" : "timestamp",
    "startedAt" : "timestamp",
    "lastUpdatedAt" : "timestamp",
    "versionNumber" : "number",
    "executionNumber": long
}
```

------
#### [ Description \(7\) ]

`jobId`  
The unique identifier you assigned to this job when it was created\.

`thingName`  
The name of the thing that is executing the job\.

`jobDocument`  
The content of the job document\.

`status`  
The status of the job execution\. Can be one of: QUEUED, IN\_PROGRESS, FAILED, SUCCEEDED, CANCELED, TIMED\_OUT, REJECTED, or REMOVED\.

`statusDetails`  
A collection of name\-value pairs that describe the status of the job execution\. 

`queuedAt`  
The time, in seconds since the epoch, when the job execution was enqueued\. 

`startedAt`  
The time, in seconds since the epoch, when the job execution was started\. 

`lastUpdatedAt`  
The time, in seconds since the epoch, when the job execution was last updated\.

`versionNumber`  
The version of the job execution\. Job execution versions are incremented each time they are updated by a device\.

`executionNumber`  
A number that identifies a job execution on a device\. It can be used later in commands that return or update job execution information\.

------

## JobExecutionState<a name="jobs-mqtt-job-execution-state"></a>

------
#### [ JobExecutionState data type ]

Contains data about the state of a job execution\.

------
#### [ Syntax \(8\) ]

```
{
    "status": "QUEUED|IN_PROGRESS|FAILED|SUCCEEDED|CANCELED|TIMED_OUT|REJECTED|REMOVED",
    "statusDetails": {
        "string": "string"
        ...
    }
    "versionNumber": "number"
}
```

------
#### [ Description \(8\) ]

`status`  
The status of the job execution\. Can be one of: QUEUED, IN\_PROGRESS, FAILED, SUCCEEDED, CANCELED, TIMED\_OUT, REJECTED, or REMOVED\.

`statusDetails`  
A collection of name\-value pairs that describe the status of the job execution\.

`versionNumber`  
The version of the job execution\. Job execution versions are incremented each time they are updated by a device\.

------

## JobExecutionSummary<a name="jobs-mqtt-job-execution-summary"></a>

------
#### [ JobExecutionSummary data type ]

Contains a subset of information about a job execution\.

------
#### [ Syntax \(9\) ]

```
{
    "jobId": "string",
    "queuedAt": timestamp,
    "startedAt": timestamp,
    "lastUpdatedAt": timestamp,
    "versionNumber": "number",
    "executionNumber": long 
}
```

------
#### [ Description \(9\) ]

`jobId`  
The unique identifier you assigned to this job when it was created\.

`queuedAt`  
The time, in seconds since the epoch, when the job execution was enqueued\.

`startedAt`  
The time, in seconds since the epoch, when the job execution started\.

`lastUpdatedAt`  
The time, in seconds since the epoch, when the job execution was last updated\.

`versionNumber`  
The version of the job execution\. Job execution versions are incremented each time the AWS IoT Jobs service receives an update from a device\.

`executionNumber`  
A number that identifies a job execution on a device\.

------

## ErrorResponse<a name="jobs-mqtt-error-response"></a>

------
#### [ ErrorResponse data type ]

Contains information about an error that occurred during an AWS IoT Jobs service operation\.

------
#### [ Syntax \(10\) ]

```
{
    "code": "ErrorCode",
    "message": "string",
    "clientToken": "string",
    "timestamp": timestamp,
    "executionState": JobExecutionState
}
```

------
#### [ Description \(10\) ]

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
A [JobExecutionState](#jobs-mqtt-job-execution-state) object\. This field is included only when the `code` field has the value `InvalidStateTransition` or `VersionMismatch`\. This makes it unnecessary in these cases to perform a separate `DescribeJobExecution` request to obtain the current job execution status data\.

------