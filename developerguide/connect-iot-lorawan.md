# AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan"></a>

The [LoRa Alliance](https://lora-alliance.org/about-lorawan) describes LoRaWAN as, *"a Low Power, Wide Area \(LPWA\) networking protocol designed to wirelessly connect battery operated ‘things’ to the internet in regional, national or global networks, and targets key Internet of Things \(IoT\) requirements such as bi\-directional communication, end\-to\-end security, mobility and localization services\."*\.

## What is LoRaWAN?<a name="connect-iot-lorawan-what-is-lorawan"></a>

The LoRaWAN protocol is a Low Power Wide Area Networking \(LPWAN\) protocol that functions on LoRa, which is a wireless audio frequency technology that operates in a license\-free radio frequency spectrum\. The LoRaWAN specification is open so anyone can set up and operate a LoRa network\. The LoRa technology supports long\-range communication at the cost of a narrow bandwidth\. It uses a narrow band waveform with a central frequency to send data, which makes it robust to interference\. Following are the characteristics of the LoRaWAN technology\.

**Characteristics of LoRaWAN technology**
+ Long range communication up to 10 miles in line of sight\.
+ Long battery duration of up to 10 years\. For enhanced battery life, you can operate your devices in Class A or Class B mode, which requires increased downlink latency\.
+ Low cost for devices and maintenance\.
+ License\-free radio spectrum but region\-specific regulations apply\.
+ Low power but has a limited payload size of 51 bytes to 241 bytes depending on the data rate\. The data rate can be 0,3 Kbit/s – 27 Kbit/s data rate with a 222 maximal payload size\.

## What AWS IoT Core for LoRaWAN can do<a name="connect-iot-lorawan-what-is-iot-lorawan"></a>

The LoRaWAN network architecture is deployed in a star of stars topology in which gateways relay information between end devices and the LoRaWAN Network Server \(LNS\)\.

AWS IoT Core for LoRaWAN helps you connect and manage wireless LoRaWAN \(low\-power long\-range Wide Area Network\) devices and replaces the need for you to develop and operate a LNS\. Long range WAN \(LoRaWAN\) devices and gateways can connect to AWS IoT Core by using AWS IoT Core for LoRaWAN\.

![\[Image showing how AWS IoT Core provides device endpoints to connect IoT devices to AWS IoT and service endpoints to connect apps and other services to AWS IoT Core.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-endpoints.png)

LoRaWAN devices communicate with AWS IoT Core through LoRaWAN gateways\.
+ **Devices**: LoRaWAN end devices connect to LoRaWAN gateways using the LoRa communication protocol\.
+ **Gateways**: LoRaWAN gateways connect to AWS IoT Core using LoRa BasicStation protocol over secure WebSockets\.
+ **AWS IoT Core for LoRaWAN**: AWS IoT Core for LoRaWAN manages the service and device policies that AWS IoT Core requires to manage and communicate with the LoRaWAN gateways and devices\. AWS IoT Core for LoRaWAN also manages the destinations that describe the AWS IoT rules that send device data to other services\.
+ **Apps and services**: AWS IoT rules send LoRaWAN device messages to other AWS services and can process the device messages to format the data for the services\.

## Learning resources for AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-training-learn-more"></a>

The following resources will help you get familiar with the LoRaWAN technology and AWS IoT Core for LoRaWAN\.

**Learn more about LoRaWAN**  
The following links contain helpful information about the LoRaWAN technology and about LoRa Basics Station, which is the software that runs on your LoRaWAN gateways for connecting end devices to AWS IoT Core for LoRaWAN\.
+ 

**[ The Things Fundamentals on LoRaWAN](https://www.thethingsnetwork.org/docs/lorawan/)**  
The Things Fundamentals on LoRaWAN contains an introductory video that covers the fundamentals of LoRaWAN and a series of chapters that'll help you learn about LoRa and LoRaWAN\.
+ 

**[ What is LoRaWAN](https://lora-alliance.org/resource_hub/what-is-lorawan/)**  
LoRa Alliance provides a technical overview of LoRa and LoRaWAN including a summary of the LoRaWAN specifications in different regions\.
+ 

**[ LoRa Basics Station](https://lora-developers.semtech.com/resources/tools/lora-basics/)**  
Semtech Corporation provides helpful concepts about LoRa basics for gateways and end nodes\. LoRa Basics Station, an open source software that runs on your LoRaWAN gateway, is maintained and distributed via Semtech Corporation's [ GitHub](https://github.com/lorabasics/basicstation) repository\. You can also learn about the LNS and CUPS protocols that describe how to exchange LoRaWAN data and perform configuration updates\.

**AWS IoT Core for LoRaWAN Resources**  
The following links contain helpful information about getting started with AWS IoT Core for LoRaWAN and how it can help you build IoT solutions\.
+ 

**[ Getting Started with AWS IoT Core for LoRaWAN](https://www.youtube.com/watch?v=6-ZrdRjqdTk/)**  
 The following video describes how AWS IoT Core for LoRaWAN works and walks you through the process of adding LoRaWAN gateways from the AWS Management Console\.
+ 

**[AWS IoT Core for LoRaWAN workshop](https://iotwireless.workshop.aws/en/)**  
The workshop covers fundamentals of LoRaWAN technology and their implementation with AWS IoT Core for LoRaWAN\. You can also use the workshop to walk through labs that show how to connect your gateway and device to AWS IoT Core for LoRaWAN to building a sample IoT solution\.