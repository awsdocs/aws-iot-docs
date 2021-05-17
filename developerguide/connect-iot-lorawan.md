# Connecting devices and gateways to AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan"></a>

Long range WAN \(LoRaWAN\) devices and gateways can connect to AWS IoT Core by using AWS IoT Core for LoRaWAN\. The [LoRa Alliance](https://lora-alliance.org/about-lorawan) describes LoRaWAN as, *"a Low Power, Wide Area \(LPWA\) networking protocol designed to wirelessly connect battery operated ‘things’ to the internet in regional, national or global networks, and targets key Internet of Things \(IoT\) requirements such as bi\-directional communication, end\-to\-end security, mobility and localization services\."* 

LoRaWAN devices communicate with AWS IoT Core through LoRaWAN gateways\. AWS IoT rules send LoRaWAN device messages to other AWS services and can process the device messages to format the data for the services\. 

AWS IoT Core for LoRaWAN manages the service and device policies that AWS IoT Core requires to manage and communicate with the LoRaWAN gateways and devices\. AWS IoT Core for LoRaWAN also manages the destinations that describe the AWS IoT rules that send device data to other services\.

**To get started using AWS IoT Core for LoRaWAN**
+ 

**Select the wireless devices and LoRaWAN gateways that you'll need**  
The [AWS Partner Device Catalog](https://devices.amazonaws.com/search?page=1&sv=iotclorawan) contains gateways and developer kits that are qualified for use with AWS IoT Core for LoRaWAN\.
+ 

**Add your wireless devices and LoRaWAN gateways to AWS IoT Core for LoRaWAN**  
[Adding LoRaWAN gateways and devices](#connect-iot-lorawan-getting-started-overview) describes how to add your wireless devices and LoRaWAN gateways to AWS IoT Core for LoRaWAN\. You'll also learn how to configure the other AWS IoT Core for LoRaWAN resources that you'll need to manage these devices and send their data to AWS services\.
+ 

**Complete your AWS IoT Core for LoRaWAN solution**  
Start with [our sample AWS IoT Core for LoRaWAN solution](https://github.com/aws-samples/aws-iot-core-lorawan) and make it yours\.

**Topics**
+ [Adding LoRaWAN gateways and devices](#connect-iot-lorawan-getting-started-overview)
+ [Using the console to add AWS IoT Core for LoRaWAN resources](connect-iot-lorawan-console.md)
+ [Using the API to manage AWS IoT Core for LoRaWAN resources](connect-iot-lorawan-developer.md)
+ [Data security with AWS IoT Core for LoRaWAN](connect-iot-lorawan-security.md)

## Adding LoRaWAN gateways and devices<a name="connect-iot-lorawan-getting-started-overview"></a>

If you're using AWS IoT Core for LoRaWAN for the first time, you can add your first LoRaWAN gateway and device by using the [AWS IoT Core for LoRaWAN](https://console.aws.amazon.com/iot/home#/wireless/landing) Intro page of the AWS IoT console and choosing **Get started**\.

Whether you [use the console](connect-iot-lorawan-console.md) or [use the API](connect-iot-lorawan-developer.md) to add your AWS IoT Core for LoRaWAN resources, consider the following topics before you get started\. Adding the resources can be easier when you have the following information ready before you start\.

1. 

**The naming conventions for your devices, gateways, profiles, and destinations**  
AWS IoT Core for LoRaWAN assigns unique IDs to the resources you create for wireless devices, gateways, and profiles; however, you can also give your resources more descriptive names to make it easier to identify them\. Before you add devices, gateways, profiles, and destinations to AWS IoT Core for LoRaWAN, consider how you'll name them to make them easier to manage\.

   You can also add tags to the resources you create\. Before you add your LoRaWAN devices, consider how you might use tags to identify and manage your AWS IoT Core for LoRaWAN resources\. Tags can be modified after you add them\. 

   For more information about naming and tagging, see [Describe your AWS IoT Core for LoRaWAN resources](connect-iot-lorawan-describe-resource.md)\.

1. 

**The selection of LoRa frequency bands for your gateways and device connection**  
AWS IoT Core for LoRaWAN supports EU863\-870, US902\-928, AU915, and AS923\-1 frequency bands, which you can use to connect your gateways and devices that are physically present in countries that support the frequency ranges and characteristics of these bands\. The EU863\-870 and US902\-928 bands are commonly used in Europe and North America, respectively\. The AS923\-1 band is commonly used in Australia, New Zealand, Japan, and Singapore among other countries\. The AU915 is used in Australia and Argentina among other countries\. To learn more about which frequency band to use in your region or country, see [ LoRaWAN® Regional Parameters](https://lora-alliance.org/resource_hub/rp2-101-lorawan-regional-parameters-2/)\.

   LoRa Alliance publishes LoRaWAN specifications and regional parameter documents that are available for download from the LoRa Alliance website\. The LoRa Alliance regional parameters help companies decide which frequency band to use in their region or country\. AWS IoT Core for LoRaWAN's frequency band implementation follows the recommendation in the regional parameters specification document\. These regional parameters are grouped into a set of radio parameters, along with a frequency allocation that is adapted to the Industrial, Scientific, and Medical \(ISM\) band\. We recommend that you work with the compliance teams to ensure that you meet any applicable regulatory requirements\. 

1. 

**The configuration parameters of the wireless devices**  
Some wireless device configuration parameters are shared by many devices and can be stored in AWS IoT Core for LoRaWAN as device and service profiles\. Collecting these parameters in advance can make it easier to identify and enter them\.
   + Wireless device configuration parameters include: the device's EUI, application and session security keys, and device profile settings such as data rate and channel information\.
   + Wireless gateway configuration parameters include: the gateway's EUI and its LoRa frequency band\.
   + Refer to the documentation about each device that its vendor provides for the complete listing of its specifications and configuration parameters\.

   Having the configuration parameters that are unique to each device ready to enter in advance makes entering the data into the console go more smoothly\. The specific parameters that you need to enter depend on the LoRaWAN specification that the device uses\.

1. 

**The configuration parameters of the LoRaWAN gateways**  
Having the configuration parameters that are unique to each gateway ready to enter in advance makes entering the data into the console go more smoothly\.

1. 

**The mapping of the device data to service data**  
The data from LoRaWAN wireless devices is often encoded to optimize bandwidth\. These encoded messages arrive at AWS IoT Core for LoRaWAN in a format that might not be easily used by other AWS services\. AWS IoT Core for LoRaWAN uses AWS IoT rules that can use AWS Lambda functions to process and decode the device messages to a format that other AWS services can use\.

   To transform device data and send it to other AWS services, you need to know:
   + The format and contents of the data that the wireless devices send\.
   + The service to which you want to send the data\.
   + The format that service requires\.

   Using that information, you can create the AWS IoT rule that performs the conversion and sends the converted data to the AWS services that will use it\.