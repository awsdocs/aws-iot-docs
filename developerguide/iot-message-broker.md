# Message Broker for AWS IoT<a name="iot-message-broker"></a>

The AWS IoT message broker makes it possible for clients to communicate with AWS IoT and for AWS IoT to communicate with clients\. Clients send data by publishing a message on a topic\. Clients receive messages by subscribing to a topic\. When the message broker receives a message, it forwards the message to all clients subscribed to the topic\.

The message broker maintains a list of all client sessions and the subscriptions for each session\. When a message is published on a topic, the broker checks for sessions with subscriptions that map to the topic\. The broker then forwards the published message to all sessions that have a currently connected client\.

If your use case does not require AWS IoT, see [AWS Messaging](https://aws.amazon.com/messaging/) for information about other AWS messaging services that better fit your requirements\.