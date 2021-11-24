# Job executions timeout configuration<a name="job-timeout-retry"></a>

Job timeouts make it possible to be notified whenever a job execution gets stuck in the `IN_PROGRESS` state for an unexpectedly long period of time\. There are two types of timers: in\-progress timers and step timers\. When the job is `IN_PROGRESS`, you can monitor and track the progress of your job execution\.

When you create a job, you can set a value for the `inProgressTimeoutInMinutes` property of the optional [TimeoutConfig](https://docs.aws.amazon.com/iot/latest/apireference/API_TimeoutConfig.html) object\. The in\-progress timer can't be updated and applies to all job executions for the job\. Whenever a job execution remains in the `IN_PROGRESS` status for longer than this interval, the job execution fails and switches to the terminal `TIMED_OUT` status\. AWS IoT also publishes an MQTT notification\.

You can also set a step timer for a job execution by setting a value for `stepTimeoutInMinutes` when you call [UpdateJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html)\. The step timer applies only to the job execution that you update\. You can set a new value for this timer each time you update a job execution\. You can also create a step timer when you call [StartNextPendingJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_StartNextPendingJobExecution.html)\. If the job execution remains in the `IN_PROGRESS` status for longer than the step timer interval, it fails and switches to the terminal `TIMED_OUT` status\. The step timer has no effect on the in\-progress timer that you set when you create a job\.

 The following diagram and description is an example that illustrates the ways in which in\-progress timeouts and step timeouts interact with each other in a 20\-minute timeout period\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/timeout-diagram.png)

**Job Creation:** `CreateJob` sets an in\-progress timer that expires in twenty minutes\. This timer applies to all job executions and can't be updated\.

**12:00 PM:** The job execution starts and switches to `IN_PROGRESS` status\. The in\-progress timer starts to run\.

**12:05 PM:** `UpdateJobExecution` creates a step timer with a value of 7 minutes\. If a new step timer isn't created, the job execution times out at 12:12 PM\.

**12:10 PM:** `UpdateJobExecution` creates a new step timer with a value of 5 minutes\. The previous step timer is discarded\. If a new step timer isn't created, the job execution times out at 12:15 PM\.

**12:13 PM:** `UpdateJobExecution` creates a new step timer with a value of 9 minutes\. The job execution times out at 12:20 because the in\-progress timer expires at 12:20\. The step timer can't exceed the absolute bound created by the in\-progress timer\.

`UpdateJobExecution` can also discard a step timer that has already been created by creating a new step timer with a value of \-1\.