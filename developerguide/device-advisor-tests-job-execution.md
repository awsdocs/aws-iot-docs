# Job Execution<a name="device-advisor-tests-job-execution"></a>

**"Device can complete a job execution"**  
This test case helps you validate if your device is able to receive updates using AWS IoT Jobs, and publish the status of successful updates\. For more information on AWS IoT Jobs, see [ Jobs](https://docs.aws.amazon.com/iot/latest/developerguide/iot-jobs.html)\.  
*API test case definition:*  
`EXECUTION_TIMEOUT` has a default value of 5 minutes\. We recommend a timeout value of 2 minutes\. 

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