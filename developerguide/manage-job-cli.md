# Creating and Managing Jobs \(CLI\)<a name="manage-job-cli"></a>

This section describes how to create and manage jobs\.

## Create Jobs<a name="create-job"></a>

You use the CreateJob command to create an AWS IoT job\. The job is queued for execution on the targets \(things or thing groups\) that you specify\. To create an AWS IoT job, you need a job document that can be included in the body of the request or as a link to an Amazon S3 document\. If the job includes downloading files using presigned Amazon S3 URLs, you need an IAM role ARN that has permission to download the file and grants permission to AWS IoT Jobs to assume the role\.

### Code\-signing with Jobs<a name="code-signing-with-jobs"></a>

If you are using Code\-signing for AWS IoT, you must start a code\-signing job and include the output in your job document\. Use the [start\-signing\-job](https://docs.aws.amazon.com/signer/latest/developerguide/api-startsigningjob.html) command to create a code\-signing job\. `start-signing-job` returns a job ID\. Use the describe\-signing\-job command to get the Amazon S3 location where the signature is stored\. You can then download the signature from Amazon S3\. For more information about code\-signing jobs, see [Code\-signing for AWS IoT](https://docs.aws.amazon.com/signer/latest/developerguide/Welcome.html)\.

Your job document must contain a pre\-signed URL placeholder for your code file and the JSON signature output placed in an Amazon S3 bucket using the start\-signing\-job command, enclosed in a `codesign`element:

```
{
    "presign": "${aws:iot:s3-presigned-url:https://s3.region.amazonaws.com/bucket/image}",
    "codesign": {
        "rawPayloadSize": <image-file-size>,
        "signature": <signature>,
        "signatureAlgorithm": <signature-algorithm>,
        "payloadLocation": {
            "s3": {
                "bucketName": <my-s3-bucket>,
                "key": <my-code-file>,
                "version": <code-file-version-id>
            }
        }
    }
}
```

### Creating a Job with a Job Document<a name="create-job-with-document"></a>

The following command shows how to create a job using a job document \(*job\-document\.json*\) stored in an Amazon S3 bucket \(*jobBucket*\) and a role with permission to download files from Amazon S3 \(*S3DownloadRole*\)\.

```
aws iot create-job  \
      --job-id 010  \
      --targets arn:aws:iot:us-east-1:123456789012:thing/thingOne  \
      --document-source https://s3.amazonaws.com/my-s3-bucket/job-document.json  \
      --timeout-config inProgressTimeoutInMinutes=100 \
      --job-executions-rollout-config "{ \"exponentialRate\": { \"baseRatePerMinute\": 50, \"incrementFactor\": 2, \"rateIncreaseCriteria\": { \"numberOfNotifiedThings\": 1000, \"numberOfSucceededThings\": 1000}, \"maximumPerMinute\": 1000}}" \
      --abort-config "{ \"criteriaList\": [ { \"action\": \"CANCEL\", \"failureType\": \"FAILED\", \"minNumberOfExecutedThings\": 100, \"thresholdPercentage\": 20}, { \"action\": \"CANCEL\", \"failureType\": \"TIMED_OUT\", \"minNumberOfExecutedThings\": 200, \"thresholdPercentage\": 50}]}" \	      
      --presigned-url-config "{\"roleArn\":\"arn:aws:iam::123456789012:role/S3DownloadRole\", \"expiresInSec\":3600}"
```

The job is executed on *thingOne*\.

The optional `timeout-config` parameter specifies the amount of time each device has to finish its execution of the job\. The timer starts when the job execution status is set to `IN_PROGRESS`\. If the job execution status is not set to another terminal state  before the time expires, it is set to `TIMED_OUT`\.

The in\-progress timer can't be updated and applies to all job executions for the job\. Whenever a job execution remains in the `IN_PROGRESS` status for longer than this interval, the job execution fails and switches to the terminal `TIMED_OUT` status\. AWS IoT also publishes an MQTT notification\.

For more information about creating configurations about job rollouts and aborts, see [Job Rollout and Abort Configuration](job-rollout-abort.html)\.

**Note**  
Job documents that are specified as Amazon S3 files are retrieved at the time you create the job\. Changing the contents of the Amazon S3 file you used as the source of your job document after you have created the job does not change what is sent to the targets of the job\.

## Update a Job<a name="update-job"></a>

You use the UpdateJob command to update a job\. You can update the `description`, `presignedUrlConfig`, `jobExecutionsRolloutConfig`, `abortConfig`, and `timeoutConfig` fields of a job\.

```
aws iot update-job  \
  --job-id 010  \
  --description "updated description" \
  --timeout-config inProgressTimeoutInMinutes=100 \
  --job-executions-rollout-config "{ \"exponentialRate\": { \"baseRatePerMinute\": 50, \"incrementFactor\": 2, \"rateIncreaseCriteria\": { \"numberOfNotifiedThings\": 1000, \"numberOfSucceededThings\": 1000}, \"maximumPerMinute\": 1000}}" \
  --abort-config "{ \"criteriaList\": [ { \"action\": \"CANCEL\", \"failureType\": \"FAILED\", \"minNumberOfExecutedThings\": 100, \"thresholdPercentage\": 20}, { \"action\": \"CANCEL\", \"failureType\": \"TIMED_OUT\", \"minNumberOfExecutedThings\": 200, \"thresholdPercentage\": 50}]}" \	      
  --presigned-url-config "{\"roleArn\":\"arn:aws:iam::123456789012:role/S3DownloadRole\", \"expiresInSec\":3600}"
```

For more information about configuring job rollouts and aborts, see [Job Rollout and Abort Configuration](job-rollout-abort.html)\.

## Cancel a Job<a name="cancel-job"></a>

You use the CancelJob command to cancel a job\. Canceling a job stops AWS IoT from rolling out any new job executions for the job\. It also cancels any job executions that are in a `QUEUED` state\. AWS IoT leaves any job executions in a terminal state  untouched because the device has already completed the job\. If the status of a job execution is `IN_PROGRESS`, it also remains untouched unless you use the optional `--force` parameter\.

The following command shows how to cancel a job with ID 010\.

```
aws iot cancel-job --job-id 010
```

The command displays no output\.

When you cancel a job, job executions that are in a `QUEUED` state are canceled\. Job executions that are in an `IN_PROGRESS` state are canceled if you specify the optional `--force` parameter\. Job executions in a terminal state are not canceled\.

**Warning**  
Canceling a job that is in the `IN_PROGRESS` state \(by setting the `--force` parameter\) cancels any job executions that are in progress and causes the device that is executing the job to be unable to update the job execution status\. Use caution and make sure that each device executing a canceled job is able to recover to a valid state\.

The status of a canceled job or of one of its job executions is eventually consistent\. AWS IoT stops scheduling new job executions and `QUEUED` job executions for that job to devices as soon as possible\. Changing the status of a job execution to `CANCELED` might take some time, depending on the number of devices and other factors\.

If a job is cancelled because it has met the criteria defined by an `AbortConfig` object, the service will add auto\-populated values for the `comment` and `reasonCode` fields\. Customers can create their own values for `reasonCode` when the job cancellation is user\-driven\.

## Cancel a Job Execution<a name="cancel-job-execution"></a>

You use the CancelJobExecution command to cancel a job execution on a device\. It cancels a job execution that is in a `QUEUED` state\. If you want to cancel a job execution that is in progress, you must use the `--force` parameter\.

The following command shows how to cancel the job execution from job 010 running on `myThing`\.

```
aws iot cancel-job-execution --job-id 010 --thing-name myThing
```

The command displays no output\.

A job execution that is in a `QUEUED` state is canceled\. A job execution that is in an `IN_PROGRESS` state is canceled if you specify the optional `--force` parameter\. Job executions in a terminal state cannot be canceled\. 

**Warning**  
When you cancel a job execution that is in the `IN_PROGRESS` state, the device is unable to update the job execution status\. Use caution and ensure that the device is able to recover to a valid state\.

If the job execution is in a terminal state or if the job execution is in an `IN_PROGRESS` state and the `--force` parameter is not set to `true`, this command causes an `InvalidStateTransitionException`\.

The status of a canceled job execution is eventually consistent\. Changing the status of a job execution to `CANCELED` might take some time, depending various factors\.

## Delete a Job<a name="delete-job"></a>

You use the DeleteJob command to delete a job and its job executions\. By default, you can only delete a job that is in a terminal state \(`SUCCEEDED` or `CANCELED`\)\. Otherwise, an exception occurs\. You can delete a job in the `IN_PROGRESS` state if the `force` parameter is set to `true`\.

Run the following command to delete a job:

```
aws iot delete-job --job-id 010 --force|--no-force
```

The command displays no output\.

**Warning**  
When you delete a job that is in the `IN_PROGRESS` state, the device that is executing the job cannot access job information or update the job execution status\. Use caution and make sure that each device executing a job that has been deleted is able to recover to a valid state\.

It can take some time to delete a job, depending on the number of job executions created for the job and various other factors\. While the job is being deleted, `DELETION_IN_PROGRESS` appears as the status of the job\. An error results if you attempt to delete or cancel a job whose status is already `DELETION_IN_PROGRESS`\.

Only 10 jobs can have a status of `DELETION_IN_PROGRESS` at the same time\. Otherwise, a `LimitExceededException` occurs\.

## Get a Job Document<a name="get-job-document"></a>

You use the GetJobDocumentcommand to retrieve a job document for a job\. A job document is a description of the remote operations to be performed by the devices\.

Run the following command to get a job document:

```
aws iot get-job-document --job-id 010
```

The command returns the job document for the specified job:

```
{
    "document": "{\n\t\"operation\":\"install\",\n\t\"url\":\"http://amazon.com/firmWareUpate-01\",\n\t\"data\":\"${aws:iot:s3-presigned-url:https://s3.amazonaws.com/job-test-bucket/datafile}\"\n}"
}
```

**Note**  
When you use this command to retrieve a job document, placeholder URLs are not replaced by presigned Amazon S3 URLs\. When a device calls the [GetPendingJobExecutions](jobs-api.md#mqtt-getpendingjobexecutions) MQTT API, the placeholder URLs are replaced by presigned Amazon S3 URLs in the job document\. 

## List Jobs<a name="list-jobs"></a>

You use the ListJobs command to get a list of all jobs in your AWS account\. Job data and job execution data are purged after 90 days\. Run the following command to list all jobs in your AWS account:

```
aws iot list-jobs
```

The command returns all jobs in your account, sorted by the job status:

```
{
    "jobs": [
        {
            "status": "IN_PROGRESS", 
            "lastUpdatedAt": 1486687079.743, 
            "jobArn": "arn:aws:iot:us-east-1:123456789012:job/013", 
            "createdAt": 1486687079.743, 
            "targetSelection": "SNAPSHOT",
            "jobId": "013"
        }, 
        {
            "status": "SUCCEEDED", 
            "lastUpdatedAt": 1486685868.444, 
            "jobArn": "arn:aws:iot:us-east-1:123456789012:job/012", 
            "createdAt": 1486685868.444, 
            "completedAt": 148668789.690,
            "targetSelection": "SNAPSHOT",
            "jobId": "012"
        }, 
        {
            "status": "CANCELED", 
            "lastUpdatedAt": 1486678850.575, 
            "jobArn": "arn:aws:iot:us-east-1:123456789012:job/011", 
            "createdAt": 1486678850.575, 
            "targetSelection": "SNAPSHOT",
            "jobId": "011"
        }
    ]
}
```

## Describe a Job<a name="describe-job"></a>

Run the DescribeJob command to get the status of a specific job\. The following command shows how to describe a job:

```
$ aws iot describe-job --job-id 010
```

The command returns the status of the specified job\. For example:

```
{
    "documentSource": "https://s3.amazonaws.com/job-test-bucket/job-document.json", 
    "job": {
        "status": "IN_PROGRESS", 
        "jobArn": "arn:aws:iot:us-east-1:123456789012:job/010", 
        "targets": [
            "arn:aws:iot:us-east-1:123456789012:thing/myThing"
        ], 
        "jobProcessDetails": {
            "numberOfCanceledThings": 0, 
            "numberOfFailedThings": 0,
            "numberOfInProgressThings": 0,
            "numberOfQueuedThings": 0,
            "numberOfRejectedThings": 0,
            "numberOfRemovedThings": 0,
            "numberOfSucceededThings": 0,
            "numberOfTimedOutThings": 0,
            "processingTargets": [
                arn:aws:iot:us-east-1:123456789012:thing/thingOne, 
                arn:aws:iot:us-east-1:123456789012:thinggroup/thinggroupOne, 
                arn:aws:iot:us-east-1:123456789012:thing/thingTwo, 
                arn:aws:iot:us-east-1:123456789012:thinggroup/thinggroupTwo 
            ]
        }, 
        "presignedUrlConfig": {
            "expiresInSec": 60, 
            "roleArn": "arn:aws:iam::123456789012:role/S3DownloadRole"
        }, 
        "jobId": "010", 
        "lastUpdatedAt": 1486593195.006, 
        "createdAt": 1486593195.006,
        "targetSelection": "SNAPSHOT",
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
           "inProgressTimeoutInMinutes": number
          }
    }
}
```

## List Executions for a Job<a name="list-job-executions-for-job"></a>

A job running on a specific device is represented by a job execution object\. Run the ListJobExecutionsForJob command to list all job executions for a job\. The following shows how to list the executions for a job:

```
aws iot list-job-executions-for-job --job-id 010
```

The command returns a list of job executions:

```
{
    "executionSummaries": [
    {
        "thingArn": "arn:aws:iot:us-east-1:123456789012:thing/thingOne", 
        "jobExecutionSummary": {
            "status": "QUEUED", 
            "lastUpdatedAt": 1486593196.378, 
            "queuedAt": 1486593196.378,
            "executionNumber": 1234567890
        }
    },
    {
        "thingArn": "arn:aws:iot:us-east-1:123456789012:thing/thingTwo", 
        "jobExecutionSummary": {
            "status": "IN_PROGRESS", 
            "lastUpdatedAt": 1486593345.659, 
            "queuedAt": 1486593196.378,
            "startedAt": 1486593345.659,
            "executionNumber": 4567890123
        }
    }
    ]
}
```

## List Job Executions for a Thing<a name="list-job-executions-for-thing"></a>

Run the ListJobExecutionsForThing command to list all job executions running on a thing\. The following shows how to list job executions for a thing:

```
aws iot list-job-executions-for-thing --thing-name thingOne
```

The command returns a list of job executions that are running or have run on the specified thing:

```
{
    "executionSummaries": [
    {
        "jobExecutionSummary": {
            "status": "QUEUED", 
            "lastUpdatedAt": 1486687082.071, 
            "queuedAt": 1486687082.071,
            "executionNumber": 9876543210
        }, 
        "jobId": "013"
    }, 
    {
        "jobExecutionSummary": {
            "status": "IN_PROGRESS",
            "startAt": 1486685870.729, 
            "lastUpdatedAt": 1486685870.729, 
            "queuedAt": 1486685870.729,
            "executionNumber": 1357924680
        }, 
        "jobId": "012"
    }, 
    {
        "jobExecutionSummary": {
            "status": "SUCCEEDED", 
            "startAt": 1486678853.415,
            "lastUpdatedAt": 1486678853.415, 
            "queuedAt": 1486678853.415,
            "executionNumber": 4357680912
        }, 
        "jobId": "011"
    }, 
    {
        "jobExecutionSummary": {
            "status": "CANCELED",
            "startAt": 1486593196.378,
            "lastUpdatedAt": 1486593196.378, 
            "queuedAt": 1486593196.378,
            "executionNumber": 2143174250
        }, 
        "jobId": "010"
    }
    ]
}
```

## Describe Job Execution<a name="describe-job-execution"></a>

Run the DescribeJobExecution command to get the status of a job execution\. You must specify a job ID and thing name and, optionally, an execution number to identify the job execution\. The following shows how to describe a job execution:

```
aws iot describe-job-execution --job-id 017 --thing-name thingOne
```

The command returns the [JobExecution](jobs-api.md#jobs-job-execution)\. For example:

```
{
    "execution": {
        "jobId": "017", 
        "executionNumber": 4516820379,
        "thingArn": "arn:aws:iot:us-east-1:123456789012:thing/thingOne", 
        "versionNumber": 123,
        "createdAt": 1489084805.285, 
        "lastUpdatedAt": 1489086279.937, 
        "startedAt": 1489086279.937, 
        "status": "IN_PROGRESS",
        "approximateSecondsBeforeTimedOut": 100,
        "statusDetails": {
            "status": "IN_PROGRESS", 
            "detailsMap": {
                "percentComplete": "10"
            }
        }
    }
}
```

## Delete Job Execution<a name="delete-job-execution"></a>

Run the DeleteJobExecution command to delete a job execution\. You must specify a job ID and a thing name and, optionally, an execution number to identify the job execution\. The following shows how to delete a job execution:

```
aws iot delete-job-execution --job-id 017 --thing-name thingOne --execution-number 1234567890 --force|--no-force
```

The command displays no output\.

By default, the status of the job execution must be `QUEUED` or in a terminal state \(`SUCCEEDED`, `FAILED`, `REJECTED`, `TIMED_OUT`, `REMOVED` or `CANCELED`\)\. Otherwise, an error occurs\. To delete a job execution with a status of `IN_PROGRESS`, you can set the `force` parameter to `true`\.

**Warning**  
When you delete a job execution with a status of `IN_PROGRESS`, the device that is executing the job cannot access job information or update the job execution status\. Use caution and make sure that the device is able to recover to a valid state\.