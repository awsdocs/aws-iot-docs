# Tutorial: Preparing your Raspberry Pi to run the shadow application<a name="create-resources-shadow"></a>

This tutorial demonstrates how to set up and configure a Raspberry Pi device and create the AWS IoT resources that a device requires to connect and exchange MQTT messages\.

**Note**  
If you're planning to [Create a virtual device with Amazon EC2](creating-a-virtual-thing.md), you can skip this page and continue to [Configure your device](configure-device.md)\. You'll create these resources when you create your virtual thing\. If you would like to use a different device instead of the Raspberry Pi, you can try to follow these tutorials by adapting them to a device of your choice\.

**In this tutorial, you'll learn how to:**
+ Set up a Raspberry Pi device and configure it for use with AWS IoT\.
+ Create an AWS IoT policy document, which authorizes your device to interact with AWS IoT services\.
+ Create a thing resource in AWS IoT the X\.509 device certificates, and then attach the policy document\.

  The thing is the virtual representation of your device in the AWS IoT registry\. The certificate authenticates your device to AWS IoT Core, and the policy document authorizes your device to interact with AWS IoT\.

**How to run this tutorial**  
To run the `shadow.py` sample application for Device Shadows, you'll need a Raspberry Pi device that connects to AWS IoT\. We recommend that you follow this tutorial in the order it's presented here, starting with setting up the Raspberry Pi and it's accessories, and then creating a policy and attaching the policy to a thing resource that you create\. You can then follow this tutorial by using the graphical user interface \(GUI\) supported by the Raspberry Pi to open the AWS IoT console on the device's web browser, which also makes it easier to download the certificates directly to your Raspberry Pi for connecting to AWS IoT\.

**Before you start this tutorial, make sure that you have:**
+ An AWS account\. If you don't have one, complete the steps described in [Set up your AWS account](setting-up.md) before you continue\. You'll need your AWS account and AWS IoT console to complete this tutorial\. 
+ The Raspberry Pi and its necessary accessories\. You'll need:
  + A [Raspberry Pi 3 Model B](https://www.raspberrypi.org/products/) or more recent model\. This tutorial might work on earlier versions of the Raspberry Pi, but we haven't tested it\.
  + [Raspberry Pi OS \(32\-bit\)](https://www.raspberrypi.org/downloads/raspberry-pi-os/) or later\. We recommend using the latest version of the Raspberry Pi OS\. Earlier versions of the OS might work, but we haven't tested it\.
  + An Ethernet or Wi\-Fi connection\.
  + Keyboard, mouse, monitor, cables, and power supplies\.

This tutorial takes about 30 minutes to complete\.

## Step 1: Set up and configure Raspberry Pi device<a name="setup-device-shadow"></a>

In this section, we'll configure a Raspberry Pi device for use with AWS IoT\.

**Important**  
Adapting these instructions to other devices and operating systems can be challenging\. You'll need to understand your device well enough to be able to interpret these instructions and apply them to your device\. If you encounter difficulties, you might try one of the other device options as an alternative, such as [Create a virtual device with Amazon EC2](creating-a-virtual-thing.md) or [Use your Windows or Linux PC or Mac as an AWS IoT device](using-laptop-as-device.md)\. 

You'll need to configure your Raspberry Pi such that it can start the operating system \(OS\), connect to the internet, and allow you to interact with it at a command line interface\. You can also use the graphical user interface \(GUI\) supported with the Raspberry Pi to open the AWS IoT console and run the rest of this tutorial\.

**To set up the Raspberry Pi**

1. Insert the SD card into the MicroSD card slot on the Raspberry Pi\. Some SD cards come pre\-loaded with an installation manager that prompts you with a menu to install the OS after booting up the board\. You can also use the Raspberry Pi imager to install the OS on your card\.

1. Connect an HDMI TV or monitor to the HDMI cable that connects to the HDMI port of the Raspberry Pi\. 

1. Connect the keyboard and mouse to the USB ports of the Raspberry Pi and then plug in the power adapter to boot up the board\.

After the Raspberry Pi boots up, if the SD card came pre\-loaded with the installation manager, a menu appears to install the operating system\. If you have trouble installing the OS, you can try the following steps\. For more information about setting up the Raspberry Pi, see [Setting up your Raspberry Pi](https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up/)\.

**If you're having trouble setting up the Raspberry Pi:**
+ Check whether you inserted the SD card before booting up the board\. If you plug in the SD card after booting up the board, the installation menu might not appear\.
+ Make sure that the TV or monitor is turned on and the correct input is selected\.
+ Ensure that you are using Raspberry Pi compatible software\.

After you have installed and configured the Raspberry Pi OS, open the Raspberry Pi's web browser and navigate to the AWS IoT Core console to continue the rest of the steps in this tutorial\.

If you can open the AWS IoT Core console, you're Raspberry Pi is ready and you can continue to [Tutorial: Provisioning your device in AWS IoT](shadow-provision-cloud.md)\.

If you're having trouble or need additional help, see [Getting help for your Raspberry Pi](https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up/5)\.