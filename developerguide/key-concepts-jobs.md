# Jobs key concepts<a name="key-concepts-jobs"></a>

The following concepts can help you understand more about AWS IoT Jobs and how to create and deploy jobs to run remote operations on your devices\.

## Basic concepts<a name="basic-concepts-jobs"></a>

The following are basic concepts you'll need to know when using AWS IoT Jobs\.

**Job**  
A job is a remote operation that is sent to and run on one or more devices connected to AWS IoT\. For example, you can define a job that instructs a set of devices to download and install an application or run firmware updates, reboot, rotate certificates, or perform remote troubleshooting operations\.

**Job document**  
To create a job, you must first create a job document that is a description of the remote operations to be performed by the devices\.  
Job documents are UTF\-8 encoded JSON documents and contain information that your devices need to perform a job\. A job document contains one or more URLs where the device can download an update or other data\. The job document can be stored in an Amazon S3 bucket, or be included inline with the command that creates the job\.  
For job document examples, see the [jobs\-agent\.js](https://github.com/aws/aws-iot-device-sdk-js/blob/master/examples/jobs-agent.js) example in the AWS IoT SDK for JavaScript\.

**Target**  
When you create a job, you specify a list of targets that are the devices that should perform the operations\. The targets can be things or [thing groups](thing-groups.md) or both\. The AWS IoT Jobs service sends a message to each target to inform it that a job is available\.

**Deployment**  
After you create a job by providing the job document and specifying your list of targets, the job document is then deployed to the remote target devices for which you want to perform the update\. For snapshot jobs, the job will complete after deploying to the target devices\. For continuous jobs, a job is deployed to a group of devices as they are added to the groups\.

**Job execution**  
A job execution is an instance of a job on a target device\. The target starts an execution of a job by downloading the job document\. It then performs the operations specified in the document, and reports its progress to AWS IoT\. An execution number is a unique identifier of a job execution on a specific target\. The AWS IoT Jobs service provides commands to track the progress of a job execution on a target and the progress of a job across all targets\.

## Job types concepts<a name="advanced-concepts-jobs"></a>

The following concepts can help you understand more about the different types of jobs that you can create with AWS IoT Jobs\.

**Snapshot job**  
By default, a job is sent to all targets that you specify when you create the job\. After those targets complete the job \(or report that they're unable to do so\), the job is complete\.

**Continuous job**  
A continuous job is sent to all targets that you specify when you create the job\. It continues to run and is sent to any new devices \(things\) that are added to the target group\. For example, a continuous job can be used to onboard or upgrade devices as they're added to a group\. You can make a job continuous by setting an optional parameter when you create the job\.   
When targeting your IoT fleet using dynamic thing groups, we recommend that you use continuous jobs instead of snapshot jobs\. By using continuous jobs, devices that join the group receive the job execution even after the job has been created\.

## Presigned URLs<a name="security-concepts-jobs"></a><a name="presigned"></a>

For secure, time\-limited access to data that's not included in the job document, you can use presigned Amazon S3 URLs\. Place your data in an Amazon S3 bucket and add a placeholder link to the data in the job document\. When AWS IoT Jobs receives a request for the job document, it parses the job document by looking for the placeholder links, and then replaces the links with presigned Amazon S3 URLs\.

The placeholder link is in the following format:

```
${aws:iot:s3-presigned-url:https://s3.amazonaws.com/bucket/key}
```

where *bucket* is your bucket name and *key* is the object in the bucket to which you are linking\.

In the Beijing and Ningxia Regions, presigned URLs work only if the resource owner has an ICP \(Internet Content Provider\) license\. For more information, see [Amazon Simple Storage Service](https://docs.amazonaws.cn/en_us/aws/latest/userguide/s3.html) in the *Getting Started with AWS Services in China* documentation\.

## Job configuration concepts<a name="advanced-concepts-jobs"></a>

The following concepts can help you understand how to configure jobs\.

**Rollouts**  
You can specify how quickly targets are notified of a pending job execution\. This allows you to create a staged rollout to better manage updates, reboots, and other operations\. You can create a rollout configuration by using either a static rollout rate or an exponential rollout rate\. To specify the maximum number of job targets to inform per minute, use a static rollout rate\.  
For examples of setting rollout rates and for more information about configuring job rollouts, see [Job rollout and abort configuration](job-rollout-abort.html)\.

**Abort**  
You can create a set of conditions to cancel rollouts when criteria that you specify have been met\. For more information, see [Job rollout and abort configuration](job-rollout-abort.html)\.

**Timeouts**  
Job timeouts notify you whenever a job deployment gets stuck in the `IN_PROGRESS` state for an unexpectedly long period of time\. There are two types of timers: in\-progress timers and step timers\. When the job is `IN_PROGRESS`, you can monitor and track the progress of your job deployment\.  
Rollouts and abort configurations are specific to your job, whereas the timeout configuration is specific to a job deployment\. For more information, see [Job execution timeout and retry configurations](jobs-configurations-details.md#job-timeout-retry)\.

**Retries**  
Job retries make it possible to retry the job execution when a job fails, times out, or both\. You can have up to 10 attempted retries to execute the job\. You can monitor and track the progress of your retry attempt and whether the job execution succeeded\.  
Rollouts and abort configurations are specific to your job, whereas the timeout and retry configurations are specific to a job execution\. For more information, see [Job execution timeout and retry configurations](jobs-configurations-details.md#job-timeout-retry)\. 