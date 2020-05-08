# MQTT<a name="mqtt"></a>

[MQTT](http://mqtt.org/) is a lightweight and widely adopted messaging protocol designed for constrained devices\. For more information, see the [MQTT v3\.1\.1 specification](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html)\. 

The AWS IoT message broker implementation is based on MQTT version 3\.1\.1, but it differs from the specification in these ways:
+ AWS IoT Core supports MQTT Quality of Service \(QoS\) levels 0 and 1 only\. AWS IoT Core does not support publishing or subscribing with QoS level 2\. When QoS level 2 is requested, the AWS IoT message broker does not send a PUBACK or SUBACK\.
+ In AWS IoT Core, subscribing to a topic with QoS level 0 means a message is delivered zero or more times\. A message might be delivered more than once\. Messages delivered more than once might be sent with a different packet ID\. In these cases, the DUP flag is not set\.
+ When responding to a connection request, the message broker sends a CONNACK message\. This message contains a flag to indicate if the connection is resuming a previous session\.
+ When a client subscribes to a topic, there might be a delay between the time the message broker sends a SUBACK and the time the client starts receiving new matching messages\.
+ The MQTT specification provides a provision for the publisher to request that the broker retain the last message sent to a topic and send it to all future topic subscribers\. AWS IoT Core does not support retained messages\. If a request is made to retain messages, the connection is disconnected\.
+ The message broker uses the client ID to identify each client\. The client ID is passed in from the client to the message broker as part of the MQTT payload\. Two clients with the same client ID cannot be connected concurrently to the message broker\. When a client connects to the message broker using a client ID that another client is using, the new client connection is accepted and the previously connected client is disconnected\.
+ On rare occasions, the message broker might resend the same logical PUBLISH message with a different packet ID\.
+ The message broker does not guarantee the order in which messages and ACK are received\.

You connect to AWS IoT Core over MQTT by using one of the [AWS IoT device and mobile SDKs ](iot-sdks.md)\. For an example of how to connect to AWS IoT Core with MQTT, see the `basicPubSub` sample in the [Python SDK](https://github.com/aws/aws-iot-device-sdk-python)\. The other [AWS IoT SDKs](iot-sdks.md) have similar sample apps\.

For more information about authentication and port mappings for MQTT messages, see [Protocols, port mappings, and authentication](protocols.md#protocol-port-mapping)\.