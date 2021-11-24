# Add a gateway to AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-onboard-gateway-add"></a>

You can add your gateway to AWS IoT Core for LoRaWAN by using the console or the CLI\. 

Before adding your gateway, we recommend that you consider the factors mentioned in the **Before onboarding your gateway** section of [Onboard your gateways to AWS IoT Core for LoRaWAN](connect-iot-lorawan-onboard-gateways.md)\.

If you're adding your gateway for the first time, we recommend that you use the console\. If you want to add your gateway by using the CLI instead, you must have already created the necessary IAM role so that the gateway can connect with AWS IoT Core for LoRaWAN\. For information about how to create the role, see [Add an IAM role to allow the Configuration and Update Server \(CUPS\) to manage gateway credentials](connect-iot-lorawan-rfregion-permissions.md#connect-iot-lorawan-onboard-permissions)\.

## Add a gateway using the console<a name="connect-iot-lorawan-onboard-gateway-console"></a>

Navigate to the [AWS IoT Core for LoRaWAN](https://console.aws.amazon.com/iot/home#/wireless/landing) **Intro** page of the AWS IoT console and choose **Get started**, and then choose **Add gateway**\. If you've already added a gateway, choose **View gateway** to view the gateway that you added\. If you would like to add more gateways, choose **Add gateway**\. 

1. 

**Provide gateway details and frequency band information**  
Use the **Gateway details** section to provide information about the device configuration data such as the Gateway's EUI and the frequency band configuration\.
   + 

**Gateway's EUI**  
The EUI \(Extended Unique Identifier\) of the individual gateway device\. The EUI is a 16\-digit alphanumeric code, such as `c0ee40ffff29df10`, that uniquely identifies a gateway in your LoRaWAN network\. This information is specific to your gateway model and you can find it on your gateway device or in its user manual\.
**Note**  
The Gateway's EUI is different from the Wi\-Fi MAC address that you may see printed on your gateway device\. The EUI follows a EUI\-64 standard that uniquely identifies your gateway and therefore cannot be resued in other AWS accounts and regions\.
   + 

**Frequency band \(RFRegion\)**  
The gateway's frequency band\. You can choose from `US915`, `EU868`, `AU915`, or `AS923-1`, depending on what your gateway supports and which country or region the gateway is physically connecting from\. For more information about the bands, see [Consider selection of LoRa frequency bands for your gateways and device connection](connect-iot-lorawan-rfregion-permissions.md#connect-iot-lorawan-frequency-bands)\.

1. 

**Specify your wireless gateway configuration data \(optional\)**  
These fields are optional and you can use them to provide additional information about the gateway and it's configuration\.
   + 

**Name, Description, and Tags for your gateway**  
The information in these optional fields comes from how you organize and describe the elements in your wireless system\. You can assign a **Name** to the gateway, use the **Description** field to provide information about the gateway, and use **Tags** to add key\-value pairs of metadata about the gateway\. For more information on naming and describing your resources, see [Describe your AWS IoT Core for LoRaWAN resources](connect-iot-lorawan-describe-resource.md)\.
   + 

**LoRaWAN configuration using subbands and filters**  
Optionally, you can also specify LoRaWAN configuration data such as the subbands that you want to use and filters that can control the flow of traffic\. For this tutorial, you can skip these fields\. For more information, see [Configure your gateway's subbands and filtering capabilities](connect-iot-lorawan-subband-filter-configuration.md)\.

1. 

**Associate an AWS IoT thing with the gateway**  
Specify whether to create an AWS IoT thing and associate it with the gateway\. Things in AWS IoT can make it easier to search and manage your devices\. Associating a thing with your gateway lets the gateway access other AWS IoT Core features\.

1. 

**Create and download the gateway certificate**  
To authenticate your gateway so that it can securely communicate with AWS IoT, your LoRaWAN gateway must present a private key and certificate to AWS IoT Core for LoRaWAN\. Create a **Gateway certificate** so that AWS IoT can verify your gateway's identity by using the X\.509 Standard\.

   Click the **Create certificate** button and download the certificate files\. You'll use them later to configure your gateway\.

1. 

**Copy the CUPS and LNS endpoints and download certificates**  
Your LoRaWAN gateway must connect to a CUPS or LNS endpoint when establishing a connection to AWS IoT Core for LoRaWAN\. We recommend that you use the CUPS endpoint as it also provides configuration management\. To verify the authenticity of AWS IoT Core for LoRaWAN endpoints, your gateway will use a trust certificate for each of the CUPS and LNS endpoints,

   Click the **Copy** button to copy the CUPS and LNS endpoints\. You'll need this information later to configure your gateway\. Then click the **Download server trust certificates** button to download the trust certificates for the CUPS and LNS endpoints\.

1. 

**Create the IAM role for the gateway permissions**  
You need to add an IAM role that allows the Configuration and Update Server \(CUPS\) to manage gateway credentials\. You must do this before a LoRaWAN gateway tries to connect with AWS IoT Core for LoRaWAN; however, you need to do it only once\.

   To create the **IoTWirelessGatewayCertManager** IAM role for your account, click the **Create role** button\. If the role already exists, select it from the dropdown list\.

   Click **Submit** to complete the gateway creation\.

## Add a gateway by using the API<a name="connect-iot-lorawan-onboard-gateway-api"></a>

If you're adding a gateway for the first time by using the API or CLI, you must add the **IoTWirelessGatewayCertManager** IAM role so that the gateway can connect with AWS IoT Core for LoRaWAN\. For information about how to create the role, see the following section [Add an IAM role to allow the Configuration and Update Server \(CUPS\) to manage gateway credentials](connect-iot-lorawan-rfregion-permissions.md#connect-iot-lorawan-onboard-permissions)\.

The following lists describe the API actions that perform the tasks associated with adding, updating, or deleting a LoRaWAN gateway\.

**AWS IoT Wireless API actions for AWS IoT Core for LoRaWAN gateways**
+ [CreateWirelessGateway](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_CreateWirelessGateway.html)
+ [GetWirelessGateway](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetWirelessGateway.html)
+ [ListWirelessGateways](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListWirelessGateways.html)
+ [ UpdateWirelessGateway ](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateWirelessGateway.html)
+ [DeleteWirelessGateway](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_DeleteWirelessGateway.html)

For the complete list of the actions and data types available to create and manage AWS IoT Core for LoRaWAN resources, see the [AWS IoT Wireless API reference](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/welcome.html)\.

**How to use the AWS CLI to add a gateway**  
You can use the AWS CLI to create a wireless gateway by using the [create\-wireless\-gateway](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/create-wireless-gateway.html) command\. The following example creates a wireless LoRaWAN device gateway\. You can also provide an `input.json` file that will contain additional details such as the gateway certificate and provisioning credentials\.

**Note**  
You can also perform this procedure with the API by using the methods in the AWS API that correspond to the CLI commands shown here\. 

```
aws iotwireless create-wireless-gateway \
    --lorawan GatewayEui="a1b2c3d4567890ab",RfRegion="US915" \
    --name "myFirstLoRaWANGateway" \
    --description "Using my first LoRaWAN gateway"
    --cli-input-json input.json
```

For information about the CLIs that you can use, see [AWS CLI reference](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/index.html) 