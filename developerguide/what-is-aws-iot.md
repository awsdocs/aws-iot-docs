# What Is AWS IoT?<a name="what-is-aws-iot"></a>

AWS IoT provides secure, bi\-directional communication between Internet\-connected devices such as sensors, actuators, embedded micro\-controllers, or smart appliances and the AWS Cloud\. This enables you to collect telemetry data from multiple devices, and store and analyze the data\. You can also create applications that enable your users to control these devices from their phones or tablets\.

## AWS IoT Components<a name="aws-iot-components"></a>

AWS IoT consists of the following components:

**Device gateway **  
Enables devices to securely and efficiently communicate with AWS IoT\.

**Message broker **  
Provides a secure mechanism for devices and AWS IoT applications to publish and receive messages from each other\. You can use either the MQTT protocol directly or MQTT over WebSocket to publish and subscribe\. You can use the HTTP REST interface to publish\. For more information, see [Message Broker for AWS IoT](iot-message-broker.md)\.

**Rules engine **  
Provides message processing and integration with other AWS services\. You can use an SQL\-based language to select data from message payloads, and then process and send the data to other services, such as Amazon S3, Amazon DynamoDB, and AWS Lambda\. You can also use the message broker to republish messages to other subscribers\. For more information, see [Rules for AWS IoT ](iot-rules.md)\.

**Security and Identity service **  
Provides shared responsibility for security in the AWS Cloud\. Your devices must keep their credentials safe in order to securely send data to the message broker\. The message broker and rules engine use AWS security features to send data securely to devices or other AWS services\. For more information, see [Authentication](authentication.md)\.

**Registry**  
Organizes the resources associated with each device in the AWS Cloud\. You register your devices and associate up to three custom attributes with each one\. You can also associate certificates and MQTT client IDs with each device to improve your ability to manage and troubleshoot them\. For more information, see [Managing Devices with AWS IoT ](iot-thing-management.md)\.

**Group registry**  
Groups allow you to manage several devices at once by categorizing them into groups\. Groups can also contain groups—you can build a hierarchy of groups\. Any action you perform on a parent group will apply to its child groups, and to all the devices in it and in all of its child groups as well\. Permissions given to a group will apply to all devices in the group and in all of its child groups\. For more information, see [Managing Devices with AWS IoT ](iot-thing-management.md)\.

**Device shadow **  
A JSON document used to store and retrieve current state information for a device\.

**Device Shadow service **  
Provides persistent representations of your devices in the AWS Cloud\. You can publish updated state information to a device's shadow, and your device can synchronize its state when it connects\. Your devices can also publish their current state to a shadow for use by applications or other devices\. For more information, see [Device Shadow Service for AWS IoT](iot-device-shadows.md)\.

**Device Provisioning service**  
Allows you to provision devices using a template that describes the resources required for your device: a *thing*, a certificate, and one or more policies\. A thing is an entry in the registry that contains attributes that describe a device\. Devices use certificates to authenticate with AWS IoT\. Policies determine which operations a device can perform in AWS IoT\.  
The templates contain variables that are replaced by values in a dictionary \(map\)\. You can use the same template to provision multiple devices just by passing in different values for the template variables in the dictionary\. For more information, see [Device Provisioning](iot-provision.md)\.

**Custom Authentication service**  
You can define custom authorizers that allow you to manage your own authentication and authorization strategy using a custom authentication service and a Lambda function\. Custom authorizers allow AWS IoT to authenticate your devices and authorize operations using bearer token authentication and authorization strategies\.  
Custom authorizers can implement various authentication strategies \(for example, JSON Web Token verification, OAuth provider callout, and so on\) and must return policy documents that are used by the device gateway to authorize MQTT operations\. For more information, see [Custom Authentication](custom-authentication.md)\.

**Jobs service**  
Allows you to define a set of remote operations that are sent to and executed on one or more devices connected to AWS IoT\. For example, you can define a job that instructs a set of devices to download and install application or firmware updates, reboot, rotate certificates, or perform remote troubleshooting operations\.  
To create a job, you specify a description of the remote operations to be performed and a list of targets that should perform them\. The targets can be individual devices, groups or both\. For more information, see [Jobs](iot-jobs.md)\.

**Alexa Voice Service \(AVS\) Integration for AWS IoT**  
Brings Alexa Voice to any connected device\. AVS for AWS IoT reduces the cost and complexity of integrating Alexa\. This feature leverages AWS IoT to offload intensive computational and memory audio tasks from the device to the cloud\. Because of the resulting reduction in the engineering bill of materials \(eBoM\) cost, device makers can cost\-effectively bring Alexa to resource\-constrained IoT devices and enable consumers to talk directly to Alexa in parts of their home, office, or hotel rooms for an ambient experience\.  
AVS for AWS IoT enables Alexa built\-in functionality on MCUs, such as the ARM Cortex M class with less than 1 MB embedded RAM\. To do so, AVS offloads memory and compute tasks to a virtual Alexa Built\-in device in the cloud\. This reduces eBoM cost by up to 50 percent\. For more information, see [Alexa Voice Service \(AVS\) Integration for AWS IoT](avs-integration-aws-iot.md)\.

For information about AWS IoT limits, see [AWS IoT Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_iot)\.

## How to Get Started with AWS IoT<a name="aws-iot-get-started"></a>
+ To learn more about AWS IoT, see [How AWS IoT Works](aws-iot-how-it-works.md)\.
+ To learn how to connect a device to AWS IoT, see [Getting Started with AWS IoT](iot-gs.md)\.

## Accessing AWS IoT<a name="aws-iot-interfaces"></a>

AWS IoT provides the following interfaces to create and interact with your devices:
+ **AWS Command Line Interface \(AWS CLI\)**—Run commands for AWS IoT on Windows, macOS, and Linux\. These commands allow you to create and manage things, certificates, rules, and policies\. To get started, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\. For more information about the commands for AWS IoT, see [iot](https://docs.aws.amazon.com/cli/latest/reference/iot/index.html) in the *AWS CLI Command Reference*\.
+ **AWS IoT API**—Build your IoT applications using HTTP or HTTPS requests\. These API actions allow you to programmatically create and manage things, certificates, rules, and policies\. For more information about the API actions for AWS IoT, see [Actions](https://docs.aws.amazon.com/iot/latest/apireference/API_Operations.html) in the *AWS IoT API Reference*\.
+ **AWS SDKs**—Build your IoT applications using language\-specific APIs\. These SDKs wrap the HTTP/HTTPS API and allow you to program in any of the supported languages\. For more information, see [AWS SDKs and Tools](http://aws.amazon.com/tools/#sdk)\.
+ **AWS IoT Device SDKs**—Build applications that run on devices that send messages to and receive messages from AWS IoT\. For more information see, [AWS IoT SDKs](https://docs.aws.amazon.com/iot/latest/developerguide/iot-sdks.html)\.

## Related Services<a name="aws-iot-related-services"></a>

AWS IoT integrates directly with the following AWS services:
+ **Amazon Simple Storage Service**—Provides scalable storage in the AWS Cloud\. For more information, see [Amazon S3](https://aws.amazon.com/s3/)\.
+ **Amazon DynamoDB**—Provides managed NoSQL databases\. For more information, see [Amazon DynamoDB](https://aws.amazon.com/dynamodb/)\.
+ **Amazon Kinesis**—Enables real\-time processing of streaming data at a massive scale\. For more information, see [Amazon Kinesis](https://aws.amazon.com/kinesis/)\.
+ **AWS Lambda**—Runs your code on virtual servers from Amazon EC2 in response to events\. For more information, see [AWS Lambda](https://aws.amazon.com/lambda/)\.
+ **Amazon Simple Notification Service**—Sends or receives notifications\. For more information, see [Amazon SNS](https://aws.amazon.com/sns/)\.
+ **Amazon Simple Queue Service**—Stores data in a queue to be retrieved by applications\. For more information, see [Amazon SQS](https://aws.amazon.com/sqs/)\.