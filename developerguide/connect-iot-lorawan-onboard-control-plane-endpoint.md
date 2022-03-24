# Onboard AWS IoT Core for LoRaWAN control plane API endpoint<a name="connect-iot-lorawan-onboard-control-plane-endpoint"></a>

You can use AWS IoT Core for LoRaWAN control plane API endpoints to interact with the AWS IoT Wireless APIs\. For example, you can use this endpoint to run the [SendDataToWirelessDevice](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_SendDataToWirelessDevice.html) API to send data from AWS IoT to your LoRaWAN device\. For more information, see [AWS IoT Core for LoRaWAN Control Plane API Endpoints](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#iot-core.html#iot-wireless-control-plane-endpoints)\.

You can use the client hosted in your Amazon VPC to access the control plane endpoints that are powered by AWS PrivateLink\. You use these endpoints to connect to the AWS IoT Wireless API through an interface endpoint in your Virtual Private Cloud \(VPC\) instead of connecting over the public internet\.

**Topics**
+ [Create your Amazon VPC and subnet](#connect-iot-lorawan-control-plane-create-vpc)
+ [Launch an Amazon EC2 instance in your subnet](#connect-iot-lorawan-launch-ec2-instance)
+ [Create Amazon VPC interface endpoint](#connect-iot-lorawan-create-vpc-endpoint)
+ [Test your connection to the interface endpoint](#connect-iot-lorawan-connect-vpc-endpoint)

## Create your Amazon VPC and subnet<a name="connect-iot-lorawan-control-plane-create-vpc"></a>

Before you can connect to the interface endpoint, you must create a VPC and subnet\. You'll then launch an EC2 instance in your subnet, which you can use to connect to the interface endpoint\. 

To create your VPC:

1. Navigate to the [VPCs](https://console.aws.amazon.com/vpc/home#/vpcs) page of the Amazon VPC console and choose **Create VPC**\.

1. On the **Create VPC** page:
   + Enter a name for **VPC Name tag \- optional** \(for example, **VPC\-A**\)\.
   + Enter an IPv4 address range for your VPC in the IPv4 CIDR block \(for example, **10\.100\.0\.0/16**\)\.

1. Keep the default values for other fields and choose **Create VPC**\.

To create your subnet:

1. Navigate to the [Subnets](https://console.aws.amazon.com/vpc/home#/subnets) page of the Amazon VPC console and choose **Create subnet**\.

1. On the **Create subnet** page:
   + For **VPC ID**, choose the VPC that you created earlier \(for example, `VPC-A`\)\.
   + Enter a name for **Subnet name** \(for example, **Private subnet**\)\.
   + Choose the **Availability Zone** for your subnet\.
   + Enter your subnet's IP address block in the **IPv4 CIDR block** in CIDR format \(for example, **10\.100\.0\.0/24**\)\.

1. To create your subnet and add it to your VPC, choose **Create subnet**\.

For more information, see [Work with VPCs and subnets](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html)\.

## Launch an Amazon EC2 instance in your subnet<a name="connect-iot-lorawan-launch-ec2-instance"></a>

To launch your EC2 instance:

1. Navigate to the [Amazon EC2](https://console.aws.amazon.com/ec2/home#/) console and choose **Launch Instance**\.

1. For AMI, choose **Amazon Linux 2 AMI \(HVM\), SSD Volume Type** and then choose the **t2 micro** instance type\. To configure the instance details, choose **Next**\.

1. In the **Configure Instance Details** page:
   + For **Network**, choose the VPC that you created earlier \(for example, `VPC-A`\)\.
   + For **Subnet**, choose the subnet that you created earlier \(for example, **Private subnet**\)\.
   + For **IAM role**, choose the role **AWSIoTWirelessFullAccess** to grant AWS IoT Core for LoRaWAN full access policy\. For more information, see [`AWSIoTWirelessFullAccess` policy summary](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIoTWirelessFullAccess$serviceLevelSummary)\.
   + For **Assume Private IP**, use an IP address, for example, **10\.100\.0\.42**\.

1. Choose **Next: Add Storage** and then choose **Next: Add Tags**\. You can optionally add any tags to associate with your EC2 instance\. Choose **Next: Configure Security Group**\.

1. In the **Configure Security Group** page, configure the security group to allow:
   + Open **All TCP** for Source as `10.200.0.0/16`\.
   + Open **All ICMP \- IPV4** for Source as `10.200.0.0/16`\.

1. To review the instance details and launch your EC2 instance, choose **Review and Launch**\.

For more information, see [Get started with Amazon EC2 Linux instances](https://docs.aws.amazon.com/AWSEC2/latest/userguide/EC2_GetStarted.html)\.

## Create Amazon VPC interface endpoint<a name="connect-iot-lorawan-create-vpc-endpoint"></a>

You can create a VPC endpoint for your VPC, which can then be accessed by the EC2 API\. To create the endpoint:

1. Navigate to the [VPC](https://console.aws.amazon.com/vpc/home#/endpoints) **Endpoints** console and choose **Create Endpoint**\.

1. In the **Create Endpoint** page, specify the following information\.
   + Choose **AWS services** for **Service category**\.
   + For **Service Name**, search by entering the keyword **iotwireless**\. In the list of `iotwireless` services displayed, choose the control plane API endpoint for your Region\. The endpoint will be in the format `com.amazonaws.region.iotwireless.api`\.
   + For **VPC** and **Subnets**, choose the VPC where you want to create the endpoint, and the Availability Zones \(AZs\) in which you want to create the endpoint network\.
**Note**  
The `iotwireless` service might not support all Availability Zones\.
   + For **Enable DNS name**, choose **Enable for this endpoint**\. 

     Choosing this option will automatically resolve the DNS and create a route in Amazon Route 53 Public Data Plane so that the APIs you use later to test the connection will go through the privatelink endpoints\.
   + For **Security group**, choose the security groups you want to associate with the endpoint network interfaces\.
   + Optionally, you can add or remove tags\. Tags are name\-value pairs that you use to associate with your endpoint\. 

1. To create your VPC endpoint, choose **Create endpoint**\.

## Test your connection to the interface endpoint<a name="connect-iot-lorawan-connect-vpc-endpoint"></a>

You can use an SSH to access your Amazon EC2 instance and then use the AWS CLI to connect to the privatelink interface endpoints\.

Before you connect to the interface endpoint, download the most recent AWS CLI version by following the instructions described in [Installing, updating, and uninstalling AWS CLI version 2 on Linux](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html)\.

The following examples show how you can test your connection to the interface endpoint using the CLI\.

```
aws iotwireless create-service-profile \ 
    --endpoint-url https://api.iotwireless.region.amazonaws.com  \ 
    --name='test-privatelink'
```

The following shows an example of running the command\.

```
Response:
{
 "Arn": "arn:aws:iotwireless:region:acct_number:ServiceProfile/1a2345ba-4c5d-67b0-ab67-e0c8342f2857",
 "Id": "1a2345ba-4c5d-67b0-ab67-e0c8342f2857"
}
```

Similarly, you can run the following commands to get the service profile information or list all service profiles\.

```
aws iotwireless get-service-profile \ 
    --endpoint-url https://api.iotwireless.region.amazonaws.com  
    --id="1a2345ba-4c5d-67b0-ab67-e0c8342f2857"
```

The following shows an example for the list\-device\-profiles command\.

```
aws iotwireless list-device-profiles \ 
    --endpoint-url https://api.iotwireless.region.amazonaws.com
```