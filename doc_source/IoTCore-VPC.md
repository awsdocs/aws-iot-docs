# Using AWS IoT Core with interface VPC endpoints<a name="IoTCore-VPC"></a>

With AWS IoT Core you can create [IoT data endpoints](https://docs.aws.amazon.com/iot/latest/developerguide/iot-connect-devices.html) within your VPC by using [interface VPC endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint)\. Interface VPC endpoints are powered by AWS PrivateLink, an AWS technology that you can use to access services running on AWS by using private IP addresses\. For more information, see [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html)\.

In order to connect devices in the field on remote networks, such as a corporate network to your AWS VPC, refer to the various options listed in the [Network\-to\-Amazon VPC connectivity matrix](https://docs.aws.amazon.com/whitepapers/latest/aws-vpc-connectivity-options/network-to-amazon-vpc-connectivity-options.html)\.

**Note**  
VPC endpoints for IoT Core are currently not supported in AWS China Regions\.

**Chapter Topics:**
+ [Creating VPC endpoints for AWS IoT Core](#Create-VPC-endpoints)
+ [Controlling Access to AWS IoT Core over VPC endpoints](#Control-VPC-access)
+ [Limitations of VPC endpoints](#VPC-limitations)
+ [Scaling VPC endpoints with IoT Core](#Scaling-VPC-endpoints)
+ [Using custom domains with VPC endpoints](#VPC-custom-domains)
+ [Availability of VPC endpoints for AWS IoT Core](#VPC-availability)

## Creating VPC endpoints for AWS IoT Core<a name="Create-VPC-endpoints"></a>

To get started with VPC endpoints, simply [create an interface VPC endpoint](https://docs.aws.amazon.com/vpc/latest/privatelink/vpce-interface.html#create-interface-endpoint), and select AWS IoT Core as the AWS service\. If you are using the CLI, first call [describe\-vpc\-endpoint\-services](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-vpc-endpoint-services.html) to ensure that you are choosing an Availability Zone where AWS IoT Core is present in your particular AWS Region\. For example, in us\-east\-1, this command would look like:

```
aws ec2 describe-vpc-endpoint-services --service-name com.amazonaws.us-east-1.iot.data
```

**Note**  
We recommend that you do not use the PrivateDNS option for your VPC endpoints with AWS IoT Core\. See [Limitations of VPC endpoints](#VPC-limitations)\.

For routing DNS queries from your devices to the VPC endpoint Interfaces correctly, you must create DNS records in a Private Hosted Zone that is attached to your VPC\. To get started, see [Creating A Private Hosted Zone](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/hosted-zone-private-creating.html)\. Within your Private Hosted Zone, you must create an alias record for each ENI IP for the VPC endpoint\. If you have multiple ENI IPs, for multiple VPC endpoints, then you should create weighted DNS records with equal weights across all the weighted records\. These IP addresses are available from the DescribeNetworkInterfaces API Call when filtered by the VPC endpoint ID in the description field\.

## Controlling Access to AWS IoT Core over VPC endpoints<a name="Control-VPC-access"></a>

You can restrict device access to AWS IoT Core to be allowed only through VPC endpoint by using VPC [condition context keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html)\. AWS IoT Core supports the following VPC related context keys:
+ [SourceVpc](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcevpc)
+ [SourceVpce](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcevpce)
+ [VPCSourceIp](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-vpcsourceip)

**Note**  
AWS IoT Core does not support [https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-access.html#vpc-endpoint-policies](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-access.html#vpc-endpoint-policies)VPC endpoint policies at this time\.

 For example, the following policy grants permission to connect to AWS IoT Core using a client ID that matches the thing name and to publish to any topic prefixed by the thing name, conditional on the device connecting to a VPC endpoint with a particular VPC Endpoint ID\. This policy would deny connection attempts to your public IoT data endpoint\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iot:Connect"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:client/${iot:Connection.Thing.ThingName}"
            ],
            "Condition": {
                "StringEquals": {
                    "aws:SourceVpce": "vpce-1a2b3c4d"
                }
            }
            
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Publish"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/${iot:Connection.Thing.ThingName}/*"
            ]
        }
    ]
}
```

## Limitations of VPC endpoints<a name="VPC-limitations"></a>

This section covers the limitations of VPC endpoints compared to public endpoints\.
+ VPC endpoints are currently supported for [IoT data endpoints](https://docs.aws.amazon.com/iot/latest/developerguide/iot-connect-devices.html) only
+ MQTT keep alive periods are limited to 230 seconds\. Keep alives longer than that period will be automatically reduced to 230 seconds
+ Each VPC endpoint supports 100,000 total concurrent connected devices\. If you require more connections see [Scaling VPC endpoints with IoT Core](#Scaling-VPC-endpoints)\.
+ VPC endpoints support IPv4 traffic only\.
+ VPC endpoints will serve [ATS certificates](https://docs.aws.amazon.com/iot/latest/developerguide/server-authentication.html) only, except for custom domains\.
+ [VPC endpoint policies](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-access.html) are not supported at this time\.

## Scaling VPC endpoints with IoT Core<a name="Scaling-VPC-endpoints"></a>

AWS IoT Core Interface VPC endpoints are limited to 100,000 connected devices over a single interface endpoint\. If your use case calls for more concurrent connections to the broker, then we recommend using multiple VPC endpoints and manually routing your devices across your interface endpoints\. When creating private DNS records to route traffic to your VPC endpoints, make sure to create as many weighted records as you have VPC endpoints to distribute traffic across your multiple endpoints\. 

## Using custom domains with VPC endpoints<a name="VPC-custom-domains"></a>

If you want to use custom domains with VPC endpoints, you must create your custom domain name records in a Private Hosted Zone and create routing records in Route53\. For more information, see [Creating A Private Hosted Zone](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/hosted-zone-private-creating.html)\.

## Availability of VPC endpoints for AWS IoT Core<a name="VPC-availability"></a>

AWS IoT Core Interface VPC endpoints are available in all [AWS IoT Core supported regions](http://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/), with the exception of AWS China Regions\.