# Amazon Sidewalk Integration for AWS IoT Core<a name="iot-sidewalk"></a>

[Amazon Sidewalk](https://www.amazon.com/Amazon-Sidewalk/b?ie=UTF8&node=21328123011) is a shared network that helps devices like Amazon Echo, Ring security cams, outdoor lights, motion sensors, and tile trackers work better at home and beyond the front door\. When enabled, this network can support other Sidewalk devices in your community, and open the door to innovations such as locating items connected to Amazon Sidewalk\. Amazon Sidewalk helps your devices get connected and stay connected\. For example, if your device loses its Wi\-Fi connection, Sidewalk can simplify reconnecting to your router\. For more information, see [Amazon Sidewalk Quick Start Guide](https://developer.amazon.com/acs-devices/console/sidewalk/docs/group__quickstart__guide.html)\.

The following sections show how to onboard your Sidewalk devices with AWS IoT and use event notifications to notify you of events such as when your Sidewalk device is registered\. For information about using Amazon CloudWatch to monitor your Sidewalk devices, see [Monitoring and logging for AWS IoT Wireless using Amazon CloudWatch](connect-iot-lorawan-logging-monitoring.md)\.

## How to onboard your Sidewalk device<a name="iot-sidewalk-how-use"></a>

You can onboard your Sidewalk devices to AWS IoT by using the console or the AWS IoT Wireless API\. After your Amazon Sidewalk devices are authenticated, their messages are sent to AWS IoT Core\. You can then start developing your business applications on the AWS Cloud, which uses the data from your Amazon Sidewalk devices\.

**Using the console**  
To onboard your Sidewalk devices by using the AWS Management Console, first register your device in the [Sidewalk Developer Service \(SDS\) console](http://developer.amazon.com/acs-devices/console/Sidewalk) account and then associate your Amazon ID with your AWS account\. To see the Sidewalk devices you've added and manage them, sign in to the AWS Management Console and navigate to the [Devices](https://console.aws.amazon.com/https://console.aws.amazon.com/iot/home#/wireless/devices) page on the AWS IoT console\.

**Using the API or CLI**  
You can onboard both Sidewalk and LoRaWAN devices by using the [AWS IoT Wireless](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/) API\. The AWS IoT Wireless API that AWS IoT Core is built on is supported by the AWS SDK\. For more information, see [AWS SDKs and Toolkits](https://aws.amazon.com/getting-started/tools-sdks/)\. 

You can use the AWS CLI to run commands for onboarding and managing your Sidewalk devices\. For more information, see [AWS IoT Wireless CLI reference](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/index.html)\. 