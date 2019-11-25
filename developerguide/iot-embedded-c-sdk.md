# Using the AWS IoT Device SDK for Embedded C<a name="iot-embedded-c-sdk"></a>

This section describes how to run the AWS IoT Device SDK for Embedded C\.

## Install the AWS IoT Device SDK for Embedded C<a name="install-embedded-c-sdk"></a>

Download the AWS IoT SDK for Embedded C to your Raspberry Pi from [GitHub](https://github.com/aws/aws-iot-device-sdk-embedded-C):

git clone https://github\.com/aws/aws\-iot\-device\-sdk\-embedded\-c\.git \-b release

This creates a directory named `aws-iot-device-sdk-embedded-c` in the current directory\.

Download mbed TLS to your Raspberry Pi from the [mbed TLS website](https://tls.mbed.org/download)\.

cd into the `/home/pi/Downloads` directory\. Expand the `mbedtls-2.16.3-apache.tgz` file using tar \-xvf mbedtls\-2\.16\.3\-apache\.tgz\.

Copy the contents of `mbedtls-2.16.3` into the `aws-iot-device-sdk-embedded-C/external_libs/mbedTLS` directory using:

mv \~/Downloads/mbedtls\-2\.16\.3 \~/aws\-iot\-device\-sdk\-embedded\-c/external\_libs/mbedTLS

## Sample App Configuration<a name="iot-c-sdk-app-config"></a>

The AWS IoT SDK for Embedded C includes sample applications for you to try\. For simplicity, this tutorial uses the `subscribe_publish_sample` application, which illustrates how to connect to the AWS IoT message broker and subscribe and publish to MQTT topics\.

1. Copy the certificate, private key, and root CA certificate you created in [Create an AWS IoT Thing for Your Raspberry Pi](sdk-tutorials.md#iot-sdk-create-thing) into the `aws-iot-device-sdk-embedded-C/certs` directory\.
**Note**  
Device and root CA certificates are subject to expiration or revocation\. If your certificates expire or are revoked, you must copy a new CA certificate or private key and device certificate onto your device\.

1. You must configure the sample with your personal AWS IoT endpoint, private key, certificate, and root CA certificate\. Navigate to the `aws-iot-device-sdk-embedded-c/samples/linux/subscribe_publish_sample` directory\. 

   If you have the AWS CLI installed, you can use the aws iot describe\-endpoint \-\-endpoint\-type iot:Data\-ATS command to find your personal endpoint URL\. If you don't have the AWS CLI installed, open the AWS IoT console\. From the navigation pane, choose **Manage**, and then choose **Things**\. Choose the IoT thing for your Raspberry Pi, and then choose **Interact**\. Your endpoint is displayed in the ** HTTPS** section of the thing details page\.

1. Open the `aws_iot_config.h` file and, in the `Get from console` section, update the values for the following:  
AWS\_IOT\_MQTT\_HOST  
Your personal endpoint\.  
AWS\_IOT\_MY\_THING\_NAME  
Your thing name\.  
AWS\_IOT\_ROOT\_CA\_FILENAME  
Your root CA certificate\.  
 AWS\_IOT\_CERTIFICATE\_FILENAME  
Your certificate\.  
AWS\_IOT\_PRIVATE\_KEY\_FILENAME  
Your private key\.

   For example:

   ```
   // Get from console
   // =================================================
   #define AWS_IOT_MQTT_HOST              "a22j5sm6o3yzc5.iot.us-east-1.amazonaws.com"
   #define AWS_IOT_MQTT_PORT              8883
   #define AWS_IOT_MQTT_CLIENT_ID         "MyRaspberryPi"
   #define AWS_IOT_MY_THING_NAME          "MyRaspberryPi"
   #define AWS_IOT_ROOT_CA_FILENAME       "root-CA.crt"
   #define AWS_IOT_CERTIFICATE_FILENAME   "device.pem.crt"
   #define AWS_IOT_PRIVATE_KEY_FILENAME   "private.pem.key"
   // =================================================
   ```

## Run Sample Applications<a name="iot-c-sdk-app-run"></a>

**To run the AWS IoT Device SDK for Embedded C sample applications**

1. Use the included makefile to compile the `subscribe_publish_sample_app`\.

   `make -f Makefile`

   This generates an executable file\.

1. Run the `subscribe_publish_sample_app`\. You should see output similar to the following:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/successful-run.png)

Your Raspberry Pi is now connected to AWS IoT using the AWS IoT Device SDK for Embedded C\.