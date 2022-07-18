# How job configurations work<a name="jobs-configurations-details"></a>

You use the rollout and abort configurations when you're deploying a job, and the timeout and retry configurations for job execution\. The following sections shows more information about how these configurations work\.

**Topics**
+ [Job rollout and abort configurations](#job-rollout-abort)
+ [Job execution timeout and retry configurations](#job-timeout-retry)

## Job rollout and abort configurations<a name="job-rollout-abort"></a>

You can use the job rollout and abort configurations to define how many devices receive the job document every minute, and also the criteria for canceling a job when a certain number of devices don't receive a job document\.

### Job rollout configuration<a name="job-rollout-configuration"></a>

You can specify how quickly targets are notified of a pending job execution\. You can also create a staged rollout to better manage updates, reboots, and other operations\. To specify how your targets are notified, use job rollout rates\.

#### Job rollout rates<a name="job-rollout-using"></a>

You can create a rollout configuration by using either a constant rollout rate or an exponential rollout rate\. To specify the maximum number of job targets to inform per minute, use a constant rollout rate\.

AWS IoT jobs can be deployed using exponential rollout rates as various criteria and thresholds are met\. If the number of failed jobs matches a set of criteria that you specify, then you can cancel the job rollout\. You set the job rollout rate criteria when you create a job by using the [https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionsRolloutConfig.html](https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionsRolloutConfig.html) object\. You also set the job abort criteria at job creation by using the [https://docs.aws.amazon.com/iot/latest/apireference/API_AbortConfig.html](https://docs.aws.amazon.com/iot/latest/apireference/API_AbortConfig.html) object\.

The following example shows how rollout rates work\. For example, a job rollout with a base rate of 50 per minute, increment factor of 2, and number of notified and succeeded devices each as 1000, would work as follows: The job will start at a rate of 50 job executions per minute and continue at that rate until either 1000 things have received job execution notifications or 1000 successful job executions have occurred\. 

The following table illustrates how the rollout would proceed over the first four increments\.


|  |  |  |  |  | 
| --- |--- |--- |--- |--- |
|  Rollout rate per minute  |  50  |  100  |  200  |  400  | 
|  Number of notified devices or successful job executions to satisfy a rate increase  |  1000  |  2000  |  3000  |  4000  | 

#### Job rollout rates for continuous jobs using dynamic thing groups<a name="job-rollout-dynamic-groups"></a>

When you use a continuous job to roll out remote operations on your fleet, AWS IoT Jobs rolls out job executions for devices in your target thing group\. For any new devices that are added to the dynamic thing group, these job executions continue to roll out to the newly added devices even after the job has been created\.

The rollout configuration can control the rollout rates only for devices that are added to the group until job creation\. After a job has been created, for any new devices, the job executions are created in near\-real time as soon as the devices join the target group\.

### Job abort configuration<a name="job-abort-using"></a>

Use this configuration to create a criteria to cancel a job when a threshold percentage of devices meet that criteria\. For example, you can use this configuration to cancel a job in the following cases: 
+ When a threshold percentage of your devices don't receive the job execution notifications, such as when your device is incompatible for an OTA update\. In this case, your device can report a `REJECTED` status\.
+ When a threshold percentage of devices report failure for their job executions, such as when your device encounters a disconnection when attempting to download the job document from an S3 URL\. In such cases, your device must be programmed to report the `FAILURE` status to AWS IoT\.
+ When a `TIMED_OUT` status is reported as the job execution times out for a threshold percentage of devices after the job executions have started\. 
+ When there are multiple retry failures\. When you add a retry configuration, each retry attempt can incur additional charges to your AWS account\. In such cases, canceling the job can cancel queued job executions and avoid retry attempts for these executions\. For more information about the retry configuration and using it with the abort configuration, see [Job execution timeout and retry configurations](#job-timeout-retry)\.

You can set up a job abort condition by using the AWS IoT console or the AWS IoT Jobs API\.

## Job execution timeout and retry configurations<a name="job-timeout-retry"></a>

Use the job execution timeout configuration to send you [Jobs notifications](jobs-comm-notifications.md) when a job execution has been in progress for longer than the set duration\. Use the job execution retry configuration to retry the execution when the job fails or times out\.

### Job execution timeout configuration<a name="job-timeout-configuration"></a>

Use the job execution timeout configuration to notify you whenever a job execution gets stuck in the `IN_PROGRESS` state for an unexpectedly long period of time\. When the job is `IN_PROGRESS`, you can monitor the progress of your job execution\.

#### Timers for job timeouts<a name="job-timeout-timers"></a>

There are two types of timers: in\-progress timers and step timers\.

**In\-progress timers**  
When you create a job or a job template, you can specify a value for the in\-progress timer that's between 1 minute and 7 days\. You can update the value of this timer until the start of your job execution\. After your timer starts, it can't be updated and the timer value applies to all job executions for the job\. Whenever a job execution remains in the `IN_PROGRESS` status for longer than this interval, the job execution fails and switches to the terminal `TIMED_OUT` status\. AWS IoT also publishes an MQTT notification\.

**Step timer**  
You can also set a step timer that applies to only the job execution you want to update\. This timer has no effect on the in\-progress timer\. Each time you update a job execution, you can set a new value for the step timer\. You can also create a new step timer when starting the next pending job execution for a thing\. If the job execution remains in the `IN_PROGRESS` status for longer than the step timer interval, it fails and switches to the terminal `TIMED_OUT` status\.

**Note**  
You can set the in\-progress timer by using the AWS IoT console or the AWS IoT Jobs API\. To specify the step timer, use the API\.

#### How timers work for job timeouts<a name="job-timeout-timers-works"></a>

The following illustrates the ways in which in\-progress timeouts and step timeouts interact with each other in a 20\-minute timeout period\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/timeout-diagram.png)

The following shows the different steps:

1. 

**12:00**  
A new job is created and an in\-progress timer for twenty minutes is started when creating a job\. The in\-progress timer starts to run and the job execution switches to `IN_PROGRESS` status\.

1. 

**12:05 PM**  
A new step timer with a value of 7 minutes is created\. The job execution will now time out at 12:12 PM\.

1. 

**12:10 PM**  
A new step timer with a value of 5 minutes is created\. When a new step timer is created, the previous step timer is discarded and the job execution will now time out at 12:15 PM\.

1. 

**12:13 PM**  
A new step timer with a value of 9 minutes is created\. The previous step timer is discarded and the job execution will now time out at 12:20 PM because the in\-progress timer times out at 12:20 PM\. The step timer can't exceed the in\-progress timer's absolute bound\.

### Job execution retry configuration<a name="job-retry-configuration"></a>

You can use the retry configuration to retry the job execution when a certain set of criteria is met\. A retry can be attempted when a job times out or when the device fails\. To retry execution because of a timeout failure, you must enable the timeout configuration\.

**How to use the retry configuration**  
The following shows how to use the retry configuration:

1. Determine whether to use the retry configuration for `FAILED`, `TIMED_OUT`, or both failure criteria\. For the `TIMED_OUT` status, after the status is reported, AWS IoT Jobs automatically retries the job execution for the device\.

1. For the `FAILED` status, check whether your job execution failure can be retried\. If it's retryable, program your device to report a `FAILURE` status to AWS IoT\. The following section describes more about retryable and non\-retryable failures\. 

1. Specify the number of retries to use for each failure type by using the preceding information\. For a single device, you can specify up to 10 retries for both failure types combined\. The retry attempts stop automatically when an execution succeeds or when it reaches the specified number of attempts\.

1. Add an abort configuration to cancel the job in case of repeated retry failures, thereby avoiding additional charges from being incurred in case there's a large number of retry attempts\.

**Retry and abort configuration**  
Each retry attempt incurs additional charges to your AWS account\. To avoid incurring additional charges from repeated retry failures, we recommend adding an abort configuration\. For more information about pricing, see [AWS IoT Device Management pricing](http://aws.amazon.com/iot-device-management/pricing/)\.

You might encounter multiple retry failures when a high threshold percentage of your devices either time out or report failure\. In this case, you can use the abort configuration to cancel the job and avoid any queued job executions or further retry attempts\.

**Note**  
When the abort criteria is met for canceling a job execution, only `QUEUED` job executions are canceled\. Any queued retries for the device will not be attempted\. However, current job executions that have an `IN_PROGRESS` status will not be canceled\.

Before retrying a failed job execution, we also recommend that you check whether your job execution failure is retryable, as described in the following section\.

**Retry for failure type of `FAILED`**  
To attempt retries for failure type of `FAILED`, your devices must be programmed to report the `FAILURE` status for a failed job execution to AWS IoT\. You must also set the retry configuration with the criteria to retry `FAILED` job executions and specify the number of retries to be performed\. When AWS IoT Jobs detects the FAILURE status, it will then automatically attempt to retry the job execution for the device\. The retries continue until the job execution succeeds or when it reaches the maximum number of retry attempts\.

You can keep track of each retry attempt and the job that's running on these devices\. By tracking the execution status, after the specified number of retries have been attempted, you can use your device to report failures and initiate another retry attempt\. 

**Retryable and non\-retryable failures**  
Your job execution failure can be retryable or non\-retriable\. Each retry attempt can incur charges to your AWS account\. To avoid incurring additional charges from multiple retry attempts, first consider checking whether your job execution failure is retryable\. An example of retryable failure includes a connection error that your device encounters while attempting to download the job document from an S3 URL\. If your job execution failure is retryable, you can program your device to report a `FAILURE` status in case the job execution fails and set the retry configuration to retry `FAILED` executions\. 

If the execution cannot be retried, to avoid retrying the job execution and potentially incurring additional charges to your account, we recommend that you program the device to report a `REJECTED` status to AWS IoT\. Examples of non\-retryable failure include when your device is incompatible of receiving a job update, or when it experiences a memory error while executing a job\. In these cases, AWS IoT Jobs will not retry the job execution because it retries the job execution only when it detects a `FAILED` or `TIMED_OUT` status\. 

After you've determined that a job execution failure is retryable, if a retry attempt still fails, consider checking the device logs\.

**Retry for failure type of `TIMEOUT`**  
If you enable timeout when creating a job, then AWS IoT Jobs will attempt to retry the job execution for the device when the status changes from `IN_PROGRESS` to `TIMED_OUT`\. This status change can occur when the in\-progress timer times out, or when a step timer that you specify is in `IN_PROGRESS` and then times out\. The retries continue until the job execution succeeds or when it reaches the maximum number of retry attempts for this failure type\.

**Continuous jobs and thing group membership updates**  
For continuous jobs that have job status as `IN_PROGRESS`, the number of retry attempts is reset to zero when there are updates to a thing's group membership\. For example, consider that you specified five retry attempts and three retries have already been performed\. If a thing is now removed from the thing group and it then rejoins the group, such as in the case of dynamic thing groups, the number of retry attempts is reset to zero\. You can now perform five retry attempts for your thing group instead of the two attempts that were remaining\. In addition, when a thing is removed from the thing group, additional retry attempts are canceled\.