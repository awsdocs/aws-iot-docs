# Creating and managing job templates \(console\)<a name="job-templates-console"></a>

**Note**  
The job templates feature is in preview and subject to change\.

This topic explains how to create, delete, and view details about job templates by using the AWS IoT console\.

## Create a job template from scratch<a name="job-templates-console-create-scratch"></a>

1. Browse to the [AWS IoT console](https://console.aws.amazon.com/iot/)\.

1. In the navigation pane, choose **Manage**, and then choose **Job templates**\.
**Note**  
You can also navigate to the **Job templates** page from the **Related services** page under **Fleet Hub**\.

1. Choose **Create job template**\.

1. Enter an alphanumeric identifier for your job\.

1. Enter an alphanumeric description for your job\.
**Note**  
We do not recommend using personally identifiable information in your job IDs or descriptions\.

1. Under **Job template document**, enter the S3 URL or choose **Browse S3**, and then navigate to your job document and select it\.
**Note**  
Currently you can select only S3 buckets in your current region\.

1. Specify values for any advanced configurations, and then choose **Create job template**\. Your new job template appears on the **Job templates** page\.

   For more information about rollout and abort configurations, see [Job rollout and abort configuration](job-rollout-abort.md)\. For more information about timeout configurations, see [timeouts](iot-jobs.md#timeout)\.

## Create a job template from an existing job<a name="job-templates-console-create-exist-job"></a>

1. Browse to the [AWS IoT console](https://console.aws.amazon.com/iot/)\.

1. In the navigation pane, choose **Manage**, and then choose **Jobs**\.

1. Select the job that you want to use as the basis of the job template and choose **Copy to job template**\.

1. Enter an alphanumeric identifier for your job\.

1. Enter an alphanumeric description for your job\.

1. Optionally select a different job document or edit the advanced configurations from the original job, and then choose **Create job template**\. Your new job template appears on the **Job templates** page\.

## Create a job from a job template<a name="job-templates-console-create-job-from"></a>

1. Browse to the [AWS IoT console](https://console.aws.amazon.com/iot/)\.

1. In the navigation pane, choose **Manage**, and then choose **Job templates**\.

1. Find the job template you want to use and then navigate to its details page\. Choose **Create job with this template**\.

1. Enter a unique alphanumeric name for the job\.

1. Enter an alphanumeric description for the job\.

1. Optionally add tags to the job\. Choose **Next**\.

1. On the **File configuration** page, under **Devices**, select the things and thing groups for the job to target\.

1. Under **File**, verify that the Amazon S3 URL is correct\. If you want to use a different job document, choose **Browse** and select a different bucket and document\. Choose **Next**\.

1. On the **Job configuration** page, select the job run type\. A job can either be a continuous or a snapshot job\. A snapshot job is complete when it finishes its run on the target devices and groups\. A continuous job applies to thing groups and runs on any device that you subsequently add to a specified target group\.

1. Optionally edit the advanced configurations from the job template, and then choose **Next**\.

1. On the **Review and create** page, review your job's properties and configurations\. Choose **Edit** for any set of properties and configurations that you want to change\.

1. Choose **Submit**\. Your new job appears on the **Jobs** page\.

You can also create jobs from job templates with Fleet Hub web applications\. For information about creating jobs in Fleet Hub, see [Working with job templates in Fleet Hub for AWS IoT Device Management](https://docs.aws.amazon.com/iot/latest/fleethubuserguide/aws-iot-monitor-technician-job-templates.html)\.

## Delete a job template<a name="job-templates-console-delete-job"></a>

1. Browse to the [AWS IoT console](https://console.aws.amazon.com/iot/)\.

1. In the navigation pane, choose **Manage**, and then choose **Job templates**\.

1. Find and select the job template you want to delete\.

1. Choose **Delete**\.

1. The job template no longer appears on the **Job templates** page\.