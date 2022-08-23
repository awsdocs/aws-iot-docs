# MQTT<a name="mqtt"></a>

[MQTT](http://mqtt.org/) is a lightweight and widely adopted messaging protocol that is designed for constrained devices\. AWS IoT support for MQTT is based on the [MQTT v3\.1\.1 specification](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html), with some differences\. For information about how AWS IoT differs from the MQTT v3\.1\.1 specification, see [AWS IoT differences from MQTT version 3\.1\.1 specification](#mqtt-differences)\.

 AWS IoT Core supports device connections that use the MQTT protocol and MQTT over WSS protocol and that are identified by a *client ID*\. The [AWS IoT Device SDKs](iot-connect-devices.md#iot-connect-device-sdks) support both protocols and are the recommended ways to connect devices to AWS IoT\. The AWS IoT Device SDKs support the functions necessary for devices and clients to connect to and access AWS IoT Core services\. The Device SDKs support the authentication protocols that the AWS IoT services require and the connection ID requirements that the MQTT protocol and MQTT over WSS protocols require\. For information about how to connect to AWS IoT using the AWS Device SDKs and links to examples of AWS IoT in the supported languages, see [Connecting with MQTT using the AWS IoT Device SDKs](#mqtt-sdk)\. For more information about authentication methods and the port mappings for MQTT messages, see [Protocols, port mappings, and authentication](protocols.md#protocol-port-mapping)\.

While we recommend using the AWS IoT Device SDKs to connect to AWS IoT, they are not required\. If you do not use the AWS IoT Device SDKs, however, you must provide the necessary connection and communication security\. Clients must send the [Server Name Indication \(SNI\) TLS extension](https://tools.ietf.org/html/rfc3546#section-3.1) in the connection request\. Connection attempts that don't include the SNI are refused\. For more information, see [Transport Security in AWS IoT](transport-security.html)\. Clients that use IAM users and AWS credentials to authenticate clients must provide the correct [Signature Version 4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) authentication\. 

## Connecting with MQTT using the AWS IoT Device SDKs<a name="mqtt-sdk"></a>

This section contains links to the AWS IoT Device SDKs and to the source code of sample programs that illustrate how to connect a device to AWS IoT\. The sample apps linked here show how to connect to AWS IoT using the MQTT protocol and MQTT over WSS\.

------
#### [ C\+\+ ]

**Using the AWS IoT C\+\+ Device SDK to connect devices**
+  [Source code of a sample app that shows an MQTT connection example in C\+\+](https://github.com/aws/aws-iot-device-sdk-cpp-v2/blob/main/samples/mqtt/basic_connect/main.cpp) 
+ [AWS IoT C\+\+ Device SDK v2 on GitHub](https://github.com/aws/aws-iot-device-sdk-cpp-v2)

------
#### [ Python ]

**Using the AWS IoT Device SDK for Python to connect devices**
+  [Source code of a sample app that shows an MQTT connection example in Python](https://github.com/aws/aws-iot-device-sdk-python-v2/blob/master/samples/pubsub.py) 
+ [AWS IoT Device SDK for Python v2 on GitHub](https://github.com/aws/aws-iot-device-sdk-python-v2)

------
#### [ JavaScript ]

**Using the AWS IoT Device SDK for JavaScript to connect devices**
+  [Source code of a sample app that shows an MQTT connection example in JavaScript](https://github.com/aws/aws-iot-device-sdk-js-v2/blob/master/samples/node/pub_sub/index.ts) 
+ [AWS IoT Device SDK for JavaScript v2 on GitHub](https://github.com/aws/aws-iot-device-sdk-js-v2)

------
#### [ Java ]

**Using the AWS IoT Device SDK for Java to connect devices**
+  [Source code of a sample app that shows an MQTT connection example in Java](https://github.com/aws/aws-iot-device-sdk-java-v2/blob/master/samples/BasicPubSub/src/main/java/pubsub/PubSub.java) 
+ [AWS IoT Device SDK for Java v2 on GitHub](https://github.com/aws/aws-iot-device-sdk-java-v2)

------
#### [ Embedded C ]

**Using the AWS IoT Device SDK for Embedded C to connect devices**

**Important**  
This SDK is intended for use by experienced embedded\-software developers\.
+  [Source code of a sample app that shows an MQTT connection example in Embedded C](https://github.com/aws/aws-iot-device-sdk-embedded-C/blob/master/demos/mqtt/mqtt_demo_basic_tls/mqtt_demo_basic_tls.c) 
+ [AWS IoT Device SDK for Embedded C on GitHub](https://github.com/aws/aws-iot-device-sdk-embedded-C)

------

## MQTT Quality of Service \(QoS\) options<a name="mqtt-qos"></a>

AWS IoT and the AWS IoT Device SDKs support the [MQTT Quality of Service \(QoS\) levels `0` and `1`](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html#_Toc385349263)\. The MQTT protocol defines a third level of QoS, level `2`, but AWS IoT does not support it\. Only the MQTT protocol supports the QoS feature\. HTTPS supports QoS by passing a query string parameter `?qos=qos` where the value can be 0 or 1\.

This table describes how each QoS level affects messages published to and by the message broker\. 


|  With a QoS level of\.\.\.  |  The message is\.\.\.  |  Comments  | 
| --- | --- | --- | 
|  QoS level 0  |  Sent zero or more times  |  This level should be used for messages that are sent over reliable communication links or that can be missed without a problem\.  | 
|  QoS level 1  |  Sent at least one time, and then repeatedly until a `PUBACK` response is received  |  The message is not considered complete until the sender receives a `PUBACK` response to indicate successful delivery\.  | 

## Using MQTT persistent sessions<a name="mqtt-persistent-sessions"></a>

Persistent sessions store a client’s subscriptions and messages, with a quality of service \(QoS\) of 1, that have not been acknowledged by the client\. When a disconnected device reconnects to a persistent session, the session resumes, its subscriptions are reinstated, and subscribed messages received prior to the reconnection and that have not been acknowledged by the client are sent to the client\. 

**Creating a persistent session**  
You create an MQTT persistent session by sending a `CONNECT` message and setting the `cleanSession` flag to `0`\. If no session exists for the client sending the `CONNECT` message, a new persistent session is created\. If a session already exists for the client, the client resumes the existing session\.

**Operations during a persistent session**  
Clients use the `sessionPresent` attribute in the connection acknowledged \(`CONNACK`\) message to determine if a persistent session is present\. If `sessionPresent` is `1`, a persistent session is present and any stored messages for the client are delivered to the client immediately after the client receives the `CONNACK`, as described in [Message traffic after reconnection to a persistent session](#persistent-session-reconnect)\. If `sessionPresent` is `1`, there is no need for the client to resubscribe\. However, if `sessionPresent` is `0`, no persistent session is present and the client must resubscribe to its topic filters\.

After the client joins a persistent session, it can publish messages and subscribe to topic filters without any additional flags on each operation\.<a name="persistent-session-reconnect"></a>

**Message traffic after reconnection to a persistent session**  
A persistent session represents an ongoing connection between a client and an MQTT message broker\. When a client connects to the message broker using a persistent session, the message broker saves all subscriptions that the client makes during the connection\. When the client disconnects, the message broker stores unacknowledged QoS 1 messages and new QoS 1 messages published to topics to which the client is subscribed\. Messages are stored according to account limit, messages that exceed that limit will be dropped\. For more information about persistent message limits, see [AWS IoT Core endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#message-broker-limits)\. When the client reconnects to its persistent session, all subscriptions are reinstated and all stored messages are sent to the client at a maximum rate of 10 messages per second\.

After reconnection, the stored messages are sent to the client, at a rate that is limited to 10 stored messages per second, along with any current message traffic until the [https://docs.aws.amazon.com/general/latest/gr/iot-core.html#message-broker-limits](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#message-broker-limits) limit is reached\. Because the delivery rate of the stored messages is limited it will take several seconds to deliver all stored messages if a session has more than 10 stored messages to deliver after reconnection\.<a name="ending-a-persistent-session"></a>

**Ending a persistent session**  
The following conditions describe how persistent sessions can end\.
+ When the persistent session expiration time elapses\. The persistent session expiration timer starts when the message broker detects that a client has disconnected, either by the client disconnecting or the connection timing out\. 
+ When the client sends a `CONNECT` message that sets the `cleanSession` flag to `1`\.

**Note**  
The stored messages waiting to be sent to the client when a session ends are discarded; however, they are still billed at the standard messaging rate, even though they could not be sent\. For more information about message pricing, see [AWS IoT Core Pricing](https://aws.amazon.com/iot-core/pricing)\. You can configure the expiration time interval\.

**Reconnection after a persistent session has expired**  
If a client doesn't reconnect to its persistent session before it expires, the session ends and its stored messages are discarded\. When a client reconnects after the session has expired with a `cleanSession` flag to `0`, the service creates a new persistent session\. Any subscriptions or messages from the previous session are not available to this session because they were discarded when the previous session expired\.

**Persistent session message charges**  
Messages are charged to your AWS account when the message broker sends a message to a client or an offline persistent session\. When an offline device with a persistent session reconnects and resumes its session, the stored messages are delivered to the device and charged to your account again\. For more information about message pricing, see [AWS IoT Core pricing \- Messaging](https://aws.amazon.com/iot-core/pricing/#Messaging)\.

The default persistent session expiration time of one hour can be increased by using the standard limit increase process\. Note that increasing the session expiration time might increase your message charges because the additional time could allow for more messages to be stored for the offline device and those additional messages would be charged to your account at the standard messaging rate\. The session expiration time is approximate and a session could persist for up to 30 minutes longer than the account limit; however, a session will not be shorter than the account limit\. For more information about session limits, see [AWS Service Quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#message-broker-limits)\.

## Using MQTT retained messages<a name="mqtt-retain"></a>

AWS IoT Core supports the RETAIN flag described in [MQTT v3\.1\.1](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html#_Toc398718022)\. When a client sets the RETAIN flag on an MQTT message that it publishes, AWS IoT Core saves the message\. It can then be sent to new subscribers, retrieved by calling the [https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_GetRetainedMessage.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_GetRetainedMessage.html) operation, and viewed in the [AWS IoT console](https://console.aws.amazon.com/iot/home#/retainedMessages)\. AWS IoT Core stores retained messages for three years after the last time they were updated or accessed\. After three years, the messages are deleted\.

**Examples of using MQTT retained messages**
+ 

**As an initial configuration message**  
MQTT retained messages are sent to a client after the client subscribes to a topic\. If you want all clients that subscribe to a topic to receive the MQTT retained message immediately after their subscription, you can publish a configuration message with the RETAIN flag set\. Subscribing clients also receive updates to that configuration whenever a new configuration message is published\.
+ 

**As a last\-known state message**  
Devices can set the RETAIN flag on current\-state messages so that AWS IoT Core will save them\. When applications connect or reconnect, they can subscribe to this topic and get the last reported state immediately after subscribing to the retained message topic\. This way they can avoid having to wait until the next message from the device to see the current state\.

**Topics**
+ [Common tasks with MQTT retained messages in AWS IoT Core](#mqtt-retain-using)
+ [Billing and retained messages](#mqtt-retain-billing)
+ [Comparing MQTT retained messages and MQTT persistent sessions](#mqtt-retain-persist)
+ [MQTT retained messages and AWS IoT Device Shadows](#mqtt-retain-shadow)

### Common tasks with MQTT retained messages in AWS IoT Core<a name="mqtt-retain-using"></a>

AWS IoT Core saves MQTT messages with the RETAIN flag set\. These *retained messages* are sent to all clients that have subscribed to the topic, as a normal MQTT message, and they are also stored to be sent to new subscribers to the topic\.

MQTT retained messages require specific policy actions to authorize clients to access them\. For examples of using retained message policies, see [Retained message policy examples](retained-message-policy-examples.md)\.

This section describes common operations that involve retained messages\.
+ 

**Creating a retained message**  
The client determines whether a message is retained when it publishes an MQTT message\. Clients can set the RETAIN flag when they publish a message by using a [Device SDK](iot-sdks.md)\. Applications and services can set the RETAIN flag when they use the [`Publish` action](https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_Publish.html) to publish an MQTT message\.

  Only one message per topic name is retained\. A new message with the RETAIN flag set published to a topic replaces any existing retained message that was sent to the topic earlier\. 

  **NOTE:** You can't publish to a [reserved topic](reserved-topics.md) with the RETAIN flag set\.
+ 

**Subscribing to a retained message topic**  
Clients subscribe to retained message topics as they would any other MQTT message topic\. Retained messages received by subscribing to a retained message topic have the RETAIN flag set\. 

  Retained messages are deleted from AWS IoT Core when a client publishes a retained message with a 0\-byte message payload to the retained message topic\. Clients that have subscribed to the retained message topic will also receive the 0\-byte message\.

  Subscribing to a wild card topic filter that includes a retained message topic lets the client receive subsequent messages published to the retained message's topic, but it doesn't deliver the retained message upon subscription\.

  **NOTE:** To receive a retained message upon subscription, the topic filter in the subscription request must match the retained message topic exactly\.

  Retained messages received upon subscribing to a retained message topic have the RETAIN flag set\. Retained messages that are received by a subscribing client after subscription, don't\.
+ 

**Retrieving a retained message**  
Retained messages are delivered to clients automatically when they subscribe to the topic with the retained message\. For a client to receive the retained message upon subscription, it must subscribe to the exact topic name of the retained message\. Subscribing to a wild card topic filter that includes a retained message topic lets the client receive subsequent messages published to the retained message's topic, but it does not deliver the retained message upon subscription\.

  Services and apps can list and retrieve retained messages by calling [https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_ListRetainedMessages.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_ListRetainedMessages.html) and [https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_GetRetainedMessage.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_GetRetainedMessage.html)\.

  A client is not prevented from publishing messages to a retained message topic *without* setting the RETAIN flag\. This could cause unexpected results, such as the retained message not matching the message received by subscribing to the topic\.
+ 

**Listing retained message topics**  
You can list retained messages by calling [https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_ListRetainedMessages.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_ListRetainedMessages.html) and the retained messages can be viewed in the [AWS IoT console](https://console.aws.amazon.com/iot/home#/retainedMessages)\. 
+ 

**Getting retained message details**  
You can get retained message details by calling [https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_GetRetainedMessage.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_GetRetainedMessage.html) and they can be viewed in the [AWS IoT console](https://console.aws.amazon.com/iot/home#/retainedMessages)\.
+ 

**Retaining a Will message**  
MQTT [*Will* messages](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/errata01/os/mqtt-v3.1.1-errata01-os-complete.html#_Will_Flag) that are created when a device connects can be retained by setting the `Will Retain` flag in the `Connect Flag bits` field\.
+ 

**Deleting a retained message**  
Devices, applications, and services can delete a retained message by publishing a message with the RETAIN flag set and an empty \(0\-byte\) message payload to the topic name of the retained message to delete\. Such messages delete the retained message from AWS IoT Core, are sent to clients with a subscription to the topic, but they are not retained by AWS IoT Core\.

  Retained messages can also be deleted interactively by accessing the retained message in the [AWS IoT console](https://console.aws.amazon.com/iot/home#/retainedMessages)\. Retained messages that are deleted by using the [AWS IoT console](https://console.aws.amazon.com/iot/home#/retainedMessages) also send a 0\-byte message to clients that have subscribed to the retained message's topic\.

  Retained messages can't be restored after they are deleted\. A client would need to publish a new retained message to take the place of the deleted message\.
+ 

**Debugging and troubleshooting retained messages**  
The [AWS IoT console](https://console.aws.amazon.com/iot/home#) provides several tools to help you troubleshoot retained messages:
  + 

**The **[Retained messages](https://console.aws.amazon.com/iot/home#/retainedMessages)** page**  
The **Retained messages** page in the AWS IoT console provides a paginated list of the retained messages that have been stored by your Account in the current Region\. From this page, you can:
    + See the details of each retained message, such as the message payload, QoS, the time it was received\.
    + Update the contents of a retained message\.
    + Delete a retained message\.
  + 

**The **[MQTT test client](https://console.aws.amazon.com/iot/home#/test)****  
The **MQTT test client** page in the AWS IoT console can subscribe and publish to MQTT topics\. The publish option lets you set the RETAIN flag on the messages that you publish to simulate how your devices might behave\.

  Some unexpected results might be the result of these aspects of how retained messages are implemented in AWS IoT Core\.
  + 

**Retained message limits**  
When an account has stored the maximum number of retained messages, AWS IoT Core returns a throttled response to messages published with RETAIN set and payloads greater than 0 bytes until some retained messages are deleted and the retained message count falls below the limit\.
  + 

**Retained message delivery order**  
The sequence of retained message and subscribed message delivery is not guaranteed\.

### Billing and retained messages<a name="mqtt-retain-billing"></a>

Publishing messages with the RETAIN flag set from a client, by using AWS IoT console, or by calling [https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_Publish.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_Publish.html) incurs additional messaging charges described in [AWS IoT Core pricing \- Messaging](http://aws.amazon.com/iot-core/pricing/#Messaging)\.

Retrieving retained messages by a client, by using AWS IoT console, or by calling [https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_GetRetainedMessage.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_GetRetainedMessage.html) incurs messaging charges in addition to the normal API usage charges\. The additional charges are described in [AWS IoT Core pricing \- Messaging](http://aws.amazon.com/iot-core/pricing/#Messaging)\.

MQTT [*Will* messages](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/errata01/os/mqtt-v3.1.1-errata01-os-complete.html#_Will_Flag) that are published when a device disconnects unexpectedly incur messaging charges described in [AWS IoT Core pricing \- Messaging](http://aws.amazon.com/iot-core/pricing/#Messaging)\.

For more information about messaging costs, see [AWS IoT Core pricing \- Messaging](http://aws.amazon.com/iot-core/pricing/#Messaging)\.

### Comparing MQTT retained messages and MQTT persistent sessions<a name="mqtt-retain-persist"></a>

Retained messages and persistent sessions are standard features of MQTT 3\.1\.1 that make it possible for devices to receive messages that were published while they were offline\. Retained messages can be published from persistent sessions\. This section describes key aspects of these features and how they work together\.


|    |  Retained messages  |  Persistent sessions  |  Retained messages in persistent sessions  | 
| --- | --- | --- | --- | 
|  **Key features**  |  Retained messages can be used to configure or notify large groups of devices after they connect\. Retained messages can also be used where you want devices to receive only the last message published to a topic after a reconnection\.  |  Persistent sessions are useful for devices that have intermittent connectivity and could miss several important messages\. Devices can connect with a persistent session to receive messages sent while they are offline\.  |  Retained messages can be used in both regular and persistent sessions\.  | 
|  **Examples**  |  Retained messages can give devices configuration information about their environment when they come online\. The initial configuration could include a list of other message topics to which it should subscribe or information about how it should configure its local time zone\.  |  Devices that connect over a cellular network with intermittent connectivity could use persistent sessions to avoid missing important messages that are sent while a device is out of network coverage or needs to turn off its cellular radio\.   |  The cellular device in the persistent session sample could use a retained message to receive its initial configuration on its initial connection\.  | 
|  **Messages received on initial subscription to a topic**  |  After subscribing to a topic with a retained message, the most recent retained message is received\.  |  After subscribing to a topic without a retained message, no message is received until one is published to the topic\.  |  After subscribing to a topic with a retained message, the most recent retained message is received\.  | 
|  **Subscribed topics after reconnection**  |  Without a persistent session, the client must subscribe to topics after reconnection\.  |  Subscribed topics are restored after reconnection\.  |  Subscribed topics are restored after reconnection\.  | 
|  **Messages received after reconnection**  |  After subscribing to a topic with a retained message, the most recent retained message is received\.   |  All messages published with a QOS = 1 and subscribed to with a QOS =1 while the device was disconnected are sent after the device reconnects\.  |  All messages published with a QOS = 1 and subscribed to with a QOS =1 that were sent while the device was disconnected are sent after the device reconnects\. Updated retained messages from topics to which the client was subscribed are also sent to the client\. If more than one retained message was published to a topic while the client was offline, it can receive multiple stored retained messages to that topic after it reconnects\.  | 
|  **Data/session expiration**   |  Retained messages do not expire\. They are stored until they are replaced or deleted\.  |  Persistent sessions expire if the client doesn't reconnect within the timeout period\. After a persistent session expires, the client's subscriptions and saved messages that were published with a QOS = 1 and subscribed to with a QOS =1 while the device was disconnected are deleted\. For more information about session expirations with persistent sessions, see [Using MQTT persistent sessions](#mqtt-persistent-sessions)\.  |  Retained messages do not expire\. They are stored until they are replaced or deleted even if they are sent from within a persistent session that has expired\. After a persistent session expires, the client's subscriptions and saved messages that were published with a QOS = 1 and subscribed to with a QOS =1 while the device was disconnected are deleted\.   | 

For information about persistent sessions, see [Using MQTT persistent sessions](#mqtt-persistent-sessions)\.

With Retained Messages, the publishing client determines whether a message should be retained and delivered to a device after it connects, whether it had a previous session or not\. The choice to store a message is made by the publisher and the stored message is delivered to all current and future clients that subscribe with a QoS 0 or QoS 1 subscriptions\. Retained messages keep only one message on a given topic at a time\.

When an account has stored the maximum number of retained messages, AWS IoT Core returns a throttled response to messages published with RETAIN set and payloads greater than 0 bytes until some retained messages are deleted and the retained message count falls below the limit\.

### MQTT retained messages and AWS IoT Device Shadows<a name="mqtt-retain-shadow"></a>

Retained messages and Device Shadows both retain data from a device, but they behave differently and serve different purposes\. This section describes their similarities and differences\.


|    |  Retained messages  |  Device Shadows  | 
| --- | --- | --- | 
|  **Message payload has a pre\-defined structure or schema**  | As defined by the implementation\. MQTT does not specify a structure or schema for its message payload\. |  AWS IoT supports a specific data structure\.  | 
|  **Updating the message payload generates event messages**  | Publishing a retained message sends the message to subscribed clients, but doesn't generate additional update messages\. |  Updating a Device Shadow produces [update messages that describe the change](https://docs.aws.amazon.com/iot/latest/developerguide/device-shadow-mqtt.html#update-delta-pub-sub-topic)\.  | 
|  **Message updates are numbered**  | Retained messages are not numbered automatically\. | Device Shadow documents have automatic version numbers and timestamps\. | 
|  **Message payload is attached to a thing resource**  | Retained messages are not attached to a thing resource\. |  Device Shadows are attached to a thing resource\.  | 
|  **Updating individual elements of the message payload**  |  Individual elements of the message can't be changed without updating the entire message payload\.  |  Individual elements of a Device Shadow document can be updated without the need to update the entire Device Shadow document\.  | 
|  **Client receives message data upon subscription**  |  Client automatically receives a retained message after it subscribes to a topic with a retained message\.  |  Clients can subscribe to Device Shadow updates, but they must request the current state deliberately\.  | 
|  **Indexing and searchability**  |  Retained messages are not indexed for search\.  |  Fleet indexing indexes Device Shadow data for search and aggregation\.  | 

## Using connectAttributes<a name="connect-attribute"></a>

`ConnectAttributes` allow you to specify what attributes you want to use in your connect message in your IAM policies such as `PersistentConnect` and `LastWill`\. With `ConnectAttributes`, you can build policies that don't give devices access to new features by default, which can be helpful if a device is compromised\. 

`connectAttributes` supports the following features:

`PersistentConnect`  
Use the `PersistentConnect` feature to save all subscriptions the client makes during the connection when the connection between the client and broker is interrupted\.

`LastWill`  
Use the `LastWill` feature to publish a message to the `LastWillTopic` when a client unexpectedly disconnects\.

By default, your policy has non\-persistent connection and there are no attributes passed for this connection\. You must specify a persistent connection in your IAM policy if you want to have one\.

For `ConnectAttributes` examples, see [Connect Policy Examples](connect-policy.md)\.

## AWS IoT differences from MQTT version 3\.1\.1 specification<a name="mqtt-differences"></a>

The message broker implementation is based on the [MQTT v3\.1\.1 specification](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html), but it differs from the specification in these ways:
+ AWS IoT supports MQTT quality of service \(QoS\) levels 0 and 1 only\. AWS IoT doesn't support publishing or subscribing with QoS level 2\. When QoS level 2 is requested, the message broker doesn't send a PUBACK or SUBACK\.
+ In AWS IoT, subscribing to a topic with QoS level 0 means that a message is delivered zero or more times\. A message might be delivered more than once\. Messages delivered more than once might be sent with a different packet ID\. In these cases, the DUP flag is not set\.
+ When responding to a connection request, the message broker sends a CONNACK message\. This message contains a flag to indicate if the connection is resuming a previous session\.
+ Before sending additional control packets or a disconnect request, the client must wait for the CONNACK message to be received on their device from the AWS IoT message broker\.
+ AWS IoT returns the error code 7 instead of the CONNACK error code 5 when a connection is refused because the client isn't authorized to connect\.
+ When a client subscribes to a topic, there might be a delay between the time the message broker sends a SUBACK and the time the client starts receiving new matching messages\.
+ When a client uses the wildcard character `#` in the topic filter to subscribe to a topic, all strings at and below its level in the topic hierarchy are matched\. However, the parent topic is not matched\. For example, a subscription to the topic `sensor/#` receives messages published to the topics `sensor/`, `sensor/temperature`, `sensor/temperature/room1`, but not messages published to `sensor`\. For more information about wildcards, see [Topic filters](topics.md#topicfilters)\.
+ The message broker uses the client ID to identify each client\. The client ID is passed in from the client to the message broker as part of the MQTT payload\. Two clients with the same client ID can't be connected concurrently to the message broker\. When a client connects to the message broker using a client ID that another client is using, the new client connection is accepted and the previously connected client is disconnected\.
+ On rare occasions, the message broker might resend the same logical PUBLISH message with a different packet ID\.
+ Subscription to topic filters that contain a wildcard character can't receive retained messages\. To receive a retained message, the subscribe request must contain a topic filter that matches the retained message topic exactly\.
+ The message broker doesn't guarantee the order in which messages and ACK are received\.