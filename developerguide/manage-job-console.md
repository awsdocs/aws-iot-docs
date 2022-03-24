# Create and manage jobs by using the AWS Management Console<a name="manage-job-console"></a>

To create a job

1. 

**Choose your job type**

   1. Go to the [Job hub of the AWS IoT console](https://console.aws.amazon.com/iot/home#/jobhub) and choose **Create job**\.

   1. Depending on the device that you're using, you can create a custom job, a FreeRTOS OTA update job, or an AWS IoT Greengrass job\. For this example, choose **Create a custom job**\.

1. 

**Enter job properties**

   Enter a unique, alphanumeric job name, optional description, and tags, and then choose **Next**\.
**Note**  
We recommend that you don't use personally identifiable information in your job IDs and description\.

1. 

**Choose your targets**

   Choose as job targets the things or thing groups you want to run in this job\.

1. 

**Specify your job document**

   You can either upload your JSON job file to an S3 bucket and then use that file as the job document, or choose your job file from a template\.

   If you're using a template, you can choose from a custom job template or an AWS managed template\. If you're creating a job for performing frequently used remote actions such as rebooting your device, you can use an AWS managed template\. These templates have already been preconfigured for use\. For more information, see [Create a custom job template](job-templates-console.md#job-templates-console-create) and [Create custom job templates from managed templates](job-template-manage-console-create.md#job-template-manage-create-template)\. 

1. 

**Choose your job type**

   On the **Job configuration** page, choose the job type as continuous or a snapshot job\. A snapshot job is complete when it finishes its run on the target devices and groups\. A continuous job applies to thing groups and runs on any device that you subsequently add to a specified target group\.

1. 

**Specify additional configurations \(optional\)**

   Continue to add any additional configurations for your job and then review and create your job\. For information about the additional configurations, see:
   + [Job rollout and abort configurations](jobs-configurations.md#job-rollout-abort)
   + [Job execution timeout and retry configurations](jobs-configurations.md#job-timeout-retry)

After you create the job, the console generates a JSON signature and places it in your job document\. You can use the [AWS IoT console](https://console.aws.amazon.com/iot/) to view the status, cancel, or delete a job\. To manage jobs, go to the [Job hub of the console](https://console.aws.amazon.com/iot/home#/jobhub)\. 