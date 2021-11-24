# AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan"></a>

AWS IoT Core for LoRaWAN is a fully managed LoRaWAN network server \(LNS\) that provides gateway management using the Configuration and Update Server \(CUPS\) and Firmware Updates Over\-The\-Air \(FUOTA\) capabilities\. You can replace your private LNS with AWS IoT Core for LoRaWAN and connect your Long Range Wide Area Network \(LoRaWAN\) devices and gateways to AWS IoT Core\. By doing so, you'll reduce the maintenance, operational costs, setup time, and overhead costs\.

## Introduction<a name="connect-iot-lorawan-intro"></a>

LoRaWAN devices are long\-range, low\-power, battery\-operated devices that use the LoRaWAN protocol to operate in a license\-free radio spectrum\. LoRaWAN is a Low Power Wide Area Network \(LPWAN\) communication protocol that is built on LoRa\. LoRa is the physical layer protocol that enables low power, wide\-area communication between devices\.

You can onboard your LoRaWAN devices the same way you would onboard other IoT devices to AWS IoT\. To connect your LoRaWAN devices to AWS IoT, you must use a LoRaWAN gateway\. The gateway acts as a bridge to connect your device to AWS IoT Core for LoRaWAN and to exchange messages\. AWS IoT Core for LoRaWAN uses the AWS IoT rules engine to route the messages from your LoRaWAN devices to other AWS IoT services\.

To reduce development effort and quickly onboard your devices to AWS IoT Core for LoRaWAN, we recommend that you use LoRaWAN\-certified end devices\. For more information, see the [AWS IoT Core for LoRaWAN product overview](http://aws.amazon.com/iot/lorawan) page\. For information about getting your devices LoRaWAN certified, see [Certifying LoRaWAN products](https://lora-alliance.org/lorawan-certification/)\. 

## How to use AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-how-use"></a>

You can quickly onboard your LoRaWAN devices and gateways to AWS IoT Core for LoRaWAN by using the console or the AWS IoT Wireless API\.

**Using the console**  
To onboard your LoRaWAN devices and gateways by using the AWS Management Console, sign in to the AWS Management Console and navigate to the [AWS IoT Core for LoRaWAN](https://console.aws.amazon.com/https://console.aws.amazon.com/iot/home#/wireless/landing) page in the AWS IoT console\. You can then use the **Intro** section to add your gateways and devices to AWS IoT Core for LoRaWAN\. For more information, see [Using the console to onboard your device and gateway to AWS IoT Core for LoRaWAN](connect-iot-lorawan-getting-started.md#connect-iot-lorawan-console)\.

**Using the API or CLI**  
You can onboard both LoRaWAN and Sidewalk devices by using the [AWS IoT Wireless](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/) API\. The AWS IoT Wireless API that AWS IoT Core for LoRaWAN is built on is supported by the AWS SDK\. For more information, see [AWS SDKs and Toolkits](https://aws.amazon.com/getting-started/tools-sdks/)\. 

You can use the AWS CLI to run commands for onboarding and managing your LoRaWAN gateways and devices\. For more information, see [AWS IoT Wireless CLI reference](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/index.html)\. 

## AWS IoT Core for LoRaWAN Regions and endpoints<a name="connect-iot-lorawan-regions-endpoints"></a>

AWS IoT Core for LoRaWAN provides support for control plane and data plane API endpoints that are specific to your AWS Region\. The data plane API endpoints are specific to your AWS account and AWS Region\. For more information about the AWS IoT Core for LoRaWAN endpoints, see [AWS IoT Core for LoRaWAN Endpoints](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#iot-wireless_region) in the *AWS General Reference*\.

In the Regions US East \(N\. Virginia\) and Europe \(Ireland\), you can connect your devices to AWS IoT Core for LoRaWAN through AWS PrivateLink in your virtual private cloud \(VPC\), instead of connecting over the public internet\. For more information, see [Connecting to AWS IoT Core for LoRaWAN through a VPC interface endpoint](connect-iot-lorawan-interface-vpc-endpoint.md)\.

AWS IoT Core for LoRaWAN has quotas that apply to device data that is transmitted between the devices and the maximum TPS for the AWS IoT Wireless API operations\. For more information, see [AWS IoT Core for LoRaWAN quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#wireless-limits) in the *AWS General Reference*

## AWS IoT Core for LoRaWAN pricing<a name="connect-iot-lorawan-pricing"></a>

When you sign up for AWS, you can get started with AWS IoT Core for LoRaWAN for no charge by using the [AWS Free Tier](http://aws.amazon.com/free/)\. 

For more information about general product overview and pricing, see [AWS IoT Core pricing](http://aws.amazon.com/iot-core/pricing/)\.