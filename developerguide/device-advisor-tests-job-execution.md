# Job Execution<a name="device-advisor-tests-job-execution"></a>

**"Device can complete a job execution"**  
 This test case helps you validate if your device is able to receive updates using AWS IoT Jobs, and publish the status of successful updates\. For more information on AWS IoT Jobs, see [ Jobs](https://docs.aws.amazon.com/iot/latest/developerguide/iot-jobs.html)\.   
 In order to successfully run this test case, there are two reserved AWS topics that you need to grant your [Device Role](https://docs.aws.amazon.com/iot/latest/developerguide/device-advisor-setting-up.html#da-iam-role) \. To subscribe to job activity related messages, use the **notify** and **notify\-next** topics\. Your device role must grant PUBLISH action for the following topics:   
+ $aws/things/**thingName**/jobs/**jobId**/get
+ $aws/things/**thingName**/jobs/**jobId**/update
It is recommended to grant SUBSCRIBE and RECEIVE actions for the following topics:  
+ $aws/things/**thingName**/jobs/get/accepted
+ $aws/things/**thingName**/jobs/**jobId**/get/rejected
+ $aws/things/**thingName**/jobs/**jobId**/update/accepted
+ $aws/things/**thingName**/jobs/**jobId**/update/rejected
It is recommended to grant SUBSCRIBE action for the following topic:  
+ $aws/things/**thingName**/jobs/notify\-next
For more information about these reserved topics, see reserved topics for [AWS IoT Jobs](https://docs.aws.amazon.com/iot/latest/developerguide/reserved-topics.html#reserved-topics-job)\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 3 minutes\. Depending on the AWS IoT Job document or source provided, adjust the timeout value \(for example, if a job will take a long time to run, define a longer timeout value for the test case\)\. To run the test, either a valid AWS IoT Job document or an already existing job ID is required\. An AWS IoT Job document can be provided as a JSON document or an S3 link\. If a job document is provided, providing a job ID is optional\. If a job ID is provided, Device Advisor will use that ID while creating the AWS IoT Job on your behalf\. If the job document is not provided, you can provide an existing ID that is in the same region as you are running the test case\. In this case, Device Advisor will use that AWS IoT Job while running the test case\.

```
"tests": [
   {
      "name":"my_job_execution",
      "configuration": {
         // optional:
         // Test case will create a job task by using either JOB_DOCUMENT or JOB_DOCUMENT_SOURCE.
         // If you manage the job task on your own, leave it empty and provide the JOB_JOBID (self-managed job task).
         // JOB_DOCUMENT is a JSON formatted string
         "JOB_DOCUMENT": "{
            \"operation\":\"reboot\",
            \"files\" : {
               \"fileName\" : \"install.py\",
               \"url\" : \"${aws:iot:s3-presigned-url:https://s3.amazonaws.com/bucket-name/key}\"
            }
         }",
         // JOB_DOCUMENT_SOURCE is an S3 link to the job document. It will be used only if JOB_DOCUMENT is not provided.
         "JOB_DOCUMENT_SOURCE": "https://s3.amazonaws.com/bucket-name/key",
         // JOB_JOBID is mandatory, only if neither document nor document source is provided. (Test case needs to know the self-managed job task id).
         "JOB_JOBID": "String",
         // JOB_PRESIGN_ROLE_ARN is used for the presign Url, which will replace the placeholder in the JOB_DOCUMENT field
         "JOB_PRESIGN_ROLE_ARN": "String",
         // Presigned Url expiration time. It must be between 60 and 3600 seconds, with the default value being 3600.
         "JOB_PRESIGN_EXPIRES_IN_SEC": "Long"   
         "EXECUTION_TIMEOUT": "300", // in seconds         
      },
      "test": {
         "id": "Job_Execution",
         "version": "0.0.0"
      }
   }
]
```
For more information on creating and using job documents see [job document](https://docs.aws.amazon.com/iot/latest/developerguide/iot-jobs.html)\. 