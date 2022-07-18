# Jobs device MQTT and HTTPS APIs and data types<a name="jobs-mqtt-https-api"></a>

The following commands are available over the MQTT and HTTPS protocols\. Use these API operations on the data plane for devices executing the jobs\.

## Jobs device MQTT and HTTPS data types<a name="jobs-data-plane-data-types"></a>

The following data types are used to communicate with the AWS IoT Jobs service over the MQTT and HTTPS protocols\.

### JobExecution<a name="jobs-mqtt-job-execution-data"></a>

Contains data about a job execution\. The following example shows the syntax:

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

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_JobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_JobExecution.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot-data/job-execution.html](https://docs.aws.amazon.com/cli/latest/reference/iot-data/job-execution.html)\.

### JobExecutionState<a name="jobs-mqtt-job-execution-state"></a>

Contains data about the state of a job execution\. The following example shows the syntax:

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

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_JobExecutionState.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_JobExecutionState.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot-data/job-execution-state.html](https://docs.aws.amazon.com/cli/latest/reference/iot-data/job-execution-state.html)\.

### JobExecutionSummary<a name="jobs-mqtt-job-execution-summary"></a>

Contains a subset of information about a job execution\. The following example shows the syntax:

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

For more information, see [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_JobExecutionSummary.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_JobExecutionSummary.html) or [https://docs.aws.amazon.com/cli/latest/reference/iot-data/job-execution-summary.html](https://docs.aws.amazon.com/cli/latest/reference/iot-data/job-execution-summary.html)\.

**Topics**
+ [Jobs device MQTT and HTTPS data types](#jobs-data-plane-data-types)
+ [Jobs device MQTT API](jobs-mqtt-api.md)
+ [Jobs device HTTP API](jobs-http-device-api.md)