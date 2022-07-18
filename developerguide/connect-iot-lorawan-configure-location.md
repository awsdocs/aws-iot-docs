# Configure position of wireless resources with AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-configure-location"></a>


|  | 
| --- |
| This feature is in preview release for AWS IoT Core for LoRaWAN and is subject to change\. | 

You can use AWS IoT Core for LoRaWAN to specify your static position data or use positioning solvers that can identify the position of your device in real time\. You can add or update the position information for either LoRaWAN devices or gateways, or both\. You specify the position information either when adding your device or gateway to AWS IoT Core for LoRaWAN or when editing the configuration details of your device or gateway\.

If you have Amazon Location Service, you can activate an Amazon Location map where the position of your resource will be displayed\. You can use the map to identify and track the position of your device or gateway\.

Using the position data, you can:
+ Activate solvers to identify and obtain the position of your LoRaWAN devices\.
+ Track and monitor your gateways and devices\.
+ Define AWS IoT rules that process any updates to the position data and routes it to other AWS services\. For a list of rule actions, see [AWS IoT rule actions](iot-rule-actions.md)\.
+ Create alerts and receive notifications to devices in case of any unusual activity by using the position data and Amazon SNS\.

## Positioning solvers: Semtech GNSS<a name="connect-iot-lorawan-location-solver"></a>

Positioning solvers can be used to identify the position of your LoRaWAN devices\. You can use this information to track and monitor your device\. In this preview release, you can use the global navigation satellite system \(GNSS\) solver provided by Semtech\.

Before you can activate the GNSS solver, first add your device to AWS IoT Core for LoRaWAN\. The LoRaWAN device must have the LoRa Edge chipset, which is a Semtech product for geolocation\. LoRa Edge is an ultra\-low power platform that integrates a long range LoRa transceiver, multi\-constellation GNSS scanner, and passive Wi\-Fi MAC scanner targeting geolocation applications\.

**Note**  
The GNSS solver can only be used with LoRaWAN devices that have the LoRa Edge chip\. It can't be used with LoRaWAN gateways\. For gateways, you can still specify the static position information and add a destination that defines the AWS IoT rule for routing the position data when it's updated\. 

When your LoRaWAN device sends an uplink message, if your device has the LoRa Edge chip and you've activated the Semtech GNSS solver, AWS IoT Core for LoRaWAN uses the solver to host the data on the LoRa Cloud\. The GNSS solver then retrieves the estimated device position based on the GNSS scan results from the transceivers\. For more information, see [LoRa Edge](https://www.semtech.com/products/wireless-rf/lora-edge) in the *Semtech documentation*\.

If you've activated a positioning solver, after the solver computes the position information, you'll see the accuracy information which indicates the difference between the position computed by the solver and the static position information that you entered\. As solvers can't be used for LoRaWAN gateways, the accuracy information will be reported as `0.0`\.

For more information about the uplink message format and the frequency ports that are used for the positioning solver, see [Frequency ports and format of uplink messages](connect-iot-lorawan-location-devices.md#connect-iot-lorawan-location-devices-uplink)\.

## Overview of positioning workflow<a name="connect-iot-lorawan-location-workflow"></a>

The following diagram shows how AWS IoT Core for LoRaWAN stores and updates the position information of your devices and gateways\.

![\[Image showing how AWS IoT Core for LoRaWAN can use your static position data and raw data to compute the position in real time.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-lorawan-lms-architecture.png)

1. 

**Specify static position of your resource**  
Specify the static position information of your device or gateway using the latitude and longitude coordinates\. You can also specify an optional altitude coordinate\. These coordinates are based on the WGS84 coordinate system\. For more information, see [ World Geodetic System \(WGS84\)](https://gisgeography.com/wgs84-world-geodetic-system/)\.

1. 

**Activate positioning solvers for devices**  
If you're using LoRaWAN devices that have the LoRa Edge chip, you can optionally activate positioning solvers to track your device position in real time\. When your device sends an uplink message, AWS IoT Core for LoRaWAN uses the solver to host the data on the LoRa Cloud\. The solver computes the device position based on scan results from the LoRa Edge transceivers\. 

1. 

**Add a destination to route position data**  
You can add a destination that describes the IoT rule for processing the device data and route the updated position information to Amazon Location Service\. It is then displayed on an Amazon Location map\.

## Configuring your resource position<a name="connect-iot-lorawan-location-how"></a>

You can configure the position of your resource using the AWS Management Console, the AWS IoT Wireless API, or the AWS CLI\. 

**Note**  
You can configure your resource position only when adding your gateway or device to AWS IoT Core for LoRaWAN\. This feature isn't available when onboarding your resource to AWS IoT Core for LoRaWAN\.

If your devices have the Semtech LoRa Edge chip, you can also use the GNSS positioning solver to compute the real\-time position information\. For your gateways, you can still enter the static position coordinates and use Amazon Location to track the gateway position on an Amazon Location map\.