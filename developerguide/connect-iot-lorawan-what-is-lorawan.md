# What is LoRaWAN?<a name="connect-iot-lorawan-what-is-lorawan"></a>

The [LoRa Alliance](https://lora-alliance.org/about-lorawan) describes LoRaWAN as, *"a Low Power, Wide Area \(LPWA\) networking protocol designed to wirelessly connect battery operated ‘things’ to the internet in regional, national or global networks, and targets key Internet of Things \(IoT\) requirements such as bi\-directional communication, end\-to\-end security, mobility and localization services\."*\.

## LoRa and LoRaWAN<a name="connect-iot-lorawan-lora-and-lorawan"></a>

The LoRaWAN protocol is a Low Power Wide Area Networking \(LPWAN\) communication protocol that functions on LoRa\. The LoRaWAN specification is open so anyone can set up and operate a LoRa network\.

LoRa is a wireless audio frequency technology that operates in a license\-free radio frequency spectrum\. LoRa is a physical layer protocol that uses spread spectrum modulation and supports long\-range communication at the cost of a narrow bandwidth\. It uses a narrow band waveform with a central frequency to send data, which makes it robust to interference\.

## Characteristics of LoRaWAN technology<a name="connect-iot-lorawan-lorawan-characteristics"></a>
+ Long range communication up to 10 miles in line of sight\.
+ Long battery duration of up to 10 years\. For enhanced battery life, you can operate your devices in class A or class B mode, which requires increased downlink latency\.
+ Low cost for devices and maintenance\.
+ License\-free radio spectrum but region\-specific regulations apply\.
+ Low power but has a limited payload size of 51 bytes to 241 bytes depending on the data rate\. The data rate can be 0,3 Kbit/s – 27 Kbit/s data rate with a 222 maximal payload size\.

## Learn more about LoRaWAN<a name="connect-iot-lorawan-lorawan-learn-more"></a>

The following links contain helpful information about the LoRaWAN technology and about LoRa Basics Station, which is the software that runs on your LoRaWAN gateways for connecting end devices to AWS IoT Core for LoRaWAN\.
+ 

**[ The Things Fundamentals on LoRaWAN](https://www.thethingsnetwork.org/docs/lorawan/)**  
The Things Fundamentals on LoRaWAN contains an introductory video that covers the fundamentals of LoRaWAN and a series of chapters that'll help you learn about LoRa and LoRaWAN\.
+ 

**[ What is LoRaWAN](https://lora-alliance.org/resource_hub/what-is-lorawan/)**  
LoRa Alliance provides a technical overview of LoRa and LoRaWAN, including a summary of the LoRaWAN specifications in different Regions\.
+ 

**[ LoRa Basics Station](https://lora-developers.semtech.com/resources/tools/lora-basics/)**  
Semtech Corporation provides helpful concepts about LoRa basics for gateways and end nodes\. LoRa Basics Station, an open source software that runs on your LoRaWAN gateway, is maintained and distributed through Semtech Corporation's [ GitHub](https://github.com/lorabasics/basicstation) repository\. You can also learn about the LNS and CUPS protocols that describe how to exchange LoRaWAN data and perform configuration updates\.