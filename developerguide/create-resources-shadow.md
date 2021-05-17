# Create AWS IoT resources and connect Raspberry Pi to run shadow application<a name="create-resources-shadow"></a>

This tutorial demonstrates how to set up and configure a Raspberry Pi device and create the AWS IoT resources that a device requires to connect and exchange MQTT messages\.

**Note**  
If you're planning to [Create a virtual device with Amazon EC2](creating-a-virtual-thing.md), you can skip this page and continue to [Configure your device](configure-device.md)\. You'll create these resources when you create your virtual thing\. If you would like to use a different device instead of the Raspberry Pi, you can try to follow these tutorials by adapting them to a device of your choice\.

**In this tutorial, you'll learn how to:**
+ Set up a Raspberry Pi device and configure it for use with AWS IoT\.
+ Create an AWS IoT policy document, which authorizes your device to interact with AWS IoT services\.
+ Create a thing resource in AWS IoT the X\.509 device certificates, and then attach the policy document\.

  The thing is the virtual representation of your device in the AWS IoT registry\. The certificate authenticates your device to AWS IoT Core, and the policy document authorizes your device to interact with AWS IoT\.

**How to run this tutorial?**  
To run the `shadow.py` sample application for Device Shadows, you'll need a Raspberry Pi device that connects to AWS IoT\. We recommend that you follow this tutorial in the order it's presented here, starting with setting up the Raspberry Pi and it's accessories, and then creating a policy and attaching the policy to a thing resource that you create\. You can then follow this tutorial by using the graphical user interface \(GUI\) supported by the Raspberry Pi to open the AWS IoT console on the device's web browser, which also makes it easier to download the certificates directly to your Raspberry Pi for connecting to AWS IoT\.

**Before you start this tutorial, make sure that you have:**
+ An AWS account\. If you don't have one, complete the steps described in [Set up your AWS account](setting-up.md) before you continue\. You'll need your AWS account and AWS IoT console to complete this tutorial\. 
+ The Raspberry Pi and its necessary accessories\. You'll need: 
  + A [Raspberry Pi 3 Model B](https://www.raspberrypi.org/products/) or more recent model\. This tutorial might work on earlier versions of the Raspberry Pi, but we haven't tested it\.
  + [Raspberry Pi OS \(32\-bit\)](https://www.raspberrypi.org/downloads/raspberry-pi-os/) or later\. We recommend using the latest version of the Raspberry Pi OS\. Earlier versions of the OS might work, but we haven't tested it\.
  + An Ethernet or Wi\-Fi connection\.
  + Keyboard, mouse, monitor, cables, and power supplies\.

This tutorial takes about 30 minutes to complete\.

## Set up and configure Raspberry Pi device<a name="setup-device-shadow"></a>

In this section, we'll configure a Raspberry Pi device for use with AWS IoT\.

**Important**  
Adapting these instructions to other devices and operating systems can be challenging\. You'll need to understand your device well enough to be able to interpret these instructions and apply them to your device\. If you encounter difficulties, you might try one of the other device options as an alternative, such as [Create a virtual device with Amazon EC2](creating-a-virtual-thing.md) or [Use your Windows or Linux PC or Mac as an AWS IoT device](using-laptop-as-device.md)\. 

You'll need to configure your Raspberry Pi such that it can start the operating system \(OS\), connect to the internet, and allow you to interact with it at a command line interface\. You can also use the graphical user interface \(GUI\) supported with the Raspberry Pi to open the AWS IoT console and run the rest of this tutorial\.

**To set up the Raspberry Pi:**

1. Insert the SD card into the MicroSD card slot on the Raspberry Pi\. Some SD cards come pre\-loaded with an installation manager that prompts you with a menu to install the OS after booting up the board\. You can also use the Raspberry Pi imager to install the OS on your card\.

1. Connect an HDMI TV or monitor to the HDMI cable that connects to the HDMI port of the Raspberry Pi\. 

1. Connect the keyboard and mouse to the USB ports of the Raspberry Pi and then plug in the power adapter to boot up the board\.

After the Raspberry Pi boots up, if the SD card came pre\-loaded with the installation manager, a menu appears to install the operating system\. If you have trouble installing the OS, you can try the following steps\. For more information about setting up the Raspberry Pi, see [Setting up your Raspberry Pi](https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up/)\.

**If you're having trouble setting up the Raspberry Pi:**
+ Check whether you inserted the SD card before booting up the board\. If you plug in the SD card after booting up the board, the installation menu might not appear\.
+ Make sure that the TV or monitor is turned on and the correct input is selected\.
+ Ensure that you are using Raspberry Pi compatible software\.

If you're still having trouble or need additional help, see [Getting help for your Raspberry Pi](https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up/5)\.

After you have installed the OS, use the Raspberry Pi's web browser to open the AWS IoT console to continue the rest of the steps in this tutorial\.

## Create an AWS IoT policy for thing shadow<a name="create-policy-shadow"></a>

X\.509 certificates authenticate your device with AWS IoT Core\. AWS IoT policies are attached to the certificate that permits the device to perform AWS IoT operations, such as subscribing or publishing to MQTT reserved topics used by the Device Shadow service\. Your device presents its certificate when it connects and sends messages to AWS IoT Core\. 

In this procedure, you'll create a policy that allows your device to perform the AWS IoT operations necessary to run the example program\. We recommend that you create a policy that grants only the permissions required to perform the task\. You create the AWS IoT policy first, and then attach it to the device certificate that you'll create later\.

**To create an AWS IoT policy**

1. On the left menu, choose **Secure**, and then choose **Policies**\. If your account has existing policies, choose **Create**, otherwise, on the **You don’t have a policy yet** page, choose **Create a policy**\.

1. On the **Create a policy** page:

   1. Enter a name for the policy in the **Name** field \(for example, **My\_Device\_Shadow\_policy**\)\. Do not use personally identifiable information in your policy names\.

   1. In the policy document, you describe connect, subscribe, receive, and publish actions that give the device permission to publish and subscribe to the MQTT reserved topics\.

      Copy the following sample policy and paste it in your policy document\. Replace `thingname` with the name of the thing that you'll create \(for example, `My_light_bulb`\), `region` with the AWS IoT Region where you're using the services, and `account` with your AWS account number\. For more information about AWS IoT policies, see [AWS IoT Core policies](iot-policies.md)\.

      ```
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Effect": "Allow",
            "Action": [
              "iot:Publish"
            ],
            "Resource": [
              "arn:aws:iot:region:account:topic/$aws/things/thingname/shadow/get",
              "arn:aws:iot:region:account:topic/$aws/things/thingname/shadow/update"
            ]
          },
          {
            "Effect": "Allow",
            "Action": [
              "iot:Receive"
            ],
            "Resource": [
              "arn:aws:iot:region:account:topic/$aws/things/thingname/shadow/get/accepted",
              "arn:aws:iot:region:account:topic/$aws/things/thingname/shadow/get/rejected",
              "arn:aws:iot:region:account:topic/$aws/things/thingname/shadow/update/accepted",
              "arn:aws:iot:region:account:topic/$aws/things/thingname/shadow/update/rejected",
              "arn:aws:iot:region:account:topic/$aws/things/thingname/shadow/update/delta"
            ]
          },
          {
            "Effect": "Allow",
            "Action": [
              "iot:Subscribe"
            ],
            "Resource": [
              "arn:aws:iot:region:account:topicfilter/$aws/things/thingname/shadow/get/accepted",
              "arn:aws:iot:region:account:topicfilter/$aws/things/thingname/shadow/get/rejected",
              "arn:aws:iot:region:account:topicfilter/$aws/things/thingname/shadow/update/accepted",
              "arn:aws:iot:region:account:topicfilter/$aws/things/thingname/shadow/update/rejected",
              "arn:aws:iot:region:account:topicfilter/$aws/things/thingname/shadow/update/delta"
            ]
          },
          {
            "Effect": "Allow",
            "Action": "iot:Connect",
            "Resource": "arn:aws:iot:region:account:client/test-*"
          }
        ]
      }
      ```

## Create a thing resource and attach the policy to the thing<a name="create-thing-shadow"></a>

Devices connected to AWS IoT can be represented by *thing resources* in the AWS IoT registry\. A *thing resource* represents a specific device or logical entity, such as the light bulb in this tutorial\.

To learn how to create a thing in AWS IoT, follow the steps described in [Create a thing object](create-iot-resources.md#create-aws-thing)\. Here are some key things to note as you follow the steps in that tutorial:

1. Choose **Create a single thing**, and in the **Name** field, enter a name for the thing that is the same as the `thingname` \(for example, `My_light_bulb`\) you specified when you created the policy earlier\.

   You can't change a thing name after it has been created\. If you gave it a different name other than `thingname`, create a new thing with name as `thingname` and delete the old thing\.
**Note**  
Do not use personally identifiable information in your thing name\. The thing name can appear in unencrypted communications and reports\.

1. We recommend that you download each of the certificate files on the **Certificate created\!** page into a location where you can easily find them\. You'll need to install these files for running the sample application\.

   We recommend that you download the files into a `certs` subdirectory in your `home` directory on the Raspberry Pi and name each of them with a simpler name as suggested in the following table\.  
**Certificate file names**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/create-resources-shadow.html)

1. After you activate the certificate to enable connections to AWS IoT, choose **Attach a policy** and make sure you attach the policy that you created earlier \(for example, **My\_Device\_Shadow\_policy**\) to the thing\.

   After you've created a thing, you can see your thing resource displayed in the list of things in the AWS IoT console\.

## Review the results and next steps<a name="resources-shadow-review"></a>

**In this tutorial, you learned how to:**
+ Set up and configure the Raspberry Pi device\.
+ Create an AWS IoT policy document that authorizes your device to interact with AWS IoT services\.
+ Create a thing resource and associated X\.509 device certificate, and attach the policy document to it\.

**Next steps**  
You can now install the AWS IoT device SDK for Python, run the `shadow.py` sample application, and use Device Shadows to control the state\. For more information about how to run this tutorial, see [Install the Device SDK and run the `shadow.py` sample application for Device Shadows](lightbulb-shadow-application.md)\.

**Topics**
+ [Set up and configure Raspberry Pi device](#setup-device-shadow)
+ [Create an AWS IoT policy for thing shadow](#create-policy-shadow)
+ [Create a thing resource and attach the policy to the thing](#create-thing-shadow)
+ [Review the results and next steps](#resources-shadow-review)
+ [Install the Device SDK and run the `shadow.py` sample application for Device Shadows](lightbulb-shadow-application.md)
+ [Interact with Device Shadow using the `shadow.py` sample app and MQTT test client](interact-lights-device-shadows.md)