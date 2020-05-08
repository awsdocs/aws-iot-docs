# AWS IoT metrics<a name="aws-iot-metrics"></a>

The `AWS/IoT` namespace includes the following metrics\. AWS IoT sends the following metrics to CloudWatch once per received request\. For more information about CloudWatch metrics, see [Amazon CloudWatch Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric)\.

## AWS IoT metrics<a name="iot-metrics"></a>


| Metric | Description | 
| --- | --- | 
|  `AddThingToDynamicThingGroupsFailed`  |  The number of failure events associated with adding a thing to a dynamic thing group\. The `DynamicThingGroupName` dimension contains the name of the dynamic groups that failed to add things\.  | 
|  `NumLogBatchesFailedToPublishThrottled`  |  The singular batch of log events that has failed to publish due to throttling errors\.  | 
|  `NumLogEventsFailedToPublishThrottled`  |  The number of log events within the batch that have failed to publish due to throttling errors\.  | 
|  `RulesExecuted`  |  The number of AWS IoT rules executed\.  | 

## Rule metrics<a name="rulemetrics"></a>


| Metric | Description | 
| --- | --- | 
|  `ParseError`  |  The number of JSON parse errors that occurred in messages published on a topic on which a rule is listening\. The `RuleName` dimension contains the name of the rule\.  | 
|  `RuleMessageThrottled`  |  The number of messages throttled by the rules engine because of malicious behavior or because the number of messages exceeds the rules engine's throttle limit\. The `RuleName` dimension contains the name of the rule to be triggered\.  | 
|  `RuleNotFound`  |  The rule to be triggered could not be found\. The `RuleName` dimension contains the name of the rule\.  | 
|  `TopicMatch`  |  The number of incoming messages published on a topic on which a rule is listening\. The `RuleName` dimension contains the name of the rule\.  | 

## Rule action metrics<a name="rule-action-metrics"></a>


| Metric | Description | 
| --- | --- | 
|  `Failure`  |  The number of failed rule action invocations\. The `RuleName` dimension contains the name of the rule that specifies the action\. The `RuleName` dimension contains the name of the rule that specifies the action\. The `ActionType` dimension contains the type of action that was invoked\.  | 
|  `Success`  |  The number of successful rule action invocations\. The `RuleName` dimension contains the name of the rule that specifies the action\. The `ActionType` dimension contains the type of action that was invoked\.  | 

## HTTP action specific metrics<a name="http-action-metrics"></a>


| Metric | Description | 
| --- | --- | 
| `HttpCode_Other` | Generated if the status code of the response from the downstream web service/application is not 2xx, 4xx or 5xx\. | 
| `HttpCode_4XX` | Generated if the status code of the response from the downstream web service/application is between 400 and 499\. | 
| `HttpCode_5XX` | Generated if the status code of the response from the downstream web service/application is between 500 and 599\. | 
| `HttpInvalidUrl` | Generated if an endpoint URL, after substitution templates are replaced, does not start with `https://`\. | 
| `HttpRequestTimeout` | Generated if the downstream web service/application does not return response within request timeout limit\. For more information, see [Service Quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#limits_iot)\. | 
| `HttpUnknownHost` | Generated if the URL is valid, but the service does not exist or is unreachable\. | 

## Message broker metrics<a name="message-broker-metrics"></a>


| Metric | Description | 
| --- | --- | 
|  `Connect.AuthError`  |  The number of connection requests that could not be authorized by the message broker\. The `Protocol` dimension contains the protocol used to send the `CONNECT`\. message\.  | 
|  `Connect.ClientError`  |  The number of connection requests rejected because the MQTT message did not meet the requirements defined in [AWS IoT Limits](https://docs.aws.amazon.com/iot/latest/developerguide/iot-limits.html)\. The `Protocol` dimension contains the protocol used to send the `CONNECT` message\.  | 
| `Connect.ClientIDThrottle` | The number of connection requests throttled because the client exceeded the allowed connect request rate for a specific client ID\. The `Protocol` dimension contains the protocol used to send the `CONNECT` message\. | 
|  `Connect.ServerError`  |  The number of connection requests that failed because an internal error occurred\. The `Protocol` dimension contains the protocol used to send the `CONNECT` message\.  | 
|  `Connect.Success`  |  The number of successful connections to the message broker\. The `Protocol` dimension contains the protocol used to send the `CONNECT` message\.  | 
|  `Connect.Throttle`  |  The number of connection requests that were throttled because the account exceeded the allowed connect request rate\. The `Protocol` dimension contains the protocol used to send the `CONNECT` message\.  | 
|  `Ping.Success`  |  The number of ping messages received by the message broker\. The `Protocol` dimension contains the protocol used to send the ping message\.  | 
|  `PublishIn.AuthError`  |  The number of publish requests the message broker was unable to authorize\. The `Protocol` dimension contains the protocol used to publish the message\.  | 
|  `PublishIn.ClientError`  |  The number of publish requests rejected by the message broker because the message did not meet the requirements defined in [AWS IoT Limits](https://docs.aws.amazon.com/iot/latest/developerguide/iot-limits.html)\. The `Protocol` dimension contains the protocol used to publish the message\.  | 
|  `PublishIn.ServerError`  |  The number of publish requests the message broker failed to process because an internal error occurred\. The `Protocol` dimension contains the protocol used to send the `PUBLISH` message\.  | 
|  `PublishIn.Success`  |  The number of publish requests successfully processed by the message broker\. The `Protocol` dimension contains the protocol used to send the `PUBLISH` message\.  | 
|  `PublishIn.Throttle`  |  The number of publish request that were throttled because the client exceeded the allowed inbound message rate\. The `Protocol` dimension contains the protocol used to send the `PUBLISH` message\.  | 
|  `PublishOut.AuthError`  |  The number of publish requests made by the message broker that could not be authorized by AWS IoT\. The `Protocol` dimension contains the protocol used to send the `PUBLISH` message\.  | 
|  `PublishOut.ClientError`  |  The number of publish requests made by the message broker that were rejected because the message did not meet the requirements defined in [AWS IoT Limits](https://docs.aws.amazon.com/iot/latest/developerguide/iot-limits.html)\. The `Protocol` dimension contains the protocol used to send the `PUBLISH` message\.  | 
|  `PublishOut.Success`  |  The number of publish requests successfully made by the message broker\. The `Protocol` dimension contains the protocol used to send the `PUBLISH` message\.  | 
|  `Subscribe.AuthError`  |  The number of subscription requests made by a client that could not be authorized\. The `Protocol` dimension contains the protocol used to send the `SUBSCRIBE` message\.  | 
|  `Subscribe.ClientError`  |  The number of subscribe requests that were rejected because the `SUBSCRIBE` message did not meet the requirements defined in [AWS IoT Limits](https://docs.aws.amazon.com/iot/latest/developerguide/iot-limits.html)\. The `Protocol` dimension contains the protocol used to send the `SUBSCRIBE` message\.  | 
|  `Subscribe.ServerError`  |  The number of subscribe requests that were rejected because an internal error occurred\. The `Protocol` dimension contains the protocol used to send the `SUBSCRIBE` message\.  | 
|  `Subscribe.Success`  |  The number of subscribe requests that were successfully processed by the message broker\. The `Protocol` dimension contains the protocol used to send the `SUBSCRIBE` message\.  | 
|  `Subscribe.Throttle`  |  The number of subscribe requests that were throttled because the client exceeded the allowed subscribe request rate\. The `Protocol` dimension contains the protocol used to send the `SUBSCRIBE` message\.  | 
|  `Unsubscribe.ClientError`  |  The number of unsubscribe requests that were rejected because the `UNSUBSCRIBE` message did not meet the requirements defined in [AWS IoT Limits](https://docs.aws.amazon.com/iot/latest/developerguide/iot-limits.html)\. The `Protocol` dimension contains the protocol used to send the `UNSUBSCRIBE` message\.  | 
|  `Unsubscribe.ServerError`  |  The number of unsubscribe requests that were rejected because an internal error occurred\. The `Protocol` dimension contains the protocol used to send the `UNSUBSCRIBE` message\.  | 
|  `Unsubscribe.Success`  |  The number of unsubscribe requests that were successfully processed by the message broker\. The `Protocol` dimension contains the protocol used to send the `UNSUBSCRIBE` message\.  | 
|  `Unsubscribe.Throttle`  |  The number of unsubscribe requests that were rejected because the client exceeded the allowed unsubscribe request rate\. The `Protocol` dimension contains the protocol used to send the `UNSUBSCRIBE` message\.   | 

**Note**  
The message broker metrics are displayed in the AWS IoT console under **Protocol Metrics**\.

## Device shadow metrics<a name="shadow-metrics"></a>


| Metric | Description | 
| --- | --- | 
|  `DeleteThingShadow.Accepted`  |  The number of DeleteThingShadow requests processed successfully\. The `Protocol` dimension contains the protocol used to make the request\.  | 
|  `GetThingShadow.Accepted`  |  The number of GetThingShadow requests processed successfully\. The `Protocol` dimension contains the protocol used to make the request\.  | 
|  `UpdateThingShadow.Accepted`  |  The number of UpdateThingShadow requests processed successfully\. The `Protocol` dimension contains the protocol used to make the request\.  | 

**Note**  
The device shadow metrics are displayed in the AWS IoT console under **Protocol Metrics**\.

## Jobs metrics<a name="jobs-metrics"></a>


| Metric | Description | 
| --- | --- | 
|  `CanceledJobExecutionCount`  |  The number of job executions whose status has changed to `CANCELED` within a time period that is determined by CloudWatch\. \(For more information about CloudWatch metrics, see [Amazon CloudWatch Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric)\.\) The `JobId` dimension contains the ID of the job\.  | 
|  `CanceledJobExecutionTotalCount`  |  The total number of job executions whose status is `CANCELED` for the given job\. The `JobId` dimension contains the ID of the job\.  | 
|  `ClientError`  |  The number of client errors generated while executing the job\. The `JobId` dimension contains the ID of the job\.  | 
|  `FailedJobExecutionCount`  |  The number of job executions whose status has changed to `FAILED` within a time period that is determined by CloudWatch\. \(For more information about CloudWatch metrics, see [Amazon CloudWatch Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric)\.\) The `JobId` dimension contains the ID of the job\.  | 
|  `FailedJobExecutionTotalCount`  |  The total number of job executions whose status is `FAILED` for the given job\. The `JobId` dimension contains the ID of the job\.  | 
|  `InProgressJobExecutionCount`  |  The number of job executions whose status has changed to `IN_PROGRESS` within a time period that is determined by CloudWatch\. \(For more information about CloudWatch metrics, see [Amazon CloudWatch Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric)\.\) The `JobId` dimension contains the ID of the job\.  | 
|  `InProgressJobExecutionTotalCount`  |  The total number of job executions whose status is `IN_PROGRESS` for the given job\. The `JobId` dimension contains the ID of the job\.  | 
|  `RejectedJobExecutionTotalCount`  |  The total number of job executions whose status is `REJECTED` for the given job\. The `JobId` dimension contains the ID of the job\.  | 
|  `RemovedJobExecutionTotalCount`  |  The total number of job executions whose status is `REMOVED` for the given job\. The `JobId` dimension contains the ID of the job\.  | 
|  `QueuedJobExecutionCount`  |  The number of job executions whose status has changed to `QUEUED` within a time period that is determined by CloudWatch\. \(For more information about CloudWatch metrics, see [Amazon CloudWatch Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric)\.\) The `JobId` dimension contains the ID of the job\.  | 
|  `QueuedJobExecutionTotalCount`  |  The total number of job executions whose status is `QUEUED` for the given job\. The `JobId` dimension contains the ID of the job\.  | 
|  `RejectedJobExecutionCount`  |  The number of job executions whose status has changed to `REJECTED` within a time period that is determined by CloudWatch\. \(For more information about CloudWatch metrics, see [Amazon CloudWatch Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric)\.\) The `JobId` dimension contains the ID of the job\.  | 
|  `RemovedJobExecutionCount`  |  The number of job executions whose status has changed to `REMOVED` within a time period that is determined by CloudWatch\. \(For more information about CloudWatch metrics, see [Amazon CloudWatch Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric)\.\) The `JobId` dimension contains the ID of the job\.  | 
|  `ServerError`  |  The number of server errors generated while executing the job\. The `JobId` dimension contains the ID of the job\.  | 
|  `SuccededJobExecutionCount`  |  The number of job executions whose status has changed to `SUCCESS` within a time period that is determined by CloudWatch\. \(For more information about CloudWatch metrics, see [Amazon CloudWatch Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric)\.\) The `JobId` dimension contains the ID of the job\.  | 
|  `SuccededJobExecutionTotalCount`  |  The total number of job executions whose status is `SUCCESS` for the given job\. The `JobId` dimension contains the ID of the job\.  | 

## Device defender audit metrics<a name="device-defender-audit-metrics"></a>


| Metric | Description | 
| --- | --- | 
|  `NonCompliantResources`  |  The number of resources that were found to be noncompliant with a check\. The system reports the number of resources that were out of compliance for each check of each audit performed\.   | 
|  `ResourcesEvaluated`  |  The number of resources that were evaluated for compliance\. The system reports the number of resources that were evaluated for each check of each audit performed\.   | 

## Device defender detect metrics<a name="device-defender-detect-metrics"></a>


| Metric | Description | 
| --- | --- | 
|  `Violations`   |  The number of new violations of security profile behaviors that have been found since the last time an evaluation was performed\. The system reports the number of new violations for the account, for a specific security profile, and for a specific behavior of a specific security profile\.   | 
|  `ViolationsCleared`   |  The number of violations of security profile behaviors that have been resolved since the last time an evaluation was performed\. The system reports the number of resolved violations for the account, for a specific security profile, and for a specific behavior of a specific security profile\.   | 
|  `ViolationsInvalidated`   |  The number of violations of security profile behaviors for which information is no longer available since the last time an evaluation was performed \(because the reporting device stopped reporting, or is no longer being monitored for some reason\)\. The system reports the number of invalidated violations for the entire account, for a specific security profile, and for a specific behavior of a specific security profile\.   | 

## Device provisioning metrics<a name="provisioning-metrics"></a>


| Metric | Description | 
| --- | --- | 
|  CreateKeysAndCertificateFailed  |  The number of failures that occurred calling the `CreateKeysAndCertificate` MQTT API\.  | 
|  GetDeviceCredentialsSucceeded  |  The number of successful `GetDeviceCredentials` calls\.  | 
|  GetDeviceCredentialsFailed  |  The number of `GetDeviceCredentials` calls that failed\.  | 
|  DeviceProvisioningFailed  |  The number of devices that failed to be provisioned  | 
|  DeviceProvisioningSucceeded  |  The number of devices provisioned successfully\.  | 
|  RegisterThingFailed  |  The number of failures that occurred when calling the MQTT `RegisterThing` API\.  | 