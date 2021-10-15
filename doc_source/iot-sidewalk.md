# Amazon Sidewalk Integration for AWS IoT Core<a name="iot-sidewalk"></a>

[Amazon Sidewalk](https://www.amazon.com/Amazon-Sidewalk/b?ie=UTF8&node=21328123011) is a shared network that helps devices like Amazon Echo, Ring security cams, outdoor lights, motion sensors, and Tile trackers work better at home and beyond the front door\. When enabled, Amazon Sidewalk can support other Sidewalk devices in your community, and open the door to innovations such as locating items connected to Amazon Sidewalk\.

**Getting started with Amazon Sidewalk Integration for AWS IoT Core**  
With Amazon Sidewalk Integration for AWS IoT Core, you can add your Sidewalk device fleet to the AWS Cloud\. Use the following steps to get started\.

1. 

**Review the Sidewalk SDK and documentation**  
Learn more about Amazon Sidewalk and how your devices can use it\.

   1. Review the Amazon Sidewalk Quick Start Guide\.

   1. Download an SDK for Amazon Sidewalk\.

   1. Open the [Sidewalk Developer Service \(SDS\) console](http://developer.amazon.com/acs-devices/console/Sidewalk)\.

1. 

**Register your prototype device**  
In the SDS console, register your prototype device with Amazon Sidewalk\.

1. 

**Associate your Sidewalk Amazon ID with your AWS account**  
In the [AWS IoT console](https://console.aws.amazon.com/iot/home#/wireless/profiles/sidewalk/create), associate your Sidewalk Amazon ID with your AWS account\.

   Your Amazon Sidewalk devices appear in the **Sidewalk** tab of the **Devices** hub of the [AWS IoT console](https://console.aws.amazon.com/iot/home#/wireless/devices?tab=sidewalk)\.

1. 

**Complete the Amazon Sidewalk device configuration in the AWS IoT console**  
Create the destinations and rules your Sidewalk device needs to route and format the data for AWS services\.

After your Amazon Sidewalk devices are authenticated, their messages are sent to AWS IoT Core\. You can start developing your business applications on the AWS Cloud that uses the data from your Amazon Sidewalk devices\.

The following topics show how you can add Sidewalk devices and connect them to AWS IoT\. Before adding your devices, make sure that your AWS account has the required IAM permissions to perform the following procedures\.

**Topics**
+ [Add your Sidewalk account credentials](iot-sidewalk-add-credentials.md)
+ [Add a destination for your Sidewalk device](iot-sidewalk-add-destination.md)
+ [Create rules to process Sidewalk device messages](iot-sidewalk-create-rules.md)
+ [Connect your Sidewalk device and view uplink metadata format](iot-sidewalk-connect-uplink-metadata.md)