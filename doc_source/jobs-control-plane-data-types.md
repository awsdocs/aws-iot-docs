# Job management and control data types<a name="jobs-control-plane-data-types"></a>

The following data types are used by management and control applications to communicate with the AWS IoT Jobs service\.

## Job<a name="jobs-job"></a>

------
#### [ Job data type ]

The `Job` object contains details about a job\.

------
#### [ Syntax \(1\) ]

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

------
#### [ Description \(1\) ]

`jobArn`  
An ARN identifying the job with the format "arn:aws:iot:*region*:*account*:job/*jobId*"\.

`jobId`  
The unique identifier you assigned to this job when it was created\.

`status`  
The status of the job, one of `IN_PROGRESS`, `CANCELED`, or `SUCCEEDED`\.

`targetSelection`  
Specifies whether the job continues to run \(CONTINUOUS\) or is complete after those things specified as targets have completed the job \(SNAPSHOT\)\. If CONTINUOUS, the job might also be run on a thing when a change is detected in a target\. For example, a job runs on a thing when the thing is added to a target group, even after the job was completed by all things originally in the group\.

`comment`  
If the job was updated, describes the reason for the update\. 

`targets`  
A list of AWS IoT things and thing groups to which the job should be sent\. 

`description`  
A short text description of the job\.

`createdAt`  
The time, in seconds since the epoch, when the job was created\. 

`lastUpdatedAt`  
The time, in seconds since the epoch, when the job was last updated\.

`completedAt`  
The time, in seconds since the epoch, when the job was completed\.

`jobProcessDetails`  
Details about the job process:    
`processingTargets`  
A list of AWS IoT things and thing groups that are currently executing the job\.  
`numberOfCanceledThings`  
The number of AWS IoT things that canceled the job\.  
`numberOfSucceededThings`  
The number of AWS IoT things that successfully completed the job\.   
`numberOfFailedThings`  
The number of AWS IoT things that failed to complete the job\.   
`numberOfRejectedThings`  
The number of AWS IoT things that rejected the job\.  
`numberOfQueuedThings`  
The number of AWS IoT things that are awaiting execution of the job\.   
`numberOfInProgressThings`  
The number of AWS IoT things that are currently executing the job\.   
`numberOfRemovedThings`  
The number of AWS IoT things that are no longer scheduled to execute the job because they have been deleted or removed from the group that was a target of the job\.  
`numberOfTimedOutThings`  
The number of things whose job execution status is `TIMED_OUT`\.

`presignedUrlConfig`  
Configuration information for presigned Amazon S3 URLs\.    
`expiresInSec`  
How long \(in seconds\) presigned URLs are valid\. Valid values are 60 \- 3600\. The default value is 3600 seconds\. Presigned URLs are generated when the AWS IoT Jobs service receives an MQTT request for the job document\.  
`roleArn`  
The ARN of an IAM role that grants permission to download files from an Amazon S3 bucket\. The role must also grant permission for AWS IoT to download the files\. For more information about how to create and configure the role, see [Create jobs](manage-job-cli.md#create-job)\.

`jobExecutionRolloutConfig`  
Optional\. Allows you to create a staged rollout of a job\.    
`maximumPerMinute`  
The maximum number of things \(devices\) to which the job is sent for execution, per minute\.  
`exponentialRate`  
Allows you to create an exponential rate of rollout for a job\.    
`baseRatePerMinute`  
The minimum number of things that are notified of a pending job, per minute, at the start of job rollout\. This parameter allows you to define the initial rate of rollout\.   
`incrementFactor`  
The exponential factor to increase the rate of rollout for a job\.  
`rateIncreaseCriteria`  
The criteria to initiate the increase in rate of rollout for a job\. You can specify either `numberOfNotifiedThings` or `numberOfSucceededThing`, but not both\.    
`numberOfNotifiedThings`  
The threshold for number of notified things that initiate the increase in rate of rollout\.   
`numberOfSucceededThings`  
The threshold for number of succeeded things that initiate the increase in rate of rollout\. 

`abortConfig`  
Optional\. Details of abort criteria to abort the job\.    
`criteriaList`  
The list of abort criteria to define rules to abort the job\.    
`action`  
The type of abort action to initiate a job abort\.  
`failureType`  
The type of job execution failure to define a rule to initiate a job abort\.  
`minNumberOfExecutedThings`  
Minimum number of executed things before evaluating an abort rule\.  
`thresholdPercentage`  
The threshold as a percentage of the total number of executed things that initiate a job abort\.

`timeoutConfig`  
Optional\. Specifies the amount of time each device has to finish its execution of the job\. The timer is started when the job execution status is set to `IN_PROGRESS`\. If the job execution status is not set to another terminal state before the time expires, it is set to `TIMED_OUT`\.     
`inProgressTimeoutInMinutes`  
Specifies the amount of time, in minutes, this device has to finish execution of this job\. A timer is started, or restarted, whenever this job's execution status is specified as `IN_PROGRESS` with this field populated\. If the job execution status is not set to a terminal state before the timer expires, or before another job execution status update is sent with this field populated, the status is set to `TIMED_OUT`\.

------

## JobSummary<a name="jobs-job-summary"></a>

------
#### [ JobSummary data type ]

The `JobSummary` object contains a job summary\.

------
#### [ Syntax \(2\) ]

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

------
#### [ Description \(2\) ]

`jobArn`  
An ARN that identifies the job\.

`jobId`  
The unique identifier you assigned to this job when it was created\.

`status`  
The job status\. Can be one of `IN_PROGRESS`, `CANCELED`, or `SUCCEEDED`\.

`targetSelection`  
Specifies whether the job continues to run \(CONTINUOUS\) or is complete after all those things specified as targets have completed the job \(SNAPSHOT\)\. If CONTINUOUS, the job might also be run on a thing when a change is detected in a target\. For example, a job runs on a thing when the thing is added to a target group, even after the job was completed by all things originally in the group\.

`thingGroupId`  
The ID of the thing group\.

`createdAt`  
The UNIX timestamp for when the job was created\.

`lastUpdatedAt`  
The UNIX timestamp for when the job was last updated\.

`completedAt`  
The UNIX timestamp for when the job was completed\.

------

## JobExecution<a name="jobs-job-execution"></a>

------
#### [ JobExection data type ]

The `JobExecution` object represents the execution of a job on a device\.

------
#### [ Syntax \(3\) ]

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

------
#### [ Description \(3\) ]

approximateSecondsBeforeTimedOut  
The estimated number of seconds that remain before the job execution status is changed to `TIMED_OUT`\. The timeout interval can be anywhere between 1 minute and 7 days \(1 to 10080 minutes\)\. The actual job execution timeout can occur up to 60 seconds later than the estimated duration\.

`jobId`  
The unique identifier you assigned to this job when it was created\.

`executionNumber`  
A number that identifies this job execution on this device\. It can be used later in commands that return or update job execution information\.

`thingArn`  
The AWS IoT thing ARN\.

`queuedAt`  
The time, in seconds since the epoch, when the job execution was queued\.

`lastUpdatedAt`  
The time, in seconds since the epoch, when the job execution was last updated\.

`startedAt`  
The time, in seconds since the epoch, when the job execution was started\.

`status`  
The status of the job execution\. Can be one of `QUEUED`, `IN_PROGRESS`, `FAILED`, `SUCCEEDED`, `CANCELED`, `TIMED_OUT`, `REJECTED`, or `REMOVED`\.

`statusDetails`  
A collection of name\-value pairs that describe the status of the job execution\. 

------

## JobExecutionSummary<a name="jobs-job-execution-summary"></a>

------
#### [ JobExecutionSummary data type ]

The `JobExecutionSummary` object contains job execution summary information:

------
#### [ Syntax \(4\) ]

```
{
    "executionNumber": 1234567890,
    "queuedAt": timestamp,
    "lastUpdatedAt": timestamp,
    "startedAt": timestamp,
    "status": "QUEUED|IN_PROGRESS|FAILED|SUCCEEDED|CANCELED|TIMED_OUT|REJECTED|REMOVED"
}
```

------
#### [ Description \(4\) ]

`executionNumber`  
A number that identifies a job execution on a device\. It can be used later in commands that return or update job execution information\.

`queuedAt`  
The time, in seconds since the epoch, when the job execution was queued\.

`lastUpdatedAt`  
The time, in seconds since the epoch, when the job execution was last updated\.

`startAt`  
The time, in seconds since the epoch, when the job execution was started\.

`status`  
The status of the job execution: `QUEUED`, `IN_PROGRESS`, `FAILED`, `SUCCEEDED`, `CANCELED`, `TIMED_OUT`, `REJECTED`, or `REMOVED`\.

------

## JobExecutionSummaryForJob<a name="jobs-job-execution-summary-for-job"></a>

------
#### [ JobExecutionSummaryForJob data type ]

The `JobExecutionSummaryForJob` object contains a summary of information about job executions for a specific job\.

------
#### [ Syntax \(5\) ]

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

------
#### [ Description \(5\) ]

`thingArn`  
The AWS IoT thing ARN\.

`jobExecutionSummary`  
An [JobExecutionSummary](#jobs-job-execution-summary) object\.

------

## JobExecutionSummaryForThing<a name="jobs-job-execution-summary-for-thing"></a>

------
#### [ JobExecutionSummaryForThing data type ]

The `JobExecutionSummaryForThing` object contains a summary of information about a job execution on a specific thing\.

------
#### [ Syntax \(6\) ]

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

------
#### [ Description \(6\) ]

`jobId`  
The unique identifier you assigned to this job when it was created\.

`jobExecutionSummary`  
A [JobExecutionSummary](#jobs-job-execution-summary) object\.

------