# Message Broker for AWS IoT<a name="iot-message-broker"></a>

The AWS IoT message broker is a publish/subscribe broker service that enables the sending and receiving of messages to and from AWS IoT\. When communicating with AWS IoT, a client sends a message addressed to a topic like `Sensor/temp/room1`\.

**Note**  
We do not recommend using personally identifiable information in your topics\.

The message broker, in turn, sends the message to all clients that have registered to receive messages for that topic\. The act of sending the message is referred to as *publishing*\. The act of registering to receive messages for a topic filter is referred to as *subscribing*\.

The topic namespace is isolated for each AWS account and region pair\. For example, the `Sensor/temp/room1` topic for an AWS account is independent from the `Sensor/temp/room1` topic for another AWS account\. This is true of regions, too\. The `Sensor/temp/room1` topic in the same AWS account in us\-east\-1 is independent from the same topic in us\-east\-2\. AWS IoT does not support sending and receiving messages across AWS accounts and regions\.

The message broker maintains a list of all client sessions and the subscriptions for each session\. When a message is published on a topic, the broker checks for sessions with subscriptions that map to the topic\. The broker then forwards the publish message to all sessions that have a currently connected client\.

If your use case does not require IoT, see [AWS Messaging](https://aws.amazon.com/messaging/) for information about other AWS messaging services that better fit your requirements\.