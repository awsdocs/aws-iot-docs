# How AWS IoT Core for LoRaWAN works<a name="connect-iot-lorawan-what-is-iot-lorawan"></a>

The LoRaWAN network architecture is deployed in a star of stars topology in which gateways relay information between end devices and the LoRaWAN network server \(LNS\)\.

AWS IoT Core for LoRaWAN helps you connect and manage wireless LoRaWAN \(low\-power long\-range Wide Area Network\) devices and replaces the need for you to develop and operate an LNS\. Long range WAN \(LoRaWAN\) devices and gateways can connect to AWS IoT Core by using AWS IoT Core for LoRaWAN\.

The following shows how a LoRaWAN device interacts with AWS IoT Core for LoRaWAN\. It also shows how AWS IoT Core for LoRaWAN replaces an LNS and communicates with other AWS services in the AWS Cloud\.

![\[Image showing how AWS IoT Core provides device endpoints to connect IoT devices to AWS IoT and service endpoints to connect apps and other services to AWS IoT Core.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-lorawan-how-it-works.png)

LoRaWAN devices communicate with AWS IoT Core through LoRaWAN gateways\. AWS IoT Core for LoRaWAN manages the service and device policies that AWS IoT Core requires to manage and communicate with the LoRaWAN gateways and devices\. AWS IoT Core for LoRaWAN also manages the destinations that describe the AWS IoT rules that send device data to other services\.

## Get started using AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-get-started-resources"></a>

1. 

**Select the wireless devices and LoRaWAN gateways that you'll need\.**  
The [AWS Partner Device Catalog](https://devices.amazonaws.com/search?page=1&sv=iotclorawan) contains gateways and developer kits that are qualified for use with AWS IoT Core for LoRaWAN\. For more information, see [Using qualified gateways from the AWS Partner Device Catalog](connect-iot-lorawan-manage-gateways.md#connect-iot-lorawan-qualified-gateways)\. 

1. 

**Add your wireless devices and LoRaWAN gateways to AWS IoT Core for LoRaWAN\.**  
[Connecting gateways and devices to AWS IoT Core for LoRaWAN](connect-iot-lorawan-getting-started.md) gives you information about how to describe your resources and add your wireless devices and LoRaWAN gateways to AWS IoT Core for LoRaWAN\. You'll also learn how to configure the other AWS IoT Core for LoRaWAN resources that you'll need to manage these devices and send their data to AWS services\.

1. 

**Complete your AWS IoT Core for LoRaWAN solution\.**  
Start with [our sample AWS IoT Core for LoRaWAN solution](https://github.com/aws-samples/aws-iot-core-lorawan) and make it yours\.

## AWS IoT Core for LoRaWAN resources<a name="connect-iot-lorawan-iot-lorawan-learn-more"></a>

The following resources will help you get familiar with the LoRaWAN technology and AWS IoT Core for LoRaWAN\.
+ 

**[ Getting Started with AWS IoT Core for LoRaWAN](https://www.youtube.com/watch?v=6-ZrdRjqdTk/)**  
 The following video describes how AWS IoT Core for LoRaWAN works and walks you through the process of adding LoRaWAN gateways from the AWS Management Console\.
+ 

**[AWS IoT Core for LoRaWAN workshop](https://iotwireless.workshop.aws/en/)**  
The workshop covers fundamentals of LoRaWAN technology and its implementation with AWS IoT Core for LoRaWAN\. You can also use the workshop to walk through labs that show how to connect your gateway and device to AWS IoT Core for LoRaWAN for building a sample IoT solution\.