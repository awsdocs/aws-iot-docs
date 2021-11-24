# AWS IoT jobs APIs<a name="jobs-api"></a>

AWS IoT Jobs API can either be used for management and control of jobs, or for devices executing those jobs\. 

Job management and control uses an HTTPS protocol API\. Devices can use either an MQTT or an HTTPS protocol API\. The HTTPS API is designed for a low volume of calls typical when creating and tracking jobs\. It usually opens a connection for a single request, and then closes the connection after the response is received\. The MQTT API allows long polling\. It is designed for large amounts of traffic that can scale to millions of devices\.

Each AWS IoT Jobs HTTPS API has a corresponding command that allows you to call the API from the AWS CLI\. The commands are lowercase, with hyphens between the words that make up the name of the API\. For example, you can invoke the `CreateJob` API on the CLI by typing:

```
aws iot create-job ...
```

## Job management and control API<a name="jobs-http-api"></a>

**Topics**

To determine the *endpoint\-url* parameter for your CLI commands, run this command\.

```
aws iot describe-endpoint --endpoint-type=iot:Jobs
```

This command returns the following output\.

```
{
"endpointAddress": "account-specific-prefix.jobs.iot.aws-region.amazonaws.com"
}
```

**Note**  
The Jobs endpoint doesn't support ALPN `z-amzn-http-ca`\.

Use the following API operations or CLI commands:
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_AssociateTargetsWithJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_AssociateTargetsWithJob.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/associate-targets-with-job.html](https://docs.aws.amazon.com/cli/latest/reference/iot/associate-targets-with-job.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_CancelJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_CancelJob.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/cancel-job.html](https://docs.aws.amazon.com/cli/latest/reference/iot/cancel-job.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_CancelJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_CancelJobExecution.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/cancel-job-execution.html](https://docs.aws.amazon.com/cli/latest/reference/iot/cancel-job-execution.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/create-job.html](https://docs.aws.amazon.com/cli/latest/reference/iot/create-job.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJob.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/delete-job.html](https://docs.aws.amazon.com/cli/latest/reference/iot/delete-job.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJobExecution.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/deete-job-execution.html](https://docs.aws.amazon.com/cli/latest/reference/iot/deete-job-execution.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJobExecution.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/describe-job.html](https://docs.aws.amazon.com/cli/latest/reference/iot/describe-job.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJobExecution.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/describe-job-execution.html](https://docs.aws.amazon.com/cli/latest/reference/iot/describe-job-execution.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJobExecution.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/get-job-document.html](https://docs.aws.amazon.com/cli/latest/reference/iot/get-job-document.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_ListJobExecutionsForJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_ListJobExecutionsForJob.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/list-job-executions-for-job.html](https://docs.aws.amazon.com/cli/latest/reference/iot/list-job-executions-for-job.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_ListJobExecutionsForJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_ListJobExecutionsForJob.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/list-job-executions-for-thing.html](https://docs.aws.amazon.com/cli/latest/reference/iot/list-job-executions-for-thing.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_ListJobs.html](https://docs.aws.amazon.com/iot/latest/apireference/API_ListJobs.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/list-jobs.html](https://docs.aws.amazon.com/cli/latest/reference/iot/list-jobs.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateJob.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/update-job.html](https://docs.aws.amazon.com/cli/latest/reference/iot/update-job.html) 

## Job management and control data types<a name="jobs-control-plane-data-types"></a>

The following data types are used by management and control applications to communicate with AWS IoT Jobs\.
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_Job.html](https://docs.aws.amazon.com/iot/latest/apireference/API_Job.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/job.html](https://docs.aws.amazon.com/cli/latest/reference/iot/job.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_JobSummary.html](https://docs.aws.amazon.com/iot/latest/apireference/API_JobSummary.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/job-summary.html](https://docs.aws.amazon.com/cli/latest/reference/iot/job-summary.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecution.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/job-execution.html](https://docs.aws.amazon.com/cli/latest/reference/iot/job-execution.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionSummary.html](https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionSummary.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/job-execution-summary.html](https://docs.aws.amazon.com/cli/latest/reference/iot/job-execution-summary.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionSummaryForJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionSummaryForJob.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/job-execution-summary-for-job.html](https://docs.aws.amazon.com/cli/latest/reference/iot/job-execution-summary-for-job.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionSummaryForThing.html](https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionSummaryForThing.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/job-execution-summary-for-thing.html](https://docs.aws.amazon.com/cli/latest/reference/iot/job-execution-summary-for-thing.html) 

## Jobs device MQTT and HTTPS APIs<a name="jobs-mqtt-api"></a><a name="jobs-mqtt-note"></a>

Jobs device commands can be issued by publishing MQTT messages to the [Reserved topics used for Jobs commands](reserved-topics.md#reserved-topics-job)\. Your client is automatically subscribed to the response message topics of these commands, which means that the message broker will publish response message topics to the client that published the command message whether your client has subscribed to the response message topics or not\. When subscribing to the job and `jobExecution` event topics for your fleet\-monitoring solution, first enable [job and job execution events](iot-events.md) to receive any events on the cloud side\.

Because the message broker publishes response messages, even without an explicit subscription to them, your client must be configured to receive and identify the messages it receives\. Your client must also confirm that the *thingName* in the incoming message topic applies to the client's thing name before the client acts on the message\.

Messages that AWS IoT sends in response to MQTT Jobs API command messages are charged to your account, whether you subscribed to them explicitly or not\.

The following commands are available over the MQTT and HTTPS protocols\.
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_GetPendingJobExecutions.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_GetPendingJobExecutions.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot-data/get-pending-job-executions.html](https://docs.aws.amazon.com/cli/latest/reference/iot-data/get-pending-job-executions.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_StartNextPendingJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_StartNextPendingJobExecution.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot-data/start-next-pending-job-execution.html](https://docs.aws.amazon.com/cli/latest/reference/iot-data/start-next-pending-job-execution.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeJobExecution.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot-data/describe-job-execution.html](https://docs.aws.amazon.com/cli/latest/reference/iot-data/describe-job-execution.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot-data/update-job-execution.html](https://docs.aws.amazon.com/cli/latest/reference/iot-data/update-job-execution.html) 

When you use the MQTT protocol, you can also perform the following updates:

### JobExecutionsChanged<a name="mqtt-jobexecutionschanged"></a>

Sent whenever a job execution is added to or removed from the list of pending job executions for a thing\.

Use the topic:

`$aws/things/thingName/jobs/notify`

Message payload:

```
{
"jobs" : {
    "JobExecutionState": [ [ `DescribeJobExecution`](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJobExecution.html) ... ]
},
"timestamp": timestamp,
}
```

### NextJobExecutionChanged<a name="mqtt-nextjobexecutionchanged"></a>

Sent whenever there is a change to which job execution is next on the list of pending job executions for a thing, as defined for [ `DescribeJobExecution`](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJobExecution.html) with jobId `$next`\. This message is not sent when the next job's execution details change, only when the next job that would be returned by `DescribeJobExecution` with jobId `$next` has changed\. Consider job executions J1 and J2 with state QUEUED\. J1 is next on the list of pending job executions\. If the state of J2 is changed to IN\_PROGRESS while the state of J1 remains unchanged, then this notification is sent and contains details of J2\.

Use the topic:

`$aws/things/thingName/jobs/notify-next`

Message payload:

```
{
"execution" : [https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecution.html),
"timestamp": timestamp,
}
```

## Jobs device MQTT and HTTPS data types<a name="jobs-data-plane-data-types"></a>

The following data types are used to communicate with the AWS IoT Jobs service over the MQTT and HTTPS protocols\.
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_JobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_JobExecution.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot-data/job-execution.html](https://docs.aws.amazon.com/cli/latest/reference/iot-data/job-execution.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_JobExecutionState.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_JobExecutionState.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot-data/job-execution-state.html](https://docs.aws.amazon.com/cli/latest/reference/iot-data/job-execution-state.html) 
+ [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_JobExecutionSummary.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_JobExecutionSummary.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot-data/job-execution-summary.html](https://docs.aws.amazon.com/cli/latest/reference/iot-data/job-execution-summary.html) 

For errors occured during an operation, you get an error response that contains information about the error\.

### ErrorResponse<a name="jobs-mqtt-error-response"></a>

Contains information about an error that occurred during an AWS IoT Jobs service operation\.

Following shows the syntax of this operation:

```
{
    "code": "ErrorCode",
    "message": "string",
    "clientToken": "string",
    "timestamp": timestamp,
    "executionState": JobExecutionState
}
```

Following is a description of this `ErrorResponse`:

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