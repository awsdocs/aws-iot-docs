# Creating and Managing Jobs \(Console\)<a name="manage-job-console"></a>

If you are using Code\-signing for AWS IoT, you must add two placeholder URLs in your job document:

A placeholder for the code file should look like this: `${aws:iot:s3-presigned-url:https://s3.amazonaws.com/<my-s3-bucket>/<my-code-file>}`\. 

**Note**  
Currently, versions are not supported for presigned URL placeholders for jobs\. If you update your code file and copy it to the same Amazon S3 location, you must create a new signature and then reference the new signature version in your job document\.

A placeholder for the signature should look like this: `${aws:iot:code-sign-signature:s3://<region>.<my-s3-bucket>/<my-code-file>@<code-file-version-id>}`\. 

**To create a job**

1. Browse to the [AWS IoT console](https://console.aws.amazon.com/iot/)\.

1. In the navigation pane, choose **Manage**, and then choose **Jobs**\.

1. Choose **Create a job**\.

1. Choose **Create a custom job**\.

1. Enter an alphanumeric ID for your job and an optional description\.
**Note**  
We do not recommend using personally identifiable information in your job IDs or descriptions\.

1. Select the device or device groups that you want to update\.

1. Under **Add a job file**, choose **Select**, and then select your job document\.

1. Choose **Sign image for me**\. If you are not code signing your update, you can skip this step\.

1. Create or choose a code\-signing profile\. If you are not code signing your update, you can skip this step\.

1. Under **Pre\-sign resource URLs**, choose **I want to pre\-sign my URLs and have configured my job file**\. If you are not code signing your update, you can skip this step\.

1. Choose a role and an expiry time for the presigned URL\.

1. Under **Job type**, choose the appropriate option for your update, and then choose **Next**\.

1. Specify values for any advanced configurations, and then choose **Create**\.

After you create the job, the console generates a JSON signature and places it in your job document\.

You can use the [AWS IoT console](https://console.aws.amazon.com/iot/) to view the status, cancel, or delete a job\.

1. Browse to the [AWS IoT console](https://console.aws.amazon.com/iot/)\.

1. In the navigation pane, choose **Manage**, and then choose **Jobs**\.