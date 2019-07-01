# Using the AWS IoT Device SDK for JavaScript<a name="iot-device-sdk-node"></a>

This tutorial shows you how to install Node\.js, the npm package manager, and the AWS IoT Device SDK for JavaScript on a Raspberry Pi and run the device SDK sample applications\.

## Set Up the Runtime Environment for the AWS IoT Device SDK for JavaScript<a name="iot-sdk-node-runtime"></a>

To use the AWS IoT Device SDK for JavaScript, install Node and the npm package manager on your Raspberry Pi\.

1. To add the Node repository, open a terminal and run the following command: 

   curl \-sL https://deb\.nodesource\.com/setup\_11\.x \| sudo \-E bash \-

1. To install Node and npm, run the following command:

   sudo apt\-get install \-y nodejs

1. To verify the installation of Node and npm, run the following commands:

   node \-v

   and

   npm \-v

   If a version number is displayed by these commands, node and npm are installed correctly\.

## Install the AWS IoT Device SDK for JavaScript<a name="iot-sdk-node-intall-sdk"></a>

To install the AWS IoT Device SDK for JavaScript on your Raspberry Pi, create a `~/deviceSDK` directory using the following command:

mkdir deviceSDK

Use npm to install the SDK:

npm install aws\-iot\-device\-sdk

After the installation is complete, you should find a `node_modules` directory in your `~/deviceSDK` directory\.

## Prepare to Run the Sample Applications<a name="iot-sdk-node-config-app"></a>

Follow the instructions at [Getting Started with AWS IoT](iot-gs.md) to register your Raspberry Pi with AWS IoT\. Under `aws-device-sdk-js`, create a `certs` directory and copy your private key, certificate, and root CA files to the `certs` directory\.
+ Rename your private key `node-private-key.pem`\.
+ Rename your certificate `node-cert.pem`\.

To run the AWS IoT Device SDK for JavaScript samples, you need the following information:

Your AWS Region  
You can find the region you are using by browsing to the AWS IoT console and looking at the URL\. The region appears immediately after the `https://` in the URL\. For example:  
`https://us-west-2.console.aws.amazon.com/iot/home?region=us-west-2#/dashboard`  
The AWS Region also appears after the `?` in the URL\. For more information, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html)\. For region information specific to AWS IoT, see [ AWS IoT Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#iot_region)\.

A client ID  
An arbitrary alphanumeric string used to identify a device or application connecting to AWS IoT\.

Your private key  
The fully\-qualified path to your private key on your Raspberry Pi\. This is the key that is generated when you registered your Raspberry Pi with AWS IoT\.

Your AWS IoT X\.509 certificate  
The fully\-qualified path to your AWS IoT certificate on your Raspberry Pi\. This is the certifiate that is generated when you registered your Raspberry Pi with AWS IoT\.

The STS Amazon root CA  
The fully\-qualified path to the Amazon Root CA on your Raspberry Pi\.

Your AWS IoT endpoint  
You can find your endpoint by running the describe\-endpoint CLI command:  
aws iot describe\-endpoint \-\-endpoint\-type iot:Data\-ATS  
You can also find your endpoint by browsing to the AWS IoT console, choosing the IoT thing for your Raspberry Pi, and then choosing **Interact**\. Your endpoint is displayed under **HTTPS** and **Update your Device Shadow using this Rest API endpoint**\.

The port on which the AWS IoT message broker listens  
This is always `8883`\.

Your IoT thing name  
This is the name you specifed when you registered your Raspberry Pi with AWS IoT\.

## Run the Sample Applications<a name="iot-sdk-node-app-run"></a>

The AWS IoT Device SDK for JavaScript contains a number of samples in the `aws-iot-device-sdk-js/examples` directory\. We recommend that you start with device\-example\.js\. This example runs in two modes\. In mode 1, it subscribes to the MQTT topic `topic_1` and publishes a message every 4 seconds on `topic_2`\. In mode 2, it subscribes to `topic_2` and publishes a message every 4 seconds on `topic_1`\. You can run two instances of device\-example\.js, one in mode 1 and one in mode 2, and see the messages being sent and received\.

From the `aws-iot-device-sdk-js/examples` directory, run the following command to start an instance of the sample:

node device\-example \-k "\.\./certs/node\-private\-key\.pem" \-c "\.\./certs/node\-cert\.pem" \-i "*client\-id\-1*" \-H "*<your\-iot\-endpoint>*" \-p 8883 \-T "*your\-thing\-name*" \-\-test\-mode 1

Start another instance of device\-example\.js running in mode 2:

node device\-example \-k "\.\./certs/node\-private\-key\.pem" \-c "\.\./certs/node\-cert\.pem" \-i "*client\-id\-2*" \-H "*<your\-iot\-endpoint>*" \-p 8883 \-T "*your\-thing\-name*" \-\-test\-mode 2

**Important**  
Make sure that you use different client IDs when you run the two instances of device\-example\.js\. No two clients \(devices or applications\) can connect to AWS IoT using a the same client ID\. The first client's connection is termniated and the second client connection is established\.  
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

If the sample does not run correctly, try adding the `-d` option when running the sample to display debug information\.