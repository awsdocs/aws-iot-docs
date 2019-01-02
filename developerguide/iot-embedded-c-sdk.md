# Using the AWS IoT Embedded C SDK<a name="iot-embedded-c-sdk"></a>

## Set Up the Runtime Environment for the AWS IoT Embedded C SDK<a name="iot-c-sdk-runtime"></a>

1. Download the AWS IoT Device SDK for C from the following GitHub repository:

   `git clone https://github.com/aws/aws-iot-device-sdk-embedded-C.git -b release`

1. Before you can use the AWS IoT Embedded C SDK, you must download all required third\-party libraries from GitHub\. You can find instructions for doing this in the `deviceSDK/external_libs` folder\.

## Sample App Configuration<a name="iot-c-sdk-app-config"></a>

The AWS IoT Embedded C SDK includes sample apps for you to try\. For simplicity, we are going to run subscribe\_publish\_sample\.

1. Copy your certificate, private key, and root CA certificate into the `deviceSDK/certs` directory\. 

   To get the root CA certificate, review the documentation on [Server Authentication in AWS IoT Core](managing-device-certs.html#server-authentication) and follow the links to the appropriate CA certificate\. Copy the root CA text from the browser, paste it into a file, and then copy it into the `deviceSDK/certs` directory\.
**Note**  
Device and root CA certificates are subject to expiration or revocation\. If this should occur, you must copy a new CA certificate or private key and device certificate onto your device\.

1. Navigate to the `deviceSDK/sample_apps/subscribe_publish_sample` directory\. You must configure your personal endpoint, private key, and certificate\. The personal endpoint is the REST API endpoint you noted earlier\. If you don't remember the endpoint and you have access to a machine with the AWS CLI installed, you can use the `aws iot describe-endpoint` command to find your personal endpoint URL\. Or, go to the AWS IoT console\. Choose **Registry**, choose **Things**, and then choose the thing that represents your Raspberry Pi\. On the **Details** page for the thing, in the left navigation pane, choose **Interact**\. Copy everything, including "\.com", from **REST API endpoint**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/thing-details-interact-raspberry.png)

1. Open the `aws_iot_config.h` file and, in the `//Get from console` section, update the values for the following:  
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
   #define AWS_IOT_CERTIFICATE_FILENAME   "4bbdc778b9-certificate.pem.crt"
   #define AWS_IOT_PRIVATE_KEY_FILENAME   "4bbdc778b9-private.pem.key"
   // =================================================
   ```

## Run Sample Applications<a name="iot-c-sdk-app-run"></a>

1. Compile the `subscribe_publish_sample_app` using the included makefile\.

   `make -f Makefile`

   This generates an executable file\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/successful-build.png)

1. Now run the subscribe\_publish\_sample\_app\. You should see output similar to the following:  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/successful-run.png)

Your Raspberry Pi is now connected to AWS IoT using the AWS IoT Device SDK for C\.