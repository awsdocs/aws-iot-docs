# Connecting to AWS IoT Core<a name="connect-to-iot"></a>

 AWS IoT Core supports connections with IoT devices, services, and apps\. Devices connect to the AWS IoT Core so they can send data to and receive data from AWS IoT services and other devices\. Apps and other services also connect to AWS IoT Core to control and manage the IoT devices and process the data from your IoT solution\. This section describes how to choose the best way to connect and communicate with AWS IoT Core for each aspect of your IoT solution\.

![\[Image showing how AWS IoT Core provides device endpoints to connect IoT devices to AWS IoT and service endpoints to connect apps and other services to AWS IoT Core.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-endpoints.png)

In general, the two ways to interact with AWS IoT are through the [AWS IoT Core service endpoints](#iot-service-endpoint-intro) and the [AWS IoT device endpoints](#iot-device-endpoint-intro)\.

## AWS IoT Core service endpoints<a name="iot-service-endpoint-intro"></a>

The AWS IoT Core service endpoints provide access to functions that control and manage your AWS IoT solution\.
+ 

**Endpoints**  
The AWS IoT Core service endpoints are Region specific and are listed in [AWS IoT Core Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html)\. The formats of the AWS IoT Core service endpoints are as follows\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/connect-to-iot.html)
+ 

**SDKs and tools**  
The [AWS SDKs](https://aws.amazon.com/tools/#SDKs) provide language\-specific support for the AWS IoT Core APIs, and the APIs of other AWS services\. The [AWS Mobile SDKs](https://aws.amazon.com/tools/#Mobile_SDKs) provide app developers with platform\-specific support for the AWS IoT Core API, and other AWS services on mobile devices\. 

  The [AWS CLI](https://aws.amazon.com/cli/) provides command\-line access to the functions provided by the AWS IoT service endpoints\. [AWS Tools for PowerShell](https://aws.amazon.com/powershell/) provides tools to manage AWS services and resources in the PowerShell scripting environment\.
+ 

**Authentication**  
The service endpoints use IAM users and AWS credentials to authenticate users\.
+ 

**Learn more**  
For more information and links to SDK references, see [Connecting to AWS IoT Core service endpoints](iot-connect-service.md)\.

## AWS IoT device endpoints<a name="iot-device-endpoint-intro"></a>

The AWS IoT device endpoints support communication between your IoT devices and AWS IoT\.
+ 

**Endpoints**  
The device endpoints are specific to your account and you can see what they are by using the [describe\-endpoint](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/describe-endpoint.html) command\.

  For each account, AWS IoT supports the device endpoints listed in the following table\. For more information about the functions that these endpoints support, see [AWS IoT device data and service endpoints](iot-connect-devices.md#iot-connect-device-endpoints)\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/connect-to-iot.html)
+ 

**SDKs**  
The [AWS IoT Device SDKs](iot-connect-devices.md#iot-connect-device-sdks) provide language\-specific support for the Message Queueing Telemetry Transport \(MQTT\) and WebSocket Secure \(WSS\) protocols, which devices use to communicate with AWS IoT\. [AWS Mobile SDKs](iot-connect-service.md#iot-connect-mobile-sdks) also provide support for MQTT device communications, AWS IoT APIs, and the APIs of other AWS services on mobile devices\.
+ 

**Authentication**  
The device endpoints use X\.509 certificates or AWS IAM users with credentials to authenticate users\.
+ 

**Learn more**  
For more information and links to SDK references, see [AWS IoT Device SDKs](iot-connect-devices.md#iot-connect-device-sdks)\.