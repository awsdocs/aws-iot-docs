# Creating and managing job templates \(CLI\)<a name="job-templates-cli"></a>

**Note**  
The job templates feature is in preview and subject to change\.

This topic explains how to create, delete, and retrieve details about job templates by using the AWS CLI\.

## Create a job template from scratch<a name="job-templates-cli-create-scratch"></a>

The following AWS CLI command shows how to create a job using a job document \(*job\-document\.json*\) stored in an Amazon S3 bucket \(*jobBucket*\) and a role with permission to download files from Amazon S3 \(*S3DownloadRole*\)\.

```
aws iot create-job-template  \
      --job-template-id 010  \
      --document-source https://s3.amazonaws.com/my-s3-bucket/job-document.json  \
      --timeout-config inProgressTimeoutInMinutes=100 \
      --job-executions-rollout-config "{ \"exponentialRate\": { \"baseRatePerMinute\": 50, \"incrementFactor\": 2, \"rateIncreaseCriteria\": { \"numberOfNotifiedThings\": 1000, \"numberOfSucceededThings\": 1000}}, \"maximumPerMinute\": 1000}" \
      --abort-config "{ \"criteriaList\": [ { \"action\": \"CANCEL\", \"failureType\": \"FAILED\", \"minNumberOfExecutedThings\": 100, \"thresholdPercentage\": 20}, { \"action\": \"CANCEL\", \"failureType\": \"TIMED_OUT\", \"minNumberOfExecutedThings\": 200, \"thresholdPercentage\": 50}]}" \          
      --presigned-url-config "{\"roleArn\":\"arn:aws:iam::123456789012:role/S3DownloadRole\", \"expiresInSec\":3600}"
```

The optional `timeout-config` parameter specifies the amount of time each device has to finish its execution of the job\. The timer starts when the job execution status is set to `IN_PROGRESS`\. If the job execution status isn't set to another terminal state  before the time expires, it's set to `TIMED_OUT`\.

The in\-progress timer can't be updated and applies to all job executions for the job\. Whenever a job execution remains in the `IN_PROGRESS` state for longer than this interval, the job execution fails and switches to the terminal `TIMED_OUT` status\. AWS IoT also publishes an MQTT notification\.

For more information about creating configurations about job rollouts and aborts, see [Job Rollout and Abort Configuration](job-rollout-abort.html)\.

**Note**  
Job documents that are specified as Amazon S3 files are retrieved at the time you create the job\. Changing the contents of the Amazon S3 file you used as the source of your job document after you have created the job doesn't change what is sent to the targets of the job\.

## Create a job template from an existing job<a name="job-templates-cli-create-from-job"></a>

The following AWS CLI command creates a job template by specifying the Amazon Resource Name \(ARN\) of an existing job\. The new job template uses all of the configurations specified in the job\. Optionally, you can change any of the configurations in the existing job by using any of the optional parameters\.

```
aws iot create-job-template  \
      --job-arn arn:aws:iot:region:123456789012:job/job-name  \      
      --timeout-config inProgressTimeoutInMinutes=100
```

## Get details about a job template<a name="job-templates-cli-describe"></a>

The following AWS CLI command gets details about a specified job template\.

```
aws iot describe-job-template \
      --job-template-id template-id
```

The command displays the following output\.

```
{
   "abortConfig": { 
      "criteriaList": [ 
         { 
            "action": "string",
            "failureType": "string",
            "minNumberOfExecutedThings": number,
            "thresholdPercentage": number
         }
      ]
   },
   "createdAt": number,
   "description": "string",
   "document": "string",
   "documentSource": "string",
   "jobExecutionsRolloutConfig": { 
      "exponentialRate": { 
         "baseRatePerMinute": number,
         "incrementFactor": number,
         "rateIncreaseCriteria": { 
            "numberOfNotifiedThings": number,
            "numberOfSucceededThings": number
         }
      },
      "maximumPerMinute": number
   },
   "jobTemplateArn": "string",
   "jobTemplateId": "string",
   "presignedUrlConfig": { 
      "expiresInSec": number,
      "roleArn": "string"
   },
   "timeoutConfig": { 
      "inProgressTimeoutInMinutes": number
   }
}
```

## List job templates<a name="job-templates-cli-list"></a>

The following AWS CLI command lists all of the job templates in your AWS account\.

```
 aws iot list-job-templates
```

The command displays the following output\.

```
{
   "jobTemplates": [ 
      { 
         "createdAt": number,
         "description": "string",
         "jobTemplateArn": "string",
         "jobTemplateId": "string"
      }
   ],
   "nextToken": "string"
}
```

Use the value of the `nextToken` field to retrieve additional pages of results\.

## Delete a job template<a name="job-templates-cli-delete"></a>

The following AWS CLI command deletes a specified job template\.

```
aws iot delete-job-template \
      --job-template-id template-id
```

The command displays no output\.

## Create a job from a job template<a name="job-templates-cli-create-job"></a>

The following AWS CLI command creates a job from a job template\. It targets a device named `thingOne` and specifies the Amazon Resource Name \(ARN\) of the job template to use as the basis for the job\. You can override advanced configurations, such as timeout and cancel configurations, by passing the associated parameters of the `create-job` command\.

```
aws iot create-job \ 
      --targets arn:aws:iot:region:123456789012:thing/thingOne  \
      --job-template-arn arn:aws:iot:region:123456789012:jobtemplate/template-id
```