# MQTT<a name="mqtt"></a>

MQTT is a widely adopted, lightweight messaging protocol designed for constrained devices\. For more information, see [MQTT](http://www.mqtt.org)\. The AWS IoT message broker supports Quality of Service \(QoS\) levels 0 and 1\.

Although the AWS IoT message broker implementation is based on MQTT version 3\.1\.1, it deviates from the specification as follows:
+ In AWS IoT, subscribing to a topic with QoS 0 means a message is delivered zero or more times\. A message might be delivered more than once\. Messages delivered more than once might be sent with a different packet ID\. In these cases, the DUP flag is not set\.
+ AWS IoT does not support publishing and subscribing with QoS 2\. The AWS IoT message broker does not send a PUBACK or SUBACK when QoS 2 is requested\.
+ When responding to a connection request, the message broker sends a CONNACK message\. This message contains a flag to indicate if the connection is resuming a previous session\.
+ When a client subscribes to a topic, there might be a delay between the time the message broker sends a SUBACK and the time the client starts receiving new matching messages\.
+ The MQTT specification provides a provision for the publisher to request that the broker retain the last message sent to a topic and send it to all future topic subscribers\. AWS IoT does not support retained messages\. If a request is made to retain messages, the connection is disconnected\.
+ The message broker uses the client ID to identify each client\. The client ID is passed in from the client to the message broker as part of the MQTT payload\. Two clients with the same client ID are not allowed to be connected concurrently to the message broker\. When a client connects to the message broker using a client ID that another client is using, the new client connection is accepted and the previously connected client is disconnected\."
+ On rare occasions, the message broker might resend the same logical PUBLISH message with a different packet ID\.
+ The message broker does not guarantee the order in which messages and ACK are received\.