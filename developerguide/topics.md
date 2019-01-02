# Topics<a name="topics"></a>

The message broker uses topics to route messages from publishing clients to subscribing clients\. The forward slash \(/\) is used to separate topic hierarchy\. The following table lists the wildcards that can be used in the topic filter when you subscribe\. 


**Topic Wildcards**  

| Wildcard | Description | 
| --- | --- | 
| \# |  Must be the last character in the topic to which you are subscribing\. Works as a wildcard by matching the current tree and all subtrees\. For example, a subscription to `Sensor/#` will receive messages published to `Sensor/`, `Sensor/temp`, `Sensor/temp/room1`, but not the messages published to `Sensor`\.  | 
| \+ |  Matches exactly one item in the topic hierarchy\. For example, a subscription to `Sensor/+/room1` will receive messages published to `Sensor/temp/room1`, `Sensor/moisture/room1`, and so on\.   | 

## Reserved Topics<a name="reserved-topics"></a>

Any topics beginning with $ are considered reserved and are not supported for publishing and subscribing except for those topics listed below\. Any other attempts to publish or subscribe on topics beginning with $ will result in a terminated connection\.


****  

| Topic | Allowed Operations | Description | 
| --- | --- | --- | 
|  $aws/events/presence/connected/*clientId*  |  Subscribe  |  AWS IoT publishes to this topic when an MQTT client with the specified client ID connects to AWS IoT\. For more information, see [Connect/Disconnect Events](life-cycle-events.md#connect-disconnect)\.  | 
|  $aws/events/presence/disconnected/*clientId*  |  Subscribe  |  AWS IoT publishes to this topic when an MQTT client with the specified client ID disconnects to AWS IoT\. For more information, see [Connect/Disconnect Events](life-cycle-events.md#connect-disconnect)\.   | 
|  $aws/events/subscriptions/subscribed/*clientId*  |  Subscribe  |  AWS IoT publishes to this topic when an MQTT client with the specified client ID subscribes to an MQTT topic\. For more information, see [Subscribe/Unsubscribe Events](life-cycle-events.md#subscribe-unsubscribe-events)\.  | 
|  $aws/events/subscriptions/unsubscribed/*clientId*  |  Subscribe  |  AWS IoT publishes to this topic when an MQTT client with the specified client ID unsubscribes to an MQTT topic\. For more information, see [Subscribe/Unsubscribe Events](life-cycle-events.md#subscribe-unsubscribe-events)\.  | 
|  $aws/rules/*ruleName*  |  Publish  |  A device or an application publishes to this topic to trigger rules directly\. For more information see [Basic Ingest](iot-basic-ingest.md)\.   | 
|  $aws/things/*thingName*/shadow/delete  |  Publish/Subscribe  |  A device or an application publishes to this topic to delete a shadow\. For more information see [http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html#delete-pub-sub-topic](http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html#delete-pub-sub-topic)\.  | 
|  $aws/things/*thingName*/shadow/delete/accepted  |  Subscribe  |  The Device Shadow service sends messages to this topic when a shadow is deleted\. For more information, see [http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html#delete-accepted-pub-sub-topic](http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html#delete-accepted-pub-sub-topic)\.  | 
|  $aws/things/*thingName*/shadow/delete/rejected  |  Subscribe  |  The Device Shadow service sends messages to this topic when a request to delete a shadow is rejected\. For more information, see [http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html#delete-rejected-pub-sub-topic](http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html#delete-rejected-pub-sub-topic)\.  | 
|  $aws/things/*thingName*/shadow/get  |  Publish/Subscribe  |  An application or a thing publishes an empty message to this topic to get a shadow\. For more information, see [http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html](http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html)\.  | 
|  $aws/things/*thingName*/shadow/get/accepted  |  Subscribe  |  The Device Shadow service sends messages to this topic when a request for a shadow is made successfully\. For more information, see [http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html#get-accepted-pub-sub-topic](http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html#get-accepted-pub-sub-topic)\.  | 
|  $aws/things/*thingName*/shadow/get/rejected  |  Subscribe  |  The Device Shadow service sends messages to this topic when a request for a shadow is rejected\. For more information, see [http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html#get-rejected-pub-sub-topic](http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html#get-rejected-pub-sub-topic)\.  | 
|  $aws/things/*thingName*/shadow/update  |  Publish/Subscribe  |  A thing or application publishes to this topic to update a shadow\. For more information, see [http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html#update-pub-sub-topic](http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html#update-pub-sub-topic)\.  | 
|  $aws/things/*thingName*/shadow/update/accepted  |  Subscribe  |  The Device Shadow service sends messages to this topic when an update is successfully made to a shadow\. For more information, see [http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html#update-accepted-pub-sub-topic](http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html#update-accepted-pub-sub-topic)\.  | 
|  $aws/things/*thingName*/shadow/update/rejected  |  Subscribe  |  The Device Shadow service sends messages to this topic when an update to a shadow is rejected\. For more information, see [http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html#update-rejected-pub-sub-topic](http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html#update-rejected-pub-sub-topic)\.  | 
|  $aws/things/*thingName*/shadow/update/delta  |  Subscribe  |  The Device Shadow service sends messages to this topic when a difference is detected between the reported and desired sections of a shadow\. For more information, see [http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html#update-delta-pub-sub-topic](http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html#update-delta-pub-sub-topic)\.   | 
|  $aws/things/*thingName*/shadow/update/documents  |  Subscribe  |  AWS IoT publishes a state document to this topic whenever an update to the shadow is successfully performed\. For more information, see [http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html#update-documents-pub-sub-topic](http://alpha-docs-aws.amazon.com/iot/latest/developerguide//device-shadow-mqtt.html#update-documents-pub-sub-topic)\.   | 