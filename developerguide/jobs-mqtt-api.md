# Jobs device MQTT API<a name="jobs-mqtt-api"></a><a name="jobs-mqtt-note"></a>

You can issue jobs device commands by publishing MQTT messages to the [Reserved topics used for Jobs commands](reserved-topics.md#reserved-topics-job)\. 

Your device\-side client must be subscribed to the response message topics of these commands\. If you use the AWS IoT Device Client, your device will automatically subscribe to the response topics\. This means that the message broker will publish response message topics to the client that published the command message, whether your client has subscribed to the response message topics or not\. These response messages do not pass through the message broker and they cannot be subscribed to by other clients or rules\.

When subscribing to the job and `jobExecution` event topics for your fleet\-monitoring solution, first enable [job and job execution events](iot-events.md) to receive any events on the cloud side\. Job progress messages that are processed through the message broker and can be used by AWS IoT rules are published as [Jobs events](events-jobs.md)\. Because the message broker publishes response messages, even without an explicit subscription to them, your client must be configured to receive and identify the messages it receives\. Your client must also confirm that the *thingName* in the incoming message topic applies to the client's thing name before the client acts on the message\.

**Note**  
Messages that AWS IoT sends in response to MQTT Jobs API command messages are charged to your account, whether you subscribed to them explicitly or not\.

The following shows the MQTT API operations and their request and response syntax\. All MQTT API operations have the following parameters:

clientToken  
An optional client token that can be used to correlate requests and responses\. Enter an arbitrary value here and it is reflected in the response\.

`timestamp`  
The time in seconds since the epoch, when the message was sent\.

## GetPendingJobExecutions<a name="mqtt-getpendingjobexecutions"></a>

Gets the list of all jobs that are not in a terminal state, for a specified thing\.

To invoke this API, publish a message on `$aws/things/thingName/jobs/get`\.

Request payload:

```
{ "clientToken": "string" }
```

The message broker will publish `$aws/things/thingName/jobs/get/accepted` and `$aws/things/thingName/jobs/get/rejected` even without a specific subscription to them\. However, for your client to receive the messages, it must be listening for them\. For more information, see [the note about Jobs API messages](#jobs-mqtt-note)\.

Response payload:

```
{
"inProgressJobs" : [ JobExecutionSummary ... ], 
"queuedJobs" : [ JobExecutionSummary ... ],
"timestamp" : 1489096425069,
"clientToken" : "client-001"
}
```

Where `inProgressJobs` and `queuedJobs` return a list of [JobExecutionSummary](jobs-mqtt-https-api.md#jobs-mqtt-job-execution-summary) objects that have status of `IN_PROGRESS` or `QUEUED`\.

## StartNextPendingJobExecution<a name="mqtt-startnextpendingjobexecution"></a>

Gets and starts the next pending job execution for a thing \(status `IN_PROGRESS` or `QUEUED`\)\. 
+ Any job executions with status `IN_PROGRESS` are returned first\.
+ Job executions are returned in the order in which they were created\.
+ If the next pending job execution is `QUEUED`, its state changes to `IN_PROGRESS` and the job execution's status details are set as specified\.
+ If the next pending job execution is already `IN_PROGRESS`, its status details are not changed\.
+ If no job executions are pending, the response does not include the `execution` field\.
+ Optionally, you can create a step timer by setting a value for the `stepTimeoutInMinutes` property\. If you don't update the value of this property by running `UpdateJobExecution`, the job execution times out when the step timer expires\.

To invoke this API, publish a message on `$aws/things/thingName/jobs/start-next`\.

Request payload:

```
{ 
"statusDetails": {
    "string": "job-execution-state"
    ...
},
"stepTimeoutInMinutes": long,
"clientToken": "string"
}
```

`statusDetails`  
A collection of name\-value pairs that describe the status of the job execution\. If not specified, the `statusDetails` are unchanged\.

`stepTimeOutInMinutes`  
Specifies the amount of time this device has to finish execution of this job\. If the job execution status is not set to a terminal state before this timer expires, or before the timer is reset \(by calling `UpdateJobExecution`, setting the status to `IN_PROGRESS` and specifying a new timeout value in field `stepTimeoutInMinutes`\) the job execution status is set to `TIMED_OUT`\. Setting this timeout has no effect on that job execution timeout that might have been specified when the job was created \(`CreateJob` using the `timeoutConfig` field\)\. 

The message broker will publish `$aws/things/thingName/jobs/start-next/accepted` and `$aws/things/thingName/jobs/start-next/rejected` even without a specific subscription to them\. However, for your client to receive the messages, it must be listening for them\. For more information, see [the note about Jobs API messages](#jobs-mqtt-note)\.

Response payload:

```
{
"execution" : JobExecutionData,
"timestamp" : timestamp,
"clientToken" : "string"
}
```

Where `execution` is a [JobExecution](jobs-mqtt-https-api.md#jobs-mqtt-job-execution-data) object\. For example:

```
{
"execution" : {
    "jobId" : "022",
    "thingName" : "MyThing",
    "jobDocument" : "< contents of job document >",
    "status" : "IN_PROGRESS",
    "queuedAt" : 1489096123309,
    "lastUpdatedAt" : 1489096123309,
    "versionNumber" : 1,
    "executionNumber" : 1234567890
},
"clientToken" : "client-1",
"timestamp" : 1489088524284,
}
```

## DescribeJobExecution<a name="mqtt-describejobexecution"></a>

Gets detailed information about a job execution\.

You can set the `jobId` to `$next` to return the next pending job execution for a thing \(with a status of `IN_PROGRESS` or `QUEUED`\)\.

To invoke this API, publish a message on `$aws/things/thingName/jobs/jobId/get`\. 

Request payload:

```
{ 
"executionNumber": long,
"includeJobDocument": boolean,
"clientToken": "string" 
}
```

`thingName`  
The name of the thing associated with the device\.

`jobId`  
The unique identifier assigned to this job when it was created\.   
Or use `$next` to return the next pending job execution for a thing \(with a status of `IN_PROGRESS` or `QUEUED`\)\. In this case, any job executions with status `IN_PROGRESS` are returned first\. Job executions are returned in the order in which they were created\.

`executionNumber`  
\(Optional\) A number that identifies a job execution on a device\. If not specified, the latest job execution is returned\.

`includeJobDocument`  
\(Optional\) Unless set to `false`, the response contains the job document\. The default is `true`\.

The message broker will publish `$aws/things/thingName/jobs/jobId/get/accepted` and `$aws/things/thingName/jobs/jobId/get/rejected` even without a specific subscription to them\. However, for your client to receive the messages, it must be listening for them\. For more information, see [the note about Jobs API messages](#jobs-mqtt-note)\.

Response payload:

```
{
"execution" : JobExecutionData,
"timestamp": "timestamp",
"clientToken": "string"
}
```

Where `execution` is a [JobExecution](jobs-mqtt-https-api.md#jobs-mqtt-job-execution-data) object\.

## UpdateJobExecution<a name="mqtt-updatejobexecution"></a>

Updates the status of a job execution\. You can optionally create a step timer by setting a value for the `stepTimeoutInMinutes` property\. If you don't update the value of this property by running `UpdateJobExecution` again, the job execution times out when the step timer expires\.

To invoke this API, publish a message on `$aws/things/thingName/jobs/jobId/update`\. 

Request payload:

```
{
"status": "job-execution-state",
"statusDetails": { 
    "string": "string"
    ...
},
"expectedVersion": "number",
"executionNumber": long,
"includeJobExecutionState": boolean,
"includeJobDocument": boolean,
"stepTimeoutInMinutes": long,
"clientToken": "string"
}
```

`status`  
The new status for the job execution \(`IN_PROGRESS`, `FAILED`, `SUCCEEDED`, or `REJECTED`\)\. This must be specified on every update\.

`statusDetails`  
A collection of name\-value pairs that describe the status of the job execution\. If not specified, the `statusDetails` are unchanged\.

`expectedVersion`  
The expected current version of the job execution\. Each time you update the job execution, its version is incremented\. If the version of the job execution stored in the AWS IoT Jobs service does not match, the update is rejected with a `VersionMismatch` error, and an [ErrorResponse](jobs-api.md#jobs-mqtt-error-response) that contains the current job execution status data is returned\. \(This makes it unnecessary to perform a separate `DescribeJobExecution` request to obtain the job execution status data\.\)

`executionNumber`  
\(Optional\) A number that identifies a job execution on a device\. If not specified, the latest job execution is used\.

`includeJobExecutionState`  
\(Optional\) When included and set to `true`, the response contains the `JobExecutionState` field\. The default is `false`\.

`includeJobDocument`  
\(Optional\) When included and set to `true`, the response contains the `JobDocument`\. The default is `false`\.

`stepTimeoutInMinutes`  
Specifies the amount of time this device has to finish execution of this job\. If the job execution status is not set to a terminal state before this timer expires, or before the timer is reset \(by again calling `UpdateJobExecution`, setting the status to `IN_PROGRESS` and specifying a new timeout value in this field\) the job execution status is set to `TIMED_OUT`\. Setting or resetting this timeout has no effect on the job execution timeout that might have been specified when the job was created \(by using `CreateJob` with the `timeoutConfig`\)\. 

The message broker will publish `$aws/things/thingName/jobs/jobId/update/accepted` and `$aws/things/thingName/jobs/jobId/update/rejected` even without a specific subscription to them\. However, for your client to receive the messages, it must be listening for them\. For more information, see [the note about Jobs API messages](#jobs-mqtt-note)\.

Response payload:

```
{
"executionState": JobExecutionState,
"jobDocument": "string",
"timestamp": timestamp,
"clientToken": "string"
}
```

`executionState`  
A [JobExecutionState](jobs-mqtt-https-api.md#jobs-mqtt-job-execution-state) object\.

`jobDocument`  
A [job document](key-concepts-jobs.md) object\.

`timestamp`  
The time in seconds since the epoch, when the message was sent\.

`clientToken`  
A client token used to correlate requests and responses\.

When you use the MQTT protocol, you can also perform the following updates:

## JobExecutionsChanged<a name="mqtt-jobexecutionschanged"></a>

Sent whenever a job execution is added to or removed from the list of pending job executions for a thing\.

Use the topic:

`$aws/things/thingName/jobs/notify`

Message payload:

```
{
"jobs" : {
    "JobExecutionState": [ [https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionSummary.html](https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionSummary.html) ... ]
         },
    "timestamp": timestamp
}
```

## NextJobExecutionChanged<a name="mqtt-nextjobexecutionchanged"></a>

Sent whenever there is a change to which job execution is next on the list of pending job executions for a thing, as defined for [https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeJobExecution.html) with `jobId` `$next`\. This message is not sent when the next job's execution details change, only when the next job that would be returned by `DescribeJobExecution` with `jobId` `$next` has changed\. Consider job executions J1 and J2 with a status of `QUEUED`\. J1 is next on the list of pending job executions\. If the status of J2 is changed to `IN_PROGRESS` while the state of J1 remains unchanged, then this notification is sent and contains details of J2\.

Use the topic:

`$aws/things/thingName/jobs/notify-next`

Message payload:

```
{
"execution" : [https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecution.html),
"timestamp": timestamp,
}
```