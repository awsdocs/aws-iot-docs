# Reserved topics<a name="reserved-topics"></a>

Except for the topics listed here, any topic that begins with $ is considered reserved and is not supported for publishing or subscribing\. Attempt to publish or subscribe to a topic that begins with $ results in a terminated connection\.


**Event topics**  

| Topic | Allowed operations | Description | 
| --- | --- | --- | 
|  $aws/events/presence/connected/*clientId*  |  Subscribe  |  AWS IoT publishes to this topic when an MQTT client with the specified client ID connects to AWS IoT\. For more information, see [Connect/Disconnect events](life-cycle-events.md#connect-disconnect)\.  | 
|  $aws/events/presence/disconnected/*clientId*  |  Subscribe  |  AWS IoT publishes to this topic when an MQTT client with the specified client ID disconnects to AWS IoT\. For more information, see [Connect/Disconnect events](life-cycle-events.md#connect-disconnect)\.   | 
|  $aws/events/subscriptions/subscribed/*clientId*  |  Subscribe  |  AWS IoT publishes to this topic when an MQTT client with the specified client ID subscribes to an MQTT topic\. For more information, see [Subscribe/Unsubscribe events](life-cycle-events.md#subscribe-unsubscribe-events)\.  | 
|  $aws/events/subscriptions/unsubscribed/*clientId*  |  Subscribe  |  AWS IoT publishes to this topic when an MQTT client with the specified client ID unsubscribes to an MQTT topic\. For more information, see [Subscribe/Unsubscribe events](life-cycle-events.md#subscribe-unsubscribe-events)\.  | 


**Rule topics**  

| Topic | Allowed operations | Description | 
| --- | --- | --- | 
|  $aws/rules/*ruleName*  |  Publish  |  A device or an application publishes to this topic to trigger rules directly\. For more information, see [Reducing messaging costs with basic ingest](iot-basic-ingest.md)\.   | 


**Thing shadow topics**  

| Topic | Allowed operations | Description | 
| --- | --- | --- | 
|  $aws/things/*<thingName>*/shadow/delete  |  Publish/Subscribe  |  A device or an application publishes to this topic to delete a shadow\. For more information, see [/delete](device-shadow-mqtt.md#delete-pub-sub-topic)\.  | 
|  $aws/things/*<thingName>*/shadow/delete/accepted  |  Subscribe  |  The Device Shadow service sends messages to this topic when a shadow is deleted\. For more information, see [/delete/accepted](device-shadow-mqtt.md#delete-accepted-pub-sub-topic)\.  | 
|  $aws/things/*<thingName>*/shadow/delete/rejected  |  Subscribe  |  The Device Shadow service sends messages to this topic when a request to delete a shadow is rejected\. For more information, see [/delete/rejected](device-shadow-mqtt.md#delete-rejected-pub-sub-topic)\.  | 
|  $aws/things/*<thingName>*/shadow/get  |  Publish/Subscribe  |  An application or a thing publishes an empty message to this topic to get a shadow\. For more information, see [Shadow MQTT topics](device-shadow-mqtt.md)\.  | 
|  $aws/things/*<thingName>*/shadow/get/accepted  |  Subscribe  |  The Device Shadow service sends messages to this topic when a request for a shadow is made successfully\. For more information, see [/get/accepted](device-shadow-mqtt.md#get-accepted-pub-sub-topic)\.  | 
|  $aws/things/*<thingName>*/shadow/get/rejected  |  Subscribe  |  The Device Shadow service sends messages to this topic when a request for a shadow is rejected\. For more information, see [/get/rejected](device-shadow-mqtt.md#get-rejected-pub-sub-topic)\.  | 
|  $aws/things/*<thingName>*/shadow/update  |  Publish/Subscribe  |  A thing or application publishes to this topic to update a shadow\. For more information, see [/update](device-shadow-mqtt.md#update-pub-sub-topic)\.  | 
|  $aws/things/*<thingName>*/shadow/update/accepted  |  Subscribe  |  The Device Shadow service sends messages to this topic when an update is successfully made to a shadow\. For more information, see [/update/accepted](device-shadow-mqtt.md#update-accepted-pub-sub-topic)\.  | 
|  $aws/things/*<thingName>*/shadow/update/rejected  |  Subscribe  |  The Device Shadow service sends messages to this topic when an update to a shadow is rejected\. For more information, see [/update/rejected](device-shadow-mqtt.md#update-rejected-pub-sub-topic)\.  | 
|  $aws/things/*<thingName>*/shadow/update/delta  |  Subscribe  |  The Device Shadow service sends messages to this topic when a difference is detected between the reported and desired sections of a shadow\. For more information, see [/update/delta](device-shadow-mqtt.md#update-delta-pub-sub-topic)\.   | 
|  $aws/things/*<thingName>*/shadow/update/documents  |  Subscribe  |  AWS IoT publishes a state document to this topic whenever an update to the shadow is successfully performed\. For more information, see [/update/documents](device-shadow-mqtt.md#update-documents-pub-sub-topic)\.   | 


**Job topics**  

| Topic | Allowed operations | Description | 
| --- | --- | --- | 
|  $aws/things/*<thingName>*/jobs/get  |  Publish  |  Devices publish a message to this topic to make a `GetPendingJobExecutions` request\. For more information, see [Using the AWS IoT jobs APIs](jobs-api.md)\.  | 
|  $aws/things/*<thingName>*/jobs/get/accepted  |  Subscribe  |  Devices subscribe to this topic to receive successful responses from a `GetPendingJobExecutions` request\. For more information, see [Using the AWS IoT jobs APIs](jobs-api.md)\.  | 
|  $aws/things/*<thingName>*/jobs/get/rejected  |  Subscribe  |  Devices subscribe to this topic when a `GetPendingJobExecutions` request is rejected\. For more information, see [Using the AWS IoT jobs APIs](jobs-api.md)\.  | 
|  $aws/things/*<thingName>*/jobs/start\-next  |  Publish  |  Devices publish a message to this topic to make a `StartNextPendingJobExecution` request\. For more information, see [Using the AWS IoT jobs APIs](jobs-api.md)\.  | 
|  $aws/things/*<thingName>*/jobs/start\-next/accepted  |  Subscribe  |  Devices subscribe to this topic to receive successful responses to a `StartNextPendingJobExecution` request\. For more information, see [Using the AWS IoT jobs APIs](jobs-api.md)\.  | 
|  $aws/things/*<thingName>*/jobs/start\-next/rejected  |  Subscribe  |  Devices subscribe to this topic when a `StartNextPendingJobExecution` request is rejected\. For more information, see [Using the AWS IoT jobs APIs](jobs-api.md)\.  | 
|  $aws/things/*<thingName>*/jobs/*<jobId>*/get  |  Publish  |  Devices publish a message to this topic to make a `DescribeJobExecution` request\. For more information, see [Using the AWS IoT jobs APIs](jobs-api.md)\.  | 
|  $aws/things/*<thingName>*/jobs/*<jobId>*/get/accepted  |  Subscribe  |  Devices subscribe to this topic to receive successful responses to a `DescribeJobExecution` request\. For more information, see [Using the AWS IoT jobs APIs](jobs-api.md)\.  | 
|  $aws/things/*<thingName>*/jobs/*<jobId>*/get/rejected  |  Subscribe  |  Devices subscribe to this topic when a `DescribeJobExecution` request is rejected\. For more information, see [Using the AWS IoT jobs APIs](jobs-api.md)\.  | 
|  $aws/things/*<thingName>*/jobs/*<jobId>*/update  |  Publish  |  Devices publish a message to this topic to make an `UpdateJobExecution` request\. For more information, see [Using the AWS IoT jobs APIs](jobs-api.md)\.  | 
|  $aws/things/*<thingName>*/jobs/*<jobId>*/update/accepted  |  Subscribe  |  Devices subscribe to this topic to receive successful responses to an `UpdateJobExecution` request\. For more information, see [Using the AWS IoT jobs APIs](jobs-api.md)\.  Note Only the device that publishes to $aws/things/*<thingName>*/jobs/*<jobId>*/update will receive messages on this topic\.   | 
|  $aws/things/*<thingName>*/jobs/*<jobId>*/update/rejected  |  Subscribe  |  Devices subscribe to this topic when an `UpdateJobExecution` request is rejected\. For more information, see [Using the AWS IoT jobs APIs](jobs-api.md)\.  Note Only the device that publishes to $aws/things/*<thingName>*/jobs/*<jobId>*/update will receive messages on this topic\.   | 
|  $aws/things/*<thingName>*/jobs/notify  |  Subscribe  |  Devices subscribe to this topic to receive notifications when a job execution is added or removed to the list of pending executions for a thing\. For more information, see [Using the AWS IoT jobs APIs](jobs-api.md)\.  | 
|  $aws/things/*<thingName>*/jobs/notify\-next  |  Subscribe  |  Devices subscribe to this topic to receive notifications when the next pending job execution for the thing is changed\. For more information, see [Using the AWS IoT jobs APIs](jobs-api.md)\.  | 
|  $aws/events/job/*<jobId>*/completed  |  Subscribe  |  The Jobs service publishes an event on this topic when a job completes\. For more information, see [](events-jobs.md)\.  | 
|  $aws/events/job/*<jobId>*/canceled  |  Subscribe  |  The Jobs service publishes an event on this topic when a job is canceled\. For more information, see [](events-jobs.md)\.  | 
|  $aws/events/job/*<jobId>*/deleted   |  Subscribe  |  The Jobs service publishes an event on this topic when a job is deleted\. For more information, see [](events-jobs.md)\.  | 
|  $aws/events/job/*<jobId>*/cancellation\_in\_progress   |  Subscribe  |  The Jobs service publishes an event on this topic when a job cancellation begins\. For more information, see [](events-jobs.md)\.  | 
|  $aws/events/job/*<jobId>*/deletion\_in\_progress   |  Subscribe  |  The Jobs service publishes an event on this topic when a job deletion begins\. For more information, see [](events-jobs.md)\.   | 
|  $aws/events/jobExecution/*<jobId>*/succeeded   |  Subscribe  |  The Jobs service publishes an event on this topic when job execution succeeds\. For more information, see [](events-jobs.md)\.   | 
|  $aws/events/jobExecution/*<jobId>*/failed   |  Subscribe  |  The Jobs service publishes an event on this topic when a job execution fails\. For more information, see [](events-jobs.md)\.   | 
|  $aws/events/jobExecution/*<jobId>*/rejected   |  Subscribe  |  The Jobs service publishes an event on this topic when a job execution is rejected\. For more information, see [](events-jobs.md)\.   | 
|  $aws/events/jobExecution/*<jobId>*/canceled   |  Subscribe  |  The Jobs service publishes an event on this topic when a job execution is canceled\. For more information, see [](events-jobs.md)\.   | 
|  $aws/events/jobExecution/*<jobId>*/timed\_out   |  Subscribe  |  The Jobs service publishes an event on this topic when a job execution times out\. For more information, see [](events-jobs.md)\.   | 
|  $aws/events/jobExecution/*<jobId>*/removed   |  Subscribe  |  The Jobs service publishes an event on this topic when a job execution is removed\. For more information, see [](events-jobs.md)\.   | 
|  $aws/events/jobExecution/*<jobId>*/deleted   |  Subscribe  |  The Jobs service publishes an event on this topic when a job execution is deleted\. For more information, see [](events-jobs.md)\.   | 


**Fleet provisioning topics**  

| Topic | Allowed operations | Description | 
| --- | --- | --- | 
|  $aws/events/presence/connected/*<clientId>*  |  Subscribe  |  AWS IoT publishes to this topic when an MQTT client with the specified client ID connects to AWS IoT\. For more information, see [Connect/Disconnect events](life-cycle-events.md#connect-disconnect)  | 
| $aws/certificates/create/cbor | Publish | You publish to this topic to call the CreateKeysAndCertificate MQTT API\. | 
| $aws/certificates/create/json | Publish | You publish to this topic to call the CreateKeysAndCertificate MQTT API\. | 
| $aws/provisioning\-templates/<templateName>/provision/cbor | Publish | You publish to this topic to call the RegisterThing MQTT API\. | 
| $aws/provisioning\-templates/<templateName>/provision/json | Publish | You publish to this topic to call the RegisterThing MQTT API\. | 
| $aws/certificates/create/cbor/accepted | Subscribe | AWS IoT publishes to this topic when a call is successfully made to the CreateKeysAndCertificate MQTT API\. | 
| $aws/certificates/create/cbor/rejected | Subscribe | AWS IoT publishes to this topic when a call to the CreateKeysAndCertificate MQTT API fails\. | 
| $aws/certificates/create/json/accepted | Subscribe | AWS IoT publishes to this topic when a call is made successfully to the CreateKeysAndCertificate MQTT API\. | 
| $aws/certificates/create/json/rejected | Subscribe | AWS IoT publishes to this topic when a call to the CreateKeysAndCertificate MQTT API fails\. | 
| $aws/provisioning\-templates/<templateName>/provision/cbor/accepted | Subscribe | AWS IoT publishes to this topic when a call is made successfully to the RegisterThing MQTT API\. | 
| $aws/provisioning\-templates/<templateName>/provision/cbor/rejected | Subscribe | AWS IoT publishes to this topic when a call to the RegisterThing MQTT API fails\. | 
| $aws/provisioning\- templates/<templateName>/provision/json/accepted | Subscribe | AWS IoT publishes to this topic when a call is made successfully to the RegisterThing MQTT API\. | 
| $aws/provisioning\- templates/<templateName>/provision/json/rejected | Subscribe | AWS IoT publishes to this topic when a call to the RegisterThing MQTT API fails\. | 