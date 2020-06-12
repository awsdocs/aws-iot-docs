# Using the AWS IoT jobs APIs<a name="jobs-api"></a>

There are two categories of API used in the AWS IoT Jobs service: 
+ Those used for management and control of jobs\.
+ Those used by the devices executing those jobs\.

In general, job management and control uses an HTTPS protocol API\. Devices can use either an MQTT or an HTTPS protocol API\. \(The HTTPS API is designed for a low volume of calls typical when creating and tracking jobs\. It usually opens a connection for a single request, and then closes the connection after the response is received\. The MQTT API allows long polling\. It is designed for large amounts of traffic that can scale to millions of devices\.\)

**Note**  
Each AWS IoT Jobs HTTPS API has a corresponding command that allows you to call the API from the AWS CLI\. The commands are lowercase, with hyphens between the words that make up the name of the API\. For example, you can invoke the `CreateJob` API on the CLI by typing:  

```
aws iot create-job ...
```

## Job management and control API<a name="jobs-http-api"></a>

### Job management and control data types<a name="jobs-control-plane-data-types"></a>

The following data types are used by management and control applications to communicate with the AWS IoT Jobs service\.

#### Job<a name="jobs-job"></a>

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

#### JobSummary<a name="jobs-job-summary"></a>

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

#### JobExecution<a name="jobs-job-execution"></a>

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

#### JobExecutionSummary<a name="jobs-job-execution-summary"></a>

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

#### JobExecutionSummaryForJob<a name="jobs-job-execution-summary-for-job"></a>

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

#### JobExecutionSummaryForThing<a name="jobs-job-execution-summary-for-thing"></a>

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

### Job management and control HTTPS and CLI commands<a name="jobs-control-plane-commands"></a>

The following commands are available for management and control applications in the CLI and over the HTTPS protocol\. To get the value for the `endpoint-url` parameter in the CLI commands, use the following command\.

`aws iot describe-endpoint --endpoint-type=iot:Jobs`

This command generates the following output\.

```
{
    "endpointAddress": "unique-.jobs.iot.us-west-2.amazonaws.com"
}
```

#### AssociateTargetsWithJob<a name="jobs-AssociateTargetsWithJob"></a>

------
#### [ AssociateTargetsWithJob command ]

Associates a group with a continuous job\. For more information, see [CreateJob](#jobs-CreateJob)\. The following criteria must be met: 
+ The job must have been created with the `targetSelection` field set to `CONTINUOUS`\.
+ The job status must currently be `IN_PROGRESS`\.
+ The total number of targets associated with a job must not exceed 100\.

------
#### [ HTTPS \(1\) ]

Request:

```
POST /jobs/jobId/targets

{ 
    "targets": [ "string" ],
    "comment": "string"
}
```

`jobId`  
The unique identifier you assigned to this job when it was created\.

`targets`  
A list of thing group ARNs that define the targets of the job\.

`comment`  
Optional\. A comment string that describes why the job was associated with the targets\.

Response:

```
{
    "jobArn": "string",
    "jobId": "string",
    "description": "string"
}
```

`jobArn`  
An ARN identifying the job\.

`jobId`  
The unique identifier you assigned to this job when it was created\.

`description`  
A short text description of the job\.

------
#### [ CLI \(1\) ]

**Synopsis:**

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


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  targets  |  list  member: TargetArn  |  A list of thing group ARNs that define the targets of the job\.  | 
|  TargetArn  |  string  |   | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to this job when it was created\.  | 
|  comment  |  string  length max:2028  pattern: \[^\\\\p\{C\}\]\+  |  An optional string that describes why the job was associated with the targets\.  | 

Output:

```
{
  "jobArn": "string",
  "jobId": "string",
  "description": "string"
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  jobArn  |  string  |  An ARN identifying the job\.  | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to this job when it was created\.  | 
|  description  |  string  length max:2028  pattern: \[^\\\\p\{C\}\]\+  |  A short text description of the job\.  | 

------

#### CancelJob<a name="jobs-CancelJob"></a>

------
#### [ CancelJob command ]

Cancels a job\.

------
#### [ HTTPS \(2\) ]

Request:

```
PUT /jobs/jobId/cancel

{ 
    "force": boolean,
    "comment": "string",
    "reasonCode": "string"
}
```

`jobId`  
The unique identifier you assigned to this job when it was created\.

`force`  
\[Optional\] If `true`, job executions with status `IN_PROGRESS` and `QUEUED` are canceled\. Otherwise, only job executions with status `QUEUED` are canceled\. The default is `false`\.  
Canceling a job with a status of `IN_PROGRESS` causes a device that is executing the job to be unable to update the job execution status\. Use caution and make sure that each device executing a job that is canceled is able to recover to a valid state\.

`comment`  
\[Optional\] A comment string that describes why the job was canceled\.

`reasonCode`  
\[Optional\] A reason code string that explains why the job was canceled\. If a job is cancelled because it meets the conditions defined by an `abortConfig`, this field is auto\-populated\.

Response:

```
{
    "jobArn": "string",
    "jobId": "string",
    "description": "string"
}
```

`jobArn`  
The job ARN\.

`jobId`  
The unique identifier you assigned to this job when it was created\.

`description`  
A short text description of the job\.

------
#### [ CLI \(2\) ]

**Synopsis:**

```
aws iot  cancel-job \
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


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to this job when it was created\.  | 
|  force  |  boolean  |  If true, jobs with status QUEUED and IN\_PROGRESS are canceled\. Otherwise, only jobs with status QUEUED are canceled\.  Canceling a job with a status of IN\_PROGRESS causes a device that is executing the job to be unable to update the job execution status\. Use caution and make sure that each device executing a job that is canceled is able to recover to a valid state\.   | 
|  comment  |  string  length max:2028  pattern: \[^\\\\p\{C\}\]\+  |  An optional string that describes why the job was canceled\.  | 
|  reasonCode  |  string  length max:128  pattern: \[\\p\{Upper\}\\p\{Digit\}\_\]\+  |  An optional string that explains why the job was canceled\. If the job is canceled because it meets the criteria defined by the `abortConfig`, this field is auto\-populated\.  | 

Output:

```
{
  "jobArn": "string",
  "jobId": "string",
  "description": "string"
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  jobArn  |  string  |  The job ARN\.  | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to this job when it was created\.  | 
|  description  |  string  length max:2028  pattern: \[^\\\\p\{C\}\]\+  |  A short text description of the job\.  | 

------

#### CancelJobExecution<a name="jobs-CancelJobExecution"></a>

------
#### [ CancelJobExecution command ]

Cancels a job execution on a device\.

------
#### [ HTTPS \(3\) ]

Request:

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

`thingName`  
The name of the thing whose job execution will be canceled\.

`jobId`  
The unique identifier you assigned to the job when it was created\.

`force`  
Optional\. If `true`, a job execution with a status of `IN_PROGRESS` or `QUEUED` can be canceled\. Otherwise, only a job execution with status of `QUEUED` can be canceled\. If you attempt to cancel a job execution with a status of `IN_PROGRESS` and you do not set `force` to `true`, an `InvalidStateTransitionException` is thrown\. The default is `false`\.  
Canceling a job with a status of `IN_PROGRESS` causes a device that is executing the job to be unable to update the job execution status\. Use caution and make sure that each device executing a job that is canceled is able to recover to a valid state\.

`expectedVersion`  
Optional\. The expected current version of the job execution\. Each time you update the job execution, its version is incremented\. If the version of the job execution stored in the AWS IoT Jobs service does not match, the update is rejected with a `VersionConflictException` error, and an `ErrorResponse` that contains the current job execution status data is returned\. \(This makes it unnecessary to perform a separate `DescribeJobExecution` request to obtain the job execution status data\.\)

`statusDetails`  
Optional\. A collection of name\-value pairs that describe the status of the job execution\. 

Response:

```
{
}
```

------
#### [ CLI \(3\) ]

**Synopsis:**

```
aws iot  cancel-job-execution \
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


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The job to be canceled\.  | 
|  thingName  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the thing whose execution of the job will be canceled\.  | 
|  force  |  boolean  |  Optional\. If `true`, the job execution is canceled if it has status of IN\_PROGRESS or QUEUED\. Otherwise, the job execution is canceled only if it has status of QUEUED\. However, if you attempt to cancel a job execution that has a status of IN\_PROGRESS, and you do not set `--force` to `true`, an `InvalidStateTransitionException` is thrown\. The default is `false`\.   Canceling a job that has a status of IN\_PROGRESS, causes a device that is executing the job to be unable to update the job execution status\. Use caution and make sure that each device executing a job that is canceled is able to recover to a valid state\.   | 
|  expectedVersion  |  long  java class: java\.lang\.Long  |  Optional\. The expected current version of the job execution\. Each time you update the job execution, its version is incremented\. If the version of the job execution stored in the AWS IoT Jobs service does not match, the update is rejected with a `VersionMismatch` error, and an `ErrorResponse` that contains the current job execution status data is returned\. \(This makes it unnecessary to perform a separate `DescribeJobExecution` request to obtain the job execution status data\.\)  | 
|  statusDetails  |  map  key: DetailsKey  value: DetailsValue  |  A collection of name\-value pairs that describe the status of the job execution\. If not specified, the `statusDetails` are unchanged\.  | 
|  DetailsKey  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |   | 
|  DetailsValue  |  string  length max:1024 min:1  pattern: \[^\\\\p\{C\}\]\*\+  |   | 

Output:

None

------

#### CreateJob<a name="jobs-CreateJob"></a>

------
#### [ CreateJob command ]

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
#### [ HTTPS \(4\) ]

Request:

```
PUT /jobs/jobId

{
    "targets": [ "string" ],
    "document": "string",
    "documentSource": "string",
    "description": "string",
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

`jobId`  
A job identifier, which must be unique for your AWS account\. We recommend using a UUID\. Alphanumeric characters, "\-", and "\_" can be used here\.

`targets`  
A list of thing or thing group ARNs that defines the targets of the job\.

`document`  
Optional\. The job document\.

`documentSource`  
Optional\. An Amazon S3 link to the job document\.

`description`  
Optional\. A short text description of the job\.

`presignedUrlConfigData`  
Optional\. Configuration information for presigned Amazon S3 URLs\.    
`roleArn`  
The ARN of the IAM role that contains permissions to access the Amazon S3 bucket\. This is the bucket that contains the data that devices download with the presigned Amazon S3 URLs\. This role must also grant AWS IoT permission to assume the role\. For more information, see [Create jobs](manage-job-cli.md#create-job)\.  
`expiresInSec`  
How long \(in seconds\) presigned URLs are valid\. Valid values are 60 \- 3600\. The default value is 3600 seconds\. Presigned URLs are generated when the AWS IoT Jobs service receives an MQTT request for the job document\. 

`targetSelection`  
Optional\. Specifies whether the job continues to run \(CONTINUOUS\) or is complete after all those things specified as targets have completed the job \(SNAPSHOT\)\. If CONTINUOUS, the job might also be scheduled to run on a thing when a change is detected in a target\. For example, a job is scheduled to run on a thing when the thing is added to a target group, even after the job was completed by all things originally in the group\.

`jobExecutionRolloutConfig`  
Optional\. Allows you to create a staged rollout of a job\.    
`maximumPerMinute`  
The maximum number of things on which the job is sent for execution, per minute\. Valid values are 1 to 1000\. If not specified, the default is 1000\. The actual number of things that receive the job might be less during any particular minute interval \(due to system latency\), but is not more than the specified value\.  
`exponentialRate`  
Allows you to create an exponential rate of rollout for a job\.    
`baseRatePerMinute`  
The minimum number of things that are notified of a pending job, per minute, at the start of job rollout\. This parameter allows you to define the initial rate of rollout\.   
`incrementFactor`  
The exponential factor to increase the rate of rollout for a job\.  
`rateIncreaseCriteria`  
The criteria to initiate the increase in rate of rollout for a job\. Set values for either the `numberOfNotifiedThings` or `numberOfSucceededThings`, but not both\.    
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

Response:

```
{
    "jobArn": "string",
    "jobId": "string",
    "description": "string"
}
```

`jobArn`  
The ARN of the job\.

`jobId`  
The unique identifier you assigned to this job\.

`description`  
An optional short text description of the job\.

------
#### [ CLI \(4\) ]

**Synopsis:**

```
aws iot  create-job \
    --job-id <value> \
    --targets <value> \
    [--document-source <value>] \
    [--document <value>] \
    [--description <value>] \
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
  "targets": [
    "string"
  ],
  "documentSource": "string",
  "document": "string",
  "description": "string",
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


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  A job identifier which must be unique for your AWS account\. We recommend using a UUID\. Alphanumeric characters, "\-" and "\_" are valid for use here\.  | 
|  targets  |  list  member: TargetArn  |  A list of things and thing groups to which the job should be sent\.  | 
|  TargetArn  |  string  |   | 
|  documentSource  |  string  length max:1350 min:1  |  An S3 link to the job document\.  | 
|  document  |  string  length max:32768  |  The job document\.  | 
|  description  |  string  length max:2028  pattern: \[^\\\\p\{C\}\]\+  |  A short text description of the job\.  | 
|  presignedUrlConfig  |  PresignedUrlConfig  |  Configuration information for presigned S3 URLs\.  | 
|  roleArn  |  string  length max:2048 min:20  |  The ARN of an IAM role that grants permission to download files from the Amazon S3 bucket where the job data or updates are stored\. The role must also grant permission for AWS IoT to download the files\.  | 
|  expiresInSec  |  long  java class: java\.lang\.Long  range\- max:3600 min:60  |  How long \(in seconds\) presigned URLs are valid\. Valid values are 60 \- 3600\. The default value is 3600 seconds\. Presigned URLs are generated when the AWS IoT Jobs service receives an MQTT request for the job document\.  | 
|  targetSelection  |  string  enum: CONTINUOUS \| SNAPSHOT  |  Specifies whether the job continues to run \(CONTINUOUS\), or is complete after all those things specified as targets have completed the job \(SNAPSHOT\)\. If continuous, the job can also be run on a thing when a change is detected in a target\. For example, a job runs on a thing when the thing is added to a target group, even after the job was completed by all things originally in the group\.  | 
|  jobExecutionsRolloutConfig  |  JobExecutionsRolloutConfig  |  Allows you to create a staged rollout of the job\.  | 
|  maximumPerMinute  |  integer  java class: java\.lang\.Integer  range\- max:1000 min:1  |  The maximum number of things that are notified of a pending job, per minute\. This parameter allows you to create a staged rollout\.  | 
|  exponentialRate  |  ExponentialRolloutRate  |  The rate of increase for a job rollout\. This parameter allows you to define an exponential rate for a job rollout\.   | 
|  baseRatePerMinute  |  java class: java\.lang\.Integer  |  The minimum number of things that will be notified of a pending job, per minute at the start of job rollout\. This parameter allows you to define the initial rate of rollout\.   | 
|  incrementFactor  |  java class: java\.lang\.Double  |  The exponential factor to increase the rate of rollout for a job\.  | 
|  rateIncreaseCriteria  |  RateIncreaseCriteria  |  Allows you to define a criteria to initiate the increase in rate of rollout for a job\. Set a value for either `numberOfNotifiedThings` or `numberOfSucceededThings`, but not both\.  | 
|  numberOfNotifiedThings  |  java class: java\.lang\.Double  |  The threshold for number of notified things that will initiate the increase in rate of rollout\.   | 
|  numberOfSucceededThings  |  java class: java\.lang\.Double  |  The threshold for number of succeeded things that will initiate the increase in rate of rollout\.   | 
|  abortConfig  |  AbortConfig  |  Allows you to create criteria to abort a job\.  | 
|  criteriaList  |  AbortCriteria  |  The list of abort criteria to define rules to abort the job\.  | 
|  action  |  java class: java\.lang\.String \(CANCEL\)  |  The type of abort action to initiate a job abort\.  | 
|  failureType  |  java class: java\.lang\.String \(FAILED \| REJECTED \| TIMED\_OUT \| ALL\)  |  The type of job execution failure to define a rule to initiate a job abort\.  | 
|  minNumberOfExecutedThings  |  java class: java\.lang\.Integer\)  |  Minimum number of executed things before evaluating an abort rule\.  | 
|  thresholdPercentage  |  java class: java\.lang\.Double\)  |  The threshold as a percentage of the total number of executed things that will initiate a job abort\. AWS IoT supports up to two digits after the decimal \(for example, 10\.9 and 10\.99, but not 10\.999\)\.   | 
|  timeoutConfig  |  TimeoutConfig  |  Specifies the amount of time each device has to finish its execution of the job\. The timer is started when the job execution status is set to `IN_PROGRESS`\. If the job execution status is not set to another terminal state before the time expires, it is set to `TIMED_OUT`\.  | 
|  inProgressTimeoutInMinutes  |  long  |  Specifies the amount of time, in minutes, this device has to finish execution of this job\. A timer is started, or restarted, whenever this job's execution status is specified as `IN_PROGRESS` with this field populated\. If the job execution status is not set to a terminal state before the timer expires, or before another job execution status update is sent with this field populated, the status is set to `TIMED_OUT`\.  | 
|  documentParameters  |  map  key: ParameterKey  value: ParameterValue  |  Parameters for the job document\.  | 
|  ParameterKey  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |   | 
|  ParameterValue  |  string  length max:1024 min:1  pattern: \[^\\\\p\{C\}\]\+  |   | 

Output:

```
{
  "jobArn": "string",
  "jobId": "string",
  "description": "string"
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  jobArn  |  string  |  The job ARN\.  | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to this job\.  | 
|  description  |  string  length max:2028  pattern: \[^\\\\p\{C\}\]\+  |  The job description\.  | 

------

#### DeleteJob<a name="jobs-DeleteJob"></a>

------
#### [ DeleteJob command ]

Deletes a job and its related job executions\.

Deleting a job can take time, depending on the number of job executions created for the job and various other factors\. While the job is being deleted, the status of the job is shown as "DELETION\_IN\_PROGRESS"\. Attempting to delete or cancel a job whose status is already "DELETION\_IN\_PROGRESS" results in an error\.

------
#### [ HTTPS \(5\) ]

 **Request syntax:**

```
DELETE /jobs/jobId?force=force 
```


**URI request parameters:**  

|  Name  |  Type  |  Req?  |  Description  | 
| --- | --- | --- | --- | 
|  jobId  |  JobId  |  yes  |  The ID of the job to be deleted\.  | 
|  force  |  ForceFlag  |  no  |  \(Optional\) When true, you can delete a job with a status of "IN\_PROGRESS"\. Otherwise, you can only delete a job that is in a terminal state \("SUCCEEDED" or "CANCELED"\) or an exception occurs\. The default is false\.  Deleting a job with a status of "IN\_PROGRESS", causes a device that is executing the job to be unable to access job information or update the job execution status\. Use caution and make sure that each device executing a job that is deleted is able to recover to a valid state\.   | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.  
HTTP response code: 400

`InvalidStateTransitionException`  
An update attempted to change the job or job execution to a state that is invalid because of its current state \(for example, an attempt to change a request in state SUCCEEDED to state IN\_PROGRESS\)\. In this case, the body of the error message also contains the executionState field\.  
HTTP response code: 409

`ResourceNotFoundException`  
The specified resource does not exist\.  
HTTP response code: 404

`ThrottlingException`  
The rate exceeds the limit\.  
HTTP response code: 429

`ServiceUnavailableException`  
The service is temporarily unavailable\.  
HTTP response code: 503

------
#### [ CLI \(5\) ]

 **Synopsis:**

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


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The ID of the job to be deleted\.  | 
|  force  |  boolean  |  \(Optional\) When true, you can delete a job with a status of IN\_PROGRESS\. Otherwise, you can only delete a job that is in a terminal state \(SUCCEEDED or CANCELED\) or an exception occurs\. The default is false\.  Deleting a job with a status of IN\_PROGRESS, causes a device that is executing the job to be unable to access job information or update the job execution status\. Use caution and make sure that each device executing a job that is deleted is able to recover to a valid state\.   | 

Output:

None

------

#### DeleteJobExecution<a name="jobs-DeleteJobExecution"></a>

------
#### [ DeleteJobExecution command ]

Deletes a job execution\.

------
#### [ HTTPS \(6\) ]

 **Request syntax:**

```
DELETE /things/thingName/jobs/jobId/executionNumber/executionNumber?force=force 
```


**URI request parameters:**  

|  Name  |  Type  |  Req?  |  Description  | 
| --- | --- | --- | --- | 
|  jobId  |  JobId  |  yes  |  The ID of the job whose execution will be deleted\.  | 
|  thingName  |  ThingName  |  yes  |  The name of the thing whose execution of the job will be deleted\.  | 
|  executionNumber  |  ExecutionNumber  |  yes  |  The ID of the job execution to be deleted\.  | 
|  force  |  ForceFlag  |  no  |  When true, you can delete a job execution with a status of IN\_PROGRESS\. Otherwise, you can only delete a job execution that is in a terminal state \(SUCCEEDED, FAILED, TIMED\_OUT, REJECTED, REMOVED, or CANCELED\) or an exception occurs\. The default is false\.  Deleting a job execution with a status of IN\_PROGRESS causes the device to be unable to access job information or update the job execution status\. Use caution and make sure that the device is able to recover to a valid state\.   | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.  
HTTP response code: 400

`InvalidStateTransitionException`  
An update attempted to change the job execution to a state that is invalid because of the job execution's current state \(for example, an attempt to change a request in state SUCCEEDED to state IN\_PROGRESS\)\. In this case, the body of the error message also contains the `executionState` field\.  
HTTP response code: 409

`ResourceNotFoundException`  
The specified resource does not exist\.  
HTTP response code: 404

`ThrottlingException`  
The rate exceeds the limit\.  
HTTP response code: 429

`ServiceUnavailableException`  
The service is temporarily unavailable\.  
HTTP response code: 503

------
#### [ CLI \(6\) ]

 **Synopsis:**

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


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The ID of the job whose execution will be deleted\.  | 
|  thingName  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the thing whose execution of the job will be deleted\.  | 
|  executionNumber  |  long  java class: java\.lang\.Long  |  The ID of the job execution to be deleted\.  | 
|  force  |  boolean  |  When true, you can delete a job execution with a status of IN\_PROGRESS\. Otherwise, you can only delete a job execution that is in a terminal state \(SUCCEEDED, FAILED, TIMED\_OUT, REJECTED, REMOVED, or CANCELED\) or an exception occurs\. The default is false\.  Deleting a job execution with a status of IN\_PROGRESS causes the device to be unable to access job information or update the job execution status\. Use caution and make sure that the device is able to recover to a valid state\.   | 

Output:

None

------

#### DescribeJob<a name="jobs-DescribeJob"></a>

------
#### [ DescribeJob command ]

Gets the details of the specified job\.

------
#### [ HTTPS \(7\) ]

Request:

```
GET /jobs/jobId
```

`jobId`  
The unique identifier you assigned to this job when it was created\.

Response:

```
{
    "documentSource": "string",
    "job": Job 
}
```

`documentSource`  
An Amazon S3 link to the job document\.

`job`  
A [Job](#jobs-job) object\.

------
#### [ CLI \(7\) ]

**Synopsis:**

```
aws iot  describe-job \
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


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to this job when it was created\.  | 

Output:

```
{
  "documentSource": "string",
  "job": {
    "jobArn": "string",
    "jobId": "string",
    "targetSelection": "string",
    "status": "string",
    "forceCanceled": boolean,
    "comment": "string",
    "targets": [
      "string"
    ],
    "description": "string",
    "presignedUrlConfig": {
      "roleArn": "string",
      "expiresInSec": long
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
    "createdAt": "timestamp",
    "lastUpdatedAt": "timestamp",
    "completedAt": "timestamp",
    "jobProcessDetails": {
      "processingTargets": [
        "string"
      ],
      "numberOfCanceledThings": "integer",
      "numberOfSucceededThings": "integer",
      "numberOfFailedThings": "integer",
      "numberOfRejectedThings": "integer",
      "numberOfQueuedThings": "integer",
      "numberOfInProgressThings": "integer",
      "numberOfRemovedThings": "integer",
      "numberOfTimedOutThings": "integer"
    },
    "documentParameters": {
      "string": "string"
    },
    "timeoutConfig": { 
       "inProgressTimeoutInMinutes": number
    }
  }
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  documentSource  |  string  length max:1350 min:1  |  An Amazon S3 link to the job document\.  | 
|  job  |  Job  |  Information about the job\.  | 
|  jobArn  |  string  |  An ARN that identifies the job with format "arn:aws:iot:region:account:job/jobId"\.  | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to this job when it was created\.  | 
|  targetSelection  |  string  enum: CONTINUOUS \| SNAPSHOT  |  Specifies whether the job continues to run \(CONTINUOUS\), or is complete after all those things specified as targets have completed the job \(SNAPSHOT\)\. If continuous, the job can also be run on a thing when a change is detected in a target\. For example, a job runs on a device when the thing representing the device is added to a target group, even after the job was completed by all things originally in the group\.   | 
|  status  |  string  enum: IN\_PROGRESS \| CANCELED \| SUCCEEDED  |  The status of the job, one of `IN_PROGRESS`, `CANCELED`, or `SUCCEEDED`\.   | 
|  forceCanceled  |  boolean  java class: java\.lang\.Boolean  |  Is `true` if the job was canceled with the optional `force` parameter set to `true`\.  | 
|  comment  |  string  length max:2028  pattern: \[^\\\\p\{C\}\]\+  |  If the job was updated, describes the reason for the update\.  | 
|  targets  |  list  member: TargetArn  |  A list of AWS IoT things and thing groups to which the job should be sent\.  | 
|  TargetArn  |  string  |   | 
|  description  |  string  length max:2028  pattern: \[^\\\\p\{C\}\]\+  |  A short text description of the job\.  | 
|  presignedUrlConfig  |  PresignedUrlConfig  |  Configuration for presigned Amazon S3 URLs\.  | 
|  roleArn  |  string  length max:2048 min:20  |  The ARN of an IAM role that grants permission to download files from the Amazon S3 bucket where the job data or updates are stored\. The role must also grant permission for tAWS IoT Jobs service to download the files\.  | 
|  expiresInSec  |  long  java class: java\.lang\.Long  range\- max:3600 min:60  |  How long \(in seconds\) presigned URLs are valid\. Valid values are 60 \- 3600\. The default value is 3600 seconds\. Presigned URLs are generated when the AWS IoT Jobs service receives an MQTT request for the job document\.  | 
|  jobExecutionsRolloutConfig  |  JobExecutionsRolloutConfig  |  Allows you to create a staged rollout of the job\.  | 
|  maximumPerMinute  |  integer  java class: java\.lang\.Integer  range\- max:1000 min:1  |  The maximum number of things that are notified of a pending job, per minute\. This parameter allows you to create a staged rollout\.  | 
|  exponentialRate  |  ExponentialRolloutRate  |  The rate of increase for a job rollout\. This parameter allows you to define an exponential rate for a job rollout\.   | 
|  baseRatePerMinute  |  java class: java\.lang\.Integer  |  The minimum number of things that are notified of a pending job, per minute at the start of job rollout\. This parameter allows you to define the initial rate of rollout\.   | 
|  incrementFactor  |  java class: java\.lang\.Double  |  The exponential factor to increase the rate of rollout for a job\.  | 
|  rateIncreaseCriteria  |  RateIncreaseCriteria  |  Allows you to define a criteria to initiate the increase in rate of rollout for a job\. Set a value for either `numberOfNotifiedThings` or `numberOfSucceededThings`, but not both\.  | 
|  numberOfNotifiedThings  |  java class: java\.lang\.Double  |  The threshold for number of notified things that initiate the increase in rate of rollout\.   | 
|  numberOfSucceededThings  |  java class: java\.lang\.Double  |  The threshold for number of succeeded things that initiate the increase in rate of rollout\.   | 
|  abortConfig  |  AbortConfig  |  Allows you to create criteria to abort a job\.  | 
|  criteriaList  |  AbortCriteria  |  The list of abort criteria to define rules to abort the job\.  | 
|  action  |  java class: java\.lang\.String \(CANCEL\)  |  The type of abort action to initiate a job abort\.  | 
|  failureType  |  java class: java\.lang\.String \(FAILED \| REJECTED \| TIMED\_OUT \| ALL\)  |  The type of job execution failure to define a rule to initiate a job abort\.  | 
|  minNumberOfExecutedThings  |  java class: java\.lang\.Integer\)  |  Minimum number of executed things before evaluating an abort rule\.  | 
|  thresholdPercentage  |  java class: java\.lang\.Double\)  |  The threshold as a percentage of the total number of executed things that initiate a job abort\. AWS IoT supports up to two digits after the decimal \(for example, 10\.9 and 10\.99, but not 10\.999\)\.   | 
|  createdAt  |  timestamp  |  The time, in seconds since the epoch, when the job was created\.  | 
|  lastUpdatedAt  |  timestamp  |  The time, in seconds since the epoch, when the job was last updated\.  | 
|  completedAt  |  timestamp  |  The time, in seconds since the epoch, when the job was completed\.  | 
|  jobProcessDetails  |  JobProcessDetails  |  Details about the job process\.  | 
|  processingTargets  |  list  member: ProcessingTargetName  java class: java\.util\.List  |  The devices on which the job is executing\.  | 
|  ProcessingTargetName  |  string  |   | 
|  numberOfCanceledThings  |  integer  java class: java\.lang\.Integer  |  The number of things that canceled the job\.  | 
|  numberOfSucceededThings  |  integer  java class: java\.lang\.Integer  |  The number of things that successfully completed the job\.  | 
|  numberOfFailedThings  |  integer  java class: java\.lang\.Integer  |  The number of things that failed executing the job\.  | 
|  numberOfRejectedThings  |  integer  java class: java\.lang\.Integer  |  The number of things that rejected the job\.  | 
|  numberOfQueuedThings  |  integer  java class: java\.lang\.Integer  |  The number of things that are awaiting execution of the job\.  | 
|  numberOfInProgressThings  |  integer  java class: java\.lang\.Integer  |  The number of things currently executing the job\.  | 
|  numberOfRemovedThings  |  integer  java class: java\.lang\.Integer  |  The number of things that are no longer scheduled to execute the job because they have been deleted or have been removed from the group that was a target of the job\.  | 
|  numberOfTimedOutThings  |  integer  java class: java\.lang\.Integer  |  The number of things whose job execution status is `TIMED_OUT`\.  | 
|  documentParameters  |  map  key: ParameterKey  value: ParameterValue  |  The parameters specified for the job document\.  | 
|  ParameterKey  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |   | 
|  ParameterValue  |  string  length max:1024 min:1  pattern: \[^\\\\p\{C\}\]\+  |   | 
|  timeoutConfig  |  TimeoutConfig  |  Specifies the amount of time each device has to finish its execution of the job\. A timer is started when the job execution status is set to `IN_PROGRESS`\. If the job execution status is not set to another terminal state before the timer expires, it is set to `TIMED_OUT`\.   | 
|  inProgressTimeoutInMinutes  |  long  |  Specifies the amount of time, in minutes, this device has to finish execution of this job\. The timeout interval can be anywhere between 1 minute and 7 days \(1 to 10080 minutes\)\. The in\-progress timer can't be updated and applies to all job executions for the job\. Whenever a job execution remains in the `IN_PROGRESS` status for longer than this interval, the job execution fails and switches to the terminal `TIMED_OUT` status\.   | 

------

#### DescribeJobExecution<a name="jobs-DescribeJobExecution"></a>

------
#### [ DescribeJobExecution command ]

Gets details of a job execution\. The job's execution status must be `SUCCEEDED` or `FAILED`\.

------
#### [ HTTPS \(8\) ]

Request:

```
GET /things/thingName/jobs/jobId?executionNumber=executionNumber
```

`jobId`  
The unique identifier you assigned to this job when it was created\.

`thingName`  
The thing name associated with the device the job execution is running on\.

`executionNumber`  
Optional\. A number that is used to specify a job execution on a device\. \(See [JobExecution](#jobs-job-execution)\.\) If not specified, the latest job execution is returned\.

Response:

```
{
    "execution": { JobExecution }
}
```

`execution`  
A [JobExecution](#jobs-job-execution) object\.

------
#### [ CLI \(8\) ]

**Synopsis:**

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


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to this job when it was created\.  | 
|  thingName  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the thing on which the job execution is running\.  | 
|  executionNumber  |  long  java class: java\.lang\.Long  |  A string \(consisting of the digits "0" through "9"\) that is used to specify a particular job execution on a particular device\.  | 

Output:

```
{
  "execution": {
    "approximateSecondsBeforeTimedOut": "number"
    "jobId": "string",
    "status": "string",
    "forceCanceled": boolean,
    "statusDetails": {
      "detailsMap": {
        "string": "string"
      }
    },
    "thingArn": "string",
    "queuedAt": "timestamp",
    "startedAt": "timestamp",
    "lastUpdatedAt": "timestamp",
    "executionNumber": long,
    "versionNumber": long
  }
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  execution  |  JobExecution  |  Information about the job execution\.  | 
|  approximateSecondsBeforeTimedOut  |  long  |   The estimated number of seconds that remain before the job execution status is changed to `TIMED_OUT`\. The timeout interval can be anywhere between 1 minute and 7 days \(1 to 10080 minutes\)\. The actual job execution timeout can occur up to 60 seconds later than the estimated duration\. This value is not included if the job execution has reached a terminal status\.   | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to the job when it was created\.  | 
|  status  |  string  enum: QUEUED \| IN\_PROGRESS \| SUCCEEDED \| FAILED \| \| TIMED\_OUT \| REJECTED \| REMOVED \| CANCELED  |  The status of the job execution \(IN\_PROGRESS, QUEUED, FAILED, SUCCEEDED, TIMED\_OUT, CANCELED, or REJECTED\)\.  | 
|  forceCanceled  |  boolean  java class: java\.lang\.Boolean  |  Is `true` if the job execution was canceled with the optional `force` parameter set to `true`\.  | 
|  statusDetails  |  JobExecutionStatusDetails  |  A collection of name\-value pairs that describe the status of the job execution\.  | 
|  detailsMap  |  map  key: DetailsKey  value: DetailsValue  |  The job execution status\.  | 
|  DetailsKey  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |   | 
|  DetailsValue  |  string  length max:1024 min:1  pattern: \[^\\\\p\{C\}\]\*\+  |   | 
|  thingArn  |  string  |  The ARN of the thing on which the job execution is running\.  | 
|  queuedAt  |  timestamp  |  The time, in seconds since the epoch, when the job execution was queued\.  | 
|  startedAt  |  timestamp  |  The time, in seconds since the epoch, when the job execution started\.  | 
|  lastUpdatedAt  |  timestamp  |  The time, in seconds since the epoch, when the job execution was last updated\.  | 
|  executionNumber  |  long  java class: java\.lang\.Long  |  A string \(consisting of the digits "0" through "9"\) that identifies this job execution on this device\. It can be used in commands that return or update job execution information\.   | 
|  versionNumber  |  long  |  The version of the job execution\. Job execution versions are incremented each time they are updated by a device\.  | 

------

#### GetJobDocument<a name="jobs-GetJobDocument"></a>

------
#### [ GetJobDocument command ]

Gets the job document for a job\.

**Note**  
Placeholder URLs are not replaced with presigned Amazon S3 URLs in the document returned\. Presigned URLs are generated only when the AWS IoT Jobs service receives a request over MQTT\.

------
#### [ HTTPS \(9\) ]

Request:

```
GET /jobs/jobId/job-document
```

`jobId`  
The unique identifier you assigned to this job when it was created\.

Response:

```
{
    "document": "string"
}
```

`document`  
The job document content\.

------
#### [ CLI \(9\) ]

**Synopsis:**

```
aws iot  get-job-document \
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


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to this job when it was created\.  | 

Output:

```
{
  "document": "string"
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  document  |  string  length max:32768  |  The job document content\.  | 

------

#### ListJobExecutionsForJob<a name="jobs-listJobExecutionsForJob"></a>

------
#### [ ListExecutionsForJob command ]

Gets a list of job executions for a job\.

------
#### [ HTTPS \(10\) ]

Request:

```
GET /jobs/jobId/things?status=status&maxResults=maxResults&nextToken=nextToken
```

`jobId`  
The unique identifier you assigned to this job when it was created\.

`status`  
Optional\. A filter that lets you search for jobs that have the specified status: QUEUED, IN\_PROGRESS, SUCCEEDED, FAILED, TIMED\_OUT, REJECTED, REMOVED, or CANCELED\.

`maxResults`  
Optional\. The maximum number of results to be returned per request\.

`nextToken`  
Optional\. The token to retrieve the next set of results\.

Response:

```
{
    "executionSummaries": [ JobExecutionSummary ... ]
}
```

`executionSummaries`  
A list of [JobExecutionSummary](#jobs-job-execution-summary) objects associated with the specified job ID\.

------
#### [ CLI \(10\) ]

**Synopsis:**

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


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to this job when it was created\.  | 
|  status  |  string  enum: QUEUED \| IN\_PROGRESS \| SUCCEEDED \| FAILED \| TIMED\_OUT \| REJECTED \| REMOVED \| CANCELED  |  The status of the job\.  | 
|  maxResults  |  integer  java class: java\.lang\.Integer  range\- max:250 min:1  |  The maximum number of results to be returned per request\.  | 
|  nextToken  |  string  |  The token to retrieve the next set of results\.  | 

Output:

```
{
  "executionSummaries": [
    {
      "thingArn": "string",
      "jobExecutionSummary": {
        "status": "string",
        "queuedAt": "timestamp",
        "startedAt": "timestamp",
        "lastUpdatedAt": "timestamp",
        "executionNumber": long
      }
    }
  ],
  "nextToken": "string"
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  executionSummaries  |  list  member: JobExecutionSummaryForJob  java class: java\.util\.List  |  A list of job execution summaries\.  | 
|  JobExecutionSummaryForJob  |  JobExecutionSummaryForJob  |   | 
|  thingArn  |  string  |  The ARN of the thing on which the job execution is running\.  | 
|  jobExecutionSummary  |  JobExecutionSummary  |  Contains a subset of information about a job execution\.  | 
|  status  |  string  enum: QUEUED \| IN\_PROGRESS \| SUCCEEDED \| FAILED \| TIMED\_OUT \| REJECTED \| REMOVED \| CANCELED  |  The status of the job execution\.  | 
|  queuedAt  |  timestamp  |  The time, in seconds since the epoch, when the job execution was queued\.  | 
|  startedAt  |  timestamp  |  The time, in seconds since the epoch, when the job execution started\.  | 
|  lastUpdatedAt  |  timestamp  |  The time, in seconds since the epoch, when the job execution was last updated\.  | 
|  executionNumber  |  long  java class: java\.lang\.Long  |  A string \(consisting of the digits "0" through "9"\) that identifies this job execution on this device\. It can be used later in commands that return or update job execution information\.  | 
|  nextToken  |  string  |  The token for the next set of results, or **null** if there are no additional results\.  | 

------

#### ListJobExecutionsForThing<a name="jobs-ListJobExecutionsForThing"></a>

------
#### [ ListJobExecutionsForThing command ]

Gets a list of job executions for a thing\.

------
#### [ HTTPS \(11\) ]

Request:

```
GET /things/thingName/jobs?status=status&maxResults=maxResults&nextToken=nextToken
```

`thingName`  
The name of the thing for which `JobExecutions` will be listed\.

`status`  
An optional filter that lets you search for jobs that have the specified status: QUEUED, IN\_PROGRESS, SUCCEEDED, FAILED, TIMED\_OUT, REJECTED, REMOVED, or CANCELED\.

`maxResults`  
The maximum number of results to be returned per request\.

`nextToken`  
The token for the next set of results, or `null` if there are no additional results\.

Response:

```
{
    "executionSummaries": [ JobExecutionSummary ... ]
}
```

`executionSummaries`  
A list of the [JobExecutionSummary](#jobs-job-execution-summary) objects for the job executions associated with the specified thing\.

------
#### [ CLI \(11\) ]

**Synopsis:**

```
aws iot  list-job-executions-for-thing \
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


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  thingName  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The thing name\.  | 
|  status  |  string  enum: QUEUED \| IN\_PROGRESS \| SUCCEEDED \| FAILED \| TIMED\_OUT \| REJECTED \| REMOVED \| CANCELED  |  An optional filter that lets you search for jobs that have the specified status\.  | 
|  maxResults  |  integer  java class: java\.lang\.Integer  range\- max:250 min:1  |  The maximum number of results to be returned per request\.  | 
|  nextToken  |  string  |  The token to retrieve the next set of results\.  | 

Output:

```
{
  "executionSummaries": [
    {
      "jobId": "string",
      "jobExecutionSummary": {
        "status": "string",
        "queuedAt": "timestamp",
        "startedAt": "timestamp",
        "lastUpdatedAt": "timestamp",
        "executionNumber": long
      }
    }
  ],
  "nextToken": "string"
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  executionSummaries  |  list  member: JobExecutionSummaryForThing  java class: java\.util\.List  |  A list of job execution summaries\.  | 
|  JobExecutionSummaryForThing  |  JobExecutionSummaryForThing  |   | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to this job when it was created\.  | 
|  jobExecutionSummary  |  JobExecutionSummary  |  Contains a subset of information about a job execution\.  | 
|  status  |  string  enum: QUEUED \| IN\_PROGRESS \| SUCCEEDED \| FAILED \| TIMED\_OUT\| REJECTED \| REMOVED \| CANCELED  |  The status of the job execution\.  | 
|  queuedAt  |  timestamp  |  The time, in seconds since the epoch, when the job execution was queued\.  | 
|  startedAt  |  timestamp  |  The time, in seconds since the epoch, when the job execution started\.  | 
|  lastUpdatedAt  |  timestamp  |  The time, in seconds since the epoch, when the job execution was last updated\.  | 
|  executionNumber  |  long  java class: java\.lang\.Long  |  A string \(consisting of the digits "0" through "9"\) that identifies this job execution on this device\. It can be used later in commands that return or update job execution information\.  | 
|  nextToken  |  string  |  The token for the next set of results, or **null** if there are no additional results\.  | 

------

#### ListJobs<a name="jobs-listJobs"></a>

------
#### [ ListJobs command ]

Gets a list of the jobs in your AWS account\.

------
#### [ HTTPS \(12\) ]

Request:

```
GET /jobs?status=status&targetSelection=targetSelection&thingGroupName=thingGroupName&thingGroupId=thingGroupId&maxResults=maxResults&nextToken=nextToken
```

`status`  
Optional\. A filter that lets you search for jobs that have the specified status: IN\_PROGRESS, CANCELED, or SUCCEEDED\. 

`targetSelection`  
Optional\. A filter that lets you search for jobs that have the specified `targetSelection` value: CONTINUOUS or SNAPSHOT\.

`thingGroupName`  
Optional\. A filter that lets you search for jobs that have the specified thing group name as a target\. 

`thingGroupId`  
Optional\. A filter that lets you search for jobs that have the specified thing group ID as a target\. 

`maxResults`  
Optional\. The maximum number of results to be returned per request\.

`nextToken`  
Optional\. The token to retrieve the next set of results\.

Response:

```
{
    "jobs": [ JobSummary ... ],
}
```

`jobs`  
A list of [JobSummary](#jobs-job-summary) objects, one for each job in your AWS account that matches the specified filtering criteria\.

------
#### [ CLI \(12\) ]

**Synopsis:**

```
aws iot  list-jobs \
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


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  status  |  string  enum: IN\_PROGRESS \| CANCELED \| SUCCEEDED  |  An optional filter that lets you search for jobs that have the specified status\.  | 
|  targetSelection  |  string  enum: CONTINUOUS \| SNAPSHOT  |  Specifies whether the job continues to run \(CONTINUOUS\), or is complete after all those things specified as targets have completed the job \(SNAPSHOT\)\. If continuous, the job can also be run on a thing when a change is detected in a target\. For example, a job runs on a thing when the thing is added to a target group, even after the job was completed by all things originally in the group\.   | 
|  maxResults  |  integer  java class: java\.lang\.Integer  range\- max:250 min:1  |  The maximum number of results to return per request\.  | 
|  nextToken  |  string  |  The token to retrieve the next set of results\.  | 
|  thingGroupName  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  A filter that limits the returned jobs to those for the specified group\.  | 
|  thingGroupId  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9\-\]\+  |  A filter that limits the returned jobs to those for the specified group\.  | 

Output:

```
{
  "jobs": [
    {
      "jobArn": "string",
      "jobId": "string",
      "thingGroupId": "string",
      "targetSelection": "string",
      "status": "string",
      "createdAt": "timestamp",
      "lastUpdatedAt": "timestamp",
      "completedAt": "timestamp"
    }
  ],
  "nextToken": "string"
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  jobs  |  list  member: JobSummary  java class: java\.util\.List  |  A list of jobs\.  | 
|  JobSummary  |  JobSummary  |   | 
|  jobArn  |  string  |  The job ARN\.  | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to this job when it was created\.  | 
|  thingGroupId  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9\-\]\+  |  The ID of the thing group\.  | 
|  targetSelection  |  string  enum: CONTINUOUS \| SNAPSHOT  |  Specifies whether the job continues to run \(CONTINUOUS\), or is complete after all those things specified as targets have completed the job \(SNAPSHOT\)\. If continuous, the job can also be run on a thing when a change is detected in a target\. For example, a job runs on a thing when the thing is added to a target group, even after the job was completed by all things originally in the group\.  | 
|  status  |  string  enum: IN\_PROGRESS \| CANCELED \| SUCCEEDED  |  The job summary status\.  | 
|  createdAt  |  timestamp  |  The time, in seconds since the epoch, when the job was created\.  | 
|  lastUpdatedAt  |  timestamp  |  The time, in seconds since the epoch, when the job was last updated\.  | 
|  completedAt  |  timestamp  |  The time, in seconds since the epoch, when the job completed\.  | 
|  nextToken  |  string  |  The token for the next set of results, or **null** if there are no additional results\.  | 

------

#### UpdateJob<a name="jobs-UpdateJob"></a>

------
#### [ UpdateJob command ]

Updates supported fields of the specified job\. Updated values for `timeoutConfig` take effect for only newly in\-progress executions\. Currently in\-progress executions continue to execute with the old timeout configuration\.

------
#### [ HTTPS \(13\) ]

Request:

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

`jobId`  
A job identifier that must be unique for your AWS account\. We recommend using a UUID\. Alphanumeric characters, "\-", and "\_" can be used here\.

`description`  
Optional\. A short text description of the job\.

`presignedUrlConfigData`  
Optional\. Configuration information for presigned Amazon S3 URLs\.    
`roleArn`  
The ARN of the IAM role that contains permissions to access the Amazon S3 bucket\. This is the bucket that contains the data that devices download with the presigned Amazon S3 URLs\. This role must also grant AWS IoT permission to assume the role\. For more information, see [Create jobs](manage-job-cli.md#create-job)\.  
`expiresInSec`  
How long \(in seconds\) presigned URLs are valid\. Valid values are 60 \- 3600\. The default value is 3600 seconds\. Presigned URLs are generated when the AWS IoT Jobs service receives an MQTT request for the job document\. 

`jobExecutionRolloutConfig`  
Optional\. Allows you to create a staged rollout of a job\.    
`maximumPerMinute`  
The maximum number of things on which the job is sent for execution, per minute\. Valid values are 1 to 1000\. If not specified, the default is 1000\. The actual number of things that receive the job might be less during any particular minute interval \(due to system latency\), but are not more than the specified value\.  
`exponentialRate`  
Allows you to create an exponential rate of rollout for a job\.    
`baseRatePerMinute`  
The minimum number of things that are notified of a pending job, per minute at the start of job rollout\. This parameter allows you to define the initial rate of rollout\.   
`incrementFactor`  
The exponential factor to increase the rate of rollout for a job\.  
`rateIncreaseCriteria`  
The criteria to initiate the increase in rate of rollout for a job\. Set values for either the `numberOfNotifiedThings` or `numberOfSucceededThings`, but not both\.    
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

Response:

```
HTTP/1.1 200
```

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

------
#### [ CLI \(13\) ]

**Synopsis:**

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


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  A job identifier that must be unique for your AWS account\. We recommend using a UUID\. Alphanumeric characters, "\-" and "\_" are valid for use here\.  | 
|  description  |  string  length max:2028  pattern: \[^\\\\p\{C\}\]\+  |  A short text description of the job\.  | 
|  presignedUrlConfig  |  PresignedUrlConfig  |  Configuration information for presigned S3 URLs\.  | 
|  roleArn  |  string  length max:2048 min:20  |  The ARN of an IAM role that grants permission to download files from the S3 bucket where the job data or updates are stored\. The role must also grant permission for AWS IoT to download the files\.  | 
|  expiresInSec  |  long  java class: java\.lang\.Long  range\- max:3600 min:60  |  How long \(in seconds\) presigned URLs are valid\. Valid values are 60 \- 3600\. The default value is 3600 seconds\. Presigned URLs are generated when the AWS IoT Jobs service receives an MQTT request for the job document\.  | 
|  jobExecutionsRolloutConfig  |  JobExecutionsRolloutConfig  |  Allows you to create a staged rollout of the job\.  | 
|  maximumPerMinute  |  integer  java class: java\.lang\.Integer  range\- max:1000 min:1  |  The maximum number of things that are notified of a pending job, per minute\. This parameter allows you to create a staged rollout\.  | 
|  exponentialRate  |  ExponentialRolloutRate  |  The rate of increase for a job rollout\. This parameter allows you to define an exponential rate for a job rollout\.   | 
|  baseRatePerMinute  |  java class: java\.lang\.Integer  |  The minimum number of things that are notified of a pending job, per minute at the start of job rollout\. This parameter allows you to define the initial rate of rollout\.   | 
|  incrementFactor  |  java class: java\.lang\.Double  |  The exponential factor to increase the rate of rollout for a job\.  | 
|  rateIncreaseCriteria  |  RateIncreaseCriteria  |  Allows you to define a criteria to initiate the increase in rate of rollout for a job\. Set a value for either `numberOfNotifiedThings` or `numberOfSucceededThings`, but not both\.  | 
|  numberOfNotifiedThings  |  java class: java\.lang\.Double  |  The threshold for number of notified things that initiate the increase in rate of rollout\.   | 
|  numberOfSucceededThings  |  java class: java\.lang\.Double  |  The threshold for number of succeeded things that initiate the increase in rate of rollout\.   | 
|  abortConfig  |  AbortConfig  |  Allows you to create criteria to abort a job\.  | 
|  criteriaList  |  AbortCriteria  |  The list of abort criteria to define rules to abort the job\.  | 
|  action  |  java class: java\.lang\.String \(CANCEL\)  |  The type of abort action to initiate a job abort\.  | 
|  failureType  |  java class: java\.lang\.String \(FAILED \| REJECTED \| TIMED\_OUT \| ALL\)  |  The type of job execution failure to define a rule to initiate a job abort\.  | 
|  minNumberOfExecutedThings  |  java class: java\.lang\.Integer\)  |  Minimum number of executed things before evaluating an abort rule\.  | 
|  thresholdPercentage  |  java class: java\.lang\.Double\)  |  The threshold as a percentage of the total number of executed things that initiate a job abort\. AWS IoT supports up to two digits after the decimal \(for example, 10\.9 and 10\.99, but not 10\.999\)\.   | 
|  timeoutConfig  |  TimeoutConfig  |  Specifies the amount of time each device has to finish its execution of the job\. The timer is started when the job execution status is set to `IN_PROGRESS`\. If the job execution status is not set to another terminal state before the time expires, it is set to `TIMED_OUT`\.  | 
|  inProgressTimeoutInMinutes  |  long  |  Specifies the amount of time, in minutes, this device has to finish execution of this job\. A timer is started, or restarted, whenever this job's execution status is specified as `IN_PROGRESS` with this field populated\. If the job execution status is not set to a terminal state before the timer expires, or before another job execution status update is sent with this field populated, the status is set to `TIMED_OUT`\.  | 
|  documentParameters  |  map  key: ParameterKey  value: ParameterValue  |  Parameters for the job document\.  | 
|  ParameterKey  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |   | 
|  ParameterValue  |  string  length max:1024 min:1  pattern: \[^\\\\p\{C\}\]\+  |   | 

Output:

```
HTTP/1.1 200
```

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\. 

------

## Jobs device MQTT and HTTPS APIs<a name="jobs-mqtt-api"></a>

### Device MQTT and HTTPS data types<a name="jobs-data-plane-data-types"></a>

The following data types are used to communicate with the AWS IoT Jobs service over the MQTT and HTTPS protocols\.

#### JobExecution<a name="jobs-mqtt-job-execution-data"></a>

------
#### [ JobExecution data type ]

Contains data about a job execution\.

------
#### [ Syntax \(7\) ]

```
{
    "jobId" : "string",
    "thingName" : "string",
    "jobDocument" : "string",
    "status": "QUEUED|IN_PROGRESS|FAILED|SUCCEEDED|CANCELED|TIMED_OUT|REJECTED|REMOVED",
    "statusDetails": {
        "string": "string"
    },
    "queuedAt" : "timestamp",
    "startedAt" : "timestamp",
    "lastUpdatedAt" : "timestamp",
    "versionNumber" : "number",
    "executionNumber": long
}
```

------
#### [ Description \(7\) ]

`jobId`  
The unique identifier you assigned to this job when it was created\.

`thingName`  
The name of the thing that is executing the job\.

`jobDocument`  
The content of the job document\.

`status`  
The status of the job execution\. Can be one of: QUEUED, IN\_PROGRESS, FAILED, SUCCEEDED, CANCELED, TIMED\_OUT, REJECTED, or REMOVED\.

`statusDetails`  
A collection of name\-value pairs that describe the status of the job execution\. 

`queuedAt`  
The time, in seconds since the epoch, when the job execution was enqueued\. 

`startedAt`  
The time, in seconds since the epoch, when the job execution was started\. 

`lastUpdatedAt`  
The time, in seconds since the epoch, when the job execution was last updated\.

`versionNumber`  
The version of the job execution\. Job execution versions are incremented each time they are updated by a device\.

`executionNumber`  
A number that identifies a job execution on a device\. It can be used later in commands that return or update job execution information\.

------

#### JobExecutionState<a name="jobs-mqtt-job-execution-state"></a>

------
#### [ JobExecutionState data type ]

Contains data about the state of a job execution\.

------
#### [ Syntax \(8\) ]

```
{
    "status": "QUEUED|IN_PROGRESS|FAILED|SUCCEEDED|CANCELED|TIMED_OUT|REJECTED|REMOVED",
    "statusDetails": {
        "string": "string"
        ...
    }
    "versionNumber": "number"
}
```

------
#### [ Description \(8\) ]

`status`  
The status of the job execution\. Can be one of: QUEUED, IN\_PROGRESS, FAILED, SUCCEEDED, CANCELED, TIMED\_OUT, REJECTED, or REMOVED\.

`statusDetails`  
A collection of name\-value pairs that describe the status of the job execution\.

`versionNumber`  
The version of the job execution\. Job execution versions are incremented each time they are updated by a device\.

------

#### JobExecutionSummary<a name="jobs-mqtt-job-execution-summary"></a>

------
#### [ JobExecutionSummary data type ]

Contains a subset of information about a job execution\.

------
#### [ Syntax \(9\) ]

```
{
    "jobId": "string",
    "queuedAt": timestamp,
    "startedAt": timestamp,
    "lastUpdatedAt": timestamp,
    "versionNumber": "number",
    "executionNumber": long 
}
```

------
#### [ Description \(9\) ]

`jobId`  
The unique identifier you assigned to this job when it was created\.

`queuedAt`  
The time, in seconds since the epoch, when the job execution was enqueued\.

`startedAt`  
The time, in seconds since the epoch, when the job execution started\.

`lastUpdatedAt`  
The time, in seconds since the epoch, when the job execution was last updated\.

`versionNumber`  
The version of the job execution\. Job execution versions are incremented each time the AWS IoT Jobs service receives an update from a device\.

`executionNumber`  
A number that identifies a job execution on a device\.

------

#### ErrorResponse<a name="jobs-mqtt-error-response"></a>

------
#### [ ErrorResponse data type ]

Contains information about an error that occurred during an AWS IoT Jobs service operation\.

------
#### [ Syntax \(10\) ]

```
{
    "code": "ErrorCode",
    "message": "string",
    "clientToken": "string",
    "timestamp": timestamp,
    "executionState": JobExecutionState
}
```

------
#### [ Description \(10\) ]

`code`  
ErrorCode can be set to:    
InvalidTopic  
The request was sent to a topic in the AWS IoT Jobs namespace that does not map to any API\.  
InvalidJson  
The contents of the request could not be interpreted as valid UTF\-8\-encoded JSON\.  
InvalidRequest  
The contents of the request were invalid\. For example, this code is returned when an `UpdateJobExecution` request contains invalid status details\. The message contains details about the error\.  
InvalidStateTransition  
An update attempted to change the job execution to a state that is invalid because of the job execution's current state \(for example, an attempt to change a request in state SUCCEEDED to state IN\_PROGRESS\)\. In this case, the body of the error message also contains the `executionState` field\.  
ResourceNotFound  
The `JobExecution` specified by the request topic does not exist\.   
VersionMismatch  
The expected version specified in the request does not match the version of the job execution in the AWS IoT Jobs service\. In this case, the body of the error message also contains the `executionState` field\.  
InternalError  
There was an internal error during the processing of the request\.  
RequestThrottled  
The request was throttled\.  
TerminalStateReached  
Occurs when a command to describe a job is performed on a job that is in a terminal state\.

`message`  
An error message string\.

`clientToken`  
An arbitrary string used to correlate a request with its reply\.

`timestamp`  
The time, in seconds since the epoch\.

`executionState`  
A [JobExecutionState](#jobs-mqtt-job-execution-state) object\. This field is included only when the `code` field has the value `InvalidStateTransition` or `VersionMismatch`\. This makes it unnecessary in these cases to perform a separate `DescribeJobExecution` request to obtain the current job execution status data\.

------

### Device commands<a name="jobs-data-plane-commands"></a>

The following commands are available over the MQTT and HTTPS protocols\.

#### GetPendingJobExecutions<a name="mqtt-getpendingjobexecutions"></a>

------
#### [ GetPendingJobExecutions command ]

Gets the list of all jobs for a thing that are not in a terminal state\.

------
#### [ MQTT \(12\) ]

To invoke this API, publish a message on `$aws/things/thingName/jobs/get`\.

Request payload:

```
{ "clientToken": "string" }
```

`clientToken`  
Optional\. A client token used to correlate requests and responses\. Enter an arbitrary value here and it is reflected in the response\.

To receive the response, subscribe to:
+  `$aws/things/thingName/jobs/get/accepted` and 
+  `$aws/things/thingName/jobs/get/rejected` or 
+  `$aws/things/thingName/jobs/get/#` for both\. 

Response payload:

```
{
    "inProgressJobs" : [ JobExecutionSummary ... ], 
    "queuedJobs" : [ JobExecutionSummary ... ],
    "timestamp" : 1489096425069,
    "clientToken" : "client-001"
}
```

`inProgressJobs`  
A list of [JobExecutionSummary](#jobs-mqtt-job-execution-summary) objects with status `IN_PROGRESS`\.

`queuedJobs`  
A list of [JobExecutionSummary](#jobs-mqtt-job-execution-summary) objects with status `QUEUED`\.

`clientToken`  
A client token used to correlate requests and responses\.

`timestamp`  
The time, in seconds since the epoch, when the message was sent\.

------
#### [ HTTPS \(12\) ]

Request:

```
GET /things/thingName/jobs
```

`thingName`  
The name of the thing associated with the device\.

Response:

```
{
    "inProgressJobs" : [ JobExecutionSummary ... ], 
    "queuedJobs" : [ JobExecutionSummary ... ]
}
```

`inProgressJobs`  
A list of [JobExecutionSummary](#jobs-mqtt-job-execution-summary) objects\.

`queuedJobs`  
A list of [JobExecutionSummary](#jobs-mqtt-job-execution-summary) objects\.

------
#### [ CLI \(12\) ]

**Synopsis:**

```
aws iot-jobs-data  get-pending-job-executions \
    --thing-name <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "thingName": "string"
}
```


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  thingName  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the thing that is executing the job\.  | 

Output:

```
{
  "inProgressJobs": [
    {
      "jobId": "string",
      "queuedAt": long,
      "startedAt": long,
      "lastUpdatedAt": long,
      "versionNumber": long,
      "executionNumber": long
    }
  ],
  "queuedJobs": [
    {
      "jobId": "string",
      "queuedAt": long,
      "startedAt": long,
      "lastUpdatedAt": long,
      "versionNumber": long,
      "executionNumber": long
    }
  ]
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  inProgressJobs  |  list  member: JobExecutionSummary  java class: java\.util\.List  |  A list of JobExecutionSummary objects with status IN\_PROGRESS\.  | 
|  JobExecutionSummary  |  JobExecutionSummary  |   | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to this job when it was created\.  | 
|  queuedAt  |  long  |  The time, in seconds since the epoch, when the job execution was enqueued\.  | 
|  startedAt  |  long  java class: java\.lang\.Long  |  The time, in seconds since the epoch, when the job execution started\.  | 
|  lastUpdatedAt  |  long  |  The time, in seconds since the epoch, when the job execution was last updated\.  | 
|  versionNumber  |  long  |  The version of the job execution\. Job execution versions are incremented each time the AWS IoT Jobs service receives an update from a device\.  | 
|  executionNumber  |  long  java class: java\.lang\.Long  |  A number that identifies a job execution on a device\.  | 
|  queuedJobs  |  list  member: JobExecutionSummary  java class: java\.util\.List  |  A list of JobExecutionSummary objects with status QUEUED\.  | 
|  JobExecutionSummary  |  JobExecutionSummary  |   | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to this job when it was created\.  | 
|  queuedAt  |  long  |  The time, in seconds since the epoch, when the job execution was enqueued\.  | 
|  startedAt  |  long  java class: java\.lang\.Long  |  The time, in seconds since the epoch, when the job execution started\.  | 
|  lastUpdatedAt  |  long  |  The time, in seconds since the epoch, when the job execution was last updated\.  | 
|  versionNumber  |  long  |  The version of the job execution\. Job execution versions are incremented each time the AWS IoT Jobs service receives an update from a device\.  | 
|  executionNumber  |  long  java class: java\.lang\.Long  |  A number that identifies a job execution on a device\.  | 

------

#### StartNextPendingJobExecution<a name="mqtt-startnextpendingjobexecution"></a>

------
#### [ StartNextPendingJobExecution command ]

Gets and starts the next pending job execution for a thing \(status IN\_PROGRESS or QUEUED\)\. 
+ Any job executions with status IN\_PROGRESS are returned first\.
+ Job executions are returned in the order in which they were created\.
+ If the next pending job execution is QUEUED, its state is changed to IN\_PROGRESS and the job execution's status details are set as specified\.
+ If the next pending job execution is already IN\_PROGRESS, its status details are not changed\.
+ If no job executions are pending, the response does not include the `execution` field\.
+ You can optionally create a step timer by setting a value for the `stepTimeoutInMinutes` property\. If you don't update the value of this property by running `UpdateJobExecution`, the job execution times out when the step timer expires\.

------
#### [ MQTT \(13\) ]

To invoke this API, publish a message on `$aws/things/thingName/jobs/start-next`\.

Request payload:

```
{ 
    "statusDetails": {
        "string": "job-execution-state"
        ...
    },
    "stepTimeoutInMinutes": long,
    "clientToken": "string"
}
```

`statusDetails`  
A collection of name\-value pairs that describe the status of the job execution\. If not specified, the `statusDetails` are unchanged\.

`stepTimeOutInMinutes`  
Specifies the amount of time this device has to finish execution of this job\. If the job execution status is not set to a terminal state before this timer expires, or before the timer is reset \(by calling `UpdateJobExecution`, setting the status to `IN_PROGRESS` and specifying a new timeout value in field `stepTimeoutInMinutes`\) the job execution status is set to `TIMED_OUT`\. Setting this timeout has no effect on that job execution timeout that might have been specified when the job was created \(`CreateJob` using the `timeoutConfig` field\)\. 

`clientToken`  
A client token used to correlate requests and responses\. Enter an arbitrary value here and it is reflected in the response\.

To receive the response, subscribe to:
+  `$aws/things/thingName/jobs/start-next/accepted` and 
+  `$aws/things/thingName/jobs/start-next/rejected` or 
+  `$aws/things/thingName/jobs/start-next/#` for both\. 

Response payload:

```
{
    "execution" : JobExecutionData,
    "timestamp" : timestamp,
    "clientToken" : "string"
}
```

`execution`  
A [JobExecution](#jobs-mqtt-job-execution-data) object\. For example:  

```
{
    "execution" : {
        "jobId" : "022",
        "thingName" : "MyThing",
        "jobDocument" : "< contents of job document >",
        "status" : "IN_PROGRESS",
        "queuedAt" : 1489096123309,
        "lastUpdatedAt" : 1489096123309,
        "versionNumber" : 1,
        "executionNumber" : 1234567890
    },
    "clientToken" : "client-1",
    "timestamp" : 1489088524284,
}
```

`timestamp`  
The time, in milliseconds since the epoch, when the message was sent to the device\.

`clientToken`  
A client token used to correlate requests and responses\.

------
#### [ HTTPS \(13\) ]

Request:

```
PUT /things/thingName/jobs/$next
{
    "statusDetails": { 
        "string": "string" 
        ... 
    },
    "stepTimeoutInMinutes": long
}
```

`thingName`  
The name of the thing associated with the device\.

`statusDetails`  
A collection of name\-value pairs that describe the status of the job execution\. If not specified, the `statusDetails` are unchanged\.

`stepTimeOutInMinutes`  
Specifies the amount of time this device has to finish execution of this job\. If the job execution status is not set to a terminal state before this timer expires, or before the timer is reset \(by calling `UpdateJobExecution`, setting the status to `IN_PROGRESS` and specifying a new timeout value in field `stepTimeoutInMinutes`\) the job execution status is set to `TIMED_OUT`\. Setting this timeout has no effect on that job execution timeout that might have been specified when the job was created \(`CreateJob` using the `timeoutConfig` field\)\. 

Response:

```
{
    "execution" :  JobExecution 
}
```

`execution`  
A [JobExecution](#jobs-mqtt-job-execution-data) object\. For example:  

```
{
    "execution" : {
        "jobId" : "022",
        "thingName" : "MyThing",
        "jobDocument" : "< contents of job document >",
        "status" : "IN_PROGRESS",
        "queuedAt" : 1489096123309,
        "lastUpdatedAt" : 1489096123309,
        "versionNumber" : 1,
        "executionNumber" : 1234567890
    },
    "clientToken" : "client-1",
    "timestamp" : 1489088524284,
}
```

------
#### [ CLI \(13\) ]

**Synopsis:**

```
aws iot-jobs-data  start-next-pending-job-execution \
    --thing-name <value> \
    {--step-timeout-in-minutes <value>] \
    [--status-details <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "thingName": "string",
  "statusDetails": {
    "string": "string"
  },
    "stepTimeoutInMinutes": long
}
```


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  thingName  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the thing associated with the device\.  | 
|  statusDetails  |  map  key: DetailsKey  value: DetailsValue  |  A collection of name\-value pairs that describe the status of the job execution\. If not specified, the statusDetails are unchanged\.  | 
|  stepTimeOutInMinutes  |  long  |   Specifies the amount of time this device has to finish execution of this job\. If the job execution status is not set to a terminal state before this timer expires, or before the timer is reset \(by calling `UpdateJobExecution`, setting the status to `IN_PROGRESS` and specifying a new timeout value in field `stepTimeoutInMinutes`\) the job execution status is set to `TIMED_OUT`\. Setting this timeout has no effect on that job execution timeout that might have been specified when the job was created \(`CreateJob` using the `timeoutConfig` field\)\.   | 
|  DetailsKey  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |   | 
|  DetailsValue  |  string  length max:1024 min:1  pattern: \[^\\\\p\{C\}\]\*\+  |   | 

Output:

```
{
  "execution": {
    "jobId": "string",
    "thingName": "string",
    "status": "string",
    "statusDetails": {
      "string": "string"
    },
    "queuedAt": long,
    "startedAt": long,
    "lastUpdatedAt": long,
    "versionNumber": long,
    "executionNumber": long,
    "jobDocument": "string"
  }
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  execution  |  JobExecution  |  A JobExecution object\.  | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to this job when it was created\.  | 
|  thingName  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the thing that is executing the job\.  | 
|  status  |  string  enum: QUEUED \| IN\_PROGRESS \| SUCCEEDED \| FAILED \| TIMED\_OUT \| REJECTED \| REMOVED \| CANCELED  |  The status of the job execution\. Can be one of: QUEUED, IN\_PROGRESS, FAILED, SUCCEEDED, CANCELED, TIMED\_OUT, REJECTED, or REMOVED\.  | 
|  statusDetails  |  map  key: DetailsKey  value: DetailsValue  |  A collection of name\-value pairs that describe the status of the job execution\.  | 
|  DetailsKey  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |   | 
|  DetailsValue  |  string  length max:1024 min:1  pattern: \[^\\\\p\{C\}\]\*\+  |   | 
|  queuedAt  |  long  |  The time, in seconds since the epoch, when the job execution was enqueued\.  | 
|  startedAt  |  long  java class: java\.lang\.Long  |  The time, in seconds since the epoch, when the job execution was started\.  | 
|  lastUpdatedAt  |  long  |  The time, in seconds since the epoch, when the job execution was last updated\.   | 
|  versionNumber  |  long  |  The version of the job execution\. Job execution versions are incremented each time they are updated by a device\.  | 
|  executionNumber  |  long  java class: java\.lang\.Long  |  A number that identifies a job execution on a device\. It can be used later in commands that return or update job execution information\.  | 
|  jobDocument  |  string  length max:32768  |  The content of the job document\.  | 

------

#### DescribeJobExecution<a name="mqtt-describejobexecution"></a>

------
#### [ DescribeJobExecution command ]

Gets detailed information about a job execution\.

You can set the `jobId` to `$next` to return the next pending job execution for a thing \(status IN\_PROGRESS or QUEUED\)\.

------
#### [ MQTT \(14\) ]

To invoke this API, publish a message on `$aws/things/thingName/jobs/jobId/get`\. 

Request payload:

```
{ 
    "executionNumber": long,
    "includeJobDocument": boolean,
    "clientToken": "string" 
}
```

`thingName`  
The name of the thing associated with the device\.

`jobId`  
The unique identifier assigned to this job when it was created\.   
Or use `$next` to return the next pending job execution for a thing \(status IN\_PROGRESS or QUEUED\)\. In this case, any job executions with status IN\_PROGRESS are returned first\. Job executions are returned in the order in which they were created\.

`executionNumber`  
Optional\. A number that identifies a job execution on a device\. If not specified, the latest job execution is returned\.

`includeJobDocument`  
Optional\. Unless set to `false`, the response contains the job document\. The default is `true`\.

`clientToken`  
A client token used to correlate requests and responses\. Enter an arbitrary value here and it is reflected in the response\.

To receive the response, subscribe to:
+  `$aws/things/thingName/jobs/jobId/get/accepted` and 
+  `$aws/things/thingName/jobs/jobId/get/rejected` or 
+  `$aws/things/thingName/jobs/jobId/get/#` for both\. 

Response payload:

```
{
    "execution" : JobExecutionData,
    "timestamp": "timestamp",
    "clientToken": "string"
}
```

`execution`  
A [JobExecution](#jobs-mqtt-job-execution-data) object\.

`timestamp`  
The time, in seconds since the epoch, when the message was sent\.

`clientToken`  
A client token used to correlate requests and responses\.

------
#### [ HTTPS \(14\) ]

The job's execution status must be `QUEUED` or `IN_PROGRESS`\.

Request:

```
GET /things/thingName/jobs/jobId?executionNumber=executionNumber&includeJobDocument=includeJobDocument
```

`thingName`  
The name of the thing associated with the device\.

`jobId`  
The unique identifier assigned to this job when it was created\.   
Or use `$next` to return the next pending job execution for a thing \(status IN\_PROGRESS or QUEUED\)\. In this case, any job executions with status IN\_PROGRESS are returned first\. Job executions are returned in the order in which they were created\.

`includeJobDocument`  
Optional\. Unless set to `false`, the response contains the job document\. The default is `true`\.

`executionNumber`  
Optional\. A number that identifies a job execution on a device\. If not specified, the latest job execution is returned\.

Response:

```
{
    "execution" : JobExecution,
}
```

`execution`  
A [JobExecution](#jobs-mqtt-job-execution-data) object\.

------
#### [ CLI \(14\) ]

The job's execution status must be `QUEUED` or `IN_PROGRESS`\.

**Synopsis:**

```
aws iot-jobs-data  describe-job-execution \
    --job-id <value> \
    --thing-name <value> \
    [--include-job-document | --no-include-job-document] \
    [--execution-number <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "jobId": "string",
  "thingName": "string",
  "includeJobDocument": boolean,
  "executionNumber": long
}
```


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  jobId  |  string  pattern: \[a\-zA\-Z0\-9\_\-\]\+\|^$next  |  The unique identifier assigned to this job when it was created, or `$next` to return the next pending job execution for a thing \(status IN\_PROGRESS or QUEUED\)\. In this case, any job executions with status IN\_PROGRESS are returned first\. Job executions are returned in the order in which they were created\.  | 
|  thingName  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The thing name associated with the device the job execution is running on\.  | 
|  includeJobDocument  |  boolean  java class: java\.lang\.Boolean  |  Optional\. Unless set to false, the response contains the job document\. The default is true\.  | 
|  executionNumber  |  long  java class: java\.lang\.Long  |  Optional\. A number that identifies a job execution on a device\. If not specified, the latest job execution is returned\.  | 

Output:

```
{
  "execution": {
    "jobId": "string",
    "thingName": "string",
    "status": "string",
    "statusDetails": {
      "string": "string"
    },
    "queuedAt": long,
    "startedAt": long,
    "lastUpdatedAt": long,
    "versionNumber": long,
    "executionNumber": long,
    "jobDocument": "string"
  }
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  execution  |  JobExecution  |  Contains data about a job execution\.  | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier you assigned to this job when it was created\.  | 
|  thingName  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the thing that is executing the job\.  | 
|  status  |  string  enum: QUEUED \| IN\_PROGRESS \| SUCCEEDED \| FAILED \| TIMED\_OUT \| REJECTED \| REMOVED \| CANCELED  |  The status of the job execution\. Can be one of: QUEUED, IN\_PROGRESS, FAILED, SUCCEEDED, CANCELED, TIMED\_OUT, REJECTED, or REMOVED\.  | 
|  statusDetails  |  map  key: DetailsKey  value: DetailsValue  |  A collection of name\-value pairs that describe the status of the job execution\.  | 
|  DetailsKey  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |   | 
|  DetailsValue  |  string  length max:1024 min:1  pattern: \[^\\\\p\{C\}\]\*\+  |   | 
|  queuedAt  |  long  |  The time, in seconds since the epoch, when the job execution was enqueued\.  | 
|  startedAt  |  long  java class: java\.lang\.Long  |  The time, in seconds since the epoch, when the job execution was started\.  | 
|  lastUpdatedAt  |  long  |  The time, in seconds since the epoch, when the job execution was last updated\.   | 
|  versionNumber  |  long  |  The version of the job execution\. Job execution versions are incremented each time they are updated by a device\.  | 
|  executionNumber  |  long  java class: java\.lang\.Long  |  A number that identifies a job execution on a device\. It can be used later in commands that return or update job execution information\.  | 
|  jobDocument  |  string  length max:32768  |  The content of the job document\.  | 

------

#### UpdateJobExecution<a name="mqtt-updatejobexecution"></a>

------
#### [ UpdateJobExecution command ]

Updates the status of a job execution\. You can optionally create a step timer by setting a value for the `stepTimeoutInMinutes` property\. If you don't update the value of this property by running `UpdateJobExecution` again, the job execution times out when the step timer expires\.

------
#### [ MQTT \(15\) ]

To invoke this API, publish a message on `$aws/things/thingName/jobs/jobId/update`\. 

Request payload:

```
{
    "status": "job-execution-state",
    "statusDetails": { 
        "string": "string"
        ...
    },
    "expectedVersion": "number",
    "executionNumber": long,
    "includeJobExecutionState": boolean,
    "includeJobDocument": boolean,
    "stepTimeoutInMinutes": long,
    "clientToken": "string"
}
```

`status`  
The new status for the job execution \(IN\_PROGRESS, FAILED, SUCCEEDED, or REJECTED\)\. This must be specified on every update\.

`statusDetails`  
A collection of name\-value pairs that describe the status of the job execution\. If not specified, the `statusDetails` are unchanged\.

`expectedVersion`  
The expected current version of the job execution\. Each time you update the job execution, its version is incremented\. If the version of the job execution stored in the AWS IoT Jobs service does not match, the update is rejected with a `VersionMismatch` error, and an [ErrorResponse](#jobs-mqtt-error-response) that contains the current job execution status data is returned\. \(This makes it unnecessary to perform a separate `DescribeJobExecution` request to obtain the job execution status data\.\)

`executionNumber`  
Optional\. A number that identifies a job execution on a device\. If not specified, the latest job execution is used\.

`includeJobExecutionState`  
Optional\. When included and set to `true`, the response contains the `JobExecutionState` field\. The default is `false`\.

`includeJobDocument`  
Optional\. When included and set to `true`, the response contains the `JobDocument`\. The default is `false`\.

`stepTimeoutInMinutes`  
Specifies the amount of time this device has to finish execution of this job\. If the job execution status is not set to a terminal state before this timer expires, or before the timer is reset \(by again calling `UpdateJobExecution`, setting the status to `IN_PROGRESS` and specifying a new timeout value in this field\) the job execution status is set to `TIMED_OUT`\. Setting or resetting this timeout has no effect on the job execution timeout that might have been specified when the job was created \(by using `CreateJob` with the `timeoutConfig`\)\. 

`clientToken`  
A client token used to correlate requests and responses\. Enter an arbitrary value here and it is reflected in the response\.

To receive the response, subscribe to:
+  `$aws/things/thingName/jobs/jobId/update/accepted` and 
+  `$aws/things/thingName/jobs/jobId/update/rejected` or 
+  `$aws/things/thingName/jobs/jobId/update/#` for both\. 

Response payload:

```
{
    "executionState": JobExecutionState,
    "jobDocument": "string",
    "timestamp": timestamp,
    "clientToken": "string"
}
```

`executionState`  
A [JobExecutionState](#jobs-mqtt-job-execution-state) object\.

`jobDocument`  
A [job document](iot-jobs.md#key-concepts-jobs) object\.

`timestamp`  
The time, in seconds since the epoch, when the message was sent\.

`clientToken`  
A client token used to correlate requests and responses\.

------
#### [ HTTPS \(15\) ]

Request:

```
POST /things/thingName/jobs/jobId
{
    "status": "job-execution-state",
    "statusDetails": { 
        "string": "string"
        ...
    },
    "expectedVersion": "number",
    "includeJobExecutionState": boolean,
    "includeJobDocument": boolean,
    "stepTimeoutInMinutes": long,
    "executionNumber": long 
}
```

`thingName`  
The name of the thing associated with the device\.

`jobId`  
The unique identifier assigned to this job when it was created\.

`status`  
The new status for the job execution \(IN\_PROGRESS, FAILED, SUCCEEDED, or REJECTED\)\. This must be specified on every update\.

`statusDetails`  
Optional\. A collection of name\-value pairs that describe the status of the job execution\. If not specified, the `statusDetails` are unchanged\.

`expectedVersion`  
Optional\. The expected current version of the job execution\. Each time you update the job execution, its version is incremented\. If the version of the job execution stored in the AWS IoT Jobs service does not match, the update is rejected with a `VersionMismatch` error, and an [ErrorResponse](#jobs-mqtt-error-response) that contains the current job execution status data is returned\. \(This makes it unnecessary to perform a separate `DescribeJobExecution` request to obtain the job execution status data\.\)

`includeJobExecutionState`  
Optional\. When included and set to `true`, the response contains the JobExecutionState data\. The default is `false`\.

`includeJobDocument`  
Optional\. When set to `true`, the response contains the job document\. The default is `false`\.

`stepTimeoutInMinutes`  
Specifies the amount of time this device has to finish execution of this job\. If the job execution status is not set to a terminal state before this timer expires, or before the timer is reset \(by again calling `UpdateJobExecution`, setting the status to `IN_PROGRESS` and specifying a new timeout value in this field\) the job execution status is set to `TIMED_OUT`\. Setting or resetting this timeout has no effect on the job execution timeout that might have been specified when the job was created \(by using `CreateJob` with the `timeoutConfig`\)\. 

`executionNumber`  
Optional\. A number that identifies a job execution on a device\.

Response:

```
{
    "executionState": JobExecutionState,
    "jobDocument": "string"
}
```

`executionState`  
A [JobExecutionState](#jobs-mqtt-job-execution-state) object\.

`jobDocument`  
The contents of the [job document](iot-jobs.md#key-concepts-jobs)\.

------
#### [ CLI \(15\) ]

**Synopsis:**

```
aws iot-jobs-data  update-job-execution \
    --job-id <value> \
    --thing-name <value> \
    --status <value> \
    [--status-details <value>] \
    [--expected-version <value>] \
    [--include-job-execution-state | --no-include-job-execution-state] \
    [--include-job-document | --no-include-job-document] \
    [--execution-number <value>]  \
    [--cli-input-json <value>] \
    [--step-timeout-in-minutes <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "jobId": "string",
  "thingName": "string",
  "status": "string",
  "statusDetails": {
    "string": "string"
  },
  "stepTimeoutInMinutes": number,
  "expectedVersion": long,
  "includeJobExecutionState": boolean,
  "includeJobDocument": boolean,
  "executionNumber": long
}
```


**`cli-input-json` Fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  jobId  |  string  length max:64 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The unique identifier assigned to this job when it was created\.  | 
|  thingName  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the thing associated with the device\.  | 
|  status  |  string  enum: QUEUED \| IN\_PROGRESS \| SUCCEEDED \| FAILED \| TIMED\_OUT \| REJECTED \| REMOVED \| CANCELED  |  The new status for the job execution \(IN\_PROGRESS, FAILED, SUCCEEDED, or REJECTED\)\. This must be specified on every update\.  | 
|  statusDetails  |  map  key: DetailsKey  value: DetailsValue  |   Optional\. A collection of name\-value pairs that describe the status of the job execution\. If not specified, the statusDetails are unchanged\.  | 
|  DetailsKey  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |   | 
|  DetailsValue  |  string  length max:1024 min:1  pattern: \[^\\\\p\{C\}\]\*\+  |   | 
|  stepTimeoutInMinutes long Specifies the amount of time this device has to finish execution of this job\. If the job execution status is not set to a terminal state before this timer expires, or before the timer is reset \(by again calling `UpdateJobExecution`, setting the status to `IN_PROGRESS` and specifying a new timeout value in this field\) the job execution status is set to `TIMED_OUT`\. Setting or resetting this timeout has no effect on the job execution timeout that might have been specified when the job was created \(by using `CreateJob` with the `timeoutConfig`\)\.   | 
|  expectedVersion  |  long  java class: java\.lang\.Long  |  Optional\. The expected current version of the job execution\. Each time you update the job execution, its version is incremented\. If the version of the job execution stored in the AWS IoT Jobs service does not match, the update is rejected with a VersionMismatch error, and an ErrorResponse that contains the current job execution status data is returned\. \(This makes it unnecessary to perform a separate DescribeJobExecution request to obtain the job execution status data\.\)  | 
|  includeJobExecutionState  |  boolean  java class: java\.lang\.Boolean  |  Optional\. When included and set to true, the response contains the JobExecutionState data\. The default is false\.  | 
|  includeJobDocument  |  boolean  java class: java\.lang\.Boolean  |  Optional\. When set to true, the response contains the job document\. The default is false\.  | 
|  executionNumber  |  long  java class: java\.lang\.Long  |  Optional\. A number that identifies a job execution on a device\.  | 

Output:

```
{
  "executionState": {
    "status": "string",
    "statusDetails": {
      "string": "string"
    },
    "versionNumber": long
  },
  "jobDocument": "string"
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  executionState  |  JobExecutionState  |  A JobExecutionState object\.  | 
|  status  |  string  enum: QUEUED \| IN\_PROGRESS \| SUCCEEDED \| FAILED \| TIMED\_OUT \| REJECTED \| REMOVED \| CANCELED  |  The status of the job execution\. Can be one of: QUEUED, IN\_PROGRESS, FAILED, SUCCEEDED, CANCELED, TIMED\_OUT, REJECTED, or REMOVED\.  | 
|  statusDetails  |  map  key: DetailsKey  value: DetailsValue  |  A collection of name\-value pairs that describe the status of the job execution\.  | 
|  DetailsKey  |  string  length max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |   | 
|  DetailsValue  |  string  length max:1024 min:1  pattern: \[^\\\\p\{C\}\]\*\+  |   | 
|  versionNumber  |  long  |  The version of the job execution\. Job execution versions are incremented each time they are updated by a device\.  | 
|  jobDocument  |  string  length max:32768  |  The contents of the job documents\.  | 

------

#### JobExecutionsChanged<a name="mqtt-jobexecutionschanged"></a>

------
#### [ JobExecutionsChanged message ]

Sent whenever a job execution is added to or removed from the list of pending job executions for a thing\.

------
#### [ MQTT \(16\) ]

Topic: `$aws/things/thingName/jobs/notify`

Message payload:

```
{
    "jobs" : {
        "JobExecutionState": [ [JobExecutionSummary](#jobs-job-execution-summary) ... ]
    },
    "timestamp": timestamp,
}
```

------
#### [ HTTPS \(16\) ]

Not available\.

------
#### [ CLI \(16\) ]

Not available\.

------

#### NextJobExecutionChanged<a name="mqtt-nextjobexecutionchanged"></a>

------
#### [ NextJobExecutionChanged message ]

Sent whenever there is a change to which job execution is next on the list of pending job executions for a thing, as defined for [DescribeJobExecution](#mqtt-describejobexecution) with jobId `$next`\. This message is not sent when the next job's execution details change, only when the next job that would be returned by `DescribeJobExecution` with jobId `$next` has changed\. Consider job executions J1 and J2 with state QUEUED\. J1 is next on the list of pending job executions\. If the state of J2 is changed to IN\_PROGRESS while the state of J1 remains unchanged, then this notification is sent and contains details of J2\.

------
#### [ MQTT \(17\) ]

Topic: `$aws/things/thingName/jobs/notify-next`

Message payload:

```
{
    "execution" : [JobExecution](#jobs-job-execution),
    "timestamp": timestamp,
}
```

------
#### [ HTTPS \(17\) ]

Not available\.

------
#### [ CLI \(17\) ]

Not available\.

------