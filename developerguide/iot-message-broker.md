# Message broker for AWS IoT<a name="iot-message-broker"></a>

The AWS IoT message broker connects AWS IoT clients by sending messages from publishing clients to subscribing clients\. Clients send data by publishing a message on a topic and receive messages by subscribing to a topic\. When the message broker receives a message from a publishing client, it forwards the message to all clients that have subscribed to that topic\. 

Processing messages through the AWS IoT message broker lets you define [rules](iot-rules.md) that can initiate more actions based on the content of the messages\. If you do not require AWS IoT features such as [rules](iot-rules.md) or [jobs](iot-jobs.md), see [AWS Messaging](https://aws.amazon.com/messaging/) for information about other AWS messaging services that might better fit your requirements\.

**Topics**
+ [Topics](topics.md)
+ [Protocols](protocols.md)