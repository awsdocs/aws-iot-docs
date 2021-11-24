# Step 1: Install and update the device's operating system<a name="iot-dc-prepare-device-sys"></a>

The procedures in this section describe how to initialize the microSD card that the Raspberry Pi uses for its system drive\. The Raspberry Pi's microSD card contains its operating system \(OS\) software as well as space for its application file storage\. If you're not using a Raspberry Pi, follow the device's instructions to install and update the device's operating system software\.

After you complete this section, you should be able to start your IoT device and connect to it from the terminal program on your local host computer\.

**Required equipment:**
+ Your local development and testing environment
+ A Raspberry Pi that or your IoT device, that can connect to the internet
+ A microSD memory card with at least 8 GB capacity or sufficient storage for the OS and required software\.
**Note**  
When selecting a microSD card for these exercises, choose one that is as large as necessary but, as small as possible\.  
A small SD card will be faster to back up and update\. On the Raspberry Pi, you won't need more than an 8\-GB microSD card for these tutorials\. If you need more space for your specific application, the smaller image files you save in these tutorials can resize the file system on a larger card to use all the supported space of the card you choose\.

**Optional equipment:**
+ A USB keyboard connected to the Raspberry Pi
+ An HDMI monitor and cable to connect the monitor to the Raspberry Pi

**Topics**
+ [Load the device's operating system onto microSD card](#iot-dc-prepare-device-sys-step1)
+ [Start your IoT device with the new operating system](#iot-dc-prepare-device-sys-step2)
+ [Connect your local host computer to your device](#iot-dc-prepare-device-sys-step3)

## Load the device's operating system onto microSD card<a name="iot-dc-prepare-device-sys-step1"></a>

This procedure uses the local host computer to load the device's operating system onto a microSD card\.

**Note**  
If your device doesn't use a removable storage medium for its operating system, install the operating system using the procedure for that device and continue to [Start your IoT device with the new operating system](#iot-dc-prepare-device-sys-step2)\.

**To install the operating system on your Raspberry Pi**

1. On your local host computer, download and unzip the Raspberry Pi operating system image that you want to use\. The latest versions are available from [ https://www\.raspberrypi\.com/software/operating\-systems/](https://www.raspberrypi.com/software/operating-systems/) 

**Choosing a version of Raspberry Pi OS**  
This tutorial uses the **Raspberry Pi OS Lite** version because it’s the smallest version that supports these the tutorials in this learning path\. This version of the Raspberry Pi OS has only a command line interface and doesn't have a graphical user interface\. A version of the latest Raspberry Pi OS with a graphical user interface will also work with these tutorials; however, the procedures described in this learning path use only the command line interface to perform operations on the Raspberry Pi\.

1. Insert your microSD card into the local host computer\.

1. Using an SD card imaging tool, write the unzipped OS image file to the microSD card\.

1. After writing the Raspberry Pi OS image to the microSD card:

   1. Open the BOOT partition on the microSD card in a command line window or file explorer window\. 

   1. In the BOOT partition of the microSD card, in the root directory, create an empty file named `ssh` with no file extension and no content\. This tells the Raspberry Pi to enable SSH communications the first time it starts\.

1. Eject the microSD card and safely remove it from the local host computer\.

Your microSD card is ready to [Start your IoT device with the new operating system](#iot-dc-prepare-device-sys-step2)\.

## Start your IoT device with the new operating system<a name="iot-dc-prepare-device-sys-step2"></a>

This procedure installs the microSD card and starts your Raspberry Pi for the first time using the downloaded operating system\.

**To start your IoT device with the new operation system**

1. With the power disconnected from the device, insert the microSD card from the previous step, [Load the device's operating system onto microSD card](#iot-dc-prepare-device-sys-step1), into the Raspberry Pi\.

1. Connect the device to a wired network\.

1. These tutorials will interact with your Raspberry Pi from your local host computer using an SSH terminal\.

   If you also want to interact with the device directly, you can:

   1. Connect an HDMI monitor to it to watch the Raspberry Pi’s console messages before you can connect the terminal window on your local host computer to your Raspberry Pi\.

   1. Connect a USB keyboard to it if you want to interact directly with the Raspberry Pi\.

1. Connect the power to the Raspberry Pi and wait about a minute for it to initialize\.

   If you have a monitor connected to your Raspberry Pi, you can watch the start\-up process on it\.

1. 

   Find out your device’s IP address:
   + If you connected an HDMI monitor to the Raspberry Pi, the IP address appears in the messages displayed on the monitor 
   + If you have access to the router your Raspberry Pi is connects to, you can see its address in the router’s admin interface\.

After you have your Raspberry Pi's IP address, you're ready to [Connect your local host computer to your device](#iot-dc-prepare-device-sys-step3)\.

## Connect your local host computer to your device<a name="iot-dc-prepare-device-sys-step3"></a>

This procedure uses the terminal program on your local host computer to connect to your Raspberry Pi and change its default password\.

**To connect your local host computer to your device**

1. 

   On your local host computer, open the SSH terminal program:
   + Windows: `PuTTY`
   + Linux/macOS: `Terminal`
**Note**  
PuTTY isn't installed automatically on Windows\. If it's not on your computer, you might need to download and install it\.

1. Connect the terminal program to your Raspberry Pi’s IP address and log in using its default credentials\.

   ```
   username: pi
   password: raspberry
   ```

1. After you log in to your Raspberry Pi, change the password for the `pi` user\.

   ```
   passwd
   ```

   Follow the prompts to change the password\.

   ```
   Changing password for pi.
   Current password: raspberry
   New password: YourNewPassword
   Retype new password: YourNewPassword
   passwd: password updated successfully
   ```

After you have the Raspberry Pi's command line prompt in the terminal window and changed the password, you're ready to continue to [Step 2: Install and verify required software on your device](iot-dc-prepare-device-sw.md)\.