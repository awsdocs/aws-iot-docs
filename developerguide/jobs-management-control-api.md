# Jobs management and control API and data types<a name="jobs-management-control-api"></a>

**Topics**
+ [Job management and control data types](#jobs-control-plane-data-types)
+ [Job management and control API operations](#jobs-http-api)

To determine the *endpoint\-url* parameter for your CLI commands, run this command\.

```
aws iot describe-endpoint --endpoint-type=iot:Jobs
```

This command returns the following output\.

```
{
"endpointAddress": "account-specific-prefix.jobs.iot.aws-region.amazonaws.com"
}
```

**Note**  
The Jobs endpoint doesn't support ALPN `z-amzn-http-ca`\.

## Job management and control data types<a name="jobs-control-plane-data-types"></a>

The following data types are used by management and control applications to communicate with AWS IoT Jobs\.

### Job<a name="jobs-job"></a>

The `Job` object contains details about a job\. Following shows the syntax:

```
{
    "jobArn": "string", 
    "jobId": "string", 
    "status": "IN_PROGRESS|CANCELED|SUCCEEDED", 
    "forceCanceled": boolean,
    "targetSelection": "CONTINUOUS|SNAPSHOT",
    "comment": "string",
    "targets": ["string"], 
    "description": "string",
    "createdAt": timestamp,
    "lastUpdatedAt": timestamp,
    "completedAt": timestamp,
    "jobProcessDetails": {
        "processingTargets": ["string"],
        "numberOfCanceledThings": long, 
        "numberOfSucceededThings": long, 
        "numberOfFailedThings": long,
        "numberOfRejectedThings": long, 
        "numberOfQueuedThings": long, 
        "numberOfInProgressThings": long, 
        "numberOfRemovedThings": long, 
        "numberOfTimedOutThings": long
    }, 
    "presignedUrlConfig": {
        "expiresInSec": number, 
        "roleArn": "string"
    }, 
    "jobExecutionsRolloutConfig": { 
        "exponentialRate": { 
           "baseRatePerMinute": integer,
           "incrementFactor": integer,
           "rateIncreaseCriteria": { 
              "numberOfNotifiedThings": integer, // Set one or the other
              "numberOfSucceededThings": integer // of these two values.
           },
           "maximumPerMinute": integer
      }
    },    
    "abortConfig": { 
       "criteriaList": [ 
          { 
             "action": "string",
             "failureType": "string",
             "minNumberOfExecutedThings": integer,
             "thresholdPercentage": integer
          }
       ]
    },
    "timeoutConfig": {
        "inProgressTimeoutInMinutes": long
    }
}
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_Job.html](https://docs.aws.amazon.com/iot/latest/apireference/API_Job.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/job.html](https://docs.aws.amazon.com/cli/latest/reference/iot/job.html) 

### JobSummary<a name="jobs-job-summary"></a>

The `JobSummary` object contains a job summary\. Following shows the syntax:

```
{
    "jobArn": "string", 
    "jobId": "string",
    "status": "IN_PROGRESS|CANCELED|SUCCEEDED", 
    "targetSelection": "CONTINUOUS|SNAPSHOT",
    "thingGroupId": "string",
    "createdAt": timestamp, 
    "lastUpdatedAt": timestamp, 
    "completedAt": timestamp
}
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_JobSummary.html](https://docs.aws.amazon.com/iot/latest/apireference/API_JobSummary.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/job-summary.html](https://docs.aws.amazon.com/cli/latest/reference/iot/job-summary.html)

### JobExecution<a name="jobs-job-execution"></a>

The `JobExecution` object represents the execution of a job on a device\. Following shows the syntax:

```
{
    "approximateSecondsBeforeTimedOut": 50,
    "executionNumber": 1234567890,
    "forceCanceled": true|false,
    "jobId": "string",
    "lastUpdatedAt": timestamp, 
    "queuedAt": timestamp,
    "startedAt": timestamp,
    "status": "QUEUED|IN_PROGRESS|FAILED|SUCCEEDED|CANCELED|TIMED_OUT|REJECTED|REMOVED",
    "forceCanceled": boolean,
    "statusDetails": {
        "detailsMap": { 
            "string": "string" ...
        },
        "status": "string"
    }, 
    "thingArn": "string", 
    "versionNumber": 123
}
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecution.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/job-execution.html](https://docs.aws.amazon.com/cli/latest/reference/iot/job-execution.html)\.

### JobExecutionSummary<a name="jobs-job-execution-summary"></a>

The `JobExecutionSummary` object contains job execution summary information\. Following shows the syntax:

```
{
    "executionNumber": 1234567890,
    "queuedAt": timestamp,
    "lastUpdatedAt": timestamp,
    "startedAt": timestamp,
    "status": "QUEUED|IN_PROGRESS|FAILED|SUCCEEDED|CANCELED|TIMED_OUT|REJECTED|REMOVED"
}
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionSummary.html](https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionSummary.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/job-execution-summary.html](https://docs.aws.amazon.com/cli/latest/reference/iot/job-execution-summary.html)\.

### JobExecutionSummaryForJob<a name="jobs-job-execution-summary-for-job"></a>

The `JobExecutionSummaryForJob` object contains a summary of information about job executions for a specific job\. Following shows the syntax:

```
{
    "executionSummaries": [
        {
            "thingArn": "arn:aws:iot:us-west-2:123456789012:thing/MyThing", 
            "jobExecutionSummary": {
                "status": "IN_PROGRESS", 
                "lastUpdatedAt": 1549395301.389, 
                "queuedAt": 1541526002.609, 
                "executionNumber": 1
            }
        }, 
        ...
    ]
}
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionSummaryForJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionSummaryForJob.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/job-execution-summary-for-job.html](https://docs.aws.amazon.com/cli/latest/reference/iot/job-execution-summary-for-job.html)\.

### JobExecutionSummaryForThing<a name="jobs-job-execution-summary-for-thing"></a>

The `JobExecutionSummaryForThing` object contains a summary of information about a job execution on a specific thing\. Following shows the syntax:

```
{
    "executionSummaries": [
        {
            "jobExecutionSummary": {
                "status": "IN_PROGRESS", 
                "lastUpdatedAt": 1549395301.389, 
                "queuedAt": 1541526002.609, 
                "executionNumber": 1
            }, 
            "jobId": "MyThingJob"
        },
        ...
    ]
}
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionSummaryForThing.html](https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionSummaryForThing.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot/job-execution-summary-for-thing.html](https://docs.aws.amazon.com/cli/latest/reference/iot/job-execution-summary-for-thing.html)\.

## Job management and control API operations<a name="jobs-http-api"></a>

Use the following API operations or CLI commands:

### AssociateTargetsWithJob<a name="jobs-AssociateTargetsWithJob"></a>

Associates a group with a continuous job\. The following criteria must be met:
+ The job must have been created with the `targetSelection` field set to `CONTINUOUS`\.
+ The job status must currently be `IN_PROGRESS`\.
+ The total number of targets associated with a job must not exceed 100\.

------
#### [ HTTPS request ]

```
POST /jobs/jobId/targets
 
{ 
"targets": [ "string" ],
"comment": "string"
}
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_AssociateTargetsWithJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_AssociateTargetsWithJob.html) 

------
#### [ CLI syntax ]

```
aws iot  associate-targets-with-job \
--targets <value> \
--job-id <value> \
[--comment <value>]  \
[--cli-input-json <value>] \
[--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
"targets": [
"string"
],
"jobId": "string",
"comment": "string"
}
```

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/iot/associate-targets-with-job.html](https://docs.aws.amazon.com/cli/latest/reference/iot/associate-targets-with-job.html)\.

------

### CancelJob<a name="jobs-CancelJob"></a>

Cancels a job\.

------
#### [ HTTPS request ]

```
PUT /jobs/jobId/cancel
 
{ 
"force": boolean,
"comment": "string",
"reasonCode": "string"
}
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_CancelJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_CancelJob.html) 

------
#### [ CLI syntax ]

```
aws iot cancel-job \
    --job-id <value> \
    [--force <value>]  \
    [--comment <value>]  \
    [--reasonCode <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
    "jobId": "string",
    "force": boolean,
    "comment": "string"
}
```

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/iot/cancel-job.html](https://docs.aws.amazon.com/cli/latest/reference/iot/cancel-job.html)\. 

------

### CancelJobExecution<a name="jobs-CancelJobExecution"></a>

Cancels a job execution on a device\.

------
#### [ HTTPS request ]

```
PUT /things/thingName/jobs/jobId/cancel
 
{ 
"force": boolean,
"expectedVersion": "string",
"statusDetails": {
    "string": "string"
    ...
}
}
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_CancelJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_CancelJobExecution.html)\.

------
#### [ CLI syntax ]

```
aws iot cancel-job-execution \
--job-id <value> \
--thing-name <value> \
[--force | --no-force] \
[--expected-version <value>] \
[--status-details <value>]  \
[--cli-input-json <value>] \
[--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
"jobId": "string",
"thingName": "string",
"force": boolean,
"expectedVersion": long,
"statusDetails": {
"string": "string"
}
}
```

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/iot/cancel-job-execution.html](https://docs.aws.amazon.com/cli/latest/reference/iot/cancel-job-execution.html)\.

------

### CreateJob<a name="jobs-CreateJob"></a>

Creates a job\. You can provide the job document as a link to a file in an Amazon S3 bucket \(`documentSource` parameter\) or in the body of the request \(`document` parameter\)\.

A job can be made *continuous* by setting the optional `targetSelection` parameter to `CONTINUOUS`\. \(The default is `SNAPSHOT`\.\) A continuous job can be used to onboard or upgrade devices as they are added to a group because it continues to run and is executed on newly added things, even after the things in the group at the time the job was created have completed the job\.

A job can have an optional [TimeoutConfig](https://docs.aws.amazon.com/iot/latest/apireference/API_TimeoutConfig.html), which sets the value of the in\-progress timer\. The in\-progress timer can't be updated and applies to all executions of the job\.

The following validations are performed on arguments to the `CreateJob` API:
+ The `targets` argument must be a list of valid thing or thing group ARNs\. All things and thing groups must be in your AWS account\.
+ The `documentSource` argument must be a valid Amazon S3 URL to a job document\. Amazon S3 URLs are of the form: `https://s3.amazonaws.com/bucketName/objectName`\.
+ The document stored in the URL specified by the `documentSource` argument must be a UTF\-8 encoded JSON document\.
+ The size of a job document is limited to 32 KB due to the limit on the size of an MQTT message\(128 KB\) and encryption\.
+ The `jobId` must be unique in your AWS account\.

------
#### [ HTTPS request ]

```
PUT /jobs/jobId
 
{
"targets": [ "string" ],
"document": "string",
"documentSource": "string",
"description": "string",
"jobTemplateArn": "string",
"presignedUrlConfigData": {
    "roleArn": "string", 
    "expiresInSec": "integer" 
},
"targetSelection": "CONTINUOUS|SNAPSHOT",
"jobExecutionsRolloutConfig": { 
    "exponentialRate": { 
       "baseRatePerMinute": integer,
       "incrementFactor": integer,
       "rateIncreaseCriteria": { 
          "numberOfNotifiedThings": integer, // Set one or the other
          "numberOfSucceededThings": integer // of these two values.
       },
       "maximumPerMinute": integer
  }
},
"abortConfig": { 
   "criteriaList": [ 
      { 
         "action": "string",
         "failureType": "string",
         "minNumberOfExecutedThings": integer,
         "thresholdPercentage": integer
      }
   ]
},
"timeoutConfig": { 
  "inProgressTimeoutInMinutes": long
}
}
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html) 

------
#### [ CLI syntax ]

```
aws iot create-job \
    --job-id <value> \
    --targets <value> \
    [--document-source <value>] \
    [--document <value>] \
    [--description <value>] \
    [--job-template-arn <value>] \
    [--presigned-url-config <value>] \
    [--target-selection <value>] \
    [--job-executions-rollout-config <value>] \
    [--abort-config <value>] \
    [--timeout-config <value>] \
    [--document-parameters <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
    "jobId": "string",
    "targets": [ "string" ],
    "documentSource": "string",
    "document": "string",
    "description": "string",
    "jobTemplateArn": "string",
    "presignedUrlConfig": {
        "roleArn": "string",
        "expiresInSec": long
     },
    "targetSelection": "string",
    "jobExecutionsRolloutConfig": { 
          "exponentialRate": { 
              "baseRatePerMinute": integer,
              "incrementFactor": integer,
              "rateIncreaseCriteria": { 
                 "numberOfNotifiedThings": integer, // Set one or the other
                 "numberOfSucceededThings": integer // of these two values.
              },
      "maximumPerMinute": integer
      }
    }, 
    "abortConfig": { 
    "criteriaList": [ 
        { 
           "action": "string",
           "failureType": "string",
           "minNumberOfExecutedThings": integer,
           "thresholdPercentage": integer
         }
      ]
    },
    "timeoutConfig": { 
          "inProgressTimeoutInMinutes": long
    },
    "documentParameters": {
    "string": "string"
    }
}
```

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/iot/create-job.html](https://docs.aws.amazon.com/cli/latest/reference/iot/create-job.html)\. 

------

### DeleteJob<a name="jobs-DeleteJob"></a>

Deletes a job and its related job executions\.

Deleting a job can take time, depending on the number of job executions created for the job and various other factors\. While the job is being deleted, the status of the job is shown as "DELETION\_IN\_PROGRESS"\. Attempting to delete or cancel a job whose status is already "DELETION\_IN\_PROGRESS" results in an error\.

------
#### [ HTTPS request ]

```
DELETE /jobs/jobId?force=force 
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJob.html)\.

------
#### [ CLI syntax ]

```
aws iot  delete-job \
--job-id <value> \
[--force | --no-force]  \
[--cli-input-json <value>] \
[--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
"jobId": "string",
"force": boolean
}
```

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/iot/delete-job.html](https://docs.aws.amazon.com/cli/latest/reference/iot/delete-job.html)\.

------

### DeleteJobExecution<a name="jobs-DeleteJobExecution"></a>

Deletes a job execution\.

------
#### [ HTTPS request ]

```
DELETE /things/thingName/jobs/jobId/executionNumber/executionNumber?force=force
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJobExecution.html)\.

------
#### [ CLI syntax ]

```
aws iot  delete-job-execution \
--job-id <value> \
--thing-name <value> \
--execution-number <value> \
[--force | --no-force]  \
[--cli-input-json <value>] \
[--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
"jobId": "string",
"thingName": "string",
"executionNumber": long,
"force": boolean
}
```

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/iot/delete-job-execution.html](https://docs.aws.amazon.com/cli/latest/reference/iot/delete-job-execution.html)\.

------

### DescribeJob<a name="jobs-DescribeJob"></a>

Gets the details of the job execution\.

------
#### [ HTTPS request ]

```
GET /jobs/jobId
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeJob.html)\.

------
#### [ CLI syntax ]

```
aws iot describe-job \
--job-id <value>  \
[--cli-input-json <value>] \
[--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
"jobId": "string"
}
```

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/iot/describe-job.html](https://docs.aws.amazon.com/cli/latest/reference/iot/describe-job.html)\.

------

### DescribeJobExecution<a name="jobs-DescribeJobExecution"></a>

Gets details of a job execution\. The job's execution status must be `SUCCEEDED` or `FAILED`\.

------
#### [ HTTPS request ]

```
GET /things/thingName/jobs/jobId?executionNumber=executionNumber
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeJobExecution.html)\.

------
#### [ CLI syntax ]

```
aws iot  describe-job-execution \
--job-id <value> \
--thing-name <value> \
[--execution-number <value>]  \
[--cli-input-json <value>] \
[--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
"jobId": "string",
"thingName": "string",
"executionNumber": long
}
```

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/iot/describe-job-execution.html](https://docs.aws.amazon.com/cli/latest/reference/iot/describe-job-execution.html)\.

------

### GetJobDocument<a name="jobs-GetJobDocument"></a>

Gets the job document for a job\.

**Note**  
Placeholder URLs are not replaced with presigned Amazon S3 URLs in the document returned\. Presigned URLs are generated only when the AWS IoT Jobs service receives a request over MQTT\.

------
#### [ HTTPS request ]

```
GET /jobs/jobId/job-document
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_GetJobDocument.html](https://docs.aws.amazon.com/iot/latest/apireference/API_GetJobDocument.html)\.

------
#### [ CLI syntax ]

```
aws iot get-job-document \
--job-id <value>  \
[--cli-input-json <value>] \
[--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
"jobId": "string"
}
```

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/iot/get-job-document.html](https://docs.aws.amazon.com/cli/latest/reference/iot/get-job-document.html)\.

------

### ListJobExecutionsForJob<a name="jobs-listJobExecutionsForJob"></a>

Gets a list of job executions for a job\.

------
#### [ HTTPS request ]

```
GET /jobs/jobId/things?status=status&maxResults=maxResults&nextToken=nextToken
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_ListJobExecutionsForJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_ListJobExecutionsForJob.html)\.

------
#### [ CLI syntax ]

```
aws iot  list-job-executions-for-job \
--job-id <value> \
[--status <value>] \
[--max-results <value>] \
[--next-token <value>]  \
[--cli-input-json <value>] \
[--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
"jobId": "string",
"status": "string",
"maxResults": "integer",
"nextToken": "string"
}
```

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/iot/list-job-executions-for-job.html](https://docs.aws.amazon.com/cli/latest/reference/iot/list-job-executions-for-job.html)\.

------

### ListJobExecutionsForThing<a name="jobs-ListJobExecutionsForThing"></a>

Gets a list of job executions for a thing\.

------
#### [ HTTPS request ]

```
GET /things/thingName/jobs?status=status&maxResults=maxResults&nextToken=nextToken
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_ListJobExecutionsForThing.html](https://docs.aws.amazon.com/iot/latest/apireference/API_ListJobExecutionsForThing.html)\.

------
#### [ CLI syntax ]

```
aws iot list-job-executions-for-thing \
--thing-name <value> \
[--status <value>] \
[--max-results <value>] \
[--next-token <value>]  \
[--cli-input-json <value>] \
[--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
"thingName": "string",
"status": "string",
"maxResults": "integer",
"nextToken": "string"
}
```

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/iot/list-job-executions-for-thing.html](https://docs.aws.amazon.com/cli/latest/reference/iot/list-job-executions-for-thing.html)\.

------

### ListJobs<a name="jobs-listJobs"></a>

Gets a list of jobs in your AWS account\.

------
#### [ HTTPS request ]

```
GET /jobs?status=status&targetSelection=targetSelection&thingGroupName=thingGroupName&thingGroupId=thingGroupId&maxResults=maxResults&nextToken=nextToken
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_ListJobs.html](https://docs.aws.amazon.com/iot/latest/apireference/API_ListJobs.html)\.

------
#### [ CLI syntax ]

```
aws iot list-jobs \
[--status <value>] \
[--target-selection <value>] \
[--max-results <value>] \
[--next-token <value>] \
[--thing-group-name <value>] \
[--thing-group-id <value>]  \
[--cli-input-json <value>] \
[--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
"status": "string",
"targetSelection": "string",
"maxResults": "integer",
"nextToken": "string",
"thingGroupName": "string",
"thingGroupId": "string"
}
```

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/iot/list-jobs.html](https://docs.aws.amazon.com/cli/latest/reference/iot/list-jobs.html)\.

------

### UpdateJob<a name="jobs-UpdateJob"></a>

Updates supported fields of the specified job\. Updated values for `timeoutConfig` take effect for only newly in\-progress executions\. Currently in\-progress executions continue to execute with the old timeout configuration\.

------
#### [ HTTPS request ]

```
PATCH /jobs/jobId
{
"description": "string",
"presignedUrlConfig": { 
  "expiresInSec": number,
  "roleArn": "string"
},
"jobExecutionsRolloutConfig": { 
  "exponentialRate": { 
     "baseRatePerMinute": number,
     "incrementFactor": number,
     "rateIncreaseCriteria": { 
        "numberOfNotifiedThings": number,
        "numberOfSucceededThings": number
     }, 
  "maximumPerMinute": number
  },
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
"timeoutConfig": { 
  "inProgressTimeoutInMinutes": number
}
}
```

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateJob.html) \.

------
#### [ CLI syntax ]

```
aws iot  update-job \
--job-id <value> \
[--description <value>] \
[--presigned-url-config <value>] \
[--job-executions-rollout-config <value>] \
[--abort-config <value>] \
[--timeout-config <value>] \
[--cli-input-json <value>] \
[--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
"description": "string",
"presignedUrlConfig": { 
  "expiresInSec": number,
  "roleArn": "string"
},
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
"timeoutConfig": { 
  "inProgressTimeoutInMinutes": number
}
}
```

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/iot/update-job.html](https://docs.aws.amazon.com/cli/latest/reference/iot/update-job.html)\.

------