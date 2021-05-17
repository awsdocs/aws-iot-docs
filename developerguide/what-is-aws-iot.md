# What is AWS IoT?<a name="what-is-aws-iot"></a>

AWS IoT provides the cloud services that connect your IoT devices to other devices and AWS cloud services\. AWS IoT provides device software that can help you integrate your IoT devices into AWS IoT\-based solutions\. If your devices can connect to AWS IoT, AWS IoT can connect them to the cloud services that AWS provides\.

![\[AWS IoT connects IoT devices to AWS IoT services\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/what-is-aws-iot.png)

AWS IoT lets you select the most appropriate and up\-to\-date technologies for your solution\. To help you manage and support your IoT devices in the field, AWS IoT Core supports these protocols: 
+ [MQTT \(Message Queuing and Telemetry Transport\)](mqtt.md)
+ [MQTT over WSS \(Websockets Secure\)](mqtt.md)
+ [HTTPS \(Hypertext Transfer Protocol \- Secure\)](http.md) 
+ [LoRaWAN \(Long Range Wide Area Network\)](connect-iot-lorawan.md)

The AWS IoT Core message broker supports devices and clients that use MQTT and MQTT over WSS protocols to publish and subscribe to messages\. It also supports devices and clients that use the HTTPS protocol to publish messages\.

AWS IoT Core for LoRaWAN helps you connect and manage wireless LoRaWAN \(low\-power long\-range Wide Area Network\) devices\. AWS IoT Core for LoRaWAN replaces the need for you to develop and operate a LoRaWAN Network Server \(LNS\)\.

If you don't require AWS IoT features such as device communications, [rules](iot-rules.md), or [jobs](iot-jobs.md), see [AWS Messaging](https://aws.amazon.com/messaging/) for information about other AWS IoT messaging services that might better fit your requirements\.

Whether you're new to IoT or you have years of experience, make sure to review [How AWS IoT works](aws-iot-how-it-works.md)\. This topic helps you understand AWS IoT concepts and terms to help you get started with AWS IoT more effectively\. 

## How to get started with AWS IoT<a name="aws-iot-get-started"></a>
+ **Look** inside AWS IoT and its components in [How AWS IoT works](aws-iot-how-it-works.md)\.
+ [**Learn** more about AWS IoT](aws-iot-learn-more.md) from our collection of training materials and videos\. This topic also includes a list of services that AWS IoT can connect to, social media links, and links to communication protocol specifications\.
+ **Connect** your first device to AWS IoT in [Getting started with AWS IoT Core](iot-gs.md)\.
+ **Develop** your IoT solutions by [Connecting to AWS IoT Core](connect-to-iot.md) and exploring the [AWS IoT Tutorials](iot-tutorials.md)\.
+ **Test and validate** your IoT devices for secure and reliable communication by using the [Device Advisor](device-advisor.md)\.
+ **Manage** your solution by using AWS IoT Core management services such as [Fleet indexing service](iot-indexing.md), [Jobs](iot-jobs.md), and [AWS IoT Device Defender](device-defender.md)\.
+ **Analyze** the data from your devices by using the [AWS IoT data services](aws-iot-how-it-works.md#aws-iot-components-data)\.

## How your devices and apps access AWS IoT<a name="aws-iot-interfaces"></a>

AWS IoT provides the following interfaces for [AWS IoT Tutorials](iot-tutorials.md):
+ **AWS IoT Device SDKs**—Build applications on your devices that send messages to and receive messages from AWS IoT\. For more information, see [AWS IoT Device SDKs, Mobile SDKs, and AWS IoT Device Client](iot-sdks.md)\.
+ **AWS IoT Core for LoRaWAN**—Connect and manage your long range WAN \(LoRaWAN\) devices and gateways by using [AWS IoT Core for LoRaWAN](connect-iot-lorawan.md)\.
+ **AWS Command Line Interface \(AWS CLI\)**—Run commands for AWS IoT on Windows, macOS, and Linux\. These commands allow you to create and manage thing objects, certificates, rules, jobs, and policies\. To get started, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\. For more information about the commands for AWS IoT, see [iot](https://docs.aws.amazon.com/cli/latest/reference/iot/index.html) in the *AWS CLI Command Reference*\.
+ **AWS IoT API**—Build your IoT applications using HTTP or HTTPS requests\. These API actions allow you to programmatically create and manage thing objects, certificates, rules, and policies\. For more information about the API actions for AWS IoT, see [Actions](https://docs.aws.amazon.com/iot/latest/apireference/API_Operations.html) in the *AWS IoT API Reference*\.
+ **AWS SDKs**—Build your IoT applications using language\-specific APIs\. These SDKs wrap the HTTP/HTTPS API and allow you to program in any of the supported languages\. For more information, see [AWS SDKs and Tools](http://aws.amazon.com/tools/#sdk)\.

You can also access AWS IoT through the [AWS IoT console](https://console.aws.amazon.com/iot/home), which provides a graphical user interface \(GUI\) through which you can configure and manage the thing objects, certificates, rules, jobs, policies, and other elements of your IoT solutions\.