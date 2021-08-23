# Connect a Raspberry Pi or another device<a name="connecting-to-existing-device"></a>

In this section, we'll configure a Raspberry Pi for use with AWS IoT\. If you have another device that you'd like to connect, the instructions for the Raspberry Pi include references that can help you adapt these instructions to your device\.

This normally takes about 20 minutes, but it can take longer if you have many system software upgrades to install\.

**Topics**
+ [Set up your device](#gs-device-prereqs)
+ [Install the required tools and libraries for the AWS IoT Device SDK](#gs-device-sdk-tools)
+ [Install AWS IoT Device SDK](#gs-device-install-sdk)
+ [Install and run the sample app](#gs-device-node-app-run)
+ [View messages from the sample app in the AWS IoT console](#gs-device-view-msg)

**Important**  
Adapting these instructions to other devices and operating systems can be challenging\. You'll need to understand your device well enough to be able to interpret these instructions and apply them to your device\.  
If you encounter difficulties while configuring your device for AWS IoT, we can't offer any assistance beyond the instructions in this section\. However, you might try one of the other device options as an alternative, such as [Create a virtual device with Amazon EC2](creating-a-virtual-thing.md) or [Use your Windows or Linux PC or Mac as an AWS IoT device](using-laptop-as-device.md)\. 

## Set up your device<a name="gs-device-prereqs"></a>

The goal of this step is to collect what you'll need to configure your device such that it can start the operating system \(OS\), connect to the Internet, and allow you to interact with it at a command line interface\. 

To complete this tutorial, you need the following:
+ An AWS account\. If you don't have one, complete the steps described in [Set up your AWS account](setting-up.md) before you continue\. 
+ A [Raspberry Pi 3 Model B](https://www.raspberrypi.org/products/) or more recent model\. This might work on earlier versions of the Raspberry Pi, but they have not been tested\.
+ [Raspberry Pi OS \(32\-bit\)](https://www.raspberrypi.org/downloads/raspberry-pi-os/) or later\. We recommend using the latest version of the Raspberry Pi OS\. Earlier versions of the OS might work, but they have not been tested\.

  To run this example, you do not need to install the desktop with the graphical user interface \(GUI\); however, if you're new to the Raspberry Pi and your Raspberry Pi hardware supports it, using the desktop with the GUI might be easier\.
+ An Ethernet or Wi\-Fi connection\.
+ Keyboard, mouse, monitor, cables, power supplies, and other hardware required by your device\.

**Important**  
Before you continue to the next step, your device must have its operating system installed, configured, and running\. The device must be connected to the Internet and you will need to be able to access the device by using its command line interface\. Command line access can be through a directly\-connected keyboard, mouse, and monitor, or by using an SSH terminal remote interface\. 

 If you are running an operating system on your Raspberry Pi that has a graphical user interface \(GUI\), open a terminal window on the device and perform the following instructions in that window\. Otherwise, if you are connecting to your device by using a remote terminal, such as PuTTY, open a remote terminal to your device and use that\.

## Install the required tools and libraries for the AWS IoT Device SDK<a name="gs-device-sdk-tools"></a>

Before you install the AWS IoT Device SDK and sample code, make sure your system is up\-to\-date and has the required tools and libraries to install the SDKs\.

1. 

**Update the operating system and install required libraries**  
Before you install an AWS IoT Device SDK, run these commands in a terminal window on your device to update the operating system and install the required libraries\.

   ```
   sudo apt-get update
   ```

   ```
   sudo apt-get upgrade
   ```

   ```
   sudo apt-get install cmake
   ```

   ```
   sudo apt-get install libssl-dev
   ```

1. 

**Install Git**  
If your device's operating system doesn't come with Git installed, you'll need to install it to install the AWS IoT Device SDK for JavaScript\. 

   1. Test to see if Git is already installed by running this command\.

      ```
      git --version
      ```

   1. If the previous command returns the Git version, Git is already installed and you can skip to Step 3\.

   1. If an error is displayed when you run the git command, install Git by running this command\.

      ```
      sudo apt-get install git
      ```

   1. Test again to see if Git is installed by running this command\.

      ```
      git --version
      ```

   1. If Git is installed, continue to the next section\. If not, troubleshoot and correct the error before continuing\. You need Git to install the AWS IoT Device SDK for JavaScript\.

## Install AWS IoT Device SDK<a name="gs-device-install-sdk"></a>

Install the AWS IoT Device SDK\.

------
#### [ Python ]

In this section, you'll install Python, its development tools, and the AWS IoT Device SDK for Python on your device\. These instructions are for a Raspberry Pi running the latest Raspberry Pi OS\. If you have another device or are using another operating system, you might need to adapt these instructions for your device\.

1. 

**Install Python and its development tools**  
The AWS IoT Device SDK for Python requires Python v3\.5 or later to be installed on your Raspberry Pi\.

   In a terminal window to your device, run these commands\.

   1. Run this command to determine the version of Python installed on your device\.

      ```
      python3 --version
      ```

      If Python is installed, it will display its version\.

   1. If the version displayed is `Python 3.5` or greater, you can skip to Step 2\.

   1. If the version displayed is less than `Python 3.5`, you can install the correct version by running this command\.

      ```
      sudo apt install python3
      ```

   1. Run this command to confirm that the correct version of Python is now installed\.

      ```
      python3 --version
      ```

1. 

**Test for pip3**  
In a terminal window to your device, run these commands\.

   1. Run this command to see if pip3 is installed\.

      ```
      pip3 --version
      ```

   1. If the command returns a version number, pip3 is installed and you can skip to Step 3\.

   1. If the previous command returns an error, run this command to install pip3\.

      ```
      sudo apt install python3-pip
      ```

   1. Run this command to see if pip3 is installed\.

      ```
      pip3 --version
      ```

1. 

**Install the current AWS IoT Device SDK for Python**  
Install the AWS IoT Device SDK for Python and download the sample apps to your device\.

   On your device, run these commands\.

   ```
   cd ~
   python3 -m pip install awsiotsdk
   ```

   ```
   git clone https://github.com/aws/aws-iot-device-sdk-python-v2.git
   ```

------
#### [ JavaScript ]

In this section, you'll install Node\.js, the npm package manager, and the AWS IoT Device SDK for JavaScript on your device\. These instructions are for a Raspberry Pi running the Raspberry Pi OS\. If you have another device or are using another operating system, you might need to adapt these instructions for your device\.

1. 

**Install the latest version of Node\.js**  
The AWS IoT Device SDK for JavaScript requires Node\.js and the npm package manager to be installed on your Raspberry Pi\.

   1. Download the latest version of the Node repository by entering this command\.

      ```
      cd ~
      curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
      ```

   1. Install Node and npm\.

      ```
      sudo apt-get install -y nodejs
      ```

   1. Verify the installation of Node\.

      ```
      node -v
      ```

      Confirm that the command displays the Node version\. This tutorial requires Node v10\.0 or later\. If the Node version isn't displayed, try downloading the Node repository again\.

   1. Verify the installation of npm\.

      ```
      npm -v
      ```

      Confirm that the command displays the npm version\. If the npm version isn't displayed, try installing Node and npm again\.

   1. Restart the device\.

      ```
      sudo shutdown -r 0
      ```

      Continue after the device restarts\.

1. 

**Install the AWS IoT Device SDK for JavaScript**  
Install the AWS IoT Device SDK for JavaScript on your Raspberry Pi\.

   1. Install aws\-crt, the common runtime library\.

      ```
      cd ~
      npm install aws-crt
      ```

   1. Install v2 of the AWS IoT Device SDK for JavaScript\.

      ```
      npm install aws-iot-device-sdk-v2
      ```

   1. Clone the AWS IoT Device SDK for JavaScript repository into the `aws-iot-device-sdk-js-v2` directory of your *home* directory\. On the Raspberry Pi, the *home* directory is `~/`, which is used as the *home* directory in the following commands\. If your device uses a different path for the *home* directory, you must replace `~/` with the correct path for your device in the following commands\.

      These commands create the `~/aws-iot-device-sdk-js-v2` directory and copy the SDK code into it\.

      ```
      cd ~
      git clone https://github.com/aws/aws-iot-device-sdk-js-v2.git
      ```

   1. Change to the `aws-iot-device-sdk-js-v2` directory that you created in the preceding step and install the SDK\.

      ```
      cd ~/aws-iot-device-sdk-js-v2
      npm install
      ```

------

## Install and run the sample app<a name="gs-device-node-app-run"></a>

In this section, you'll install and run the `pubsub` sample app found in the AWS IoT Device SDK\. This app shows how your device uses the MQTT library to publish and subscribe to MQTT messages\. The sample app subscribes to a topic, `topic_1`, publishes 10 messages to that topic, and displays the messages as they're received from the message broker\.

**Install the certificate files**  
The sample app requires the certificate files that authenticate the device to be installed on the device\.

**To install the device certificate files for the sample app**

1. Create a `certs` subdirectory in your *home* directory by running these commands\.

   ```
   cd ~
   mkdir certs
   ```

1. Into the `~/certs` directory, copy the private key, device certificate, and root CA certificate that you created earlier in [Create AWS IoT resources](create-iot-resources.md)\.

   How you copy the certificate files to your device depends on the device and operating system and isn't described here\. However, if your device supports a graphical user interface \(GUI\) and has a web browser, you can perform the procedure described in [Create AWS IoT resources](create-iot-resources.md) from your device's web browser to download the resulting files directly to your device\.

   The commands in the next section assume that your key and certificate files are stored on the device as shown in this table\.  
**Certificate file names**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/connecting-to-existing-device.html)

To run the sample app, you need the following information:


**Application parameter values**  

|  Parameter  |  Where to find the value  | 
| --- | --- | 
| your\-iot\-endpoint |  In the [AWS IoT console](https://console.aws.amazon.com/iot/home), choose **Manage**, and then choose **Things**\. Choose the IoT thing you created for your device, **MyIotThing** was the name used earlier, and then choose **Interact**\. On the thing details page, your endpoint is displayed in the ** HTTPS** section\.  | 

The *your\-iot\-endpoint* value has a format of: `endpoint_id-ats.iot.region.amazonaws.com`, for example, `a3qj468EXAMPLE-ats.iot.us-west-2.amazonaws.com`\. 

------
#### [ Python ]

**To install and run the sample app**

1. Navigate to the sample app directory\.

   ```
   cd ~/aws-iot-device-sdk-python-v2/samples
   ```

1. In the command line window, replace *your\-iot\-endpoint* as indicated and run this command\.

   ```
   python3 pubsub.py --topic topic_1 --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key --endpoint your-iot-endpoint
   ```

1. Observe that the sample app:

   1. Connects to the AWS IoT service for your account\.

   1. Subscribes to the message topic, **topic\_1**, and displays the messages it receives on that topic\. 

   1. Publishes 10 messages to the topic, **topic\_1**\. 

   1. Displays output similar to the following:

   ```
   Connecting to a3qEXAMPLEffp-ats.iot.us-west-2.amazonaws.com with client ID 'test-0c8ae2ff-cc87-49d2-a82a-ae7ba1d0ca5a'...
   Connected!
   Subscribing to topic 'topic_1'...
   Subscribed with QoS.AT_LEAST_ONCE
   Sending 10 message(s)
   Publishing message to topic 'topic_1': Hello World! [1]
   Received message from topic 'topic_1': b'Hello World! [1]'
   Publishing message to topic 'topic_1': Hello World! [2]
   Received message from topic 'topic_1': b'Hello World! [2]'
   Publishing message to topic 'topic_1': Hello World! [3]
   Received message from topic 'topic_1': b'Hello World! [3]'
   Publishing message to topic 'topic_1': Hello World! [4]
   Received message from topic 'topic_1': b'Hello World! [4]'
   Publishing message to topic 'topic_1': Hello World! [5]
   Received message from topic 'topic_1': b'Hello World! [5]'
   Publishing message to topic 'topic_1': Hello World! [6]
   Received message from topic 'topic_1': b'Hello World! [6]'
   Publishing message to topic 'topic_1': Hello World! [7]
   Received message from topic 'topic_1': b'Hello World! [7]'
   Publishing message to topic 'topic_1': Hello World! [8]
   Received message from topic 'topic_1': b'Hello World! [8]'
   Publishing message to topic 'topic_1': Hello World! [9]
   Received message from topic 'topic_1': b'Hello World! [9]'
   Publishing message to topic 'topic_1': Hello World! [10]
   Received message from topic 'topic_1': b'Hello World! [10]'
   10 message(s) received.
   Disconnecting...
   Disconnected!
   ```

   If you're having trouble running the sample app, review [Troubleshooting problems with the sample app](gs-device-troubleshoot.md)\.

   You can also add the `--verbosity Debug` parameter to the command line so the sample app displays detailed messages about what it’s doing\. That information might provide you the help you need to correct the problem\. 

------
#### [ JavaScript ]

**To install and run the sample app**

1. In your command line window, navigate to the `~/aws-iot-device-sdk-js-v2/samples/node/pub_sub` directory that the SDK created and install the sample app by using these commands\.

   ```
   cd ~/aws-iot-device-sdk-js-v2/samples/node/pub_sub
   npm install
   ```

1. In the command line window, replace *your\-iot\-endpoint* as indicated and run this command\.

   ```
   node dist/index.js --topic topic_1 --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key --endpoint your-iot-endpoint
   ```

1. Observe that the sample app:

   1. Connects to the AWS IoT service for your account\.

   1. Subscribes to the message topic, **topic\_1**, and displays the messages it receives on that topic\. 

   1. Publishes 10 messages to the topic, **topic\_1**\. 

   1. Displays output similar to the following:

   ```
   Publish received on topic topic_1
   {"message":"Hello world!","sequence":1}
   Publish received on topic topic_1
   {"message":"Hello world!","sequence":2}
   Publish received on topic topic_1
   {"message":"Hello world!","sequence":3}
   Publish received on topic topic_1
   {"message":"Hello world!","sequence":4}
   Publish received on topic topic_1
   {"message":"Hello world!","sequence":5}
   Publish received on topic topic_1
   {"message":"Hello world!","sequence":6}
   Publish received on topic topic_1
   {"message":"Hello world!","sequence":7}
   Publish received on topic topic_1
   {"message":"Hello world!","sequence":8}
   Publish received on topic topic_1
   {"message":"Hello world!","sequence":9}
   Publish received on topic topic_1
   {"message":"Hello world!","sequence":10}
   ```

   If you're having trouble running the sample app, review [Troubleshooting problems with the sample app](gs-device-troubleshoot.md)\.

   You can also add the `--verbosity Debug` parameter to the command line so the sample app displays detailed messages about what it’s doing\. That information might provide you the help you need to correct the problem\. 

------

## View messages from the sample app in the AWS IoT console<a name="gs-device-view-msg"></a>

You can see the sample app's messages as they pass through the message broker by using the **MQTT client** in the **AWS IoT console**\. 

**To view the MQTT messages published by the sample app**

1. Review [View MQTT messages with the AWS IoT MQTT client](view-mqtt-messages.md)\. This helps you learn how to use the **MQTT client** in the **AWS IoT console** to view MQTT messages as they pass through the message broker\.

1. Open the **MQTT client** in the **AWS IoT console**\.

1. Subscribe to the topic, **topic\_1**\.

1. In your command line window, run the sample app again and watch the messages in the **MQTT client** in the **AWS IoT console**\.

------
#### [ Python ]

   ```
   cd ~/aws-iot-device-sdk-python-v2/samples
   python3 pubsub.py --topic topic_1 --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key --endpoint your-iot-endpoint
   ```

------
#### [ JavaScript ]

   ```
   cd ~/aws-iot-device-sdk-js-v2/samples/node/pub_sub
   node dist/index.js --topic topic_1 --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key --endpoint your-iot-endpoint
   ```

------