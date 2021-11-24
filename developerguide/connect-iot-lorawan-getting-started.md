# Connecting gateways and devices to AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-getting-started"></a>

AWS IoT Core for LoRaWAN helps you connect and manage wireless LoRaWAN \(low\-power long\-range Wide Area Network\) devices and replaces the need for you to develop and operate a LNS\. Long range WAN \(LoRaWAN\) devices and gateways can connect to AWS IoT Core by using AWS IoT Core for LoRaWAN\.

## Naming conventions for your devices, gateways, profiles, and destinations<a name="connect-iot-lorawan-naming-convention"></a>

Before you get started with AWS IoT Core for LoRaWAN and creating the resources, consider the naming convention of your devices, gateways, and destination\.

AWS IoT Core for LoRaWAN assigns unique IDs to the resources you create for wireless devices, gateways, and profiles; however, you can also give your resources more descriptive names to make it easier to identify them\. Before you add devices, gateways, profiles, and destinations to AWS IoT Core for LoRaWAN, consider how you'll name them to make them easier to manage\.

You can also add tags to the resources you create\. Before you add your LoRaWAN devices, consider how you might use tags to identify and manage your AWS IoT Core for LoRaWAN resources\. Tags can be modified after you add them\. 

For more information about naming and tagging, see [Describe your AWS IoT Core for LoRaWAN resources](connect-iot-lorawan-describe-resource.md)\.

## Mapping of device data to service data<a name="connect-iot-lorawan-service-device-data"></a>

The data from LoRaWAN wireless devices is often encoded to optimize bandwidth\. These encoded messages arrive at AWS IoT Core for LoRaWAN in a format that might not be easily used by other AWS services\. AWS IoT Core for LoRaWAN uses AWS IoT rules that can use AWS Lambda functions to process and decode the device messages to a format that other AWS services can use\.

To transform device data and send it to other AWS services, you need to know:
+ The format and contents of the data that the wireless devices send\.
+ The service to which you want to send the data\.
+ The format that service requires\.

Using that information, you can create the AWS IoT rule that performs the conversion and sends the converted data to the AWS services that will use it\.

## Using the console to onboard your device and gateway to AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-console"></a>

You can use the console interface or the API to add your LoRaWAN gateway and devices\. If you're using AWS IoT Core for LoRaWAN for the first time, we recommend that you use the console\. The console interface is most practical when managing a few AWS IoT Core for LoRaWAN resources at a time\. When managing large numbers of AWS IoT Core for LoRaWAN resources, consider creating more automated solutions by using the AWS IoT Wireless API\.

Much of the data that you enter when configuring AWS IoT Core for LoRaWAN resources is provided by the devices' vendors and is specific to the LoRaWAN specifications they support\. The following topics describe how you can describe your AWS IoT Core for LoRaWAN resources and use the console or the API to add your gateways and devices\.

**Topics**
+ [Naming conventions for your devices, gateways, profiles, and destinations](#connect-iot-lorawan-naming-convention)
+ [Mapping of device data to service data](#connect-iot-lorawan-service-device-data)
+ [Using the console to onboard your device and gateway to AWS IoT Core for LoRaWAN](#connect-iot-lorawan-console)
+ [Describe your AWS IoT Core for LoRaWAN resources](connect-iot-lorawan-describe-resource.md)
+ [Onboard your gateways to AWS IoT Core for LoRaWAN](connect-iot-lorawan-onboard-gateways.md)
+ [Onboard your devices to AWS IoT Core for LoRaWAN](connect-iot-lorawan-onboard-end-devices.md)