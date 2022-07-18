# Virtual private cloud \(VPC\) destinations<a name="vpc-rule-action"></a>

The Apache Kafka rule action routes data to an Apache Kafka cluster in an Amazon Virtual Private Cloud \(Amazon VPC\)\. The VPC configuration used by the Apache Kafka rule action is automatically enabled when you specify the VPC destination for your rule action\.

A VPC destination contains a list of subnets inside the VPC\. The rules engine creates an elastic network interface in each subnet that you specify in this list\. For more information about network interfaces, see [Elastic network interfaces](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html) in the Amazon EC2 User Guide\.

## Requirements and considerations<a name="vpc-action-considerations"></a>
+ If you're using a self\-managed Apache Kafka cluster that will be accessed using a public endpoint across the internet:
  + You must create a NAT gateway for instances in your subnets\. The NAT gateway has a public IP address that can connect to the internet, which allows the rules engine to forward your messages to the public Kafka cluster\.
  + As a less expensive alternative to NAT gateways, you can allocate an elastic IP address with the elastic network interfaces \(ENIs\) that are created by the VPC destination\. The security groups that you use must be configured to block incoming traffic\.
**Note**  
If the VPC destination is disabled and then re\-enabled, you must re\-associate the elastic IPs with the new ENIs\.
+ If a VPC topic rule destination doesn't receive any traffic for 30 days in a row, it will be disabled\.
+ If any resources used by the VPC destination change, the destination will be disabled and unable to be used\.
+ Some changes that can disable a VPC destination include: deleting the VPC, subnets, security groups, or the role used; modifying the role to no longer have the necessary permissions; and disabling the destination\.

## Pricing<a name="vpc-action-pricing"></a>

For pricing purposes, a VPC rule action is metered in addition to the action that sends a message to a resource when the resource is in your VPC\. For pricing information, see [AWS IoT Core pricing](https://aws.amazon.com/iot-core/pricing/)\.

## Creating virtual private cloud \(VPC\) topic rule destinations<a name="rule-destination-vpc"></a>

You create a virtual private cloud \(VPC\) destination by using the [CreateTopicRuleDestination](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateTopicRuleDestination.html) API or the AWS IoT Core console\. 

When you create a VPC destination, you must specify the following information\.

vpcId  
The unique ID of the VPC destination\.

subnetIds  
A list of subnets in which the rules engine creates elastic network interfaces\. The rules engine allocates a single network interface for each subnet in the list\.

securityGroups \(optional\)  
A list of security groups to apply to the network interfaces\.

roleArn  
The Amazon Resource Name \(ARN\) of a role that has permission to create network interfaces on your behalf\.  
This ARN should have a policy attached to it that looks like the following example\.  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:CreateNetworkInterface",
                "ec2:DescribeNetworkInterfaces",
                "ec2:DescribeVpcs",
                "ec2:DeleteNetworkInterface",
                "ec2:DescribeSubnets",
                "ec2:DescribeVpcAttribute",
                "ec2:DescribeSecurityGroups"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "ec2:CreateNetworkInterfacePermission",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "ec2:ResourceTag/VPCDestinationENI": "true"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:CreateTags"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "ec2:CreateAction": "CreateNetworkInterface",
                    "aws:RequestTag/VPCDestinationENI": "true"
                }
            }
        }
    ]
}
```

### Creating a VPC destination by using AWS CLI<a name="create-destination-vpc-cli"></a>

The following example shows how to create a VPC destination by using AWS CLI\.

```
aws --region regions iot create-topic-rule-destination --destination-configuration 'vpcConfiguration={subnetIds=["subnet-123456789101230456"],securityGroups=[],vpcId="vpc-123456789101230456",roleArn="arn:aws:iam::123456789012:role/role-name"}'
```

After you run this command, the VPC destination status will be `IN_PROGRESS`\. After a few minutes, its status will change to either `ERROR` \(if the command isn't successful\) or `ENABLED`\. When the destination status is `ENABLED`, it's ready to use\.

You can use the following command to get the status of your VPC destination\.

```
aws --region region iot get-topic-rule-destination --arn "VPCDestinationARN"
```

### Creating a VPC destination by using the AWS IoT Core console<a name="create-destination-vpc-console"></a>

The following steps describe how to create a VPC destination by using the AWS IoT Core console\.

1. Navigate to the AWS IoT Core console\. In the left pane, on the **Act** tab, choose **Destinations** \.

1. Enter values for the following fields\.
   + **VPC ID**
   + **Subnet IDs**
   + **Security Group**

1. Select a role that has the permissions required to create network interfaces\. The preceding example policy contains these permissions\.

When the VPC destination status is **ENABLED**, it's ready to use\.