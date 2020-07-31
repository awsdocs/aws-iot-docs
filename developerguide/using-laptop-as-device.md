# Use your Windows or Linux PC or Mac as an AWS IoT device<a name="using-laptop-as-device"></a>

In this tutorial, you'll configure a personal computer for use with AWS IoT\. These instructions support Windows and Linux PCs and Macs\. To accomplish this, you will need to install some software on your computer\. If you don't want to install software on your computer, you might try [Create a virtual device with Amazon EC2](creating-a-virtual-thing.md), which installs all software on a virtual machine\. 

**Topics**
+ [Set up your personal computer](#gs-pc-prereqs)
+ [Install Git, Node\.js, npm, and the AWS IoT Device SDK for JavaScript](#gs-pc-sdk-node)
+ [Install and run the sample application](#gs-pc-node-app-run)
+ [View messages from the sample app in the AWS IoT console](#gs-pc-view-msg)

## Set up your personal computer<a name="gs-pc-prereqs"></a>

To complete this tutorial, you need a Windows or Linux PC or a Mac with a connection to the Internet\.

Before you continue to the next step, make sure you can open a command line window on your computer\. Use cmd\.exe on a Windows PC\. On a Linux PC or a Mac, use Terminal\. 

## Install Git, Node\.js, npm, and the AWS IoT Device SDK for JavaScript<a name="gs-pc-sdk-node"></a>

In this section, you'll install Node\.js, the npm package manager, and the AWS IoT Device SDK for JavaScript on your computer\.

### Install the latest version of Node\.js<a name="gs-pc-node-runtime"></a>

**To download and install Node\.js and the npm package manager on your computer**

1. Check to see if you have already installed Node\. Enter this command in the command line\.

   ```
   node -v
   ```

   If the command displays the Node version, Node is already installed\. This tutorial requires Node v10\.0 or later\. 

1. Check to see if you have already installed npm\. Enter this command in the command line\.

   ```
   npm -v
   ```

   If the command displays the npm version, npm is already installed\.

1. If Node and npm are both installed, you can skip the rest of the steps in this section\. If not, continue\.

1. Open [https://nodejs\.org/](https://nodejs.org/) and download the installer for your computer\.

1. If the download didn't automatically start to install, run the downloaded program to install Node and npm\. 

1. Verify the installation of Node\.

   ```
   node -v
   ```

   Confirm that the command displays the Node version\. If the Node version isn't displayed, try downloading and installing Node again\.

1. Verify the installation of npm\.

   ```
   npm -v
   ```

   Confirm that the command displays the npm version\. If the npm version is not displayed, try downloading and installing Node again\.

### Install the AWS IoT Device SDK for JavaScript<a name="gs-pc-node-intall-sdk"></a>

**To install the AWS IoT Device SDK for JavaScript on your computer**

1. Check to see if you have Git installed on your computer\. Enter this command in the command line\.

   ```
   git --version
   ```

   If the command displays the Git version, Git is installed and you can continue to the next step\.

   If the command displays an error, open [https://git-scm.com/download](https://git-scm.com/download) and install Git for your computer\.

1. Clone the AWS IoT Device SDK for JavaScript repository into the aws\-iot\-device\-sdk\-js\-v2 directory of your home directory\. This procedure refers to the base directory for the files to install as *home*\.

   The actual location of the *home* directory depends on your operating system\. In Windows, you can find the home directory path by running this command in the `cmd` window\.

   ```
   echo %USERPROFILE%
   ```

   In macOS and Linux, the *home* directory is `~`\.

1. From the *home* directory, run this command to create a `certs` subdirectory that you'll use when you run the sample applications\.

   ```
   mkdir certs
   ```

1.  From your *home* directory, enter this command to create the `aws-iot-device-sdk-js-v2` directory and copy the SDK code into it\.

   ```
   git clone https://github.com/aws/aws-iot-device-sdk-js-v2.git
   ```

1. Change to the `aws-iot-device-sdk-js-v2` directory that was created in the preceding step\.

   ```
   cd aws-iot-device-sdk-js-v2
   ```

1. Use npm to install the SDK by running this command from your current directory\.

   ```
   npm install
   ```

### Prepare to run the sample applications<a name="gs-pc-node-config-app"></a>

**To prepare your system to run the sample application**
+ Into the `certs` directory that you created in the previous section, copy the private key, device certificate, and root CA certificate files you saved when you created and registered the thing object in [Create AWS IoT resources](create-iot-resources.md)\. The file names of each file in the destination directory should match those in the table\.

   The commands in the next section assume that your key and certificate files are stored on your device as shown in this table\.

------
#### [ Linux/macOS ]  
**Certificate file names**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/using-laptop-as-device.html)

  Run this command to list the files in the `certs` directory and compare them to those listed in the table\.

  ```
  ls -l ~/certs
  ```

------
#### [ Windows ]  
**Certificate file names**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/using-laptop-as-device.html)

  Run this command to list the files in the `certs` directory and compare them to those listed in the table\.

  ```
  dir %USERPROFILE%\certs
  ```

------

## Install and run the sample application<a name="gs-pc-node-app-run"></a>

In this section, you'll install and run the `pub-sub.js` sample app found in the `aws-iot-device-sdk-js-v2/samples/node` directory of the AWS IoT Device SDK for JavaScript\. This app shows how your device uses the MQTT library to publish and subscribe to MQTT messages\.

The `pub-sub.js` sample app subscribes to a topic, `topic_1`, publishes 10 messages to that topic, and displays the messages as they're received from the AWS IoT message broker\. 

To run the `pub-sub.js` sample app, you need the following information: 


**Application parameter values**  

|  Parameter  |  Where to find the value  | 
| --- | --- | 
| your\-iot\-endpoint |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/using-laptop-as-device.html)  | 

The *your\-iot\-endpoint* value has a format of: `endpoint_id-ats.iot.region.amazonaws.com`, for example, `a3qj468EXAMPLE-ats.iot.us-west-2.amazonaws.com`\. 

------
#### [ Linux/macOS ]

**To install and run the example app on Linux/macOS**

1. In your command line window, navigate to the `~/aws-iot-device-sdk-js-v2/samples/node/pub_sub` directory that the SDK created and install the sample app by using these commands\.

   ```
   cd ~/aws-iot-device-sdk-js-v2/samples/node/pub_sub
   npm install
   ```

1. In your command line window, replace *your\-iot\-endpoint* as indicated and run this command\. 

   ```
   node dist/index.js --topic topic_1 --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key --endpoint your-iot-endpoint
   ```

------
#### [ Windows ]

**To run the example app on a Windows PC**

1. In your command line window, navigate to the `%USERPROFILE%/aws-iot-device-sdk-js-v2/samples/node/pub_sub` directory that the SDK created and install the sample app by using these commands\.

   ```
   cd %USERPROFILE%\aws-iot-device-sdk-js-v2\samples\node\pub_sub
   npm install
   ```

1. In your command line window, replace *your\-iot\-endpoint* as indicated and run this command \.

   ```
   node dist\index.js --topic topic_1 --root-ca %USERPROFILE%\certs\Amazon-root-CA-1.pem --cert %USERPROFILE%\certs\device.pem.crt --key %USERPROFILE%\certs\private.pem.key --endpoint your-iot-endpoint
   ```

------

The sample app:

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

If you're having trouble running the sample app, add the `--verbosity debug` parameter to the command line so the sample app displays detailed messages about what it’s doing\. That information might provide you the help you need to correct the problem\. 

## View messages from the sample app in the AWS IoT console<a name="gs-pc-view-msg"></a>

You can see the sample app's messages as they pass through the AWS IoT message broker by using the **MQTT client** in the **AWS IoT console**\. 

**To view the MQTT messages published by the sample app**

1. Review [View MQTT messages with the AWS IoT MQTT client](view-mqtt-messages.md)\. This helps you learn how to use the **MQTT client** in the **AWS IoT console** to view MQTT messages as they pass through the AWS IoT message broker

1. Open the **MQTT client** in the **AWS IoT console**\.

1. Subscribe to the topic, **topic\_1**\.

1. In your command line window, run the sample app again and watch the messages in the **MQTT client** in the **AWS IoT console**\.

------
#### [ Linux/macOS ]

   ```
   cd ~/aws-iot-device-sdk-js-v2/samples/node/pub_sub
   node dist/index.js --topic topic_1 --root-ca ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key --endpoint your-iot-endpoint
   ```

------
#### [ Windows ]

   ```
   cd %USERPROFILE%\aws-iot-device-sdk-js-v2\samples\node\pub_sub
   node dist\index.js --topic topic_1 --root-ca %USERPROFILE%\certs\Amazon-root-CA-1.pem --cert %USERPROFILE%\certs\device.pem.crt --key %USERPROFILE%\certs\private.pem.key --endpoint your-iot-endpoint
   ```

------