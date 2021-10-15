# Connecting to AWS IoT Core for LoRaWAN through a VPC interface endpoint<a name="connect-iot-lorawan-interface-vpc-endpoint"></a>

You can connect directly to AWS IoT Core for LoRaWAN through [ Interface VPC endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/vpc/latest/privatelink/vpce-interface.html) in your Virtual Private Cloud \(VPC\) instead of connecting over the public internet\. When you use a VPC interface endpoint, communication between your VPC and AWS IoT Core for LoRaWAN is conducted entirely and securely within the AWS network\.

AWS IoT Core for LoRaWAN supports Amazon Virtual Private Cloud interface endpoints that are powered by AWS PrivateLink\. Each VPC endpoint is represented by one or more Elastic Network Interfaces \(ENIs\) with private IP addresses in your VPC subnets\.

For more information about VPC and endpoints, see [What is Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html#what-is-privatelink)\.

For more information about AWS PrivateLink, see [AWS PrivateLink and VPC endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/endpoint-services-overview.html)\. 

**AWS IoT Core for LoRaWAN privatelink architecture**  
The following diagram shows the privatelink architecture of AWS IoT Core for LoRaWAN\. The architecture uses a Transit Gateway and Route 53 Resolver to share the AWS PrivateLink interface endpoints between your VPC, the AWS IoT Core for LoRaWAN VPC, and an on\-premises environment\. You'll find a more detailed architecture diagram when setting up the connection to the VPC interface endpoints\.

![\[Image showing how you can use AWS PrivateLink to connect to AWS IoT Core for LoRaWAN endpoints.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iot-lorawan-privatelink-architecture.png)

**AWS IoT Core for LoRaWAN endpoints**  
AWS IoT Core for LoRaWAN has three public endpoints\. Each public endpoint has a corresponding VPC interface endpoint\. The public endpoints can be classified into control plane and data plane endpoints\. For information about these endpoints, see [AWS IoT Core for LoRaWAN API endpoints](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#iot-wireless_region)\.
+ 

**Control plane API endpoints**  
 You can use control plane API endpoints to interact with the AWS IoT Wireless APIs\. These endpoints can be accessed from a client that is hosted in your Amazon VPC by using AWS PrivateLink\.
+ 

**Data plane API endpoints**  
Data plane API endpoints are LoRaWAN Network Server \(LNS\) and Configuration and Update Server \(CUPS\) endpoints that you can use to interact with the AWS IoT Core for LoRaWAN LNS and CUPS endpoints\. These endpoints can be accessed from your LoRa gateways on premises by using AWS VPN or AWS Direct Connect\. You get these endpoints when onboarding your gateway to AWS IoT Core for LoRaWAN\. For more information, see [Add a gateway to AWS IoT Core for LoRaWAN](connect-iot-lorawan-onboard-gateway-add.md)\.

The following topics show how to onboard these endpoints\.

**Topics**
+ [Onboard AWS IoT Core for LoRaWAN control plane API endpoint](connect-iot-lorawan-onboard-control-plane-endpoint.md)
+ [Onboard AWS IoT Core for LoRaWAN data plane API endpoints](connect-iot-lorawan-onboard-lns-cups-endpoints.md)