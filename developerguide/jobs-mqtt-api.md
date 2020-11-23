# Jobs device MQTT and HTTPS APIs<a name="jobs-mqtt-api"></a><a name="jobs-mqtt-note"></a>

**Topics**
+ [GetPendingJobExecutions](#mqtt-getpendingjobexecutions)
+ [StartNextPendingJobExecution](#mqtt-startnextpendingjobexecution)
+ [DescribeJobExecution](#mqtt-describejobexecution)
+ [UpdateJobExecution](#mqtt-updatejobexecution)
+ [JobExecutionsChanged](#mqtt-jobexecutionschanged)
+ [NextJobExecutionChanged](#mqtt-nextjobexecutionchanged)

**Note**  
Jobs device commands can be issued by publishing MQTT messages to the [Reserved topics used for Jobs commands](reserved-topics.md#reserved-topics-job)\. Your client is automatically subscribed to the response message topics of these commands, which means that the message broker will publish response message topics to the client that published the command message whether your client has subscribed to the response message topic or not\.   
Because the message broker publishes response messages, even without an explicit subscription to them, your client must be configured to receive and identify the messages it receives\. Your client must also confirm that the *thingName* in the incoming message topic applies to the client's thing name before the client acts on the message\.  
Messages that AWS IoT sends in response to MQTT Jobs API command messages are charged to your account, whether you subscribed to them explicitly or not\.

## GetPendingJobExecutions<a name="mqtt-getpendingjobexecutions"></a>

------
#### [ GetPendingJobExecutions command ]

Gets the list of all jobs for a thing that are not in a terminal state\.

------
#### [ MQTT \(12\) ]

To invoke this API, publish a message on `$aws/things/thingName/jobs/get`\.

Request payload:

```
{ "clientToken": "string" }
```

`clientToken`  
Optional\. A client token used to correlate requests and responses\. Enter an arbitrary value here and it is reflected in the response\.

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

`inProgressJobs`  
A list of [JobExecutionSummary](jobs-data-plane-data-types.md#jobs-mqtt-job-execution-summary) objects with status `IN_PROGRESS`\.

`queuedJobs`  
A list of [JobExecutionSummary](jobs-data-plane-data-types.md#jobs-mqtt-job-execution-summary) objects with status `QUEUED`\.

`clientToken`  
A client token used to correlate requests and responses\.

`timestamp`  
The time, in seconds since the epoch, when the message was sent\.

------
#### [ HTTPS \(12\) ]

Request:

```
GET /things/thingName/jobs
```

`thingName`  
The name of the thing associated with the device\.

Response:

```
{
"inProgressJobs" : [ JobExecutionSummary ... ], 
"queuedJobs" : [ JobExecutionSummary ... ]
}
```

`inProgressJobs`  
A list of [JobExecutionSummary](jobs-data-plane-data-types.md#jobs-mqtt-job-execution-summary) objects\.

`queuedJobs`  
A list of [JobExecutionSummary](jobs-data-plane-data-types.md#jobs-mqtt-job-execution-summary) objects\.

------
#### [ CLI \(12\) ]

**Synopsis:**

```
aws iot-jobs-data  get-pending-job-executions \
--thing-name <value>  \
[--cli-input-json <value>] \
[--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
"thingName": "string"
}
```


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  thingName  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the thing that is executing the job\.  | 

Output:

```
{
"inProgressJobs": [
{
  "jobId": "string",
  "queuedAt": long,
  "startedAt": long,
  "lastUpdatedAt": long,
  "versionNumber": long,
  "executionNumber": long
}
],
"queuedJobs": [
{
  "jobId": "string",
  "queuedAt": long,
  "startedAt": long,
  "lastUpdatedAt": long,
  "versionNumber": long,
  "executionNumber": long
}
]
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  inProgressJobs  |  list  member: JobExecutionSummary  java class: java\.util\.List  |  A list of JobExecutionSummary objects with status IN\_PROGRESS\.  | 
|  JobExecutionSummary  |  JobExecutionSummary  |   | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to this job when it was created\.  | 
|  queuedAt  |  long  |  The time, in seconds since the epoch, when the job execution was enqueued\.  | 
|  startedAt  |  long  java class: java\.lang\.Long  |  The time, in seconds since the epoch, when the job execution started\.  | 
|  lastUpdatedAt  |  long  |  The time, in seconds since the epoch, when the job execution was last updated\.  | 
|  versionNumber  |  long  |  The version of the job execution\. Job execution versions are incremented each time the AWS IoT Jobs service receives an update from a device\.  | 
|  executionNumber  |  long  java class: java\.lang\.Long  |  A number that identifies a job execution on a device\.  | 
|  queuedJobs  |  list  member: JobExecutionSummary  java class: java\.util\.List  |  A list of JobExecutionSummary objects with status QUEUED\.  | 
|  JobExecutionSummary  |  JobExecutionSummary  |   | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to this job when it was created\.  | 
|  queuedAt  |  long  |  The time, in seconds since the epoch, when the job execution was enqueued\.  | 
|  startedAt  |  long  java class: java\.lang\.Long  |  The time, in seconds since the epoch, when the job execution started\.  | 
|  lastUpdatedAt  |  long  |  The time, in seconds since the epoch, when the job execution was last updated\.  | 
|  versionNumber  |  long  |  The version of the job execution\. Job execution versions are incremented each time the AWS IoT Jobs service receives an update from a device\.  | 
|  executionNumber  |  long  java class: java\.lang\.Long  |  A number that identifies a job execution on a device\.  | 

------

## StartNextPendingJobExecution<a name="mqtt-startnextpendingjobexecution"></a>

------
#### [ StartNextPendingJobExecution command ]

Gets and starts the next pending job execution for a thing \(status IN\_PROGRESS or QUEUED\)\. 
+ Any job executions with status IN\_PROGRESS are returned first\.
+ Job executions are returned in the order in which they were created\.
+ If the next pending job execution is QUEUED, its state is changed to IN\_PROGRESS and the job execution's status details are set as specified\.
+ If the next pending job execution is already IN\_PROGRESS, its status details are not changed\.
+ If no job executions are pending, the response does not include the `execution` field\.
+ You can optionally create a step timer by setting a value for the `stepTimeoutInMinutes` property\. If you don't update the value of this property by running `UpdateJobExecution`, the job execution times out when the step timer expires\.

------
#### [ MQTT \(13\) ]

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

`clientToken`  
A client token used to correlate requests and responses\. Enter an arbitrary value here and it is reflected in the response\.

The message broker will publish `$aws/things/thingName/jobs/start-next/accepted` and `$aws/things/thingName/jobs/start-next/rejected` even without a specific subscription to them\. However, for your client to receive the messages, it must be listening for them\. For more information, see [the note about Jobs API messages](#jobs-mqtt-note)\.

Response payload:

```
{
"execution" : JobExecutionData,
"timestamp" : timestamp,
"clientToken" : "string"
}
```

`execution`  
A [JobExecution](jobs-data-plane-data-types.md#jobs-mqtt-job-execution-data) object\. For example:  

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

`timestamp`  
The time, in milliseconds since the epoch, when the message was sent to the device\.

`clientToken`  
A client token used to correlate requests and responses\.

------
#### [ HTTPS \(13\) ]

Request:

```
PUT /things/thingName/jobs/$next
{
"statusDetails": { 
    "string": "string" 
    ... 
},
"stepTimeoutInMinutes": long
}
```

`thingName`  
The name of the thing associated with the device\.

`statusDetails`  
A collection of name\-value pairs that describe the status of the job execution\. If not specified, the `statusDetails` are unchanged\.

`stepTimeOutInMinutes`  
Specifies the amount of time this device has to finish execution of this job\. If the job execution status is not set to a terminal state before this timer expires, or before the timer is reset \(by calling `UpdateJobExecution`, setting the status to `IN_PROGRESS` and specifying a new timeout value in field `stepTimeoutInMinutes`\) the job execution status is set to `TIMED_OUT`\. Setting this timeout has no effect on that job execution timeout that might have been specified when the job was created \(`CreateJob` using the `timeoutConfig` field\)\. 

Response:

```
{
"execution" :  JobExecution 
}
```

`execution`  
A [JobExecution](jobs-data-plane-data-types.md#jobs-mqtt-job-execution-data) object\. For example:  

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

------
#### [ CLI \(13\) ]

**Synopsis:**

```
aws iot-jobs-data  start-next-pending-job-execution \
--thing-name <value> \
{--step-timeout-in-minutes <value>] \
[--status-details <value>]  \
[--cli-input-json <value>] \
[--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
"thingName": "string",
"statusDetails": {
"string": "string"
},
"stepTimeoutInMinutes": long
}
```


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  thingName  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the thing associated with the device\.  | 
|  statusDetails  |  map  key: DetailsKey  value: DetailsValue  |  A collection of name\-value pairs that describe the status of the job execution\. If not specified, the statusDetails are unchanged\.  | 
|  stepTimeOutInMinutes  |  long  |   Specifies the amount of time this device has to finish execution of this job\. If the job execution status is not set to a terminal state before this timer expires, or before the timer is reset \(by calling `UpdateJobExecution`, setting the status to `IN_PROGRESS` and specifying a new timeout value in field `stepTimeoutInMinutes`\) the job execution status is set to `TIMED_OUT`\. Setting this timeout has no effect on that job execution timeout that might have been specified when the job was created \(`CreateJob` using the `timeoutConfig` field\)\.   | 
|  DetailsKey  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |   | 
|  DetailsValue  |  string  length max:1024 min:1  pattern: \[^\\\\p\{C\}\]\*\+  |   | 

Output:

```
{
"execution": {
"jobId": "string",
"thingName": "string",
"status": "string",
"statusDetails": {
  "string": "string"
},
"queuedAt": long,
"startedAt": long,
"lastUpdatedAt": long,
"versionNumber": long,
"executionNumber": long,
"jobDocument": "string"
}
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  execution  |  JobExecution  |  A JobExecution object\.  | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to this job when it was created\.  | 
|  thingName  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the thing that is executing the job\.  | 
|  status  |  string  enum: QUEUED \| IN\_PROGRESS \| SUCCEEDED \| FAILED \| TIMED\_OUT \| REJECTED \| REMOVED \| CANCELED  |  The status of the job execution\. Can be one of: QUEUED, IN\_PROGRESS, FAILED, SUCCEEDED, CANCELED, TIMED\_OUT, REJECTED, or REMOVED\.  | 
|  statusDetails  |  map  key: DetailsKey  value: DetailsValue  |  A collection of name\-value pairs that describe the status of the job execution\.  | 
|  DetailsKey  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |   | 
|  DetailsValue  |  string  length max:1024 min:1  pattern: \[^\\\\p\{C\}\]\*\+  |   | 
|  queuedAt  |  long  |  The time, in seconds since the epoch, when the job execution was enqueued\.  | 
|  startedAt  |  long  java class: java\.lang\.Long  |  The time, in seconds since the epoch, when the job execution was started\.  | 
|  lastUpdatedAt  |  long  |  The time, in seconds since the epoch, when the job execution was last updated\.   | 
|  versionNumber  |  long  |  The version of the job execution\. Job execution versions are incremented each time they are updated by a device\.  | 
|  executionNumber  |  long  java class: java\.lang\.Long  |  A number that identifies a job execution on a device\. It can be used later in commands that return or update job execution information\.  | 
|  jobDocument  |  string  length max:32768  |  The content of the job document\.  | 

------

## DescribeJobExecution<a name="mqtt-describejobexecution"></a>

------
#### [ DescribeJobExecution command ]

Gets detailed information about a job execution\.

You can set the `jobId` to `$next` to return the next pending job execution for a thing \(status IN\_PROGRESS or QUEUED\)\.

------
#### [ MQTT \(14\) ]

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
Or use `$next` to return the next pending job execution for a thing \(status IN\_PROGRESS or QUEUED\)\. In this case, any job executions with status IN\_PROGRESS are returned first\. Job executions are returned in the order in which they were created\.

`executionNumber`  
Optional\. A number that identifies a job execution on a device\. If not specified, the latest job execution is returned\.

`includeJobDocument`  
Optional\. Unless set to `false`, the response contains the job document\. The default is `true`\.

`clientToken`  
A client token used to correlate requests and responses\. Enter an arbitrary value here and it is reflected in the response\.

The message broker will publish `$aws/things/thingName/jobs/jobId/get/accepted` and `$aws/things/thingName/jobs/jobId/get/rejected` even without a specific subscription to them\. However, for your client to receive the messages, it must be listening for them\. For more information, see [the note about Jobs API messages](#jobs-mqtt-note)\.

Response payload:

```
{
"execution" : JobExecutionData,
"timestamp": "timestamp",
"clientToken": "string"
}
```

`execution`  
A [JobExecution](jobs-data-plane-data-types.md#jobs-mqtt-job-execution-data) object\.

`timestamp`  
The time, in seconds since the epoch, when the message was sent\.

`clientToken`  
A client token used to correlate requests and responses\.

------
#### [ HTTPS \(14\) ]

The job's execution status must be `QUEUED` or `IN_PROGRESS`\.

Request:

```
GET /things/thingName/jobs/jobId?executionNumber=executionNumber&includeJobDocument=includeJobDocument
```

`thingName`  
The name of the thing associated with the device\.

`jobId`  
The unique identifier assigned to this job when it was created\.   
Or use `$next` to return the next pending job execution for a thing \(status IN\_PROGRESS or QUEUED\)\. In this case, any job executions with status IN\_PROGRESS are returned first\. Job executions are returned in the order in which they were created\.

`includeJobDocument`  
Optional\. Unless set to `false`, the response contains the job document\. The default is `true`\.

`executionNumber`  
Optional\. A number that identifies a job execution on a device\. If not specified, the latest job execution is returned\.

Response:

```
{
"execution" : JobExecution,
}
```

`execution`  
A [JobExecution](jobs-data-plane-data-types.md#jobs-mqtt-job-execution-data) object\.

------
#### [ CLI \(14\) ]

The job's execution status must be `QUEUED` or `IN_PROGRESS`\.

**Synopsis:**

```
aws iot-jobs-data  describe-job-execution \
--job-id <value> \
--thing-name <value> \
[--include-job-document | --no-include-job-document] \
[--execution-number <value>]  \
[--cli-input-json <value>] \
[--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
"jobId": "string",
"thingName": "string",
"includeJobDocument": boolean,
"executionNumber": long
}
```


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  jobId  |  string  pattern: \[a\-zA\-Z0\-9\_\-\]\+\|^$next  |  The unique identifier assigned to this job when it was created, or `$next` to return the next pending job execution for a thing \(status IN\_PROGRESS or QUEUED\)\. In this case, any job executions with status IN\_PROGRESS are returned first\. Job executions are returned in the order in which they were created\.  | 
|  thingName  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The thing name associated with the device the job execution is running on\.  | 
|  includeJobDocument  |  boolean  java class: java\.lang\.Boolean  |  Optional\. Unless set to false, the response contains the job document\. The default is true\.  | 
|  executionNumber  |  long  java class: java\.lang\.Long  |  Optional\. A number that identifies a job execution on a device\. If not specified, the latest job execution is returned\.  | 

Output:

```
{
"execution": {
"jobId": "string",
"thingName": "string",
"status": "string",
"statusDetails": {
  "string": "string"
},
"queuedAt": long,
"startedAt": long,
"lastUpdatedAt": long,
"versionNumber": long,
"executionNumber": long,
"jobDocument": "string"
}
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  execution  |  JobExecution  |  Contains data about a job execution\.  | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to this job when it was created\.  | 
|  thingName  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the thing that is executing the job\.  | 
|  status  |  string  enum: QUEUED \| IN\_PROGRESS \| SUCCEEDED \| FAILED \| TIMED\_OUT \| REJECTED \| REMOVED \| CANCELED  |  The status of the job execution\. Can be one of: QUEUED, IN\_PROGRESS, FAILED, SUCCEEDED, CANCELED, TIMED\_OUT, REJECTED, or REMOVED\.  | 
|  statusDetails  |  map  key: DetailsKey  value: DetailsValue  |  A collection of name\-value pairs that describe the status of the job execution\.  | 
|  DetailsKey  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |   | 
|  DetailsValue  |  string  length max:1024 min:1  pattern: \[^\\\\p\{C\}\]\*\+  |   | 
|  queuedAt  |  long  |  The time, in seconds since the epoch, when the job execution was enqueued\.  | 
|  startedAt  |  long  java class: java\.lang\.Long  |  The time, in seconds since the epoch, when the job execution was started\.  | 
|  lastUpdatedAt  |  long  |  The time, in seconds since the epoch, when the job execution was last updated\.   | 
|  versionNumber  |  long  |  The version of the job execution\. Job execution versions are incremented each time they are updated by a device\.  | 
|  executionNumber  |  long  java class: java\.lang\.Long  |  A number that identifies a job execution on a device\. It can be used later in commands that return or update job execution information\.  | 
|  jobDocument  |  string  length max:32768  |  The content of the job document\.  | 

------

## UpdateJobExecution<a name="mqtt-updatejobexecution"></a>

------
#### [ UpdateJobExecution command ]

Updates the status of a job execution\. You can optionally create a step timer by setting a value for the `stepTimeoutInMinutes` property\. If you don't update the value of this property by running `UpdateJobExecution` again, the job execution times out when the step timer expires\.

------
#### [ MQTT \(15\) ]

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
The new status for the job execution \(IN\_PROGRESS, FAILED, SUCCEEDED, or REJECTED\)\. This must be specified on every update\.

`statusDetails`  
A collection of name\-value pairs that describe the status of the job execution\. If not specified, the `statusDetails` are unchanged\.

`expectedVersion`  
The expected current version of the job execution\. Each time you update the job execution, its version is incremented\. If the version of the job execution stored in the AWS IoT Jobs service does not match, the update is rejected with a `VersionMismatch` error, and an [ErrorResponse](jobs-data-plane-data-types.md#jobs-mqtt-error-response) that contains the current job execution status data is returned\. \(This makes it unnecessary to perform a separate `DescribeJobExecution` request to obtain the job execution status data\.\)

`executionNumber`  
Optional\. A number that identifies a job execution on a device\. If not specified, the latest job execution is used\.

`includeJobExecutionState`  
Optional\. When included and set to `true`, the response contains the `JobExecutionState` field\. The default is `false`\.

`includeJobDocument`  
Optional\. When included and set to `true`, the response contains the `JobDocument`\. The default is `false`\.

`stepTimeoutInMinutes`  
Specifies the amount of time this device has to finish execution of this job\. If the job execution status is not set to a terminal state before this timer expires, or before the timer is reset \(by again calling `UpdateJobExecution`, setting the status to `IN_PROGRESS` and specifying a new timeout value in this field\) the job execution status is set to `TIMED_OUT`\. Setting or resetting this timeout has no effect on the job execution timeout that might have been specified when the job was created \(by using `CreateJob` with the `timeoutConfig`\)\. 

`clientToken`  
A client token used to correlate requests and responses\. Enter an arbitrary value here and it is reflected in the response\.

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
A [JobExecutionState](jobs-data-plane-data-types.md#jobs-mqtt-job-execution-state) object\.

`jobDocument`  
A [job document](iot-jobs.md#key-concepts-jobs) object\.

`timestamp`  
The time, in seconds since the epoch, when the message was sent\.

`clientToken`  
A client token used to correlate requests and responses\.

------
#### [ HTTPS \(15\) ]

Request:

```
POST /things/thingName/jobs/jobId
{
"status": "job-execution-state",
"statusDetails": { 
    "string": "string"
    ...
},
"expectedVersion": "number",
"includeJobExecutionState": boolean,
"includeJobDocument": boolean,
"stepTimeoutInMinutes": long,
"executionNumber": long 
}
```

`thingName`  
The name of the thing associated with the device\.

`jobId`  
The unique identifier assigned to this job when it was created\.

`status`  
The new status for the job execution \(IN\_PROGRESS, FAILED, SUCCEEDED, or REJECTED\)\. This must be specified on every update\.

`statusDetails`  
Optional\. A collection of name\-value pairs that describe the status of the job execution\. If not specified, the `statusDetails` are unchanged\.

`expectedVersion`  
Optional\. The expected current version of the job execution\. Each time you update the job execution, its version is incremented\. If the version of the job execution stored in the AWS IoT Jobs service does not match, the update is rejected with a `VersionMismatch` error, and an [ErrorResponse](jobs-data-plane-data-types.md#jobs-mqtt-error-response) that contains the current job execution status data is returned\. \(This makes it unnecessary to perform a separate `DescribeJobExecution` request to obtain the job execution status data\.\)

`includeJobExecutionState`  
Optional\. When included and set to `true`, the response contains the JobExecutionState data\. The default is `false`\.

`includeJobDocument`  
Optional\. When set to `true`, the response contains the job document\. The default is `false`\.

`stepTimeoutInMinutes`  
Specifies the amount of time this device has to finish execution of this job\. If the job execution status is not set to a terminal state before this timer expires, or before the timer is reset \(by again calling `UpdateJobExecution`, setting the status to `IN_PROGRESS` and specifying a new timeout value in this field\) the job execution status is set to `TIMED_OUT`\. Setting or resetting this timeout has no effect on the job execution timeout that might have been specified when the job was created \(by using `CreateJob` with the `timeoutConfig`\)\. 

`executionNumber`  
Optional\. A number that identifies a job execution on a device\.

Response:

```
{
"executionState": JobExecutionState,
"jobDocument": "string"
}
```

`executionState`  
A [JobExecutionState](jobs-data-plane-data-types.md#jobs-mqtt-job-execution-state) object\.

`jobDocument`  
The contents of the [job document](iot-jobs.md#key-concepts-jobs)\.

------
#### [ CLI \(15\) ]

**Synopsis:**

```
aws iot-jobs-data  update-job-execution \
--job-id <value> \
--thing-name <value> \
--status <value> \
[--status-details <value>] \
[--expected-version <value>] \
[--include-job-execution-state | --no-include-job-execution-state] \
[--include-job-document | --no-include-job-document] \
[--execution-number <value>]  \
[--cli-input-json <value>] \
[--step-timeout-in-minutes <value>] \
[--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
"jobId": "string",
"thingName": "string",
"status": "string",
"statusDetails": {
"string": "string"
},
"stepTimeoutInMinutes": number,
"expectedVersion": long,
"includeJobExecutionState": boolean,
"includeJobDocument": boolean,
"executionNumber": long
}
```


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier assigned to this job when it was created\.  | 
|  thingName  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the thing associated with the device\.  | 
|  status  |  string  enum: QUEUED \| IN\_PROGRESS \| SUCCEEDED \| FAILED \| TIMED\_OUT \| REJECTED \| REMOVED \| CANCELED  |  The new status for the job execution \(IN\_PROGRESS, FAILED, SUCCEEDED, or REJECTED\)\. This must be specified on every update\.  | 
|  statusDetails  |  map  key: DetailsKey  value: DetailsValue  |   Optional\. A collection of name\-value pairs that describe the status of the job execution\. If not specified, the statusDetails are unchanged\.  | 
|  DetailsKey  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |   | 
|  DetailsValue  |  string  length max:1024 min:1  pattern: \[^\\\\p\{C\}\]\*\+  |   | 
|  stepTimeoutInMinutes long Specifies the amount of time this device has to finish execution of this job\. If the job execution status is not set to a terminal state before this timer expires, or before the timer is reset \(by again calling `UpdateJobExecution`, setting the status to `IN_PROGRESS` and specifying a new timeout value in this field\) the job execution status is set to `TIMED_OUT`\. Setting or resetting this timeout has no effect on the job execution timeout that might have been specified when the job was created \(by using `CreateJob` with the `timeoutConfig`\)\.   | 
|  expectedVersion  |  long  java class: java\.lang\.Long  |  Optional\. The expected current version of the job execution\. Each time you update the job execution, its version is incremented\. If the version of the job execution stored in the AWS IoT Jobs service does not match, the update is rejected with a VersionMismatch error, and an ErrorResponse that contains the current job execution status data is returned\. \(This makes it unnecessary to perform a separate DescribeJobExecution request to obtain the job execution status data\.\)  | 
|  includeJobExecutionState  |  boolean  java class: java\.lang\.Boolean  |  Optional\. When included and set to true, the response contains the JobExecutionState data\. The default is false\.  | 
|  includeJobDocument  |  boolean  java class: java\.lang\.Boolean  |  Optional\. When set to true, the response contains the job document\. The default is false\.  | 
|  executionNumber  |  long  java class: java\.lang\.Long  |  Optional\. A number that identifies a job execution on a device\.  | 

Output:

```
{
"executionState": {
"status": "string",
"statusDetails": {
  "string": "string"
},
"versionNumber": long
},
"jobDocument": "string"
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  executionState  |  JobExecutionState  |  A JobExecutionState object\.  | 
|  status  |  string  enum: QUEUED \| IN\_PROGRESS \| SUCCEEDED \| FAILED \| TIMED\_OUT \| REJECTED \| REMOVED \| CANCELED  |  The status of the job execution\. Can be one of: QUEUED, IN\_PROGRESS, FAILED, SUCCEEDED, CANCELED, TIMED\_OUT, REJECTED, or REMOVED\.  | 
|  statusDetails  |  map  key: DetailsKey  value: DetailsValue  |  A collection of name\-value pairs that describe the status of the job execution\.  | 
|  DetailsKey  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |   | 
|  DetailsValue  |  string  length max:1024 min:1  pattern: \[^\\\\p\{C\}\]\*\+  |   | 
|  versionNumber  |  long  |  The version of the job execution\. Job execution versions are incremented each time they are updated by a device\.  | 
|  jobDocument  |  string  length max:32768  |  The contents of the job documents\.  | 

------

## JobExecutionsChanged<a name="mqtt-jobexecutionschanged"></a>

------
#### [ JobExecutionsChanged message ]

Sent whenever a job execution is added to or removed from the list of pending job executions for a thing\.

------
#### [ MQTT \(16\) ]

Topic: `$aws/things/thingName/jobs/notify`

Message payload:

```
{
"jobs" : {
    "JobExecutionState": [ [JobExecutionSummary](jobs-control-plane-data-types.md#jobs-job-execution-summary) ... ]
},
"timestamp": timestamp,
}
```

------
#### [ HTTPS \(16\) ]

Not available\.

------
#### [ CLI \(16\) ]

Not available\.

------

## NextJobExecutionChanged<a name="mqtt-nextjobexecutionchanged"></a>

------
#### [ NextJobExecutionChanged message ]

Sent whenever there is a change to which job execution is next on the list of pending job executions for a thing, as defined for [DescribeJobExecution](#mqtt-describejobexecution) with jobId `$next`\. This message is not sent when the next job's execution details change, only when the next job that would be returned by `DescribeJobExecution` with jobId `$next` has changed\. Consider job executions J1 and J2 with state QUEUED\. J1 is next on the list of pending job executions\. If the state of J2 is changed to IN\_PROGRESS while the state of J1 remains unchanged, then this notification is sent and contains details of J2\.

------
#### [ MQTT \(17\) ]

Topic: `$aws/things/thingName/jobs/notify-next`

Message payload:

```
{
"execution" : [JobExecution](jobs-control-plane-data-types.md#jobs-job-execution),
"timestamp": timestamp,
}
```

------
#### [ HTTPS \(17\) ]

Not available\.

------
#### [ CLI \(17\) ]

Not available\.

------