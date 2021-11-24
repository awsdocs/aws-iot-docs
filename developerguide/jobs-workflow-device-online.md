# Device workflow<a name="jobs-workflow-device-online"></a>

A device can handle jobs that it runs using either of the following ways\. 
+ 

**Get the next job**

  1. When a device first comes online, it should subscribe to the device's `notify-next` topic\.

  1. Call the [ `DescribeJobExecution`](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_DescribeJobExecution.html) MQTT API with jobId `$next` to get the next job, its job document, and other details, including any state saved in `statusDetails`\. If the job document has a code file signature, you must verify the signature before proceeding with processing the job request\.

  1. Call the [UpdateJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html) MQTT API to update the job status\. Or, to combine this and the previous step in one call, the device can call [ `StartNextPendingJobExecution`](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_StartNextPendingJobExecution.html)\.

  1. \(Optional\) You can add a step timer by setting a value for `stepTimeoutInMinutes` when you call either [UpdateJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html) or [ `StartNextPendingJobExecution`](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_StartNextPendingJobExecution.html)

  1. Perform the actions specified by the job document using the [UpdateJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html) MQTT API to report on the progress of the job\.

  1. Continue to monitor the job execution by calling the [ `DescribeJobExecution`](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_DescribeJobExecution.html) MQTT API with this jobId\. If the job execution is deleted, `DescribeJobExecution` returns a `ResourceNotFoundException`\.

     The device should be able to recover to a valid state if the job execution is canceled or deleted while the device is running the job\.

  1. Call the [UpdateJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html) MQTT API when finished with the job to update the job status and report success or failure\.

  1. Because this job's execution status has been changed to a terminal state, the next job available for execution \(if any\) changes\. The device is notified that the next pending job execution has changed\. At this point, the device should continue as described in step 2\.

     If the device remains online, it continues to receive a notifications of the next pending job execution, including its job execution data, when it completes a job or a new pending job execution is added\. When this occurs, the device continues as described in step 2\.
+ 

**Pick from available jobs**

  1. When a device first comes online, it should subscribe to the thing's `notify` topic\.

  1. Call the [ `GetPendingJobExecutions`](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_GetPendingJobExecutions.html) MQTT API to get a list of pending job executions\.

  1. If the list contains one or more job executions, pick one\.

  1. Call the [ `DescribeJobExecution`](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_DescribeJobExecution.html) MQTT API to get the job document and other details, including any state saved in `statusDetails`\.

  1. Call the [UpdateJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html)MQTT API to update the job status\. If the `includeJobDocument` field is set to `true` in this command, the device can skip the previous step and retrieve the job document at this point\.

  1. Optionally, you can add a step timer by setting a value for `stepTimeoutInMinutes` when you call [UpdateJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html)\.

  1. Perform the actions specified by the job document using the [UpdateJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html) MQTT API to report on the progress of the job\.

  1. Continue to monitor the job execution by calling the [ `DescribeJobExecution`](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_DescribeJobExecution.html) MQTT API with this jobId\. If the job execution is canceled or deleted while the device is running the job, the device should be capable of recovering to a valid state\.

  1. Call the [UpdateJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html) MQTT API when finished with the job to update the job status and to report success or failure\.

     If the device remains online, it is notified of all pending job executions when a new pending job execution becomes available\. When this occurs, the device can continue as described in step 2\.

If the device is unable to execute the job, it calls the [UpdateJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html) MQTT API to update the job status to `REJECTED`\.