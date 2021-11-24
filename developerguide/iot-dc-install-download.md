# Step 1: Download and save the AWS IoT Device Client<a name="iot-dc-install-download"></a>

The procedures in this section download the AWS IoT Device Client, compile it, and install it on your Raspberry Pi\. After you test the installation, you can save the image of the Raspberry Pi's microSD card to use later when you want to try the tutorials again\.

**Topics**
+ [Download and build the AWS IoT Device Client](#iot-dc-install-dc-download)
+ [Create the directories used by the tutorials](#iot-dc-install-dc-files)

## Download and build the AWS IoT Device Client<a name="iot-dc-install-dc-download"></a>

This procedure installs the AWS IoT Device Client on your Raspberry Pi\.

Perform these commands in the terminal window on your local host computer that is connected to your Raspberry Pi\.

**To install the AWS IoT Device Client on your Raspberry Pi**

1. Enter these commands to download and build the AWS IoT Device Client on your Raspberry Pi\.

   ```
   cd ~
   git clone https://github.com/awslabs/aws-iot-device-client aws-iot-device-client
   mkdir ~/aws-iot-device-client/build && cd ~/aws-iot-device-client/build
   cmake ../
   ```

1. Run this command to build the AWS IoT Device Client\. This command can take up to 15 minutes to complete\.

   ```
   cmake --build . --target aws-iot-device-client
   ```

   The warning messages displayed as the AWS IoT Device Client compiles can be ignored\.

   These tutorials have been tested with the AWS IoT Device Client built on gcc, version \(Raspbian 10\.2\.1\-6\+rpi1\) 10\.2\.1 20210110 on the Oct 30th 2021 version of Raspberry Pi OS \(bullseye\) on gcc, version \(Raspbian 8\.3\.0\-6\+rpi1\) 8\.3\.0 on the May 7th 2021 version of the Raspberry Pi OS \(buster\)\.

1. After the AWS IoT Device Client finishes building, test it by running this command\.

   ```
   ./aws-iot-device-client --help
   ```

If you see the command line help for the AWS IoT Device Client, the AWS IoT Device Client has been built successfully and is ready for you to use\.

## Create the directories used by the tutorials<a name="iot-dc-install-dc-files"></a>

This procedure creates the directories on the Raspberry Pi that will be used to store the files used by the tutorials in this learning path\.

**To create the directories used by the tutorials in this learning path:**

1. Run these commands to create the required directories\.

   ```
   mkdir ~/dc-configs
   mkdir ~/policies
   mkdir ~/messages
   mkdir ~/certs/testconn
   mkdir ~/certs/pubsub
   mkdir ~/certs/jobs
   ```

1. Run these commands to set the permissions on the new directories\.

   ```
   chmod 745 ~
   chmod 700 ~/certs/testconn
   chmod 700 ~/certs/pubsub
   chmod 700 ~/certs/jobs
   ```

After you create these directories and set their permission, continue to [\(Optional\) Save the microSD card image](iot-dc-install-dc-save.md)\.