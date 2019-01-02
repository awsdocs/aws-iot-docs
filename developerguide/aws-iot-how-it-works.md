# How AWS IoT Works<a name="aws-iot-how-it-works"></a>

AWS IoT enables Internet\-connected devices to connect to the AWS Cloud and lets applications in the cloud interact with Internet\-connected devices\. Common IoT applications either collect and process telemetry from devices or enable users to control a device remotely\.

Devices report their state by publishing messages, in JSON format, on MQTT topics\. Each MQTT topic has a hierarchical name that identifies the device whose state is being updated\. When a message is published on an MQTT topic, the message is sent to the AWS IoT MQTT message broker, which is responsible for sending all messages published on an MQTT topic to all clients subscribed to that topic\. 

Communication between a device and AWS IoT is protected through the use of X\.509 certificates\. AWS IoT can generate a certificate for you or you can use your own\. In either case, the certificate must be registered and activated with AWS IoT, and then copied onto your device\. When your device communicates with AWS IoT, it presents the certificate to AWS IoT as a credential\.

We recommend that all devices that connect to AWS IoT have an entry in the registry\. The registry stores information about a device and the certificates that are used by the device to secure communication with AWS IoT\.

You can create rules that define one or more actions to perform based on the data in a message\. For example, you can insert, update, or query a DynamoDB table or invoke a Lambda function\. Rules use expressions to filter messages\. When a rule matches a message, the rules engine triggers the action using the selected properties\. Rules also contain an IAM role that grants AWS IoT permission to the AWS resources used to perform the action\.

![\[A high-level view of AWS IoT\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/aws_iot_data_services.png)

Each device has a shadow that stores and retrieves state information\. Each item in the state information has two entries: the state last reported by the device and the desired state requested by an application\. An application can request the current state information for a device\. The shadow responds to the request by providing a JSON document with the state information \(both reported and desired\), metadata, and a version number\. An application can control a device by requesting a change in its state\. The shadow accepts the state change request, updates its state information, and sends a message to indicate the state information has been updated\. The device receives the message, changes its state, and then reports its new state\.