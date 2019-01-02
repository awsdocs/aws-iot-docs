# Managing Jobs<a name="create-manage-jobs"></a>

You can use the [AWS IoT console](https://console.aws.amazon.com/iot/), the Jobs HTTPS API, the AWS Command Line Interface, or the AWS SDKs to create and manage jobs\. For more information, see [Job Management and Control API](jobs-api.md#jobs-http-api), [AWS CLI Command Reference: iot](http://alpha-docs-aws.amazon.com/cli/latest/reference/iot/index.html) or [AWS SDKs and Tools](http://aws.amazon.com/tools/#sdk)\.

The primary purpose of jobs is to notify devices of a software or firmware update\. When sending code to devices, the best practice is to sign the code file\. This allows devices to detect if the code has been modified in transit\. The instructions in the following section are written with the assumption that you want to code\-sign the software update you are sending to your devices\.

For more information about code\-signing, see [What Is Code Signing for AWS IoT?](https://docs.aws.amazon.com/signer/latest/developerguide/Welcome.html)

Before you create a job, you must create a job document\. If you are using Code\-signing for AWS IoT, you must upload your job document to a versioned Amazon S3 bucket\. For more information about creating an Amazon S3 bucket and uploading files to it, see [Getting Started with Amazon Simple Storage Service](http://alpha-docs-aws.amazon.com/AmazonS3/latest/gsg/GetStartedWithS3.html) in the *Amazon S3 Getting Started Guide*\.

Your job document can contain a presigned Amazon S3 URL that points to your code file \(or other file\)\. Presigned Amazon S3 URLs are valid for a limited amount of time and so are not generated until a device requests a job document\. Because the presigned URL has not been created when you are creating the job document, you put a placeholder URL in your job document instead\. A placeholder URL looks like the following: `${aws:iot:s3-presigned-url:https://s3.region.amazonaws.com/<bucket>/<code file>}` where *bucket* is the Amazon S3 bucket that contains the code file and *code file* is the Amazon S3 key of the code file\.

When a device requests the job document, AWS IoT generates the presigned URL and replaces the placeholder URL with the presigned URL\. Your job document is then sent to the device\.

When you create a job that uses presigned Amazon S3 URLs, you must provide an IAM role that grants permission to download files from the Amazon S3 bucket where the data or updates are stored\. The role must also grant permission for AWS IoT to assume the role\.

You can specify an optional timeout for the presigned URL\. For more information, see [CreateJob](jobs-api.md#jobs-CreateJob)\.

**To grant Jobs permission to assume your role**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. Search for your role and choose it\.

1. Choose the **Trust Relationships** tab, and then choose **Edit Trust Relationship**\.

1. On the **Edit Trust Relationship** page, replace the policy document with the following JSON:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "",
         "Effect": "Allow",
         "Principal": {
           "Service": [
             "iot.amazonaws.com"
           ]
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. Choose **Update Trust Policy**\.

1. If your job uses a job document that is an Amazon S3 object, choose **Permissions** and with the following JSON, add a policy that grants permission to download files from your Amazon S3 bucket:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "s3:GetObject",
               "Resource": "arn:aws:s3:::your_S3_bucket/*"
           }
       ]
   }
   ```