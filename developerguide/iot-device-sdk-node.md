# Using the AWS IoT Device SDK for JavaScript<a name="iot-device-sdk-node"></a>

The easiest way to install the AWS IoT Device SDK for Node\.js is to use `npm`\. In this section we describe how to install Node and npm\.

## Set Up the Runtime Environment for the AWS IoT Device SDK for JavaScript<a name="iot-sdk-node-runtime"></a>

To use the AWS IoT Device SDK for JavaScript, install Node and the npm development tool on your Raspberry Pi\. These packages are not installed by default\. 

**Note**  
Before you continue, you might want to configure the keyboard mapping for your Raspberry Pi\. For more information, see [Configure Raspberry Pi Keyboard Mapping](https://www.raspberrypi.org/documentation/configuration/localisation.md)\.

1. To add the Node repository, open a terminal and run the following command: 

   ```
   curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
   ```

1. To install Node, run 

   ```
   sudo apt-get install nodejs
   ```

1. Run `npm -v` to determine if npm is installed\. If no version information is returned, install npm as follows:

   ```
   sudo apt-get install npm
   ```

   You should see output similar to the following:  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/Selection_035.png)

1. To verify the installation of Node and npm, run the following:

   ```
   node -v
   ```

   and

   ```
   npm -v
   ```

   You should see output similar to the following:  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/Selection_036.png)

## Install the AWS IoT Device SDK for JavaScript<a name="iot-sdk-node-intall-sdk"></a>

To install the AWS IoT Device SDK for JavaScript/Node\.js on your Raspberry Pi, open a console window and from your `~/deviceSDK` directory, use npm to install the SDK:

```
npm install aws-iot-device-sdk
```

After the installation is complete, you should find a `node_modules` directory in your `~/deviceSDK` directory\.

## Prepare to Run the Sample Applications<a name="iot-sdk-node-config-app"></a>

The AWS IoT Device SDK for JavaScript includes sample apps for you to try\. To run them, you must configure your certificates and private key\. 

Edit the file \~/deviceSDK/node\_modules/aws\-iot\-device\-sdk/examples/lib/cmdline\.js to change the default names for the private key \(`privateKey`\), certificate \(`clientCert`\), and CA root certificate \(`caCert`\) used by the samples\. For example:

```
default: {
    region: 'us-east-1',
    clientId: clientIdDefault,
    privateKey: '4bbdc778b9-private.pem.key',
    clientCert: '4bbdc778b9-certificate.pem.crt',
    caCert: 'root-CA.crt,
    testMode: 1,
    reconnectPeriod: 3 * 1000, 	/* milliseconds */
    delay: 4 * 1000						/* milliseconds */   
};
```

## Run the Sample Applications<a name="iot-sdk-node-app-run"></a>

Run the examples using 

```
node examples/<YourDesiredExample>.js -f <certs location> -H <PREFIX>.iot.<REGION>.amazonaws.com
```

Assuming you are in the directory `~/deviceSDK/node_modules/aws-iot-device-sdk/`, the certificates location should be `~/deviceSDK/certs/`\. Use the REST endpoint of your Raspberry Pi as the value of the \-H parameter\.

To see all available command line parameters for each sample, run the program and specify the '\-h' parameter:

```
node examples/<YourDesiredExample>.js -h
```

For more information about how you can use command line options to specify the certificates location and your own host address, see the [Certificates](https://github.com/aws/aws-iot-device-sdk-js#certificates) section of the AWS IoT Device SDK for JavaScript Readme on GitHub\. The sample descriptions that follow this section of the Readme show additional command line parameters that are required for each sample\.

If you want to create a configuration file for use with the command line option `--configuration-file (-F)`, create a file \(in JSON format\) with the following properties\. For example:

```
{
    "host":           "a22j5sm6o3yzc5.iot.us-east-1.amazonaws.com",
    "port":           8883,
    "clientId":       "MyRaspberryPi",
    "thingName":      "MyRaspberryPi",
    "caCert":         "root-CA.crt",
    "clientCert":     "4bbdc778b9-certificate.pem.crt",
    "privateKey":     "4bbdc778b9-private.pem.key"
}
```

Your Raspberry Pi is now connected to AWS IoT using the AWS IoT SDK for JavaScript\.