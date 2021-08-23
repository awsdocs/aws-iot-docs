# Consider frequency band selection and add necessary IAM role<a name="connect-iot-lorawan-rfregion-permissions"></a>

Before you add your gateway to AWS IoT Core for LoRaWAN, we recommend that you consider the frequency band in which your gateway will be operating and add the necessary IAM role for connecting your gateway to AWS IoT Core for LoRaWAN\.

**Note**  
If you're adding your gateway using the console, click **Create role** in the console to create the necessary IAM role so you can then skip these steps\. You need to perform these steps only if you're using the CLI to create the gateway\.

## Consider selection of LoRa frequency bands for your gateways and device connection<a name="connect-iot-lorawan-frequency-bands"></a>

AWS IoT Core for LoRaWAN supports EU863\-870, US902\-928, AU915, and AS923\-1 frequency bands, which you can use to connect your gateways and devices that are physically present in countries that support the frequency ranges and characteristics of these bands\. The EU863\-870 and US902\-928 bands are commonly used in Europe and North America, respectively\. The AS923\-1 band is commonly used in Australia, New Zealand, Japan, and Singapore among other countries\. The AU915 is used in Australia and Argentina among other countries\. For more information about which frequency band to use in your region or country, see [ LoRaWAN® Regional Parameters](https://lora-alliance.org/resource_hub/rp2-101-lorawan-regional-parameters-2/)\. 

LoRa Alliance publishes LoRaWAN specifications and regional parameter documents that are available for download from the LoRa Alliance website\. The LoRa Alliance regional parameters help companies decide which frequency band to use in their region or country\. AWS IoT Core for LoRaWAN's frequency band implementation follows the recommendation in the regional parameters specification document\. These regional parameters are grouped into a set of radio parameters, along with a frequency allocation that is adapted to the Industrial, Scientific, and Medical \(ISM\) band\. We recommend that you work with the compliance teams to ensure that you meet any applicable regulatory requirements\. 

## Add an IAM role to allow the Configuration and Update Server \(CUPS\) to manage gateway credentials<a name="connect-iot-lorawan-onboard-permissions"></a>

This procedure describes how to add an IAM role that will allow the Configuration and Update Server \(CUPS\) to manage gateway credentials\. Make sure you perform this procedure before a LoRaWAN gateway tries to connect with AWS IoT Core for LoRaWAN; however, you need to do this only once\.

**Add the IAM role to allow the Configuration and Update Server \(CUPS\) to manage gateway credentials**

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