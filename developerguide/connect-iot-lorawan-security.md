# Data security with AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-security"></a>

Two methods secure the data from your AWS IoT Core for LoRaWAN devices:
+ 

**The security that wireless devices use to communicate with the gateways\.**  
The LoRaWAN devices follow the security practices described in [LoRaWAN ™ SECURITY: A White Paper Prepared for the LoRa Alliance™ by Gemalto, Actility, and Semtech](https://lora-alliance.org/sites/default/files/2019-05/lorawan_security_whitepaper.pdf) to communicate with the gateways\. 
+ 

**The security that AWS IoT Core uses to connect gateways to AWS IoT Core for LoRaWAN and send the data to other AWS services\.**  
AWS IoT Core security is described in [Data protection in AWS IoT Core](data-protection.md)\.

## How data is secured throughout the system<a name="connect-iot-lorawan-security-data-how"></a>

This diagram identifies the key elements in a LoRaWAN system connected to AWS IoT Core for LoRaWAN to identify how data is secured throughout\.

![\[Image showing how AWS IoT Core for LoRaWAN data is passed from a wireless device to AWS IoT and other services.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-lorawan-data-flow.png)

1. The LoRaWAN wireless device encrypts its binary messages using AES128 CTR mode before it transmits them\.

1. Gateway connections to AWS IoT Core for LoRaWAN are secured by TLS as described in [Transport security in AWS IoT](transport-security.md)\. AWS IoT Core for LoRaWAN decrypts the binary message and encodes the decrypted binary message payload as a base64 string\.

1. The resulting base64\-encoded message is sent as the message payload to the AWS IoT rule described in the destination assigned to the device\. Data within AWS is encrypted using AWS\-owned keys\.

1. The AWS IoT rule directs the message data to the services described in the rule's configuration\. Data within AWS is encrypted using AWS\-owned keys\.

## LoRaWAN device and gateway transport security<a name="connect-iot-lorawan-security-devices"></a>

LoRaWAN devices and AWS IoT Core for LoRaWAN store pre\-shared root keys\. Session keys are derived by both LoRaWAN devices and AWS IoT Core for LoRaWAN following the protocols\. The symmetric session keys are used for encryption and decryption in a standard AES\-128 CTR mode\. A 4\-byte message integrity code \(MIC\) is also used to check the data integrity following a standard AES\-128 CMAC algorithm\. The session keys can be updated by using the Join/Rejoin process\.

 The security practice for LoRa gateways is described in the LoRaWAN specifications\. LoRa gateways connect to AWS IoT Core for LoRaWAN through a web socket using a [https://lora-developers.semtech.com/resources/tools/lora-basics/lora-basics-for-gateways/](https://lora-developers.semtech.com/resources/tools/lora-basics/lora-basics-for-gateways/)\. AWS IoT Core for LoRaWAN supports only `Basics Station` version 2\.0\.4 and later\.

Before the web socket connection is established, AWS IoT Core for LoRaWAN uses the [TLS Server and Client Authentication mode](transport-security.md) to authenticate the gateway\. AWS IoT Core for LoRaWAN also maintains a Configuration and Update Server \(CUPS\) that configures and updates the certificates and keys used for TLS authentication\. 