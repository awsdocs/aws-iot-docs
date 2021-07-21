# Create VPC interface endpoint and private hosted zone<a name="connect-iot-lorawan-create-vpc-lns-cups"></a>

AWS IoT Core for LoRaWAN has two data plane endpoints, Configuration and Update Server \(CUPS\) endpoint and LoRaWAN Network Server \(LNS\) endpoint\. The setup process to establish a privatelink connection to both endpoints is the same, so we can use the LNS endpoint for illustration purposes\.

For your data plane endpoints, the LoRa gateways first connect to your AWS account in your Amazon VPC, which then connects to the VPC endpoint in the AWS IoT Core for LoRaWAN VPC\.

When connecting to the endpoints, the DNS names can be resolved within one VPC but can't be resolved across multiple VPCs\. To disable private DNS when creating the endpoint, disable the **Enable DNS name** setting\. You can use private hosted zone to provide information about how you want Route 53 to respond to DNS queries for your VPCs\. To share your VPC with an on\-premises environment, you can use a Route 53 Resolver to facilitate hybrid DNS\.

**Topics**
+ [Create an Amazon VPC and subnet](#connect-iot-lorawan-lns-create-vpc)
+ [Create an Amazon VPC interface endpoint](#connect-iot-lorawan-create-vpc-endpoint-lns)
+ [Configure private hosted zone](#connect-iot-lorawan-create-phz-lns)
+ [Configure Route 53 inbound resolver](#connect-iot-lorawan-configure-route53-resolver)
+ [Next steps](#connect-iot-lorawan-lns-cups-next-steps)

## Create an Amazon VPC and subnet<a name="connect-iot-lorawan-lns-create-vpc"></a>

You can reuse your Amazon VPC and subnet that you created when onboarding your control plane endpoint\. For information, see [Create your Amazon VPC and subnet](connect-iot-lorawan-onboard-control-plane-endpoint.md#connect-iot-lorawan-control-plane-create-vpc)\.

## Create an Amazon VPC interface endpoint<a name="connect-iot-lorawan-create-vpc-endpoint-lns"></a>

You can create a VPC endpoint for your VPC, which is similar to how you would create one for your control plane endpoint\.

1. Navigate to the [VPC](https://console.aws.amazon.com/vpc/home#/endpoints) **Endpoints** console and choose **Create Endpoint**\.

1. In the **Create Endpoint** page, specify the following information\.
   + Choose **AWS services** for **Service category**\.
   + For **Service Name**, search by entering the keyword **lns**\. In the list of `lns` services displayed, choose the LNS data plane API endpoint for your Region\. The endpoint will be of the format `com.amazonaws.region.lorawan.lns`\.
**Note**  
If you're following this procedure for your CUPS endpoint, search for `cups`\. The endpoint will be of the format `com.amazonaws.region.lorawan.cups`\.
   + For **VPC** and **Subnets**, choose the VPC where you want to create the endpoint, and the Availability Zones \(AZs\) in which you want to create the endpoint network\.
**Note**  
The `iotwireless` service might not support all Availability Zones\.
   + For **Enable DNS name**, make sure that **Enable for this endpoint** is not selected\.

     By not selecting this option, you can disable private DNS for the VPC endpoint and use private hosted zone instead\.
   + For **Security group**, choose the security groups you want to associate with the endpoint network interfaces\.
   + Optionally, you can add or remove tags\. Tags are name\-value pairs that you use to associate with your endpoint\. 

1. To create your VPC endpoint, choose **Create endpoint**\.

## Configure private hosted zone<a name="connect-iot-lorawan-create-phz-lns"></a>

After you create the privatelink endpoint, in the **Details** tab of your endpoint, you'll see a list of DNS names\. You can use one of these DNS names to configure your private hosted zone\. The DNS name will be of the format `vpce-xxxx.lns.lorawan.region.vpce.amazonaws.com`\.

**Create the private hosted zone**  
To create the private hosted zone:

1. Navigate to the [Route 53](https://console.aws.amazon.com/route53/v2/hostedzones#/) **Hosted zones** console and choose **Create hosted zone**\.

1. In the **Create hosted zone** page, specify the following information\.
   + For **Domain name**, enter the full service name for your LNS endpoint, **lns\.lorawan\.region\.amazonaws\.com**\.
**Note**  
If you're following this procedure for your CUPS endpoint, enter **cups\.lorawan\.region\.amazonaws\.com**\.
   + For **Type**, choose **Private hosted zone**\.
   + Optionally, you can add or remove tags to associate with your hosted zone\.

1. To create your private hosted zone, choose **Create hosted zone**\.

For more information, see [Creating a private hosted zone](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/hosted-zone-private-creating.html)\.

After you have created a private hosted zone, you can create a record that tells the DNS how you want traffic to be routed to that domain\.

**Create a record**  
After you have created a private hosted zone, you can create a record that tells the DNS how you want traffic to be routed to that domain\. To create a record:

1. In the list of hosted zones displayed, choose the private hosted zone that you created earlier and choose **Create record**\.

1. Use the wizard method to create the record\. If the console presents you the **Quick create** method, choose **Switch to wizard**\.

1. Choose **Simple Routing** for **Routing policy** and then choose **Next**\.

1. In the **Configure records** page, choose **Define simple record**\.

1. In the **Define simple record** page:
   + For **Record name**, enter the alias of your AWS account number\. You get this value when onboarding your gateway or by using the [https://docs.aws.amazon.com/iotwireless/latest/apireference/API_GetServiceEndpoint.html](https://docs.aws.amazon.com/iotwireless/latest/apireference/API_GetServiceEndpoint.html) REST API\.
   + For **Record type**, keep the value as `A - Routes traffic to an IPv4 address and some AWS resources`\.
   + For **Value/Route traffic to**, choose **Alias to VPC endpoint**\. Then choose your **Region** and then choose the endpoint that you created previously, as described in [Create an Amazon VPC interface endpoint](#connect-iot-lorawan-create-vpc-endpoint-lns) from the list of endpoints displayed\.

1. Choose **Define simple record** to create your record\.

## Configure Route 53 inbound resolver<a name="connect-iot-lorawan-configure-route53-resolver"></a>

To share a VPC endpoint to an on\-premises environment, a Route 53 Resolver can be used to facilitate hybrid DNS\. The inbound resolver will enable you to route traffic from the on\-premises network to the data plane endpoints without going over the public internet\. To return the private IP address values for your service, create the Route 53 Resolver in the same VPC as the VPC endpoint\.

When you create the inbound resolver, you only have to specify your VPC and the subnets that you created previously in your Availability Zones \(AZs\)\. The Route 53 Resolver uses this information to automatically assigns an IP address to route traffic to each of the subnets\.

To create the inbound resolver:

1. Navigate to the [Route 53](https://console.aws.amazon.com/route53/v2/inbound-endpoints#/) **Inbound endpoints** console and choose **Create inbound endpoint**\.
**Note**  
Make sure that you're using the same AWS Region that you used when creating the endpoint and private hosted zone\.

1. In the **Create inbound endpoint** page, specify the following information\.
   + Enter a name for **Endpoint name** \(for example, **VPC\_A\_Test**\)\.
   + For **VPC in the region**, choose the same VPC that you used when creating the VPC endpoint\.
   + Configure the **Security group for this endpoint** to allow incoming traffic from the on premises network\.
   + For IP address, choose **Use an IP address that is selected automatically\.**

1. Choose **Submit** to create your inbound resolver\.

For this eample, let's assume that the IP addresses `10.100.0.145` and `10.100.192.10` were assigned for the inbound Route 53 Resolver for routing traffic\.

## Next steps<a name="connect-iot-lorawan-lns-cups-next-steps"></a>

You've created the private hosted zone and an inbound resolver to route traffic for your DNS entries\. You can now use either a Site\-to\-Site VPN or a Client VPN endpoint\. For more information, see [Use VPN to connect LoRa gateways to your AWS account](connect-iot-lorawan-create-vpc-vpn-connection.md)\.