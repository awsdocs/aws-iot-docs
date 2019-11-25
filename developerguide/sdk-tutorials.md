# Using the AWS IoT SDKs on a Raspberry Pi<a name="sdk-tutorials"></a>

The AWS IoT Device SDKs help you to connect your devices to AWS IoT easily and quickly\. The AWS IoT Device SDKs include open\-source libraries, developer guides with samples, and porting guides so that you can build innovative IoT products or solutions on your choice of hardware platforms\.

**Important**  
Before you start this tutorial, complete the steps in [Getting Started with AWS IoT](iot-gs.md)\. 

These tutorials provide step\-by\-step instructions for connecting your Raspberry Pi to the [Message Broker for AWS IoT](iot-message-broker.md) using the AWS IoT Device SDK for Embedded C and the AWS IoT Device SDK for JavaScript\. After you complete the steps in these tutorials, you can connect to the AWS IoT platform and run the sample applications included with the AWS IoT Device SDKs\.

**Topics**
+ [Prerequisites](#iot-sdk-prereqs)
+ [Create an AWS IoT Thing for Your Raspberry Pi](#iot-sdk-create-thing)
+ [SDK Sample Configuration](#iot-sdk-config)
+ [Using the AWS IoT Device SDK for Embedded C](iot-embedded-c-sdk.md)
+ [Using the AWS IoT Device SDK for JavaScript and Node](iot-device-sdk-node.md)

## Prerequisites<a name="iot-sdk-prereqs"></a>

To complete this tutorial, you need the following:
+ A [Raspberry Pi 2 Model B](https://www.raspberrypi.org/help/quick-start-guide/) or more recent model\.
+ [Raspbian Wheezy](http://archive.raspbian.org/raspbian/dists/) or later\. We recommend using the latest version of Raspbian, which is available from the [Raspberry Pi website](https://www.raspberrypi.org/downloads/raspbian/)\.
+ An ethernet or Wi\-Fi connection\.
+ An AWS account\. If you don't already have an AWS account, you can get one for free by going to the [Amazon AWS Getting Started Resource Center](https://aws.amazon.com)\.

## Create an AWS IoT Thing for Your Raspberry Pi<a name="iot-sdk-create-thing"></a>

A *thing* represents a device whose state is stored in the AWS Cloud\. The device's state is stored in a JSON document known as the device's * shadow*\. The shadow is used to both store and retrieve state information\. The [Device Shadow service](iot-device-shadows.html) maintains a shadow for each device that is registered in AWS IoT\.

**To register your Raspberry Pi with AWS IoT**

1. On your Raspberry Pi, browse to the AWS IoT console\. 

1. In the navigation pane, choose **Secure**, and the choose **Policies**\. 

1. On the **Policies** page, choose **Create a policy**\.

1. On the **Create a policy** page:

   1. Enter a name for the policy \(for example, **RaspberryPi\-Policy**\)\.

   1. For **Action**, enter **iot:\***\.

   1. For **Resource ARN**, enter **\***\.

   1. Under **Effect**, choose **Allow**, and then choose **Create**\.

      This policy allows your Raspberry Pi to publish messages to AWS IoT\.
**Important**  
These settings are overly permissive\. In a production environment narrow the scope of the permissions to that which are required by your device\. For more information, see [Authorization](iot-authorization.md)\.

1. In the console navigation pane, choose **Manage**, and then choose **Things**\.

1. Choose **Create**\.

1. On the **Creating AWS IoT things** page, choose **Create a single thing**\.

1. On the **Add your device to the device registry** page, enter **RaspberryPi**, and then choose **Next**\.

1. On the **Add a certificate for your thing** page, choose **Create certificate**\.

1. On the **Certificate created** page, download your private key, device certificate, and a root certificate authority \(CA\) for AWS IoT\. \(Choose the **Download** link for each\.\) These files are saved in your `/home/pi/Downloads` directory\. 

1. Choose **Activate** to activate the X\.509 certificate, and then choose **Attach a policy**\.

1. On the **Add a policy for your thing** page, choose **RaspberryPi\-policy** and then choose **Register Thing**\.

## SDK Sample Configuration<a name="iot-sdk-config"></a>