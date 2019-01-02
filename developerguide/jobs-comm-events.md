# Jobs Events<a name="jobs-comm-events"></a>

AWS IoT Jobs publishes MQTT messages to reserved topics when jobs are completed, canceled, or deleted, and when a device reports success or failure after executing a job\.

Because it can take some time to cancel or delete a job, two messages are sent to indicate the start and end of a request\. For example, when a cancellation request starts, a message is sent to the `$aws/events/job/jobID/cancellation_in_progress` topic\. When the cancellation request is complete, a message is sent to the `$aws/events/job/jobID/canceled` topic\. A similar process occurs for a job deletion request\. Management and monitoring applications can subscribe to these topics to keep track of the status of jobs\.

For more information about publishing and subscribing to MQTT topics, see [Message Broker for AWS IoT](iot-message-broker.md)\.

------
#### [ job completed/canceled/deleted/cancellation\_in\_progress/deletion\_in\_progress ]

AWS IoT Jobs publishes a message on an MQTT topic when a job is completed, canceled, or deleted\.

+ `$aws/events/job/jobID/completed`

+ `$aws/events/job/jobID/canceled`

+ `$aws/events/job/jobID/deleted`

+ `$aws/events/job/jobID/cancellation_in_progress`

+ `$aws/events/job/jobID/deletion_in_progress`

------
#### [ more info \(12\) ]

The `completed` message contains the following example payload:

```
{
      "eventType": "JOB",
      "eventId": "7364ffd1-8b65-4824-85d5-6c14686c97c6",
      "timestamp": 1234567890,
      "operation": "completed",
      "jobId": "27450507-bf6f-4012-92af-bb8a1c8c4484",
      "status": "SUCCEEDED",
      "targetSelection": "SNAPSHOT|CONTINUOUS",
      "targets": [
        "arn:aws:iot:us-east-1:123456789012:thing/a39f6f91-70cf-4bd2-a381-9c66df1a80d0",
        "arn:aws:iot:us-east-1:123456789012:thinggroup/2fc4c0a4-6e45-4525-a238-0fe8d3dd21bb"
      ],
      "description": "My Job Description",
      "completedAt": 1234567890123,
      "createdAt": 1234567890123,
      "lastUpdatedAt": 1234567890123,
      "comment": "Comment for this operation",
      "jobProcessDetails": {
        "numberOfCanceledThings": 0,
        "numberOfRejectedThings": 0,
        "numberOfFailedThings": 0,
        "numberOfRemovedThings": 0,
        "numberOfSucceededThings": 3,
        "numberOfTimedOutThings": 0
      }
    }
```

The `canceled` message contains the following example payload:

```
{
      "eventType": "JOB",
      "eventId": "568d2ade-2e9c-46e6-a115-18afa1286b06",
      "timestamp": 1234567890,
      "operation": "canceled",
      "jobId": "4d2a531a-da2e-47bb-8b9e-ff5adcd53ef0",
      "status": "CANCELED",
      "forceCanceled": true,
      "targetSelection": "SNAPSHOT|CONTINUOUS",
      "targets": [
        "arn:aws:iot:us-east-1:123456789012:thing/Thing0-947b9c0c-ff10-4a80-b4b3-cd33d0145a0f",
        "arn:aws:iot:us-east-1:123456789012:thinggroup/ThingGroup1-95c644d5-1621-41a6-9aa5-ad2de581d18f"
      ],
      "description": "My job description",
      "createdAt": 1234567890123,
      "lastUpdatedAt": 1234567890123,
      "comment": "Comment for this operation",
      "jobProcessDetails": {
        "numberOfCanceledThings": 0,
        "numberOfRejectedThings": 0,
        "numberOfFailedThings": 0,
        "numberOfRemovedThings": 0,
        "numberOfSucceededThings": 3,
        "numberOfTimedOutThings": 0
      }
    }
```

The `deleted` message contains the following example payload:

```
{
      "eventType": "JOB",
      "eventId": "568d2ade-2e9c-46e6-a115-18afa1286b06",
      "timestamp": 1234567890,
      "operation": "deleted",
      "jobId": "4d2a531a-da2e-47bb-8b9e-ff5adcd53ef0",
      "status": "DELETED",
      "targetSelection": "SNAPSHOT|CONTINUOUS",
      "targets": [
        "arn:aws:iot:us-east-1:123456789012:thing/Thing0-947b9c0c-ff10-4a80-b4b3-cd33d0145a0f",
        "arn:aws:iot:us-east-1:123456789012:thinggroup/ThingGroup1-95c644d5-1621-41a6-9aa5-ad2de581d18f"
      ],
      "description": "My job description",
      "createdAt": 1234567890123,
      "lastUpdatedAt": 1234567890123,
      "comment": "Comment for this operation"
    }
```

The `cancellation_in_progress` message contains the following example payload:

```
{
      "eventType": "JOB",
      "eventId": "568d2ade-2e9c-46e6-a115-18afa1286b06",
      "timestamp": 1234567890,
      "operation": "cancellation_in_progress",
      "jobId": "4d2a531a-da2e-47bb-8b9e-ff5adcd53ef0",
      "status": "CANCELLATION_IN_PROGRESS",
      "targetSelection": "SNAPSHOT|CONTINUOUS",
      "targets": [
        "arn:aws:iot:us-east-1:123456789012:thing/Thing0-947b9c0c-ff10-4a80-b4b3-cd33d0145a0f",
        "arn:aws:iot:us-east-1:123456789012:thinggroup/ThingGroup1-95c644d5-1621-41a6-9aa5-ad2de581d18f"
      ],
      "description": "My job description",
      "createdAt": 1234567890123,
      "lastUpdatedAt": 1234567890123,
      "comment": "Comment for this operation"
    }
```

The `deletion_in_progress` message contains the following example payload:

```
{
      "eventType": "JOB",
      "eventId": "568d2ade-2e9c-46e6-a115-18afa1286b06",
      "timestamp": 1234567890,
      "operation": "deletion_in_progress",
      "jobId": "4d2a531a-da2e-47bb-8b9e-ff5adcd53ef0",
      "status": "DELETION_IN_PROGRESS",
      "targetSelection": "SNAPSHOT|CONTINUOUS",
      "targets": [
        "arn:aws:iot:us-east-1:123456789012:thing/Thing0-947b9c0c-ff10-4a80-b4b3-cd33d0145a0f",
        "arn:aws:iot:us-east-1:123456789012:thinggroup/ThingGroup1-95c644d5-1621-41a6-9aa5-ad2de581d18f"
      ],
      "description": "My job description",
      "createdAt": 1234567890123,
      "lastUpdatedAt": 1234567890123,
      "comment": "Comment for this operation"
    }
```

------

------
#### [ job execution terminal status ]

The AWS IoT Jobs service publishes a message when a device updates a job execution to a terminal status:

+ `$aws/events/jobExecution/jobID/succeeded`

+ `$aws/events/jobExecution/jobID/failed`

+ `$aws/events/jobExecution/jobID/rejected`

+ `$aws/events/jobExecution/jobID/canceled`

+ `$aws/events/jobExecution/jobID/timed_out`

+ `$aws/events/jobExecution/jobID/removed`

+ `$aws/events/jobExecution/jobID/deleted`

------
#### [ more info \(13\) ]

The message contains the following example payload:

```
{
      "eventType": "JOB_EXECUTION",
      "eventId": "cca89fa5-8a7f-4ced-8c20-5e653afb3572",
      "timestamp": 1234567890,
      "operation": "succeeded|failed|rejected|timed_out|canceled|removed|deleted",
      "jobId": "154b39e5-60b0-48a4-9b73-f6f8dd032d27",
      "thingArn": "arn:aws:iot:us-east-1:123456789012:myThing/6d639fbc-8f85-4a90-924d-a2867f8366a7",
      "status": "SUCCEEDED|FAILED|REJECTED|TIMED_OUT|CANCELED|REMOVED|DELETED",
      "statusDetails": {
        "key": "value"
      }
    }
```

**Note**  
When you delete a job, the `operation` field in the message reflects this, but the `status` field reflects the job execution status prior to the deletion\.

------