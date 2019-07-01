# Topics<a name="topics"></a>

The message broker uses topics to route messages from publishing clients to subscribing clients\. The forward slash \(/\) is used to separate topic hierarchy\.

**Note**  
We do not recommend using personally identifiable information in your topics\.

The following table lists the wildcards that can be used in the topic filter when you subscribe\. 


**Topic Wildcards**  

| Wildcard | Description | 
| --- | --- | 
| \# |  Must be the last character in the topic to which you are subscribing\. Works as a wildcard by matching the current tree and all subtrees\. For example, a subscription to `Sensor/#` receives messages published to `Sensor/`, `Sensor/temp`, `Sensor/temp/room1`, but not the messages published to `Sensor`\.  | 
| \+ |  Matches exactly one item in the topic hierarchy\. For example, a subscription to `Sensor/+/room1` receives messages published to `Sensor/temp/room1`, `Sensor/moisture/room1`, and so on\.   | 

## Reserved Topics<a name="reserved-topics"></a>

Except for those topics listed here, any topics beginning with $ are considered reserved and are not supported for publishing and subscribing\. Any attempts to publish or subscribe to topics beginning with $ result in a terminated connection\.


**Event Topics**  

| Topic | Allowed Operations | Description | 
| --- | --- | --- | 
|  $aws/events/presence/connected/*clientId*  |  Subscribe  |  AWS IoT publishes to this topic when an MQTT client with the specified client ID connects to AWS IoT\. For more information, see [Connect/Disconnect Events](life-cycle-events.md#connect-disconnect)\.  | 
|  $aws/events/presence/disconnected/*clientId*  |  Subscribe  |  AWS IoT publishes to this topic when an MQTT client with the specified client ID disconnects to AWS IoT\. For more information, see [Connect/Disconnect Events](life-cycle-events.md#connect-disconnect)\.   | 
|  $aws/events/subscriptions/subscribed/*clientId*  |  Subscribe  |  AWS IoT publishes to this topic when an MQTT client with the specified client ID subscribes to an MQTT topic\. For more information, see [Subscribe/Unsubscribe Events](life-cycle-events.md#subscribe-unsubscribe-events)\.  | 
|  $aws/events/subscriptions/unsubscribed/*clientId*  |  Subscribe  |  AWS IoT publishes to this topic when an MQTT client with the specified client ID unsubscribes to an MQTT topic\. For more information, see [Subscribe/Unsubscribe Events](life-cycle-events.md#subscribe-unsubscribe-events)\.  | 


**Rule Topics**  

| Topic | Allowed Operations | Description | 
| --- | --- | --- | 
|  $aws/rules/*ruleName*  |  Publish  |  A device or an application publishes to this topic to trigger rules directly\. For more information, see [Basic Ingest](iot-basic-ingest.md)\.   | 


**Thing Shadow Topics**  

| Topic | Allowed Operations | Description | 
| --- | --- | --- | 
|  $aws/things/*<thingName>*/shadow/delete  |  Publish/Subscribe  |  A device or an application publishes to this topic to delete a shadow\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#delete-pub-sub-topic](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#delete-pub-sub-topic)\.  | 
|  $aws/things/*<thingName>*/shadow/delete/accepted  |  Subscribe  |  The Device Shadow service sends messages to this topic when a shadow is deleted\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#delete-accepted-pub-sub-topic](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#delete-accepted-pub-sub-topic)\.  | 
|  $aws/things/*<thingName>*/shadow/delete/rejected  |  Subscribe  |  The Device Shadow service sends messages to this topic when a request to delete a shadow is rejected\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#delete-rejected-pub-sub-topic](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#delete-rejected-pub-sub-topic)\.  | 
|  $aws/things/*<thingName>*/shadow/get  |  Publish/Subscribe  |  An application or a thing publishes an empty message to this topic to get a shadow\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html)\.  | 
|  $aws/things/*<thingName>*/shadow/get/accepted  |  Subscribe  |  The Device Shadow service sends messages to this topic when a request for a shadow is made successfully\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#get-accepted-pub-sub-topic](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#get-accepted-pub-sub-topic)\.  | 
|  $aws/things/*<thingName>*/shadow/get/rejected  |  Subscribe  |  The Device Shadow service sends messages to this topic when a request for a shadow is rejected\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#get-rejected-pub-sub-topic](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#get-rejected-pub-sub-topic)\.  | 
|  $aws/things/*<thingName>*/shadow/update  |  Publish/Subscribe  |  A thing or application publishes to this topic to update a shadow\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#update-pub-sub-topic](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#update-pub-sub-topic)\.  | 
|  $aws/things/*<thingName>*/shadow/update/accepted  |  Subscribe  |  The Device Shadow service sends messages to this topic when an update is successfully made to a shadow\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#update-accepted-pub-sub-topic](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#update-accepted-pub-sub-topic)\.  | 
|  $aws/things/*<thingName>*/shadow/update/rejected  |  Subscribe  |  The Device Shadow service sends messages to this topic when an update to a shadow is rejected\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#update-rejected-pub-sub-topic](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#update-rejected-pub-sub-topic)\.  | 
|  $aws/things/*<thingName>*/shadow/update/delta  |  Subscribe  |  The Device Shadow service sends messages to this topic when a difference is detected between the reported and desired sections of a shadow\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#update-delta-pub-sub-topic](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#update-delta-pub-sub-topic)\.   | 
|  $aws/things/*<thingName>*/shadow/update/documents  |  Subscribe  |  AWS IoT publishes a state document to this topic whenever an update to the shadow is successfully performed\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#update-documents-pub-sub-topic](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#update-documents-pub-sub-topic)\.   | 


**Job Topics**  

| Topic | Allowed Operations | Description | 
| --- | --- | --- | 
|  $aws/things/*<thingName>*/jobs/get  |  Publish  |  Devices publish a message to this topic to make a `GetPendingJobExecutions` request\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html](https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html)\.  | 
|  $aws/things/*<thingName>*/jobs/get/accepted  |  Subscribe  |  Devices subscribe to this topic to receive successful responses from a `GetPendingJobExecutions` request\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html](https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html)\.  | 
|  $aws/things/*<thingName>*/jobs/get/accepted  |  Subscribe  |  Devices subscribe to this topic to receive successful responses to a `GetPendingJobExecutions` request\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html](https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html)\.  | 
|  $aws/things/*<thingName>*/jobs/start\-next  |  Publish  |  Devices publish a message to this topic to make a `StartNextPendingJobExecution` request\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html](https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html)\.  | 
|  $aws/things/*<thingName>*/jobs/start\-next/accepted  |  Subscribe  |  Devices subscribe to this topic to receive successful responses to a `StartNextPendingJobExecution` request\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html](https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html)\.  | 
|  $aws/things/*<thingName>*/jobs/start\-next/rejected  |  Subscribe  |  Devices subscribe to this topic to receive successful responses to a `StartNextPendingJobExecution`\. request\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html](https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html)\.  | 
|  $aws/things/*<thingName>*/jobs/jobId/get  |  Publish  |  Devices publish a message to this topic to make a `DescribeJobExecution` request\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html](https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html)\.  | 
|  $aws/things/*<thingName>*/jobs/*<jobId>*/get/accepted  |  Subscribe  |  Devices subscribe to this topic to receive successful responses to a `DescribeJobExecution` request\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html](https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html)\.  | 
|  $aws/things/*<thingName>*/jobs/*<jobId>*/get/rejected  |  Subscribe  |  Devices subscribe to this topic to receive successful responses to a `DescribeJobExecution` request\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html](https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html)\.  | 
|  $aws/things/*<thingName>*/jobs/*<jobId>*/update  |  Publish  |  Devices publish a message to this topic to make a `UpdateJobExecution` request\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html](https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html)\.  | 
|  $aws/things/*<thingName>*/jobs/*<jobId>*/update/accepted  |  Subscribe  |  Devices subscribe to this topic to receive successful responses to a `UpdateJobExecution` request\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html](https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html)\.  | 
|  $aws/things/*<thingName>*/jobs/*<jobId>*/update/rejected  |  Subscribe  |  Devices subscribe to this topic to receive successful responses to a `UpdateJobExecution` request\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html](https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html)\.  | 
|  $aws/things/*<thingName>*/jobs/notify  |  Subscribe  |  Devices subscribe to this topic to receive notifications when a job execution is added or removed to the list of pending executions for a thing\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html](https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html)\.  | 
|  $aws/things/*<thingName>*/jobs/notify\-next  |  Subscribe  |  Devices subscribe to this topic to receive notifications when the next pending job execution for the thing is changed\. For more information, see [https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html](https://docs.aws.amazon.com/iot/latest/developerguide/jobs-api.html)\.  | 
|  $aws/events/job/*<jobId>*/completed  |  Subscribe  |  The Jobs service publishes an event on this topic when a job completes\. For more information see: [Job Events](https://docs.aws.amazon.com/iot/latest/developerguide/events-jobs.html)\.  | 
|  $aws/events/job/*<jobId>*/canceled  |  Subscribe  |  The Jobs service publishes an event on this topic when a job is canceled\. For more information see: [Job Events](https://docs.aws.amazon.com/iot/latest/developerguide/events-jobs.html)\.  | 
|  $aws/events/job/*<jobId>*/deleted   |  Subscribe  |  The Jobs service publishes an event on this topic when a job is deleted\. For more information see: [Job Events](https://docs.aws.amazon.com/iot/latest/developerguide/events-jobs.html)\.  | 
|  $aws/events/job/*<jobId>*/cancellation\_in\_progress   |  Subscribe  |  The Jobs service publishes an event on this topic when a job cancellation begins\. For more information see: [Job Events](https://docs.aws.amazon.com/iot/latest/developerguide/events-jobs.html)\.  | 
|  $aws/events/job/*<jobId>*/deletion\_in\_progress   |  Subscribe  |  The Jobs service publishes an event on this topic when a job deletion begins\. For more information see: [Job Events](https://docs.aws.amazon.com/iot/latest/developerguide/events-jobs.html)\.   | 
|  $aws/events/jobExecution/*<jobId>*/succeeded   |  Subscribe  |  The Jobs service publishes an event on this topic when job execution succeeds\. For more information see: [Job Events](https://docs.aws.amazon.com/iot/latest/developerguide/events-jobs.html)\.   | 
|  $aws/events/jobExecution/*<jobId>*/failed   |  Subscribe  |  The Jobs service publishes an event on this topic when a job execution fails\. For more information see: [Job Events](https://docs.aws.amazon.com/iot/latest/developerguide/events-jobs.html)\.   | 
|  $aws/events/jobExecution/*<jobId>*/rejected   |  Subscribe  |  The Jobs service publishes an event on this topic when a job execution is rejected\. For more information see: [Job Events](https://docs.aws.amazon.com/iot/latest/developerguide/events-jobs.html)\.   | 
|  $aws/events/jobExecution/*<jobId>*/canceled   |  Subscribe  |  The Jobs service publishes an event on this topic when a job execution is canceled\. For more information see: [Job Events](https://docs.aws.amazon.com/iot/latest/developerguide/events-jobs.html)\.   | 
|  $aws/events/jobExecution/*<jobId>*/timed\_out   |  Subscribe  |  The Jobs service publishes an event on this topic when a job execution times out\. For more information see: [Job Events](https://docs.aws.amazon.com/iot/latest/developerguide/events-jobs.html)\.   | 
|  $aws/events/jobExecution/*<jobId>*/removed   |  Subscribe  |  The Jobs service publishes an event on this topic when a job execution is removed\. For more information see: [Job Events](https://docs.aws.amazon.com/iot/latest/developerguide/events-jobs.html)\.   | 
|  $aws/events/jobExecution/*<jobId>*/deleted   |  Subscribe  |  The Jobs service publishes an event on this topic when a job execution is deleted\. For more information see: [Job Events](https://docs.aws.amazon.com/iot/latest/developerguide/events-jobs.html)\.   | 