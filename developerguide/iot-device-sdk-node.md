# Using the AWS IoT Device SDK for JavaScript and Node<a name="iot-device-sdk-node"></a>

This tutorial shows you how to install Node\.js, the npm package manager, and the AWS IoT Device SDK for JavaScript on a Raspberry Pi and run the sample applications\.

## Install the Latest Version of Node\.js<a name="iot-sdk-node-runtime"></a>

To use the AWS IoT Device SDK for JavaScript, install Node\.js and the npm package manager on your Raspberry Pi\.

1. To download the latest version of the Node repository, open a terminal window and run the following command: 

   curl \-sL https://deb\.nodesource\.com/setup\_12\.x \| sudo \-E bash \-

1. Run the following command to install Node and npm\.

   sudo apt\-get install \-y nodejs

1. Run the following commands to verify the installation of Node and npm\.

   node \-v

   and

   npm \-v

   If a version number is displayed, node and npm are installed correctly\.

## Install the AWS IoT Device SDK for JavaScript<a name="iot-sdk-node-intall-sdk"></a>

Install the AWS IoT Device SDK for JavaScript on your Raspberry Pi\.

1. In the terminal window, use the following command to clone the AWS IoT Device SDK for JavaScript repository into your home directory \(by default, `/home/pi`\)\.

   git clone https://github\.com/aws/aws\-iot\-device\-sdk\-js\.git

1. cd into the `aws-iot-device-sdk-js` directory\.

1. Use npm to install the SDK:

   npm install

## Prepare to Run the Sample Applications<a name="iot-sdk-node-config-app"></a>

Under `aws-iot-device-sdk-js`, create a `certs` directory and copy your private key, device certificate, and root CA certificate into it\.

To run the AWS IoT Device SDK for JavaScript samples, you need the following information:

Your AWS Region  
To find the Region you are using, open the AWS IoT console and look at the URL\. The Region appears immediately after the `https://`\. For example:  
`https://us-west-2.console.aws.amazon.com/iot/home?region=us-west-2#/dashboard`  
For more information, see [AWS IoT Service Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#iot_region)\.

A client ID  
An arbitrary alphanumeric string used to identify a device or application that connects to AWS IoT\. This should be the same as your IoT thing name for this tutorial\.

Your private key  
The fully qualified path to your private key on your Raspberry Pi \(for example, `/home/pi/aws-iot-device-sdk-js/certs/private.pem.key`\)\.

Your AWS IoT X\.509 certificate  
The fully qualified path to your AWS IoT certificate on your Raspberry Pi \(for example, `/home/pi/aws-iot-device-sdk-js/certs/device.pem.crt`\)\.

The STS Amazon root CA  
The fully qualified path to the Amazon root CA on your Raspberry Pi \(for example, `/home/pi/aws-iot-device-sdk-js/certs/Amazon-root-CA-1.pem`\)\.

Your AWS IoT endpoint  
If you have installed the AWS CLI on your Raspberry Pi, you can run the describe\-endpoint CLI command to find your endpoint:  
aws iot describe\-endpoint \-\-endpoint\-type iot:Data\-ATS   
If you don't have the AWS CLI installed, open the AWS IoT console\. From the navigation pane, choose **Manage**, and then choose **Things**\. Choose the IoT thing for your Raspberry Pi, and then choose **Interact**\. Your endpoint is displayed in the ** HTTPS** section of the thing details page\.

The port on which the AWS IoT message broker listens  
This is always `8883`\.

Your IoT thing name  
This is the name you used when you registered your Raspberry Pi with AWS IoT\.

## Run the Sample Applications<a name="iot-sdk-node-app-run"></a>

The AWS IoT Device SDK for JavaScript contains a number of samples in the `aws-iot-device-sdk-js/examples` directory\. We recommend that you start with `device-example.js`\. This example runs in two modes\. In mode 1, it subscribes to the MQTT topic, `topic_1`, and publishes a message every 4 seconds on `topic_2`\. In mode 2, it subscribes to `topic_2` and publishes a message every 4 seconds on `topic_1`\. You can run two instances of `device-example.js` \(one in mode 1 and one in mode 2\) and see the messages being sent and received\.

From the `aws-iot-device-sdk-js/examples` directory, run the following command to start an instance of the sample:

node device\-example \-k "\.\./certs/private\.pem\.key" \-c "\.\./certs/device\.pem\.crt" \-i "*raspberry\-pi\-1*" \-a "\.\./certs/Amazon\-root\-CA\-1" \-H "*<your\-iot\-endpoint>*" \-p 8883 \-T "*your\-thing\-name*" \-\-test\-mode 1

Start another instance of `device-example.js` running in mode 2:

node device\-example \-k "\.\./certs/private\.pem\.key" \-c "\.\./certs/device\.pem\.crt" \-i "*raspberry\-pi\-2*" \-a "\.\./certs/Amazon\-root\-CA\-1" \-H "*<your\-iot\-endpoint>*" \-p 8883 \-T "*your\-thing\-name*" \-\-test\-mode 2

**Important**  
Make sure that you use different client IDs when you run the two instances of `device-example.js`\. No two clients \(devices or applications\) can connect to AWS IoT using a the same client ID\. The first client's connection is terminated and the second client connection is established\.  
The thing name is important only when you create a policy specific to an IoT thing\. In the AWS IoT Getting Started tutorial, you do not create such a policy, so you can use the same thing name for both instances\.

Your Raspberry Pi is now connected to AWS IoT using the AWS IoT SDK for JavaScript\.

If the sample instances are running correctly, the output from the instance running in mode 1 should look like this:

```
substituting 250ms dely for truems...
connect
message topic_1 {"mode1Process":1}
message topic_1 {"mode1Process":2}
message topic_1 {"mode1Process":3}
message topic_1 {"mode1Process":4}
...
```

The output from the instance running in mode 2 should look like this:

```
substituting 250ms dely for truems...
connect
message topic_2 {"mode2Process":1}
message topic_2 {"mode2Process":2}
message topic_2 {"mode2Process":3}
message topic_2 {"mode2Process":4}
...
```

If the sample does not run correctly, try adding the `-d` option to display debug information\.