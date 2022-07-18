# Jobs device HTTP API<a name="jobs-http-device-api"></a>

Devices can communicate with AWS IoT Jobs using HTTP Signature Version 4 on port 443\. This is the method used by the AWS SDKs and CLI\. For more information about those tools, see [AWS CLI Command Reference:iot\-jobs\-data](https://docs.aws.amazon.com/cli/latest/reference/iot-jobs-data/index.html) or [AWS SDKs and Tools](http://aws.amazon.com/tools/#sdk)\.

The following commands are available for devices executing the jobs\. For information about using API operations with the MQTT protocol, see [Jobs device MQTT API](jobs-mqtt-api.md)\.

## GetPendingJobExecutions<a name="http-getpendingjobexecutions"></a>

Gets the list of all jobs that are not in a terminal state, for a specified thing\.

------
#### [ HTTPS request ]

```
GET /things/thingName/jobs
```

Response:

```
{
"inProgressJobs" : [ JobExecutionSummary ... ], 
"queuedJobs" : [ JobExecutionSummary ... ]
}
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_GetPendingJobExecutions.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_GetPendingJobExecutions.html)\. 

------
#### [ CLI syntax ]

```
aws iot-jobs-data get-pending-job-executions \
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

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/iot-jobs-data/get-pending-job-executions.html](https://docs.aws.amazon.com/cli/latest/reference/iot-jobs-data/get-pending-job-executions.html)\.

------

## StartNextPendingJobExecution<a name="http-startnextpendingjobexecution"></a>

Gets and starts the next pending job execution for a thing \(with a status of `IN_PROGRESS` or `QUEUED`\)\. 
+ Any job executions with status `IN_PROGRESS` are returned first\.
+ Job executions are returned in the order in which they were created\.
+ If the next pending job execution is `QUEUED`, its status changes to `IN_PROGRESS` and the job execution's status details are set as specified\.
+ If the next pending job execution is already `IN_PROGRESS`, its status details do not change\.
+ If no job executions are pending, the response does not include the `execution` field\.
+ Optionally, you can create a step timer by setting a value for the `stepTimeoutInMinutes` property\. If you don't update the value of this property by running `UpdateJobExecution`, the job execution times out when the step timer expires\.

------
#### [ HTTPS request ]

The following example shows the request syntax:

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

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_StartNextPendingJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_StartNextPendingJobExecution.html)\.

------
#### [ CLI syntax ]

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

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/iot-jobs-data/start-next-pending-job-execution.html](https://docs.aws.amazon.com/cli/latest/reference/iot-jobs-data/start-next-pending-job-execution.html)\.

------

## DescribeJobExecution<a name="http-describejobexecution"></a>

Gets detailed information about a job execution\.

You can set the `jobId` to `$next` to return the next pending job execution for a thing\. The job's execution status must be `QUEUED` or `IN_PROGRESS`\.

------
#### [ HTTPS request ]

Request:

```
GET /things/thingName/jobs/jobId?executionNumber=executionNumber&includeJobDocument=includeJobDocument
```

Response:

```
{
"execution" : JobExecution,
}
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_DescribeJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_DescribeJobExecution.html)\.

------
#### [ CLI syntax ]

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

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/iot-data/describe-job-execution.html](https://docs.aws.amazon.com/cli/latest/reference/iot-data/describe-job-execution.html)\.

------

## UpdateJobExecution<a name="http-updatejobexecution"></a>

Updates the status of a job execution\. Optionally, you can create a step timer by setting a value for the `stepTimeoutInMinutes` property\. If you don't update the value of this property by running `UpdateJobExecution` again, the job execution times out when the step timer expires\.

------
#### [ HTTPS request ]

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

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html)\.

------
#### [ CLI syntax ]

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

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/iot-data/update-job-execution.html](https://docs.aws.amazon.com/cli/latest/reference/iot-data/update-job-execution.html)\.

------