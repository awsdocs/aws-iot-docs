# Jobs<a name="iot-jobs"></a>

AWS IoT jobs can be used to define a set of remote operations that are sent to and executed on one or more devices connected to AWS IoT\.

## Jobs Key Concepts<a name="key-concepts-jobs"></a>

job  
A job is a remote operation that is sent to and executed on one or more devices connected to AWS IoT\. For example, you can define a job that instructs a set of devices to download and install application or firmware updates, reboot, rotate certificates, or perform remote troubleshooting operations\.

job document  
To create a job, you must first create a job document that is a description of the remote operations to be performed by the devices\.  
Job documents are UTF\-8 encoded JSON documents and should contain information that your devices need to perform a job\. A job document contains one or more URLs where the device can download an update or some other data\. The job document can be stored in an Amazon S3 bucket, or be included inline with the command that creates the job\.

target  
When you create a job, you specify a list of targets that are the devices that should perform the operations\. The targets can be things or [thing groups](thing-groups.md) or both\. The AWS IoT Jobs service sends a message to each target to inform it that a job is available\.

job execution  
A job execution is an instance of a job on a target device\. The target starts an execution of a job by downloading the job document\. It then performs the operations specified in the document, and reports its progress to AWS IoT\. An execution number is a unique identifier of a job execution on a specific target\. The Jobs service provides commands to track the progress of a job execution on a target and the progress of a job across all targets\.

snapshot job  
By default, a job is sent to all targets that you specify when you create the job\. After those targets complete the job \(or report that they are unable to do so\), the job is complete\.

continuous job  
A continuous job is sent to all targets that you specify when you create the job\. It continues to run and is sent to any new devices \(things\) that are added to the target group\. For example, a continuous job can be used to onboard or upgrade devices as they are added to a group\. You can make a job continuous by setting an optional parameter when you create the job\.

rollouts  
You can specify how quickly targets are notified of a pending job execution\. This allows you to create a staged rollout to better manage updates, reboots, and other operations\.  
The following field can be added to the `CreateJob` request to specify the maximum number of job targets to inform per minute\. This example sets a static rollout rate\.  

```
    "jobExecutionRolloutConfig": {
        "maximumPerMinute": "integer"
    }
```
You can also set a variable rollout rate with the `exponentialRate` field\. The following example creates a rollout that has an exponential rate\.  

```
  "jobExecutionsRolloutConfig": { 
      "exponentialRate": { 
         "baseRatePerMinute": integer,
         "incrementFactor": integer,
         "rateIncreaseCriteria": { 
            "numberOfNotifiedThings": integer, // Set one or the other
            "numberOfSucceededThings": integer // of these two values.
         },
         "maximumPerMinute": integer
    }
}
```
For more information about configuring job rollouts, see [Job Rollout and Abort Configuration](job-rollout-abort.html)\.

abort  
You can create a set of conditions to abort rollouts when criteria that you specify have been met\. For more information, see [Job Rollout and Abort Configuration](job-rollout-abort.html)\.

presigned URLs  
To allow a device secure, time\-limited access to data beyond that included in the job document itself, you can use presigned Amazon S3 URLs\. You can place your data in an Amazon S3 bucket and add a placeholder link to the data in the job document\. When the Jobs service receives a request for the job document, it parses the job document looking for placeholder links and it replaces them with presigned Amazon S3 URLs\.   
The placeholder link is of the following form:  

```
${aws:iot:s3-presigned-url:https://s3.amazonaws.com/bucket/key}
```
where *bucket* is your bucket name and *key* is the object in the bucket to which you are linking\.

timeouts  
The job timeout feature isn't currently available in the AWS GovCloud \(US\) Region\.
Job timeouts make it possible to be notified whenever a job execution gets stuck in the `IN_PROGRESS` state for an unexpectedly long period of time\. There are two types of timers: in\-progress timers and step timers\.  
When you create a job, you can set a value for the `inProgressTimeoutInMinutes` property of the optional [TimeoutConfig](https://docs.aws.amazon.com/iot/latest/apireference/API_TimeoutConfig.html) object\. The in\-progress timer can't be updated and applies to all job executions for the job\. Whenever a job execution remains in the `IN_PROGRESS` status for longer than this interval, the job execution fails and switches to the terminal `TIMED_OUT` status\. AWS IoT also publishes an MQTT notification\.  
You can also set a step timer for a job execution by setting a value for `stepTimeoutInMinutes` when you call [UpdateJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html)\. The step timer applies only to the job execution that you update\. You can set a new value for this timer each time you update a job execution\. You can also create a step timer when you call [StartNextPendingJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_StartNextPendingJobExecution.html)\. If the job execution remains in the `IN_PROGRESS` status for longer than the step timer interval, it fails and switches to the terminal `TIMED_OUT` status\. The step timer has no effect on the in\-progress timer that you set when you create a job\.  
The following diagram and description illustrate the ways in which in\-progress timeouts and step timeouts interact with each other\.  

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/timeout-diagram.png)
**Job Creation:** `CreateJob` sets an in\-progress timer that expires in twenty minutes\. This timer applies to all job executions and can't be updated\.  
**12:00 PM:** The job execution starts and switches to `IN_PROGRESS` status\. The in\-progress timer starts to run\.  
**12:05 PM:** `UpdateJobExecution` creates a step timer with a value of 7 minutes\. If a new step timer isn't created, the job execution times out at 12:12 PM\.  
**12:10 PM:** `UpdateJobExecution` creates a new step timer with a value of 5 minutes\. The previous step timer is discarded\. If a new step timer isn't created, the job execution times out at 12:15 PM\.  
**12:13 PM:** `UpdateJobExecution` creates a new step timer with a value of 9 minutes\. The job execution times out at 12:20 because the in\-progress timer expires at 12:20\. The step timer can't exceed the absolute bound created by the in\-progress timer\.  
`UpdateJobExecution` can also discard a step timer that has already been created by creating a new step timer with a value of \-1\.