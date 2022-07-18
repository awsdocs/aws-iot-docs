# Building demos with the AWS IoT Device Client<a name="iot-tutorials-dc-intro"></a>

The tutorials in this learning path walk you through the steps to develop demonstration software by using the AWS IoT Device Client\. The AWS IoT Device Client provides software that runs on your IoT device to test and demonstrate aspects of an IoT solution that's built on AWS IoT\.

The goal of these tutorials is to facilitate exploration and experimentation so you can feel confident that AWS IoT supports your solution before you develop your device software\.

**What you'll learn in these tutorials:**
+ How to prepare a Raspberry Pi for use as an IoT device with AWS IoT
+ How to demonstrate AWS IoT features by using the AWS IoT Device Client on your device

In this learning path, you'll install the AWS IoT Device Client on your own Raspberry Pi and create the AWS IoT resources in the cloud to demonstrate IoT solution ideas\. While the tutorials in this learning path demonstrate features by using a Raspberry Pi, they explain the goals and procedures to help you adapt them to other devices\.

## Prerequisites to building demos with the AWS IoT Device Client<a name="iot-dc-tutorial-overview"></a>

This section describes what you'll need to have before you start the tutorials in this learning path\.

**To complete the tutorials in this learning path, you'll need:**
+ 

**An AWS account**  
You can use your existing AWS account, if you have one, but you might need to add additional roles or permissions to use the AWS IoT features these tutorials use\.

  If you need to create a new AWS account, see [Set up your AWS account](setting-up.md)\.
+ 

**A Raspberry Pi or compatible IoT device**  
The tutorials use a [Raspberry Pi](https://www.raspberrypi.org/) because it comes in different form factors, it's ubiquitous, and it's a relatively inexpensive demonstration device\. The tutorials have been tested on the [Raspberry Pi 3 Model B\+](https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/), the [Raspberry Pi 4 Model B](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/), and on an Amazon EC2 instance running Ubuntu Server 20\.04 LTS \(HVM\)\. To use the AWS CLI and run the commands, We recommend that you use the the latest version of the Raspberry Pi OS \([Raspberry Pi OS \(64\-bit\)](https://www.raspberrypi.org/downloads/raspberry-pi-os/) or the OS Lite\)\. Earlier versions of the OS might work, but we haven't tested it\.
**Note**  
The tutorials explain the goals of each step to help you adapt them to IoT hardware that we haven't tried them on; however, they do not specifically describe how to adapt them to other devices\.
+ 

**Familiarity with the IoT device's operating system**  
The steps in these tutorials assume that you are familiar with using basic Linux commands and operations from the command line interface supported by a Raspberry Pi\. If you're not familiar with these operations, you might want to give yourself more time to complete the tutorials\.

  To complete these tutorials, you should already understand how to:
  + Safely perform basic device operations such as assembling and connecting components, connecting the device to required power sources, and installing and removing memory cards\.
  + Upload and download system software and files to the device\. If your device doesn't use a removable storage device, such as a microSD card, you'll need to know how to connect to your device and upload and download system software and files to the device\.
  + Connect your device to the networks that you plan to use it on\.
  + Connect to your device from another computer using an SSH terminal or similar program\.
  + Use a command line interface to create, copy, move, rename, and set the permissions of files and directories on the device\.
  + Install new programs on the device\.
  + Transfer files to and from your device using tools such as FTP or SCP\.
+ 

**A development and testing environment for your IoT solution**  
The tutorials describe the software and hardware required; however, the tutorials assume that you'll be able to perform operations that might not be described explicitly\. Examples of such hardware and operations include:
  + 

**A local host computer to download and store files on**  
For the Raspberry Pi, this is usually a personal computer or laptop that can read and write to microSD memory cards\. The local host computer must:
    + Be connected to the Internet\.
    + Have the [AWS CLI](http://aws.amazon.com/cli/) installed and configured\.
    + Have a web browser that supports the AWS console\.
  + 

**A way to connect your local host computer to your device to communicate with it, to enter commands, and to transfer files**  
On the Raspberry Pi, this is often done using SSH and SCP from the local host computer\.
  + 

**A monitor and keyboard to connect to your IoT device**  
These can be helpful, but are not required to complete the tutorials\.
  + 

**A way for your local host computer and your IoT devices to connect to the internet**  
This could be a cabled or a wireless network connection to a router or gateway that's connected to the internet\. The local host must also be able to connect to the Raspberry Pi\. This might require them to be on the same local area network\. The tutorials can't show you how to set this up for your particular device or device configuration, but they show how you can test this connectivity\.
  + 

**Access to your local area network's router to view the connected devices**  
To complete the tutorials in this learning path, you'll need to be able to find the IP address of your IoT device\.

    On a local area network, this can be done by accessing the admin interface of the network router your devices connect to\. If you can assign a fixed IP address for your device in the router, you can simplify reconnection after each time the device restarts\.

    If you have a keyboard and a monitor attached to the device, ifconfig can display the device's IP address\.

    If none of these are an option, you'll need to find a way to identify the device's IP address after each time it restarts\. 

After you have all your materials, continue to [Tutorial: Preparing your devices for the AWS IoT Device Client](iot-dc-prepare-device.md)\. 

**Topics**
+ [Prerequisites to building demos with the AWS IoT Device Client](#iot-dc-tutorial-overview)
+ [Tutorial: Preparing your devices for the AWS IoT Device Client](iot-dc-prepare-device.md)
+ [Tutorial: Installing and configuring the AWS IoT Device Client](iot-dc-install-dc.md)
+ [Tutorial: Demonstrate MQTT message communication with the AWS IoT Device Client](iot-dc-testconn.md)
+ [Tutorial: Demonstrate remote actions \(jobs\) with the AWS IoT Device Client](iot-dc-runjobs.md)
+ [Tutorial: Cleaning up after running the AWS IoT Device Client tutorials](iot-dc-cleanup.md)