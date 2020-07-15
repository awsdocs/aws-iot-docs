# MQTT persistent sessions<a name="mqtt-persistent-sessions"></a>

Persistent sessions store subscription information and pending Quality of Service \(QoS\) 1 messages in case your devices become disconnected\. When a device reconnects, its persistent session resumes and its subscriptions are automatically reinstated\. Also, any stored messages will be delivered\.

A persistent session represents an ongoing connection to an MQTT message broker\. When a client connects to the AWS IoT message broker using a persistent session, the message broker saves all subscriptions that the client makes during the connection\. When the client disconnects, the message broker stores unacknowledged QoS 1 messages and new QoS 1 messages published to topics to which the client is subscribed\. When the client reconnects to the persistent session, all subscriptions are reinstated and all stored messages are sent to the client at a maximum rate of 10 messages per second\.

You create an MQTT persistent session by sending a CONNECT message, setting the `cleanSession` flag to 0\. If no session exists for the client sending the CONNECT message, a new persistent session is created\. If a session already exists for the client, it is resumed\.

After the client joins the session, it can continue to publish messages and subscribe to topic filters without any additional flags on each operation\. The following conditions describe how persistent sessions begin and end\.
+ A persistent session ends and can't be resumed when the client sends a CONNECT message that sets the `cleanSession` flag to 1\.
+ By default, a persistent session expires one hour after the message broker detects that a client has disconnected\. You can configure this time interval\.
+ When a client reconnects after the session has expired and sets the `cleanSession` flag to 0, the service creates a new persistent session\. Subscriptions and messages from the previous session are discarded\.

Devices use the `sessionPresent` attribute in the connection acknowledged \(CONNACK\) message to determine if a persistent session is present\. If `sessionPresent` is set to 1, a persistent session is present and stored messages are delivered to the client\. This starts immediately after the device receives the CONNACK\. There is no need to resubscribe\. If `sessionPresent` is set to 0, no persistent session is present and the client must resubscribe to its topic filters\. 

Persistent sessions have a default expiry period of one hour\. The expiry period begins when the message broker detects that a client disconnects \(MQTT disconnect or timeout\)\. The persistent session expiry period can be increased through the standard limit increase process\. If a client has not resumed its session within the expiry period, the session is terminated and any associated stored messages are discarded\. The expiry period is approximate\. Sessions might be persisted for up to 30 minutes longer, but not less than the configured duration\. For more information, see [ AWS Service Quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)\. Any messages stored for persistent sessions are billed at the standard messaging rate\. For more information, see [AWS IoT Core Pricing](https://aws.amazon.com/iot-core/pricing)\.

## Connect Attribute Condition<a name="connect-attribute"></a>

`ConnectAttributes` allow you to specify what attributes you want to use in your connect message in your IAM policies such as `PersistentConnect` and `LastWill`\. With `ConnectAttributes` you can build policies that don't give devices access to new features by default, which can be helpful if a device is compromised\. 

`connectAttributes` supports the following features:

`PersistentConnect`  
Use the `PersistentConnect` feature to save all subscriptions the client makes during the connection when the connectionbetween the client and broker is interrupted\.

`LastWill`  
Use the `LastWill` feature to publish a message to the `LastWillTopic` when a client unexpectedly disconnects\.

By default, your policy has non\-persistent connection and there are no attributes passed for this connection\. You must specify a persistent connection in your IAM policy if you wish to have one\.

For `ConnectAttributes` examples, see [Connect Policy Examples](https://docs.aws.amazon.com/iot/latest/developerguide/connect-policy.html.html)\.