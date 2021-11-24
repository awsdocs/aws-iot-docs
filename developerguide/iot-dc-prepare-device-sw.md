# Step 2: Install and verify required software on your device<a name="iot-dc-prepare-device-sw"></a>

The procedures in this section continue from [the previous section](iot-dc-prepare-device-sys.md) to bring your Raspberry Pi's operating system up to date and install the software on the Raspberry Pi that will be used in the next section to build and install the AWS IoT Device Client\.

After you complete this section, your Raspberry Pi will have an up\-to\-date operating system, the software required by the tutorials in this learning path, and it will be configured for your location\.

**Required equipment:**
+ Your local development and testing environment from [the previous section](iot-dc-prepare-device-sys.md)
+ The Raspberry Pi that you used in [the previous section](iot-dc-prepare-device-sys.md)
+ The microSD memory card from [the previous section](iot-dc-prepare-device-sys.md)

**Note**  
The Raspberry Pi Model 3\+ and Raspberry Pi Model 4 can perform all the commands described in this learning path\. If your IoT device can't compile software or run the AWS Command Line Interface, you might need to install the required compilers on your local host computer to build the software and then transfer it to your IoT device\. For more information about how to install and build software for your device, see the documentation for your device's software\.

**Topics**
+ [Update the operating system software](#iot-dc-prepare-device-sw-step1)
+ [Install the required applications and libraries](#iot-dc-prepare-device-sw-step2)
+ [\(Optional\) Save the microSD card image](#iot-dc-prepare-device-sw-step3)

## Update the operating system software<a name="iot-dc-prepare-device-sw-step1"></a>

This procedure updates the operating system software\.

**To update the operating system software on the Raspberry Pi**

Perform these steps in the terminal window of your local host computer\.

1. Enter these commands to update the system software on your Raspberry Pi\.

   ```
   sudo apt-get -y update
   sudo apt-get -y upgrade
   sudo apt-get -y autoremove
   ```

1. Update the Raspberry Pi's locale and time zone settings \(optional\)\.

   Enter this command to update the device's locale and time zone settings\.

   ```
   sudo raspi-config
   ```

   1. To set the device's locale:

      1. In the **Raspberry Pi Software Configuration Tool \(raspi\-config\)** screen, choose option **5**\.

         **`5 Localisation Options Configure language and regional settings`**

         Use the Tab key to move to **<Select>,** and then press the space bar\.

      1. In the localization options menu, choose option **L1**\.

         **`L1 Locale Configure language and regional settings`**

         Use the Tab key to move to **<Select>,** and then press the space bar\.

      1. In the list of locale options, choose the locales that you want to install on your Raspberry Pi by using the arrow keys to scroll and the space bar to mark those that you want\. 

         In the United States, **`en_US.UTF-8`** is a good one to choose\.

      1. After selecting the locales for your device, use the Tab key to choose **<OK>**, and then press the space bar to display the **Configuring locales** confirmation page\.

   1. To set the device’s time zone:

      1. In the **raspi\-config** screen, choose option **5**\.

         **`5 Localisation Options Configure language and regional settings`**

         Use the Tab key to move to **<Select>,** and then press the space bar\.

      1. In the localization options menu, use the arrow key to choose option **L2**:

         **`L2 time zone Configure time zone`**

         Use the Tab key to move to **<Select>,** and then press the space bar\.

      1. In the **Configuring tzdata** menu, choose your geographical area from the list\. 

         Use the Tab key to move to **<OK>**, and then press the space bar\.

      1. In the list of cities, use the arrow keys to choose a city in your time zone\.

         To set the time zone, use the Tab key to move to **<OK>**, and then press the space bar\.

   1. When you’ve finished updating the settings, use the Tab key to move to **<Finish>**, and then press the space bar to close the **raspi\-config** app\.

1. Enter this command to restart your Raspberry Pi\.

   ```
   sudo shutdown -r 0
   ```

1. Wait for your Raspberry Pi to restart\.

1. After your Raspberry Pi has restarted, reconnect the terminal window on your local host computer to your Raspberry Pi\.

Your Raspberry Pi system software is now configured and you're ready to continue to [Install the required applications and libraries](#iot-dc-prepare-device-sw-step2)\.

## Install the required applications and libraries<a name="iot-dc-prepare-device-sw-step2"></a>

This procedure installs the application software and libraries that the subsequent tutorials use\.

If you are using a Raspberry Pi, or if you can compile the required software on your IoT device, perform these steps in the terminal window on your local host computer\. If you must compile software for your IoT device on your local host computer, review the software documentation for your IoT device for information about how to do these steps on your device\.

**To install the application software and libraries on your Raspberry Pi**

1. Enter this command to install the application software and libraries\.

   ```
   sudo apt-get -y install build-essential libssl-dev cmake unzip git python3-pip
   ```

1. Enter these commands to confirm that the correct version of the software was installed\.

   ```
   gcc --version
   cmake --version
   openssl version
   git --version
   ```

1. 

   Confirm that these versions of the application software are installed:
   + `gcc`: 9\.3\.0 or later
   + `cmake`: 3\.10\.x or later
   + `OpenSSL`: 1\.1\.1 or later
   + `git`: 2\.20\.1 or later

If your Raspberry Pi has acceptable versions of the required application software, you're ready to continue to [\(Optional\) Save the microSD card image](#iot-dc-prepare-device-sw-step3)\.

## \(Optional\) Save the microSD card image<a name="iot-dc-prepare-device-sw-step3"></a>

Throughout the tutorials in this learning path, you'll encounter these procedures to save a copy of the Raspberry Pi's microSD card image to a file on your local host computer\. While encouraged, they are not required tasks\. By saving the microSD card image where suggested, you can skip the procedures that precede the save point in this learning path, which can save time if you find the need to retry something\. The consequence of not saving the microSD card image periodically is that you might have to restart the tutorials in the learning path from the beginning if your microSD card is damaged or if you accidentally configure an app or its settings incorrectly\.

At this point, your Raspberry Pi's microSD card has an updated OS and the basic application software loaded\. You can save the time it took you to complete the preceding steps by saving the contents of the microSD card to a file now\. Having the current image of your device's microSD card image lets you start from this point to continue or retry a tutorial or procedure without the need to install and update the software from scratch\.

**To save the microSD card image to a file**

1. Enter this command to shut down the Raspberry Pi\.

   ```
   sudo shutdown -h 0
   ```

1. After the Raspberry Pi shuts down completely, remove its power\.

1. Remove the microSD card from the Raspberry Pi\.

1. On your local host computer: 

   1. Insert the microSD card\.

   1. Using your SD card imaging tool, save the microSD card’s image to a file\.

   1. After the microSD card’s image has been saved, eject the card from the local host computer\.

1. With the power disconnected from the Raspberry Pi, insert the microSD card into the Raspberry Pi\.

1. Apply power to the Raspberry Pi\.

1. After waiting about a minute, on the local host computer, reconnect the terminal window on your local host computer that was connected to your Raspberry Pi\., and then log in to the Raspberry Pi\.