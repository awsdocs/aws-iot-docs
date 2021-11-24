# Managing jobs<a name="create-manage-jobs"></a>

Use jobs to notify devices of a software or firmware update\. You can use the [AWS IoT console](https://console.aws.amazon.com/iot/), the [Job management and control API](jobs-api.md#jobs-http-api), the [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/reference/iot/index.html), or the [AWS SDKs](http://aws.amazon.com/tools/#sdk) to create and manage jobs\.

## Code signing for jobs<a name="create-manage-jobs-code-signing"></a>

 When sending code to devices, for devices to detect whether the code has been modified in transit, we recommend that you sign the code file by using the AWS CLI\. For instructions, see [Create and manage jobs by using the AWS CLI](manage-job-cli.md)\.

For more information, see [What Is Code Signing for AWS IoT?](https://docs.aws.amazon.com/signer/latest/developerguide/Welcome.html)\.

## Job document<a name="create-manage-jobs-job-doc"></a>

Before you create a job, you must create a job document\. If you're using code signing for AWS IoT, you must upload your job document to a versioned Amazon S3 bucket\. For more information about creating an Amazon S3 bucket and uploading files to it, see [Getting Started with Amazon Simple Storage Service](https://docs.aws.amazon.com/AmazonS3/latest/gsg/GetStartedWithS3.html) in the *Amazon S3 Getting Started Guide*\.

**Tip**  
For job document examples, see the [jobs\-agent\.js](https://www.npmjs.com/package/aws-iot-device-sdk#jobs-agentjs) example in the AWS IoT SDK for JavaScript\.

## Presigned URLs<a name="create-manage-jobs-presigned-URLs"></a>

Your job document can contain a presigned Amazon S3 URL that points to your code file \(or other file\)\. Presigned Amazon S3 URLs are valid only for a limited amount of time and are generated when a device requests a job document\. Because the presigned URL isn't created when you're creating the job document, use a placeholder URL in your job document instead\. A placeholder URL looks like the following:

`${aws:iot:s3-presigned-url:https://s3.region.amazonaws.com/<bucket>/<code file>}`

where:
+ *bucket* is the Amazon S3 bucket that contains the code file\.
+ *code file* is the Amazon S3 key of the code file\.

When a device requests the job document, AWS IoT generates the presigned URL and replaces the placeholder URL with the presigned URL\. Your job document is then sent to the device\.

**IAM role to grant permission to download files from S3**  
When you create a job that uses presigned Amazon S3 URLs, you must provide an IAM role that grants permission to download files from the Amazon S3 bucket where the data or updates are stored\. The role must also grant permission for AWS IoT to assume the role\.

You can specify an optional timeout for the presigned URL\. For more information, see [CreateJob](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html)\.

**Grant AWS IoT Jobs permission to assume your role**

1. Go to the [Roles hub of the IAM console](https://console.aws.amazon.com/iamv2/home#/roles) and choose your role\.

1. On the **Trust Relationships** tab, choose **Edit Trust Relationship** and replace the policy document with the following JSON\. Choose **Update Trust Policy**\.

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

1. If your job uses a job document that's an Amazon S3 object, choose **Permissions** and with the following JSON, add a policy that grants permission to download files from your Amazon S3 bucket:

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

**Topics**
+ [Code signing for jobs](#create-manage-jobs-code-signing)
+ [Job document](#create-manage-jobs-job-doc)
+ [Presigned URLs](#create-manage-jobs-presigned-URLs)
+ [Create and manage jobs by using the AWS Management Console](manage-job-console.md)
+ [Create and manage jobs by using the AWS CLI](manage-job-cli.md)