# Add your gateways and wireless devices to AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-onboard-devices"></a>

LoRaWAN gateways connect wireless devices to AWS IoT Core for LoRaWAN\. The data from wireless devices must be processed by an AWS IoT rule before it can be used by AWS IoT and other services\. Adding a gateway to AWS IoT Core for LoRaWAN lets AWS IoT communicate with and manage the gateway\. Adding devices to AWS IoT Core for LoRaWAN lets AWS IoT process the messages received from the devices for use by AWS IoT and other services\.

## Add your gateways to AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-onboard-gateways"></a>

 Adding a gateway to AWS IoT Core for LoRaWAN so that AWS IoT can communicate with and manage the gateway requires the following information\.
+ 

**Device configuration data**  
These parameters are found on the gateway or in the gateway's documentation that its vendor provides\.
  + 

**Gateway EUI**  
The EUI of the individual gateway device\.
  + 

**Frequency band \(RFRegion\)**  
The gateway's frequency band\. You can choose from `US915`, `EU868`, or `AS923-1`, depending on what your gateway supports and which country or region the gateway is physically connecting from\. For more information about the bands, see [Connecting devices and gateways to AWS IoT Core for LoRaWAN](connect-iot-lorawan.md)\.
+ 

**Your wireless system configuration data**  
The information in these optional fields comes from how you organize and describe the elements in your wireless system\. For more information on naming and describing your resources, see [Describe your AWS IoT Core for LoRaWAN resources](connect-iot-lorawan-describe-resource.md)\.
  + 

**Name**  
The name you can assign to the gateway\.
  + 

**Description**  
Information about the gateway\.
  + 

**Tags**  
Key/value pairs of metadata about the gateway\.
+ 

**Thing association**  
Whether to create an AWS IoT thing and associate it with the gateway\. Associating a thing with your gateway lets the gateway access other AWS IoT Core features\.
+ 

**An IAM role that allows the Configuration and Update Server \(CUPS\) to manage gateway credentials**  
You need to add an IAM role that allows the Configuration and Update Server \(CUPS\) to manage gateway credentials\. You must do this before a LoRaWAN gateway tries to connect with AWS IoT Core for LoRaWAN; however, you need to do it only once\.

  For more information about adding the IAM role, see [Add an IAM role to allow the Configuration and Update Server \(CUPS\) to manage gateway credentials](#connect-iot-lorawan-onboard-permissions)\.
+ 

**Gateway device documentation**  
After adding the gateway's information to AWS IoT Core for LoRaWAN, add some AWS IoT Core for LoRaWAN information to the gateway device\. The documentation provided by the gateway's vendor should describe the process for uploading the certificate files to the gateway and configuring the gateway device to communicate with AWS IoT Core for LoRaWAN\.

## Add your wireless devices to AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-onboard-end-devices"></a>

 Before you add a wireless device to AWS IoT Core for LoRaWAN so that AWS IoT can process data from the device for use by AWS IoT and other services, make sure you have the following information\.
+ 

**LoRaWAN specification and wireless device configuration**  
The device's LoRaWAN specification is provided by the device's vendor in its documentation or on the device itself\. The device's configuration parameters depend on the LoRaWAN specification it uses and are also found in the device's documentation or on the device itself\.
+ 

**Device name and description**  
The information in these optional fields comes from how you organize and describe the elements in your wireless system\. For more information on naming and describing your resources, see [Describe your AWS IoT Core for LoRaWAN resources](connect-iot-lorawan-describe-resource.md)\.
+ 

**Thing association**  
Whether to create an AWS IoT thing and associate it with the device\. Associating a thing with your gateway lets the gateway access other AWS IoT Core features\.
+ 

**Device and service profiles**  
The device's configuration parameters depend on the LoRaWAN specification it uses\. The configuration parameters are found in the device's documentation or on the device itself\. You'll want to identify a device profile that matches the configuration parameters of the device, or create one if necessary, before you add the device\. For more information, see see [Add profiles to AWS IoT Core for LoRaWAN](connect-iot-lorawan-define-profiles.md)\.
+ 

**AWS IoT Core for LoRaWAN destination**  
Each device must be assigned to a destination that will process its messages to send to AWS IoT and other services\. The AWS IoT rules that process and send the device messages are specific to the device's message format\. To process the messages from the device and send them to the correct service, identify the destination you created to use with the device's messages and assign it to the device\.

  If you don't have a destination defined for this device, you'll need to add it before you add the device\.

## Add an IAM role to allow the Configuration and Update Server \(CUPS\) to manage gateway credentials<a name="connect-iot-lorawan-onboard-permissions"></a>

This procedure describes how to add an IAM role that will allow the Configuration and Update Server \(CUPS\) to manage gateway credentials\. Make sure you perform this procedure before a LoRaWAN gateway tries to connect with AWS IoT Core for LoRaWAN; however, you need to do this only once\.

**To add the IAM role to allow the Configuration and Update Server \(CUPS\) to manage gateway credentials:**

1. Open the [ Roles hub of the IAM console](https://console.aws.amazon.com/iam/home#/roles) and choose **Create role**\.

1. If you think that you might have already added the **IoTWirelessGatewayCertManagerRole** role, in the search bar, enter **IoTWirelessGatewayCertManagerRole**\.

   If you see an **IoTWirelessGatewayCertManagerRole** role in the search results, you have the necessary IAM role\. You can leave the procedure now\.

   If the search results are empty, you don't have the necessary IAM role\. Continue the procedure to add it\.

1. In **Select type of trusted entity**, choose **Another AWS account**\.

1. In **Account ID**, enter your AWS account ID, and then choose **Next: Permissions**\.

1. In the search box, enter **AWSIoTWirelessGatewayCertManager**\.

1. In the list of search results, select the policy named **AWSIoTWirelessGatewayCertManager**\.

1. Choose **Next: Tags**, and then choose **Next: Review**\.

1. In **Role name**, enter **IoTWirelessGatewayCertManagerRole**, and then choose **Create role**\.

1. To edit the new role, in the confirmation message, choose **IoTWirelessGatewayCertManagerRole**\.

1. In **Summary**, choose the **Trust relationships** tab, and then choose **Edit trust relationship**\.

1. In **Policy Document**, change the `Principal` property to look like this example\.

   ```
   "Principal": { 
       "Service": "iotwireless.amazonaws.com" 
   },
   ```

   After you change the `Principal` property, the complete policy document should look like this example\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": "iotwireless.amazonaws.com"
         },
         "Action": "sts:AssumeRole",
         "Condition": {}
       }
     ]
   }
   ```

1. To save your changes and exit, choose **Update Trust Policy**\.

You’ve now created the **IoTWirelessGatewayCertManagerRole**\. You won’t need to do this again\.

If you performed this procedure while you were adding a gateway, you can close this window and the IAM console and return to the AWS IoT console to finish adding the gateway\. 