# Reserved topics<a name="reserved-topics"></a>

Topics that begin with a dollar sign \($\) are reserved for use by AWS IoT\. You can subscribe and publish to these reserved topics as they allow; however, you can't create new topics that begin with a dollar sign\. Unsupported publish or subscribe operations to reserved topics can result in a terminated connection\.

## Asset model topics<a name="reserved-topics-other"></a>


| Topic | Client operations allowed | Description | 
| --- | --- | --- | 
|  $aws/sitewise/asset\-models/*assetModelId*/assets/*assetId*/properties/*propertyId*  |  Subscribe  |  AWS IoT SiteWise publishes asset property notifications to this topic\. For more information, see [Interacting with other AWS services](https://docs.aws.amazon.com/iot-sitewise/latest/userguide/interact-with-other-services.html) in the *AWS IoT SiteWise User Guide*\.  | 

## Device Defender topics<a name="reserved-topics-device-defender"></a>

These messages support response buffers in Concise Binary Object Representation \(CBOR\) format and JavaScript Object Notation \(JSON\), depending on the *payload\-format* of the topic\.


| *payload\-format* | Response format data type | 
| --- | --- | 
| cbor | Concise Binary Object Representation \(CBOR\) | 
| json | JavaScript Object Notation \(JSON\) | 

For more information, see [Sending metrics from devices](detect-device-side-metrics.md#DetectMetricsMessages)\.


| Topic | Allowed operations | Description | 
| --- | --- | --- | 
|  $aws/things/*thingName*/defender/metrics/*payload\-format*  |  Publish  |  Device Defender agents publish metrics to this topic\. For more information, see [Sending metrics from devices](detect-device-side-metrics.md#DetectMetricsMessages)\.   | 
|  $aws/things/*thingName*/defender/metrics/*payload\-format*/accepted  |  Subscribe  |  AWS IoT publishes to this topic after a Device Defender agent publishes a successful message to $aws/things/*thingName*/defender/metrics/*payload\-format*\. For more information, see [Sending metrics from devices](detect-device-side-metrics.md#DetectMetricsMessages)\.   | 
|  $aws/things/*thingName*/defender/metrics/*payload\-format*/rejected  |  Subscribe  |  AWS IoT publishes to this topic after a Device Defender agent publishes an unsuccessful message to $aws/things/*thingName*/defender/metrics/*payload\-format*\. For more information, see [Sending metrics from devices](detect-device-side-metrics.md#DetectMetricsMessages)\.   | 

## Event topics<a name="reserved-topics-event"></a>


| Topic | Client operations allowed | Description | 
| --- | --- | --- | 
|  $aws/events/certificates/registered/*caCertificateId*  |  Subscribe  |  AWS IoT publishes this message when AWS IoT automatically registers a certificate and when a client presents a certificate with the `PENDING_ACTIVATION` status\. For more information, see [Configure the first connection by a client for automatic registration](auto-register-device-cert.md#configure-auto-reg-first-connect)\.  | 
|  $aws/events/presence/connected/*clientId*  |  Subscribe  |  AWS IoT publishes to this topic when an MQTT client with the specified client ID connects to AWS IoT\. For more information, see [Connect/Disconnect events](life-cycle-events.md#connect-disconnect)\.  | 
|  $aws/events/presence/disconnected/*clientId*  |  Subscribe  |  AWS IoT publishes to this topic when an MQTT client with the specified client ID disconnects to AWS IoT\. For more information, see [Connect/Disconnect events](life-cycle-events.md#connect-disconnect)\.   | 
|  $aws/events/subscriptions/subscribed/*clientId*  |  Subscribe  |  AWS IoT publishes to this topic when an MQTT client with the specified client ID subscribes to an MQTT topic\. For more information, see [Subscribe/Unsubscribe events](life-cycle-events.md#subscribe-unsubscribe-events)\.  | 
|  $aws/events/subscriptions/unsubscribed/*clientId*  |  Subscribe  |  AWS IoT publishes to this topic when an MQTT client with the specified client ID unsubscribes to an MQTT topic\. For more information, see [Subscribe/Unsubscribe events](life-cycle-events.md#subscribe-unsubscribe-events)\.  | 
|  $aws/events/thing/*thingName*/created  |  Subscribe  |  AWS IoT publishes to this topic when the *thingName* thing is created\. For more information, see [Registry events](registry-events.md)\.  | 
|  $aws/events/thing/*thingName*/updated  |  Subscribe  |  AWS IoT publishes to this topic when the *thingName* thing is updated\. For more information, see [Registry events](registry-events.md)\.  | 
|  $aws/events/thing/*thingName*/deleted  |  Subscribe  |  AWS IoT publishes to this topic when the *thingName* thing is deleted\. For more information, see [Registry events](registry-events.md)\.  | 
|  $aws/events/thingGroup/*thingGroupName*/created  |  Subscribe  |  AWS IoT publishes to this topic when thing group, *thingGroupName*, is created\. For more information, see [Registry events](registry-events.md)\.  | 
|  $aws/events/thingGroup/*thingGroupName*/updated  |  Subscribe  |  AWS IoT publishes to this topic when thing group, *thingGroupName*, is updated\. For more information, see [Registry events](registry-events.md)\.  | 
|  $aws/events/thingGroup/*thingGroupName*/deleted  |  Subscribe  |  AWS IoT publishes to this topic when thing group, *thingGroupName*, is deleted\. For more information, see [Registry events](registry-events.md)\.  | 
|  $aws/events/thingType/*thingTypeName*/created  |  Subscribe  |  AWS IoT publishes to this topic when the *thingTypeName* thing type is created\. For more information, see [Registry events](registry-events.md)\.  | 
|  $aws/events/thingType/*thingTypeName*/updated  |  Subscribe  |  AWS IoT publishes to this topic when the *thingTypeName* thing type is updated\. For more information, see [Registry events](registry-events.md)\.  | 
|  $aws/events/thingType/*thingTypeName*/deleted  |  Subscribe  |  AWS IoT publishes to this topic when the *thingTypeName* thing type is deleted\. For more information, see [Registry events](registry-events.md)\.  | 
|  $aws/events/thingTypeAssociation/thing/*thingName*/*thingTypeName*  |  Subscribe  |  AWS IoT publishes to this topic when thing, *thingName*, is associated with or disassociated from thing type, *thingTypeName*\. For more information, see [Registry events](registry-events.md)\.  | 
|  $aws/events/thingGroupMembership/thingGroup/*thingGroupName*/thing/*thingName*/added  |  Subscribe  |   AWS IoT publishes to this topic when thing, *thingName*, is added to thing group, *thingGroupName*\. For more information, see [Registry events](registry-events.md)\.   | 
|  $aws/events/thingGroupMembership/thingGroup/*thingGroupName*/thing/*thingName*/removed  |  Subscribe  |   AWS IoT publishes to this topic when thing, *thingName*, is removed from thing group, *thingGroupName*\. For more information, see [Registry events](registry-events.md)\.   | 
|   $aws/events/thingGroupHierarchy/thingGroup/*parentThingGroupName*/childThingGroup/*childThingGroupName*/added  |  Subscribe  |   AWS IoT publishes to this topic when thing group, *childThingGroupName*, is added to thing group, *parentThingGroupName*\. For more information, see [Registry events](registry-events.md)\.   | 
|   $aws/events/thingGroupHierarchy/thingGroup/*parentThingGroupName*/childThingGroup/*childThingGroupName*/removed  |  Subscribe  |   AWS IoT publishes to this topic when thing group, *childThingGroupName*, is removed from thing group, *parentThingGroupName*\. For more information, see [Registry events](registry-events.md)\.   | 

## Fleet provisioning topics<a name="reserved-topics-fleet"></a>

These messages support response buffers in Concise Binary Object Representation \(CBOR\) format and JavaScript Object Notation \(JSON\), depending on the *payload\-format* of the topic\.


| *payload\-format* | Response format data type | 
| --- | --- | 
| cbor | Concise Binary Object Representation \(CBOR\) | 
| json | JavaScript Object Notation \(JSON\) | 

For more information, see [Device provisioning MQTT API](fleet-provision-api.md)\.


| Topic | Client operations allowed | Description | 
| --- | --- | --- | 
|  $aws/certificates/create/*payload\-format*  |  Publish  |  Publish to this topic to create a certificate from a certificate signing request \(CSR\)\.  | 
|  $aws/certificates/create/*payload\-format*/accepted  |  Subscribe  |  AWS IoT publishes to this topic after a successful call to $aws/certificates/create/*payload\-format*\.  | 
|  $aws/certificates/create/*payload\-format*/rejected  |  Subscribe  |  AWS IoT publishes to this topic after an unsuccessful call to $aws/certificates/create/*payload\-format*\.  | 
|  $aws/certificates/create\-from\-csr/*payload\-format*  |  Publish  |  Publishes to this topic to create a certificate from a CSR\.  | 
|  $aws/certificates/create\-from\-csr/*payload\-format*/accepted  |  Subscribe  |  AWS IoT publishes to this topic a successful call to $aws/certificates/create\-from\-csr/*payload\-format*\.  | 
|  $aws/certificates/create\-from\-csr/*payload\-format*/rejected  |  Subscribe  |  AWS IoT publishes to this topic an unsuccessful call to $aws/certificates/create\-from\-csr/*payload\-format*\.  | 
|  $aws/events/presence/connected/*clientId*  |  Subscribe  |  AWS IoT publishes to this topic when an MQTT client with the specified client ID connects to AWS IoT\. For more information, see [Connect/Disconnect events](life-cycle-events.md#connect-disconnect)\.  | 
|  $aws/provisioning\-templates/*templateName*/provision/*payload\-format*  |  Publish  |  Publish to this topic to register a thing\.  | 
|  $aws/provisioning\-templates/*templateName*/provision/*payload\-format*/accepted  |  Subscribe  |  AWS IoT publishes to this topic after a successful call to $aws/provisioning\-templates/*templateName*/provision/*payload\-format*\.  | 
|  $aws/provisioning\-templates/*templateName*/provision/*payload\-format*/rejected  |  Subscribe  |  AWS IoT publishes to this topic after an unsuccessful call to $aws/provisioning\-templates/*templateName*/provision/*payload\-format*\.  | 

## Job topics<a name="reserved-topics-job"></a>

**Note**  
The client operations noted as **Receive** in this table indicate topics that AWS IoT publishes directly to the client that requested it, whether the client has subscribed to the topic or not\. Clients should also expect to receive these response messages even if they have not subscribed to them\.  
These response messages do not pass through the message broker and they cannot be subscribed to by other clients or rules\. To subscribe to job activity related messages, use the `notify` and `notify-next` topics\.  
For more information, see [Jobs device MQTT and HTTPS APIs](jobs-mqtt-api.md)\.


| Topic | Client operations allowed | Description | 
| --- | --- | --- | 
|  $aws/things/*thingName*/jobs/get  |  Publish  |  Devices publish a message to this topic to make a `GetPendingJobExecutions` request\. For more information, see [Jobs device MQTT and HTTPS APIs](jobs-mqtt-api.md)\.  | 
|  $aws/things/*thingName*/jobs/get/accepted  |  Subscribe, Receive  |  Devices subscribe to this topic to receive successful responses from a `GetPendingJobExecutions` request\. For more information, see [Jobs device MQTT and HTTPS APIs](jobs-mqtt-api.md)\.   | 
|  $aws/things/*thingName*/jobs/get/rejected  |  Subscribe, Receive  |  Devices subscribe to this topic when a `GetPendingJobExecutions` request is rejected\. For more information, see [Jobs device MQTT and HTTPS APIs](jobs-mqtt-api.md)\.  | 
|  $aws/things/*thingName*/jobs/start\-next  |  Publish  |  Devices publish a message to this topic to make a `StartNextPendingJobExecution` request\. For more information, see [Jobs device MQTT and HTTPS APIs](jobs-mqtt-api.md)\.  | 
|  $aws/things/*thingName*/jobs/start\-next/accepted  |  Subscribe, Receive  |  Devices subscribe to this topic to receive successful responses to a `StartNextPendingJobExecution` request\. For more information, see [Jobs device MQTT and HTTPS APIs](jobs-mqtt-api.md)\.  | 
|  $aws/things/*thingName*/jobs/start\-next/rejected  |  Subscribe, Receive  |  Devices subscribe to this topic when a `StartNextPendingJobExecution` request is rejected\. For more information, see [Jobs device MQTT and HTTPS APIs](jobs-mqtt-api.md)\.  | 
|  $aws/things/*thingName*/jobs/*jobId*/get  |  Publish  |  Devices publish a message to this topic to make a `DescribeJobExecution` request\. For more information, see [Jobs device MQTT and HTTPS APIs](jobs-mqtt-api.md)\.  | 
|  $aws/things/*thingName*/jobs/*jobId*/get/accepted  |  Subscribe, Receive  |  Devices subscribe to this topic to receive successful responses to a `DescribeJobExecution` request\. For more information, see [Jobs device MQTT and HTTPS APIs](jobs-mqtt-api.md)\.  | 
|  $aws/things/*thingName*/jobs/*jobId*/get/rejected  |  Subscribe, Receive  |  Devices subscribe to this topic when a `DescribeJobExecution` request is rejected\. For more information, see [Jobs device MQTT and HTTPS APIs](jobs-mqtt-api.md)\.  | 
|  $aws/things/*thingName*/jobs/*jobId*/update  |  Publish  |  Devices publish a message to this topic to make an `UpdateJobExecution` request\. For more information, see [Jobs device MQTT and HTTPS APIs](jobs-mqtt-api.md)\.  | 
|  $aws/things/*thingName*/jobs/*jobId*/update/accepted  |  Subscribe, Receive  |  Devices subscribe to this topic to receive successful responses to an `UpdateJobExecution` request\. For more information, see [Jobs device MQTT and HTTPS APIs](jobs-mqtt-api.md)\.  Note Only the device that publishes to $aws/things/*thingName*/jobs/*jobId*/update receives messages on this topic\.   | 
|  $aws/things/*thingName*/jobs/*jobId*/update/rejected  |  Subscribe, Receive  |  Devices subscribe to this topic when an `UpdateJobExecution` request is rejected\. For more information, see [Jobs device MQTT and HTTPS APIs](jobs-mqtt-api.md)\.  Note Only the device that publishes to $aws/things/*thingName*/jobs/*jobId*/update receives messages on this topic\.   | 
|  $aws/things/*thingName*/jobs/notify  |  Subscribe  |  Devices subscribe to this topic to receive notifications when a job execution is added or removed to the list of pending executions for a thing\. For more information, see [Jobs device MQTT and HTTPS APIs](jobs-mqtt-api.md)\.  | 
|  $aws/things/*thingName*/jobs/notify\-next  |  Subscribe  |  Devices subscribe to this topic to receive notifications when the next pending job execution for the thing is changed\. For more information, see [Jobs device MQTT and HTTPS APIs](jobs-mqtt-api.md)\.  | 
|  $aws/events/job/*jobId*/completed  |  Subscribe  |  The Jobs service publishes an event on this topic when a job completes\. For more information, see [Jobs events](events-jobs.md)\.  | 
|  $aws/events/job/*jobId*/canceled  |  Subscribe  |  The Jobs service publishes an event on this topic when a job is canceled\. For more information, see [Jobs events](events-jobs.md)\.  | 
|  $aws/events/job/*jobId*/deleted   |  Subscribe  |  The Jobs service publishes an event on this topic when a job is deleted\. For more information, see [Jobs events](events-jobs.md)\.  | 
|  $aws/events/job/*jobId*/cancellation\_in\_progress   |  Subscribe  |  The Jobs service publishes an event on this topic when a job cancellation begins\. For more information, see [Jobs events](events-jobs.md)\.  | 
|  $aws/events/job/*jobId*/deletion\_in\_progress   |  Subscribe  |  The Jobs service publishes an event on this topic when a job deletion begins\. For more information, see [Jobs events](events-jobs.md)\.   | 
|  $aws/events/jobExecution/*jobId*/succeeded   |  Subscribe  |  The Jobs service publishes an event on this topic when job execution succeeds\. For more information, see [Jobs events](events-jobs.md)\.   | 
|  $aws/events/jobExecution/*jobId*/failed   |  Subscribe  |  The Jobs service publishes an event on this topic when a job execution fails\. For more information, see [Jobs events](events-jobs.md)\.   | 
|  $aws/events/jobExecution/*jobId*/rejected   |  Subscribe  |  The Jobs service publishes an event on this topic when a job execution is rejected\. For more information, see [Jobs events](events-jobs.md)\.   | 
|  $aws/events/jobExecution/*jobId*/canceled   |  Subscribe  |  The Jobs service publishes an event on this topic when a job execution is canceled\. For more information, see [Jobs events](events-jobs.md)\.   | 
|  $aws/events/jobExecution/*jobId*/timed\_out   |  Subscribe  |  The Jobs service publishes an event on this topic when a job execution times out\. For more information, see [Jobs events](events-jobs.md)\.   | 
|  $aws/events/jobExecution/*jobId*/removed   |  Subscribe  |  The Jobs service publishes an event on this topic when a job execution is removed\. For more information, see [Jobs events](events-jobs.md)\.   | 
|  $aws/events/jobExecution/*jobId*/deleted   |  Subscribe  |  The Jobs service publishes an event on this topic when a job execution is deleted\. For more information, see [Jobs events](events-jobs.md)\.   | 

## Rule topics<a name="reserved-topics-rule"></a>


| Topic | Client operations allowed | Description | 
| --- | --- | --- | 
|  $aws/rules/*ruleName*  |  Publish  |  A device or an application publishes to this topic to trigger rules directly\. For more information, see [Reducing messaging costs with basic ingest](iot-basic-ingest.md)\.   | 

## Secure tunneling topics<a name="reserved-topics-secure"></a>


| Topic | Client operations allowed | Description | 
| --- | --- | --- | 
|  $aws/things/*thing\-name*/tunnels/notify  |  Subscribe  |   AWS IoT publishes this message for an IoT agent to start a local proxy on the remote device\. For more information, see [IoT agent snippet](agent-snippet.md)\.   | 

## Shadow topics<a name="reserved-topics-shadow"></a>

The topics in this section are used by named and unnamed shadows\. The topics used by each differ only in the topic prefix\. This table shows the topic prefix used by each shadow type\.


| *ShadowTopicPrefix* value | Shadow type | 
| --- | --- | 
| $aws/things/thingName/shadow | Unnamed \(classic\) shadow | 
| $aws/things/thingName/shadow/name/shadowName | Named shadow | 

To create a complete topic, select the *ShadowTopicPrefix* for the type of shadow to which you want to refer, replace *thingName* and if applicable, *shadowName*, with their corresponding values, and then append that with the topic stub as shown in the following table\. Remember that topics are case sensitive\.


| Topic | Client operations allowed | Description | 
| --- | --- | --- | 
|  *ShadowTopicPrefix*/delete  |  Publish/Subscribe  |  A device or an application publishes to this topic to delete a shadow\. For more information, see [/delete](device-shadow-mqtt.md#delete-pub-sub-topic)\.  | 
|  *ShadowTopicPrefix*/delete/accepted  |  Subscribe  |  The Device Shadow service sends messages to this topic when a shadow is deleted\. For more information, see [/delete/accepted](device-shadow-mqtt.md#delete-accepted-pub-sub-topic)\.  | 
|  *ShadowTopicPrefix*/delete/rejected  |  Subscribe  |  The Device Shadow service sends messages to this topic when a request to delete a shadow is rejected\. For more information, see [/delete/rejected](device-shadow-mqtt.md#delete-rejected-pub-sub-topic)\.  | 
|  *ShadowTopicPrefix*/get  |  Publish/Subscribe  |  An application or a thing publishes an empty message to this topic to get a shadow\. For more information, see [Device Shadow MQTT topics](device-shadow-mqtt.md)\.  | 
|  *ShadowTopicPrefix*/get/accepted  |  Subscribe  |  The Device Shadow service sends messages to this topic when a request for a shadow is made successfully\. For more information, see [/get/accepted](device-shadow-mqtt.md#get-accepted-pub-sub-topic)\.  | 
|  *ShadowTopicPrefix*/get/rejected  |  Subscribe  |  The Device Shadow service sends messages to this topic when a request for a shadow is rejected\. For more information, see [/get/rejected](device-shadow-mqtt.md#get-rejected-pub-sub-topic)\.  | 
|  *ShadowTopicPrefix*/update  |  Publish/Subscribe  |  A thing or application publishes to this topic to update a shadow\. For more information, see [/update](device-shadow-mqtt.md#update-pub-sub-topic)\.  | 
|  *ShadowTopicPrefix*/update/accepted  |  Subscribe  |  The Device Shadow service sends messages to this topic when an update is successfully made to a shadow\. For more information, see [/update/accepted](device-shadow-mqtt.md#update-accepted-pub-sub-topic)\.  | 
|  *ShadowTopicPrefix*/update/rejected  |  Subscribe  |  The Device Shadow service sends messages to this topic when an update to a shadow is rejected\. For more information, see [/update/rejected](device-shadow-mqtt.md#update-rejected-pub-sub-topic)\.  | 
|  *ShadowTopicPrefix*/update/delta  |  Subscribe  |  The Device Shadow service sends messages to this topic when a difference is detected between the reported and desired sections of a shadow\. For more information, see [/update/delta](device-shadow-mqtt.md#update-delta-pub-sub-topic)\.   | 
|  *ShadowTopicPrefix*/update/documents  |  Subscribe  |  AWS IoT publishes a state document to this topic whenever an update to the shadow is successfully performed\. For more information, see [/update/documents](device-shadow-mqtt.md#update-documents-pub-sub-topic)\.   | 

## Streaming service topics<a name="reserved-topics-streaming"></a>

These messages support response buffers in Concise Binary Object Representation \(CBOR\) format and JavaScript Object Notation \(JSON\), depending on the *payload\-format* of the topic\.


| *payload\-format* | Response format data type | 
| --- | --- | 
| cbor | Concise Binary Object Representation \(CBOR\) | 
| json | JavaScript Object Notation \(JSON\) | 


| Topic | Client operations allowed | Description | 
| --- | --- | --- | 
|  $aws/things/*ThingName*/streams/*StreamId*/data/*payload\-format*  |  Subscribe  |  The AWS Streaming service publishes to this topic if the "GetStream" request from a device is accepted\. The payload contains the stream data\. For more information, see [Using the AWS IoT Streaming service in devices](streaming-service-in-devices.md)\.   | 
|  $aws/things/*ThingName*/streams/*StreamId*/get/*payload\-format*  |  Publish  |  A device publishes to this topic to perform a "GetStream" request\. For more information, see [Using the AWS IoT Streaming service in devices](streaming-service-in-devices.md)\.   | 
|  $aws/things/*ThingName*/streams/*StreamId*/description/*payload\-format*  |  Subscribe  |  The AWS Streaming service publishes to this topic if the "DescribeStream" request from a device is accepted\. The payload contains the stream description\. For more information, see [Using the AWS IoT Streaming service in devices](streaming-service-in-devices.md)\.   | 
|  $aws/things/*ThingName*/streams/*StreamId*/describe/*payload\-format*  |  Publish  |  A device publishes to this topic to perform a "DescribeStream" request\. For more information, see [Using the AWS IoT Streaming service in devices](streaming-service-in-devices.md)\.   | 
|  $aws/things/*ThingName*/streams/*StreamId*/rejected/*payload\-format*  |  Subscribe  |  The AWS Streaming service publishes to this topic if a "DescribeStream" or "GetStream" request from a device is rejected\. For more information, see [Using the AWS IoT Streaming service in devices](streaming-service-in-devices.md)\.   | 

## Reserved topic ARN<a name="reserved-topicnames-arn"></a>

All reserved topic ARNs \(Amazon Resource Names\) have the following form:

```
arn:aws:iot:aws-region:AWS-account-ID:topic/Topic
```

For example, `arn:aws:iot:us-west-2:123EXAMPLE456:topic/$aws/things/thingName/jobs/get/accepted` is an ARN for the reserved topic `$aws/things/thingName/jobs/get/accepted`\.