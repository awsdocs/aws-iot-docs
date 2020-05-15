# How AWS IoT Works<a name="aws-iot-how-it-works"></a>

AWS IoT enables internet\-connected devices to connect to the AWS Cloud and lets applications in the cloud interact with internet\-connected devices\. Common IoT applications either collect and process telemetry from devices or enable users to control a device remotely\.

The state of each device connected to AWS IoT is stored in a device shadow\. The Device Shadow service manages device shadows by responding to requests to retrieve or update device state data\. The Device Shadow service makes it possible for devices to communicate with applications and for applications to communicate with devices\.

Communication between a device and AWS IoT is protected through the use of X\.509 certificates\. AWS IoT can generate a certificate for you or you can use your own\. In either case, the certificate must be registered and activated with AWS IoT, and then copied onto your device\. When your device communicates with AWS IoT, it presents the certificate to AWS IoT as a credential\.

We recommend that all devices that connect to AWS IoT have an entry in the registry\. The registry stores information about a device and the certificates that are used by the device to secure communication with AWS IoT\.

You can create rules that define one or more actions to perform based on the data in a message\. For example, you can insert, update, or query a DynamoDB table or invoke a Lambda function\. Rules use expressions to filter messages\. When a rule matches a message, the rules engine triggers the action using the selected properties\. Rules also contain an IAM role that grants AWS IoT permission to the AWS resources used to perform the action\.

![\[A high-level view of AWS IoT\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/aws_iot_data_services.png)