# Using shadows in devices<a name="device-shadow-comms-device"></a>

This section describes device communications with shadows using MQTT messages, the preferred method for devices to communicate with the AWS IoT Device Shadow service\.

Shadow communications emulate a request/response model using the publish/subscribe communication model of MQTT\. Every shadow action consists of a request topic, a successful response topic \(`accepted`\), and an error response topic \(`rejected`\)\. 

If you want apps and services to be able to determine whether a device is connected, see [Detecting a device is connected](device-shadow-comms-app.md#thing-connection)\.

**Important**  
Because MQTT uses a publish/subscribe communication model, you must subscribe to the response topics *before* publishing a request topic\. If you don't, you won't receive the response to the request that you publish\.

The examples in this section use an abbreviated form of the topic where the *ShadowTopicPrefix* can refer to either a named or an unnamed shadow, as described in this table\. `ShadowTopicPrefix` can refer to either a named or an unnamed shadow as described in this table:

Shadows can be named or unnamed \(classic\)\. The topics used by each differ only in the topic prefix\. This table shows the topic prefix used by each shadow type\.


| *ShadowTopicPrefix* value | Shadow type | 
| --- | --- | 
| $aws/things/thingName/shadow | Unnamed \(classic\) shadow | 
| $aws/things/thingName/shadow/name/shadowName | Named shadow | 

To create a complete topic, select the *ShadowTopicPrefix* for the type of shadow to which you want to refer, replace *thingName*, and *shadowName* if applicable, with their corresponding values, and then append that with the topic stub as shown in the following table\. Topic names are case sensitive\.

For more information about the reserved topics for shadows, see [Shadow topics](reserved-topics.md#reserved-topics-shadow)\.

**Important**  
Make sure that your app's or service's use of the shadows is consistent and supported by the corresponding implementations in your devices\. For example, consider how shadows are created, updated, and deleted\. Also consider how updates are handled in the device and the apps or services that access the device through a shadow\. Your design should be clear about how the device's state is updated and reported and how your apps and services interact with the device and its shadows\.

To create a complete topic, select the `ShadowTopicPrefix` for the type of shadow to which you want to refer, replace `thingName`, and `shadowName` if applicable, with their corresponding values, and then append that with the topic stub as shown in the following table\. Remember that topics are case sensitive\.

See [Shadow topics](reserved-topics.md#reserved-topics-shadow) for more information about the reserved topics for shadows\.

**Important**  
You should make sure that your app's or service's use of the shadows is consistent and supported by the corresponding implementations in your devices\. You should consider, for example, how shadows are created, updated, and deleted and how updates are handled in the device and the apps or services that access it through a shadow\. Your design should be clear about how the device's state is updated and reported and how your apps and services interact with the device and its shadows\.

## Initializing the device on first connection to AWS IoT<a name="device-shadow-comms-device-first-connect"></a>

After a device registers with AWS IoT, it should subscribe to these MQTT messages for the shadows that it supports\.


| Topic | Meaning | Action a device should take when this topic is received | 
| --- | --- | --- | 
|  `ShadowTopicPrefix/delete/accepted`  |  The `delete` request was accepted and AWS IoT deleted the shadow\.   |  The actions necessary to accommodate the deleted shadow, such as stop publishing updates\.  | 
|  `ShadowTopicPrefix/delete/rejected`  |  The `delete` request was rejected by AWS IoT and the shadow was not deleted\. The message body contains the error information\.   |  Respond to the error message in the message body\.  | 
|  `ShadowTopicPrefix/get/accepted`  |  The `get` request was accepted by AWS IoT, and the message body contains the current shadow document\.   |  The actions necessary to process the state document in the message body\.  | 
|  `ShadowTopicPrefix/get/rejected`  |  The `get` request was rejected by AWS IoT, and the message body contains the error information\.   | Respond to the error message in the message body\. | 
|  `ShadowTopicPrefix/update/accepted`  |  The `update` request was accepted by AWS IoT, and the message body contains the current shadow document\.   |  Confirm the updated data in the message body matches the device state\.  | 
|  `ShadowTopicPrefix/update/rejected`  |  The `update` request was rejected by AWS IoT, and the message body contains the error information\.   | Respond to the error message in the message body\. | 
|  `ShadowTopicPrefix/update/delta`  |  The shadow document was updated by a request to AWS IoT, and the message body contains the changes requested\.   |  Update the device's state to match the desired state in the message body\.  | 
|  `ShadowTopicPrefix/update/documents`  |  An update to the shadow was recently completed, and the message body contains the current shadow document\.   |  Confirm the updated state in the message body matches the device's state\.  | 

After subscribing to the messages in the preceding table for each shadow, the device should test to see if the shadows that it supports have already been created by publishing a `/get` topic to each shadow\. If a `/get/accepted` message is received, the message body contains the shadow document, which the device can use to initialize its state\. If a `/get/rejected` message is received, the shadow should be created by publishing an `/update` message with the current device state\.

## Processing messages while the device is connected to AWS IoT<a name="device-shadow-comms-device-while-connected"></a>

While a device is connected to AWS IoT, it can receive **/update/delta** messages and should keep the device state matched to the changes in its shadows by:

1. Reading all **/update/delta** messages received and synchronizing the device state to match\.

1. Publishing an **/update** message with a `reported` message body that has the device’s current state, whenever the device's state changes\.

While a device is connected, it should publish these messages when indicated\.


| Indication | Topic | Payload | 
| --- | --- | --- | 
| The device's state has changed\. |  `ShadowTopicPrefix/update`  | A shadow document with the `reported` property\. | 
| The device might not be synchronized with the shadow\. | `ShadowTopicPrefix/get` | \(empty\) | 
| An action on the device indicates that a shadow will no longer be supported by the device, such as when the device is being remove or replaced\. | `ShadowTopicPrefix/delete` | When the device's state has changed | 
| The device might not be synchronized with the shadow | `ShadowTopicPrefix/get` | \(empty\) | 
| An action on the device indicates that a shadow will no longer be supported by the device, such as when the device is being remove or replaced | `ShadowTopicPrefix/delete` | \(empty\) | 

## Processing messages when the device reconnects to AWS IoT<a name="device-shadow-comms-device-reconnect"></a>

When a device with one or more shadows connects to AWS IoT, it should synchronize its state with that of all the shadows that it supports by:

1. Reading all **/update/delta** messages received and synchronizing the device state to match\.

1. Publishing an **/update** message with a `reported` message body that has the device’s current state\.