# Create and Track an AWS IoT Core Job<a name="ios-sdk-create-job"></a>

 You can use AWS IoT Core jobs to deploy and track management tasks in your device fleet\. You can use jobs to send remote actions to one or many devices at once, control the deployment of jobs to your devices, and track the current and past status of job executions for each device\.

This topic shows you how to create and deploy a sample job to a device\. It walks you through the steps required to create a job and track its events on a device that is configured to communicate with AWS IoT Core\. These instructions are written with the assumption that you're using a Raspberry Pi, but they can be adapted for other Linux\-based devices\. 

Here are some possible scenarios for using jobs:
+ Updating device firmware, software, or files, such as security certificates\.
+ Performing administrative tasks, such as restarting devices or performing diagnostics\.
+ Restoring devices to factory settings or other known good states\.

## Connect Your Device to AWS IoT<a name="ios-sdk-jobs-device-setup"></a>

Perform the following steps to connect a Raspberry Pi to AWS IoT Core\.

1. Follow the instructions in [Connecting Your Raspberry Pi](sdk-tutorials.html)\. When you're finished, you'll have an AWS IoT Core thing registered in your AWS account\. You'll also have fully configured security certificates on your device\.

1. Complete the steps in the [Using the AWS IoT Device SDK for JavaScript](iot-device-sdk-node.html) tutorial\. When you're done, your device is connected to AWS IoT Core, and you can run the sample code that comes with the AWS IoT Device SDK for JavaScript\.

Now your device is ready to use AWS IoT Core jobs\.

## Run the Jobs Sample<a name="ios-sdk-jobs-run-sample"></a>

The AWS IoT Device SDK for JavaScript includes a sample named [jobs\-example\.js](https://github.com/aws/aws-iot-device-sdk-js/blob/master/examples/jobs-example.js)\. This sample can receive messages from the [AWS IoT console](https://console.aws.amazon.com/iot) to verify connectivity\. It can also receive and process job executions that originate from the AWS IoT Core Jobs service\.

You can run this sample by using the following command\. Use the REST endpoint of your Raspberry Pi as the value of the `-H` parameter\.

```
node examples/jobs-example.js -f ~/certs -H <PREFIX>.iot.<REGION>.amazonaws.com -T thingName
```

If you've created a configuration file that contains the thing name and the host endpoint \(the REST endpoint of your device\), you can use the following command\.

```
node examples/jobs-example.js –f ./certs –F your config file name.json
```

## Create a Job Document<a name="ios-sdk-jobs-create-job-document"></a>

A job document is a JSON document that provides all of the information that your device needs to execute a job\. The AWS IoT Device SDK for JavaScript uses a property named `operation` to route job documents to specific handlers\. The `jobs-example.js` program has a sample handler for an operation named `customJob`\. To create a job document named `example-job.json` for this handler, the file should contain the following JSON object\.

```
{
    "operation":"customJob",
    "otherInfo":"someValue"
}
```

For more sample job documents, see the documentation for the [jobs\-agent\.js](https://www.npmjs.com/package/aws-iot-device-sdk#jobs-agentjs) sample\.

## Create a Job<a name="ios-sdk-jobs-create-job"></a>

Now you're ready to create a job that delivers the job document to all of the devices that you specify\. You can use the AWS IoT console, the AWS IoT SDK, or the AWS IoT CLI to create a job\.

The following example shows how to use the [AWS IoT CLI](https://docs.aws.amazon.com/cli/latest/reference/iot/create-job.html) to create a job\.

```
 aws iot create-job \
--job-id "example-job-01" \
--targets "arn:aws:iot:::thing/MyRaspberryPi" \
--document file:///example-job.json \
--description "My First test job" \
--target-selection SNAPSHOT
```

If you store your job document in an S3 bucket, use the `document-source` parameter instead of the `document` parameter to specify the Amazon S3 URL for the job document\.

If you prefer to use the AWS IoT console, follow these steps to create a job\.

1. Upload the job document to an S3 bucket\. For information, see [How Do I Upload Files and Folders to an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html) in the *Amazon Simple Storage Service Console User Guide*\.

1. In the AWS IoT console, choose **Manage**, and then choose **Jobs**\.

1. Choose **Create a job**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/start-job.png)

1. On the **Select a job** page, choose **Create custom job**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/select-job.png)

1. On the **Create a job** page, enter a unique job ID\.
**Note**  
We do not recommend using personally identifiable information in your job ID\.

   Under **Select devices to update**, select the device that you connected to AWS IoT\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/create-job.png)

1. Scroll down to **Add a job file** and choose the job document file that you uploaded to Amazon S3\. Under **Job type**, select **Your job will complete after deploying to the selected devices/groups \(snapshot\)**\. \(The other option, **Your job will continue deploying to any devices added to the selected groups \(continuous\)**, is for deploying a job to groups of devices as devices are added to each group\.\) Leave the **Job executions rollout configuration** unchanged\. Choose **Create**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/add-job-file.png)

1. Your new job appears on the **Jobs** page\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/newly-created-job.png)

For more information about creating and deploying jobs, see [AWS IoT Core Jobs](https://docs.aws.amazon.com/iot/latest/developerguide/iot-jobs.html)\.

## Execute the Job on a Device<a name="ios-sdk-jobs-execute-job"></a>

After your job is created, the Jobs service sends a notification of a pending job to your device\. Your device gets the job details and job document through the [NextJobExecutionChanged](jobs-api.html#jobs-mqtt-api) API\. The `jobs-example.js` sample that you've already run executes the job on the device\. When the job is complete, the sample publishes its completed status by using the [UpdateJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html) API\. When you run the sample on your device, you see the following output\.

```
node examples/jobs-example.js -f ./certs -F config.json
connect
startJobNotifications completed for thing: MyRaspberryPi
customJob operation handler invoked, jobId: example-job-01
```

When you refresh the **Jobs** page, you can see that your job was completed successfully\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/completed-job.png)

## Tracking Job Progress with Job and Job Execution Events<a name="ios-sdk-jobs-track-job"></a>

You can use `Job` events and `JobExecution` events to track the progress of your job\. 

This is a helpful way to alert users, system administrators, and others that a job is complete or that a job execution has changed its status\. For example, you can alert a user about a firmware update on a device or inform a system administrator about an issue in the device fleet that must be investigated and resolved\.

Job events for the job in this example are published to the following topics when the job is completed or canceled\.

```
$aws/events/job/example-job-01/completed
$aws/events/job/example-job-01/canceled
```

Job execution events for the job in this example are sent to the following topics when the job execution reaches one of the possible final statuses\.

```
$aws/events/jobExecution/example-job-01/succeeded
$aws/events/jobExecution/example-job-01/failed
$aws/events/jobExecution/example-job-01/rejected
$aws/events/jobExecution/example-job-01/canceled
$aws/events/jobExecution/example-job-01/removed
```

When the job execution on your device succeeds, AWS IoT Core publishes a [JobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_JobExecution.html) succeeded event\. You can see this event in the console by navigating to the **Test** page and subscribing to the `$aws/events/jobExecution/example-job-01/succeeded` topic in the MQTT client\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/subscribe-job-topic.png)

The following message appears when the job execution for your device has completed successfully\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/job-mqtt-message-succeeded.png)

AWS IoT Core also publishes a completed `job` event\. You can see this event by subscribing to the `$aws/events/job/example-job-01/completed` topic in the MQTT client\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/job-mqtt-message-completed.png)