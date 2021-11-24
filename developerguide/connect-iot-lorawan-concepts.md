# What is AWS IoT Core for LoRaWAN?<a name="connect-iot-lorawan-concepts"></a>

AWS IoT Core for LoRaWAN replaces a private LoRaWAN network server \(LNS\) by connecting your LoRaWAN devices and gateways to AWS\. Using the AWS IoT rules engine, you can route messages received from LoRaWAN devices, where they can be formatted and sent to other AWS IoT services\. To secure device communications with AWS IoT, AWS IoT Core for LoRaWAN uses X\.509 certificates\.

AWS IoT Core for LoRaWAN manages the service and device policies that AWS IoT Core requires to communicate with the LoRaWAN gateways and devices\. AWS IoT Core for LoRaWAN also manages the destinations that describe the AWS IoT rules that send device data to other services\.

With AWS IoT Core for LoRaWAN, you can:
+ Onboard and connect LoRaWAN devices and gateways to AWS IoT without the need to set up and manage a private LNS\.
+ Connect LoRaWAN devices that comply to 1\.0\.x or 1\.1 LoRaWAN specifications standardized by LoRa Alliance\. These devices can operate in class A, class B, or class C mode\.
+ Use LoRaWAN gateways that support LoRa Basics Station version 2\.0\.4 or later\. All gateways that are qualified for AWS IoT Core for LoRaWAN run a compatible version of LoRa Basics Station\.
+ Monitor signal strength, bandwidth, and spreading factor by using AWS IoT Core for LoRaWAN's adaptive data rate, and optimize the data rate if needed\.
+ Update LoRaWAN gateways' firmware using the CUPS service and the firmware of LoRaWAN devices using Firmware Updates Over\-The\-Air \(FUOTA\)\.

**Topics**
+ [What is LoRaWAN?](connect-iot-lorawan-what-is-lorawan.md)
+ [How AWS IoT Core for LoRaWAN works](connect-iot-lorawan-what-is-iot-lorawan.md)