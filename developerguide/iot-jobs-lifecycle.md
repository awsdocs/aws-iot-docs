# Jobs and job execution states<a name="iot-jobs-lifecycle"></a>

The following sections describe the lifecycle of an AWS IoT job and the lifecycle of a job execution\.

## Job states<a name="iot-jobs-states"></a>

The following diagram shows the different states of an AWS IoT job\.

![\[Image showing the different states of an AWS IoT job.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/job-states-diagram.png)

A job that you create using AWS IoT Jobs can be in one of the following states:
+ 

**IN\_PROGRESS**  
When you create a job using the AWS IoT console or the [CreateJob](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html) API, the job status changes to `IN_PROGRESS`\. All jobs that are in this state count towards the active jobs limit\.

  During job creation, AWS IoT Jobs starts rolling out job executions to devices in your target group\. A job in this phase has the `IsConcurrent` property set to true and counts towards the job concurrency limit\. Once all job executions have rolled out, AWS IoT Jobs waits for devices to complete the remote action\. A job in this phase has the `IsConcurrent` property set to false and does not count towards the job concurrency limit\.

  The active jobs limit and the job concurrency limit apply to both continuous and snapshot jobs\. For more information, see [Job limits](job-limits.md)\.
+ 

**COMPLETED**  
A continuous job is always in progress and continues to run for any new devices that are added to the target group\. For a snapshot job, the job status changes to `COMPLETED` when all its job executions enter a terminal state, such as `SUCCEEDED`, `FAILED`, `TIMED_OUT`, `REMOVED`, or `CANCELED`\.
+ 

**CANCELED**  
When you cancel a job using the AWS IoT console, the [CancelJob](https://docs.aws.amazon.com/iot/latest/apireference/API_CancelJob.html) API, or the [Job abort configuration](jobs-configurations-details.md#job-abort-using), the job status changes to `CANCELED`\.

  During job cancelation, AWS IoT Jobs starts canceling previously created job executions\. A job in this phase has the `IsConcurrent` property set to true and counts towards the job concurrency limit\. Once all job executions have been canceled, the `IsConcurrent` property is set to false and the job does not count towards the job concurrency limit\.

  The limit applies to both continuous and snapshot jobs\. For more information, see [Job limits](job-limits.md)\.
+ 

**DELETION\_IN\_PROGRESS**  
When you delete a job using the AWS IoT console or the [DeleteJob](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJob.html) API, the job status changes to `DELETION_IN_PROGRESS`\. During job deletion, AWS IoT Jobs starts deleting previously created job executions\. After all job executions have been deleted, the job disappears from your AWS account\.

## Job execution states<a name="iot-job-execution-states"></a>

The following diagram shows the different states of an AWS IoT job execution\.

![\[Image showing the different states of an AWS IoT job execution.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/job-execution-states-diagram.png)

A job execution that's rolled out when you create a job with AWS IoT Jobs can be in one of the following states:
+ 

**QUEUED**  
When AWS IoT Jobs rolls out a job execution for a target device, the job execution status is set to `QUEUED`\. The job execution remains in the `QUEUED` state until:
  + Your devices receives the job execution and invokes the Jobs APIs and reports the status as `IN_PROGRESS`\.
  + You cancel the job or job execution, or when the abort criteria that you specified using an abort configuration is met, and the status changes to `CANCELED`\.
  + Your device is removed from the target group and the status changes to `REMOVED`\.
+ 

**IN\_PROGRESS**  
If your IoT device subscribes to the reserved [Job topics](reserved-topics.md#reserved-topics-job) `$notify` and `$notify-next`, and your device invokes either the `StartPendingJobExecution` API or the `UpdateJobExecution` API with a status of `IN_PROGRESS`, AWS IoT Jobs will set the job execution status to `IN_PROGRESS`\. The `UpdateJobExecution` API can be invoked multiple times with a status of `IN_PROGRESS` and additional details about the execution steps using the `statusDetails` object\.
**Note**  
If you create multiple jobs for each device, AWS IoT Jobs and the MQTT protocol doesn't guarantee order of delivery\.
+ 

**SUCCEEDED**  
When your device successfully completes the remote operation, the device must invoke the `UpdateJobExecution` API with a status of `SUCCEEDED` to indicate that the job execution succeeded\. AWS IoT Jobs then updates and returns the job execution status as `SUCCEEDED`\. This is a terminal status\.
+ 

**FAILED**  
When your device fails to complete the remote operation, the device must invoke the `UpdateJobExecution` API with a status of `Failed` to indicate that the job execution failed\. AWS IoT Jobs then updates and returns the job execution status as `Failed`\. This is a terminal status\. You can retry this job execution for the device using the [Job execution retry configuration](jobs-configurations-details.md#job-retry-configuration)\.
+ 

**TIMED\_OUT**  
When your device fails to complete the remote operation within the timeout duration of the in\-progress timer, AWS IoT Jobs sets the job execution status to `TIMED_OUT`\. This timer duration is specified using the `inProgressTimeoutInMinutes` property of the [Job execution timeout configuration](jobs-configurations-details.md#job-timeout-configuration)\. This is a terminal status\. You can retry this job execution for the device using the [Job execution retry configuration](jobs-configurations-details.md#job-retry-configuration)\.
+ 

**REJECTED**  
When your device receives an invalid or incompatible request, the device must invoke the `UpdateJobExecution` API with a status of `REJECTED`\. AWS IoT Jobs then updates and returns the job execution status as `REJECTED`\. This is a terminal status\. You cannot retry this job execution for the device\.
+ 

**REMOVED**  
When your device is no longer a valid target for the job execution, such as when it's detached from a dynamic thing group, AWS IoT Jobs sets the job execution status to `REMOVED`\. You can re\-attach the thing to your target group and restart the job execution for the device\.
+ 

**CANCELED**  
When you cancel a job or cancel a job execution using the console or the `CancelJob` or `CancelJobExecution` API, or when the abort criteria specified using the [Job abort configuration](jobs-configurations-details.md#job-abort-using) is met, AWS IoT Jobs cancels the job and sets the job execution status to `CANCELED`\.