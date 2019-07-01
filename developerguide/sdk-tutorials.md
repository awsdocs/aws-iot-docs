# AWS IoT SDK Tutorials<a name="sdk-tutorials"></a>

The AWS IoT Device SDKs help you to connect your devices to AWS IoT easily and quickly\. The AWS IoT Device SDKs include open\-source libraries, developer guides with samples, and porting guides so that you can build innovative IoT products or solutions on your choice of hardware platforms\.

**Important**  
Before going through this tutorial, please go through the [Getting Started with AWS IoT](iot-gs.md)\. 

These tutorials provide step\-by\-step instructions for connecting your Raspberry Pi to the [Message Broker for AWS IoT](iot-message-broker.md), using with the AWS IoT Device SDK for Embedded C and the AWS IoT Device SDK for JavaScript\. After following these instructions, you can connect to the AWS IoT platform and run the sample applications included with the AWS IoT Device SDKs\.

**Topics**
+ [Prerequisites](#iot-sdk-prereqs)
+ [Create an AWS IoT Thing for Your Raspberry Pi](#iot-sdk-create-thing)
+ [Using the AWS IoT Embedded C SDK](iot-embedded-c-sdk.md)
+ [Using the AWS IoT Device SDK for JavaScript](iot-device-sdk-node.md)

## Prerequisites<a name="iot-sdk-prereqs"></a>

This tutorial requires the following:
+ A [Raspberry Pi 2 Model B](https://www.raspberrypi.org/help/quick-start-guide/), or newer\.
+ [Raspbian Wheezy](http://archive.raspbian.org/raspbian/dists/) OS, or newer\.
+ [Chromium](https://www.chromium.org/getting-involved/download-chromium) or [Iceweasel](https://packages.debian.org/search?keywords=iceweasel) web browser\.
+ Your Raspberry Pi must be connected to the internet using a WiFi or ethernet connection\.
+ An AWS account\. If you don't already have an AWS account, you can get one for free by going to the [Amazon AWS Getting Started Resource Center](https://aws.amazon.com)\.

## Create an AWS IoT Thing for Your Raspberry Pi<a name="iot-sdk-create-thing"></a>

A *thing* represents a device whose status or data is stored in the AWS cloud\. The device's status or data is stored in a JSON document known as the device's *shadow*\. The shadow is used to both store and retrieve state information\. The [Device Shadow service](iot-device-shadows.html) maintains a shadow for each device that is connected to AWS IoT\.

**Create an AWS IoT Thing**

1. On your Raspberry Pi, open a web browser and go to the [AWS IoT console](https://console.aws.amazon.com/iot/home)\. You may be prompted to sign in\.

1. In the [AWS IoT console](https://console.aws.amazon.com/iot/home), you see the **Monitor** page\. In the navigation pane, choose **Manage**\.  
![\[The AWS IoT console\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/monitor-page.png)

1. Choose **Create**\.  
![\[Create an AWS IoT thing\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/sdk-create-thing.png)

1. On the **Creating AWS IoT things** page, choose **Create a single thing**\.  
![\[Create a single thing\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/create-single-thing-new.png)

1. On the **Add your device to the device registry** page, enter **MyRaspberryPi** for the device **Name**\. Leave the default values for all the other fields, and then choose **Next**\.  
![\[Add your device to the device registry\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/add-device.png)

1. On the **Add a certificate for your thing** page, choose **Create certificate**\. This generates an X\.509 certificate and key pair\.  
![\[Add a certificate for your thing\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-create-cert.png)

1. On the **Certificate created\!** page, download your public and private keys, certificate, and root certificate authority \(CA\)\. Save them on your Raspbery Pi, you will copy them to a different directory later on in this tutorial\. Choose **Activate** to activate the X\.509 certificate, and then choose **Attach a policy**\.  
![\[Certificate created!\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/sdk-attach-policy.png)

1. On the **Add a policy for your thing** page, choose **Register Thing**\.

   After you register your thing, you will need to create and attach a new policy to the certificate\.  
![\[Register thing\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/add-policy-for-thing.png)

1. On the AWS IoT console, in the navigation pane, choose **Secure** and **Policies**\. In the **Policies** page, choose **Create a policy**\.  
![\[Policies\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/raspi-create-policy.png)

1. On the **Create a policy** page:

   1. Enter a **Name** for the policy\. For **Action**,

   1. enter **iot:\***\. For **Resource ARN**, enter **\***\.

   1. Under **Effect**, choose **Allow**, and then choose **Create**\.

      This policy allows your Raspberry Pi to publish messages to AWS IoT\.
**Important**  
These settings are overly permissive\. In a production environment narrow the scope of the permissions to that which are required by your device\. For more information, see [Authorization](authorization.md)\.  
![\[Create policy\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/create-policy-raspberry.png)

1. In the AWS IoT console, choose **Manage** and **Things**\. On the **Things** page, choose **MyRaspberryPi**\.  
![\[Manage MyRaspberryPi\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/choose-thing-raspberry.png)

1. On the thing's **Details** page, in the left navigation pane, choose **Interact**\.  
![\[MyRaspberryPi details\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/thing-details-interact-raspberry.png)

1. Make a note of the REST API endpoint\. You will need it to connect to AWS IoT\. In the navigation pane, choose **Security**\.  
![\[MyRaspberryPi interact\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/thing-rest-api-raspberry.png)

1. Choose the certificate that you created earlier\.  
![\[MyRaspberryPi security\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/choose-certificates-raspberry.png)

1. On the certificate's **Details** page, in **Actions**, choose **Attach policy**\.  
![\[Certificate details\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/attach-policy-to-cert-raspberry.png)

1. On the **Attach policies to certificate\(s\)** page, choose the policy you created, and then choose **Attach**\.  
![\[Attach policy to certificate\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/attach-policy-to-cert-raspberry-2.png)