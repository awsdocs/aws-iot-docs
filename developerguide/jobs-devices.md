# Devices and Jobs<a name="jobs-devices"></a>

------
#### [ device communication with Jobs ]

Devices can communicate with the AWS IoT Jobs service through these methods: 
+ MQTT
+ HTTP Signature Version 4
+ HTTP TLS

------
#### [ using the MQTT protocol ]

Communication between the AWS IoT Jobs service and your devices can occur over the MQTT protocol\. Devices subscribe to MQTT topics to be notified of new jobs and to receive responses from the AWS IoT Jobs service\. Devices publish on MQTT topics to query or update the state of a job execution\. Each device has its own general MQTT topic\. For more information about publishing and subscribing to MQTT topics, see [Message Broker for AWS IoT](iot-message-broker.md)\.

**Note**  
You must use the correct endpoint when you communicate with the AWS IoT Jobs service through MQTT\. Use the DescribeEndpoint command to find it\. For example, if you run this command:   

```
aws iot describe-endpoint --endpoint-type iot:Data
```
you get a result similar to the following:  

```
{
    "endpointAddress": "a1b2c3d4e5f6g7.iot.us-west-2.amazonaws.com"
}
```
With this method, your device uses its device\-specific certificate and private key to authenticate with the AWS IoT Jobs service\.

Devices can:
+ Be notified when a job execution is added or removed from the list of pending job executions by subscribing to the `$aws/things/thing-name/jobs/notify` MQTT topic, where `thing-name` is the name of the thing associated with the device\.
+ Be notified when the next pending job execution has changed by subscribing to the `$aws/things/thing-name/jobs/notify-next` MQTT topic, where `thing-name` is the name of the thing associated with the device\.
+ Update the status of a job execution by calling the [UpdateJobExecution](jobs-api.md#mqtt-updatejobexecution) API\.
+ Query the status of a job execution by calling the [DescribeJobExecution](jobs-api.md#mqtt-describejobexecution) API\.
+ Retrieve a list of pending job executions by calling the [GetPendingJobExecutions](jobs-api.md#mqtt-getpendingjobexecutions) API\.
+ Retrieve the next pending job execution by calling the [DescribeJobExecution](jobs-api.md#mqtt-describejobexecution) API with jobId `$next`\.
+ Get and start the next pending job execution by calling the [StartNextPendingJobExecution](jobs-api.md#mqtt-startnextpendingjobexecution) API\.

The AWS IoT Jobs service publishes success and failure messages on an MQTT topic\. The topic is formed by appending `accepted` or `rejected` to the topic used to make the request\. For example, if a request message is published on the `$aws/things/myThing/jobs/get` topic, the AWS IoT Jobs service publishes success messages on the `$aws/things/myThing/jobs/get/accepted` topic and publishes rejected messages on the `$aws/things/myThing/jobs/get/rejected` topic\.

------
#### [ using HTTP Signature Version 4 ]

Communication between the AWS IoT Jobs service and your devices can occur over HTTP Signature Version 4 on port 443\. This is the method used by the AWS SDKs and CLI\. For more information about those tools, see [AWS CLI Command Reference: iot\-jobs\-data](https://docs.aws.amazon.com/cli/latest/reference/iot-jobs-data/index.html) or [AWS SDKs and Tools](http://aws.amazon.com/tools/#sdk) and refer to the IotJobsDataPlane section for your preferred language\.

**Note**  
You must use the correct endpoint when you communicate with the AWS IoT Jobs service through HTTP Signature Version 4 or using an AWS SDK or CLI IotJobsDataPlane command\. Use the DescribeEndpoint command to find it\. For example, if you run this command:  

```
aws iot describe-endpoint --endpoint-type iot:Jobs
```
you get a result similar to the following:  

```
{
    "endpointAddress": "a1b2c3d4e5f6g7.jobs.iot.us-west-2.amazonaws.com"
}
```
With this method of communication, your device uses IAM credentials to authenticate with the AWS IoT Jobs service\.

The following commands are available using this method: 
+ DescribeJobExecution

  `aws iot-jobs-data describe-job-execution ...` 
+ GetPendingJobExecutions

  `aws iot-jobs-data get-pending-job-executions ...` 
+ StartNextPendingJobExecution

  `aws iot-jobs-data start-next-pending-job-execution ...` 
+ UpdateJobExecution

  `aws iot-jobs-data update-job-execution ...` 

------
#### [ using HTTP TLS ]

Communication between the AWS IoT Jobs service and your devices can occur over HTTP TLS on port 8443 using a third\-party software client that supports this protocol\.

**Note**  
You must use the correct endpoint when you communicate with the AWS IoT Jobs service through HTTP TLS\. Use the DescribeEndpoint command to find it\. For example, if you run this command:   

```
aws iot describe-endpoint --endpoint-type iot:Jobs 
```
you get a result similar to the following:  

```
{
    "endpointAddress": "a1b2c3d4e5f6g7.jobs.iot.us-west-2.amazonaws.com"
}
```
With this method, your device uses X\.509 certificate\-based authentication \(for example, its device\-specific certificate and private key\)\.

The following commands are available using this method: 
+ DescribeJobExecution
+ GetPendingJobExecutions
+ StartNextPendingJobExecution
+ UpdateJobExecution

------

## Programming Devices to Work with Jobs<a name="programming-devices"></a>

The examples in this section use MQTT to illustrate how a device works with the AWS IoT Jobs service\. Alternatively, you could use the corresponding API or CLI commands\. For these examples, we assume a device called `MyThing` subscribes to the following MQTT topics:
+ `$aws/things/MyThing/jobs/notify` \(or `$aws/things/MyThing/jobs/notify-next`\)
+ `$aws/things/MyThing/jobs/get/accepted`
+ `$aws/things/MyThing/jobs/get/rejected`
+ `$aws/things/MyThing/jobs/jobId/get/accepted`
+ `$aws/things/MyThing/jobs/jobId/get/rejected`

 If you are using Code\-signing for AWS IoT your device code must verify the signature of your code file\. The signature will be in the job document within the `codesign` property\. For more information about verifying a code file signature, see [Device Agent Sample](https://github.com/aws/aws-iot-device-sdk-js#jobsAgent)\.

### Device Workflow<a name="jobs-workflow-device-online"></a>

There are two ways a device can handle the jobs it is given to execute\. 

------
#### [ Option A: Get the next job ]

1. When a device first comes online, it should subscribe to the device's `notify-next` topic\.

1. Call the [DescribeJobExecution](jobs-api.md#mqtt-describejobexecution) MQTT API with jobId `$next` to get the next job, its job document, and other details, including any state saved in `statusDetails`\. If the job document has a code file signature, you must verify the signature before proceeding with processing the job request\.

1. Call the [UpdateJobExecution](jobs-api.md#mqtt-updatejobexecution) MQTT API to update the job status\. Or, to combine this and the previous step in one call, the device can call [StartNextPendingJobExecution](jobs-api.md#mqtt-startnextpendingjobexecution)\.

1. Optionally, you can add a step timer by setting a value for `stepTimeoutInMinutes` when you call either [UpdateJobExecution](jobs-api.md#mqtt-updatejobexecution) or [StartNextPendingJobExecution](jobs-api.md#mqtt-startnextpendingjobexecution)\.

1. Perform the actions specified by the job document using the [UpdateJobExecution](jobs-api.md#mqtt-updatejobexecution) MQTT API to report on the progress of the job\.

1. Continue to monitor the job execution by calling the [DescribeJobExecution](jobs-api.md#mqtt-describejobexecution) MQTT API with this jobId\. If the job execution is canceled or deleted while the device is running the job, the device should be capable of recovering to a valid state\.

1. Call the [UpdateJobExecution](jobs-api.md#mqtt-updatejobexecution) MQTT API when finished with the job to update the job status and report success or failure\.

1. Because this job's execution status has been changed to a terminal state, the next job available for execution \(if any\) changes\. The device is notified that the next pending job execution has changed\. At this point, the device should continue as described in step 2\. 

If the device remains online, it continues to receive a notifications of the next pending job execution, including its job execution data, when it completes a job or a new pending job execution is added\. When this occurs, the device continues as described in step 2\.

------
#### [ Option B: Pick from available jobs ]

1. When a device first comes online, it should subscribe to the thing's `notify` topic\.

1. Call the [GetPendingJobExecutions](jobs-api.md#mqtt-getpendingjobexecutions) MQTT API to get a list of pending job executions\.

1. If the list contains one or more job executions, pick one\.

1. Call the [DescribeJobExecution](jobs-api.md#mqtt-describejobexecution) MQTT API to get the job document and other details, including any state saved in `statusDetails`\.

1. Call the [UpdateJobExecution](jobs-api.md#mqtt-updatejobexecution) MQTT API to update the job status\. If the `includeJobDocument` field is set to `true` in this command, the device can skip the previous step and retrieve the job document at this point\.

1. Optionally, you can add a step timer by setting a value for `stepTimeoutInMinutes` when you call [UpdateJobExecution](jobs-api.md#mqtt-updatejobexecution)\.

1. Perform the actions specified by the job document using the [UpdateJobExecution](jobs-api.md#mqtt-updatejobexecution) MQTT API to report on the progress of the job\.

1. Continue to monitor the job execution by calling the [DescribeJobExecution](jobs-api.md#mqtt-describejobexecution) MQTT API with this jobId\. If the job execution is canceled or deleted while the device is running the job, the device should be capable of recovering to a valid state\.

1. Call the [UpdateJobExecution](jobs-api.md#mqtt-updatejobexecution) MQTT API when finished with the job to update the job status and to report success or failure\.

If the device remains online, it is notified of all pending job executions when a new pending job execution becomes available\. When this occurs, the device can continue as described in step 2\.

------

If the device is unable to execute the job, it should call the [UpdateJobExecution](jobs-api.md#mqtt-updatejobexecution) MQTT API to update the job status to `REJECTED`\.

### Starting a New Job<a name="jobs-respond-new-job"></a>

------
#### [ new job notification ]

When a new job is created, the AWS IoT Jobs service publishes a message on the `$aws/things/thing-name/jobs/notify` topic for each target device\.

------
#### [ more info \(1\) ]

The message contains the following information:

```
{
    "timestamp":1476214217017,
    "jobs":{
        "QUEUED":[{
            "jobId":"0001",
            "queuedAt":1476214216981,
            "lastUpdatedAt":1476214216981,
            "versionNumber" : 1
        }]
    }
}
```

The device receives this message on the `'$aws/things/thingName/jobs/notify'` topic when the job execution is queued\.

------

------
#### [ get job information ]

To get more information about a job execution, the device calls the [DescribeJobExecution](jobs-api.md#mqtt-describejobexecution) MQTT API with the `includeJobDocument` field set to `true` \(the default\)\.

------
#### [ more info \(2\) ]

If the request is successful, the AWS IoT Jobs service publishes a message on the `$aws/things/MyThing/jobs/0023/get/accepted` topic:

```
{
    "clientToken" : "client-001",
    "timestamp" : 1489097434407,
    "execution" : {
        "approximateSecondsBeforeTimedOut": number,
        "jobId" : "023",
        "status" : "QUEUED",
        "queuedAt" : 1489097374841,
        "lastUpdatedAt" : 1489097374841,
        "versionNumber" : 1,
        "jobDocument" : {
            < contents of job document >
        }
    }
}
```

**Note**  
If the request fails, the AWS IoT Jobs service publishes a message on the `$aws/things/MyThing/jobs/0023/get/rejected` topic\.

------

The device now has the job document that it can use to perform the remote operations for the job\. If the job document contains an Amazon S3 presigned URL, the device can use that URL to download any required files for the job\.

### Report Job Execution Status<a name="jobs-job-processing"></a>

------
#### [ update execution status ]

As the device is executing the job, it can call the [UpdateJobExecution](jobs-api.md#mqtt-updatejobexecution) MQTT API to update the status of the job execution\.

------
#### [ more info \(3\) ]

For example, a device can update the job execution status to `IN_PROGRESS` by publishing the following message on the `$aws/things/MyThing/jobs/0023/update` topic:

```
{
    "status":"IN_PROGRESS",
    "statusDetails": {
        "progress":"50%"
    },
    "expectedVersion":"1",
    "clientToken":"client001"
}
```

Jobs responds by publishing a message to the `$aws/things/MyThing/jobs/0023/update/accepted` or `$aws/things/MyThing/jobs/0023/update/rejected` topic:

```
{
    "clientToken":"client001",
    "timestamp":1476289222841
}
```

The device can combine the two previous requests by calling [StartNextPendingJobExecution](jobs-api.md#mqtt-startnextpendingjobexecution)\. That gets and starts the next pending job execution and allows the device to update the job execution status\. This request also returns the job document when there is a job execution pending\.

If the job contains a [TimeoutConfig](https://docs.aws.amazon.com/iot/latest/apireference/API_TimeoutConfig.html), the in\-progress timer starts running\. You can also set a step timer for a job execution by setting a value for `stepTimeoutInMinutes` when you call [UpdateJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html)\. The step timer applies only to the job execution that you update\. You can set a new value for this timer each time you update a job execution\. You can also create a step timer when you call [StartNextPendingJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_StartNextPendingJobExecution.html)\. If the job execution remains in the `IN_PROGRESS` status for longer than the step timer interval, it fails and switches to the terminal `TIMED_OUT` status\. The step timer has no effect on the in\-progress timer that you set when you create a job\.

**Note**  
The job timeout feature isn't currently available in the PDT \(us\-gov\-west\-1\) Region\.

The `status` field can be set to `IN_PROGRESS`, `SUCCEEDED`, or `FAILED`\. You cannot update the status of a job execution that is already in a terminal state\.

------

------
#### [ report execution completed ]

When the device is finished executing the job, it calls the [UpdateJobExecution](jobs-api.md#mqtt-updatejobexecution) MQTT API\. If the job was successful, set `status` to `SUCCEEDED` and, in the message payload, in `statusDetails`, add other information about the job as name\-value pairs\. The in\-progress and step timers end when the job execution is complete\.

------
#### [ more info \(4\) ]

For example:

```
{
    "status":"SUCCEEDED",
    "statusDetails": {
        "progress":"100%"
    },
    "expectedVersion":"2",
    "clientToken":"client-001"
}
```

If the job was not successful, set `status` to `FAILED` and, in `statusDetails`, add information about the error that occurred:

```
{
    "status":"FAILED",
    "statusDetails": {
        "errorCode":"101",
        "errorMsg":"Unable to install update"
    },
    "expectedVersion":"2",
    "clientToken":"client-001"
}
```

**Note**  
The `statusDetails` attribute can contain any number of name\-value pairs\.

When the AWS IoT Jobs service receives this update, it publishes a message on the `$aws/things/MyThing/jobs/notify` topic to indicate the job execution is complete:

```
{
    "timestamp":1476290692776,
    "jobs":{}
}
```

------

### Additional Jobs<a name="jobs-additional-job"></a>

------
#### [ additional jobs ]

If there are other job executions pending for the device, they are included in the message published to `$aws/things/MyThing/jobs/notify`\.

------
#### [ more info \(5\) ]

For example:

```
{
    "timestamp":1476290692776,
    "jobs":{
        "QUEUED":[{
            "jobId":"0002",
            "queuedAt":1476290646230,
            "lastUpdatedAt":1476290646230
        }],
        "IN_PROGRESS":[{
            "jobId":"0003",
            "queuedAt":1476290646230,
            "lastUpdatedAt":1476290646230
        }]
    }
}
```

------

### Jobs Notifications<a name="jobs-comm-notifications"></a>

The AWS IoT Jobs service publishes MQTT messages to reserved topics when jobs are pending or when the first job execution in the list changes\. Devices can keep track of pending jobs by subscribing to these topics\.

Job notifications are published to MQTT topics as JSON payloads\. There are two kinds of notifications:
+ A `ListNotification` contains a list of no more than 10 pending job executions\. The job executions in this list have status values of either `IN_PROGRESS` or `QUEUED`\. They are sorted by status \(`IN_PROGRESS` job executions before `QUEUED` job executions\) and then by the times when they were queued\.

  A `ListNotification` is published whenever one of the following criteria is met\.
  + A new job execution is queued or changes to a non\-terminal status \(`IN_PROGRESS` or `QUEUED`\)\.
  + An old job execution changes to a terminal status \(`FAILED`, `SUCCEEDED`, `CANCELED`, `TIMED_OUT`, `REJECTED`, or `REMOVED`\)\.
+ A `NextNotification` contains summary information about the one job execution that is next in the queue\.

  A `NextNotification` is published whenever the first job execution in the list changes\.
  + A new job execution is added to the list as `QUEUED`, and it is the first one in the list\.
  + The status of an existing job execution that was not the first one in the list changes from `QUEUED` to `IN_PROGRESS` and becomes the first one in the list\. \(This happens when there are no other `IN_PROGRESS` job executions in the list or when the job execution whose status changes from `QUEUED` to `IN_PROGRESS` was queued earlier than any other `IN_PROGRESS` job execution in the list\.\) 
  + The status of the job execution that is first in the list changes to a terminal status and is removed from the list\.

For more information about publishing and subscribing to MQTT topics, see [Message Broker for AWS IoT](iot-message-broker.md)\.

**Note**  
Notifications are not available when you use HTTP Signature Version 4 or HTTP TLS to communicate with jobs\.

------
#### [ job pending ]

The AWS IoT Jobs service publishes a message on an MQTT topic when a job is added to or removed from the list of pending job executions for a thing or the first job execution in the list changes:
+ `$aws/things/thingName/jobs/notify`
+ `$aws/things/thingName/jobs/notify-next`

------
#### [ more info \(6\) ]

The messages contain the following example payloads:

`$aws/things/thingName/jobs/notify`:

```
{
  "timestamp" : 10011,
  "jobs" : {
    "IN_PROGRESS" : [ {
      "jobId" : "other-job",
      "queuedAt" : 10003,
      "lastUpdatedAt" : 10009,
      "executionNumber" : 1,
      "versionNumber" : 1
    } ],
    "QUEUED" : [ {
      "jobId" : "this-job",
      "queuedAt" : 10011,
      "lastUpdatedAt" : 10011,
      "executionNumber" : 1,
      "versionNumber" : 0
    } ]
  }
}
```

`$aws/things/thingName/jobs/notify-next`:

```
{
  "timestamp" : 10011,
  "execution" : {
    "jobId" : "other-job",
    "status" : "IN_PROGRESS",
    "queuedAt" : 10009,
    "lastUpdatedAt" : 10009,
    "versionNumber" : 1,
    "executionNumber" : 1,
    "jobDocument" : {"c":"d"}
  }
}
```

Possible job execution status values are `QUEUED`, `IN_PROGRESS`, `FAILED`, `SUCCEEDED`, `CANCELED`, `TIMED_OUT`, `REJECTED`, and `REMOVED`\.

The following series of examples show the notifications that are published to each topic as job executions are created and change from one state to another\. 

First, one job, called `job1`, is created\. This notification is published to the `jobs/notify` topic:

```
{
  "timestamp": 1517016948,
  "jobs": {
    "QUEUED": [
      {
        "jobId": "job1",
        "queuedAt": 1517016947,
        "lastUpdatedAt": 1517016947,
        "executionNumber": 1,
        "versionNumber": 1
      }
    ]
  }
}
```

This notification is published to the `jobs/notify-next` topic:

```
{
  "timestamp": 1517016948,
  "execution": {
    "jobId": "job1",
    "status": "QUEUED",
    "queuedAt": 1517016947,
    "lastUpdatedAt": 1517016947,
    "versionNumber": 1,
    "executionNumber": 1,
    "jobDocument": {
      "operation": "test"
    }
  }
}
```

When another job is created \(`job2`\), this notification is published to the `jobs/notify` topic:

```
{
  "timestamp": 1517017192,
  "jobs": {
    "QUEUED": [
      {
        "jobId": "job1",
        "queuedAt": 1517016947,
        "lastUpdatedAt": 1517016947,
        "executionNumber": 1,
        "versionNumber": 1
      },
      {
        "jobId": "job2",
        "queuedAt": 1517017191,
        "lastUpdatedAt": 1517017191,
        "executionNumber": 1,
        "versionNumber": 1
      }
    ]
  }
}
```

A notification is not published to the `jobs/notify-next` topic because the next job in the queue \(`job1`\) has not changed\. When `job1` starts to execute, its status changes to `IN_PROGRESS`\. No notifications are published because the list of jobs and the next job in the queue have not changed\.

When a third job \(`job3`\) is added, this notification is published to the `jobs/notify` topic:

```
{
  "timestamp": 1517017906,
  "jobs": {
    "IN_PROGRESS": [
      {
        "jobId": "job1",
        "queuedAt": 1517016947,
        "lastUpdatedAt": 1517017472,
        "startedAt": 1517017472,
        "executionNumber": 1,
        "versionNumber": 2
      }
    ],
    "QUEUED": [
      {
        "jobId": "job2",
        "queuedAt": 1517017191,
        "lastUpdatedAt": 1517017191,
        "executionNumber": 1,
        "versionNumber": 1
      },
      {
        "jobId": "job3",
        "queuedAt": 1517017905,
        "lastUpdatedAt": 1517017905,
        "executionNumber": 1,
        "versionNumber": 1
      }
    ]
  }
}
```

A notification is not published to the `jobs/notify-next` topic because the next job in the queue is still `job1`\.

When `job1` is complete, its status changes to `SUCCEEDED`, and this notification is published to the `jobs/notify` topic:

```
{
  "timestamp": 1517186269,
  "jobs": {
    "QUEUED": [
      {
        "jobId": "job2",
        "queuedAt": 1517017191,
        "lastUpdatedAt": 1517017191,
        "executionNumber": 1,
        "versionNumber": 1
      },
      {
        "jobId": "job3",
        "queuedAt": 1517017905,
        "lastUpdatedAt": 1517017905,
        "executionNumber": 1,
        "versionNumber": 1
      }
    ]
  }
}
```

At this point, `job1` has been removed from the queue, and the next job to be executed is `job2`\. This notification is published to the `jobs/notify-next` topic:

```
{
  "timestamp": 1517186269,
  "execution": {
    "jobId": "job2",
    "status": "QUEUED",
    "queuedAt": 1517017191,
    "lastUpdatedAt": 1517017191,
    "versionNumber": 1,
    "executionNumber": 1,
    "jobDocument": {
      "operation": "test"
    }
  }
}
```

If `job3` must begin executing before `job2` \(which is not recommended\), the status of `job3` can be changed to `IN_PROGRESS`\. If this happens, `job2` is no longer next in the queue, and this notification is published to the `jobs/notify-next` topic:

```
{
  "timestamp": 1517186779,
  "execution": {
    "jobId": "job3",
    "status": "IN_PROGRESS",
    "queuedAt": 1517017905,
    "startedAt": 1517186779,
    "lastUpdatedAt": 1517186779,
    "versionNumber": 2,
    "executionNumber": 1,
    "jobDocument": {
      "operation": "test"
    }
  }
}
```

No notification is published to the `jobs/notify` topic because no job has been added or removed\.

If the device rejects `job2` and updates its status to `REJECTED`, this notification is published to the `jobs/notify` topic:

```
{
  "timestamp": 1517189392,
  "jobs": {
    "IN_PROGRESS": [
      {
        "jobId": "job3",
        "queuedAt": 1517017905,
        "lastUpdatedAt": 1517186779,
        "startedAt": 1517186779,
        "executionNumber": 1,
        "versionNumber": 2
      }
    ]
  }
}
```

If `job3` \(which is still in progress\) is force deleted, this notification is published to the `jobs/notify` topic:

```
{
  "timestamp": 1517189551,
  "jobs": {}
}
```

At this point, the queue is empty\. This notification is published to the `jobs/notify-next` topic:

```
{
  "timestamp": 1517189551
}
```

------