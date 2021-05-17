# Working with topic rule destinations<a name="rule-destination"></a>

A destination is a resource that defines where rules engine can route data\. The AWS IoT rules engine supports two kinds of destinations: HTTP destinations and VPC destinations\. Destinations make it possible for the rules engine to send data to other services that are not natively integrated with AWS IoT\. A destination can be reused across rules\.

HTTP destinations might require confirmation or configuration before they can be used\. The following paragraphs describe how the confirmation flow for HTTP destinations works\.

Topic rule destinations are used to verify that you own or have access to the endpoint to which you want to route data\. When you create a rule action with an HTTP endpoint \(for example, an `http` action\) or update an existing rule action's endpoint, AWS IoT sends a confirmation message to the endpoint that contains a unique token\. To verify your endpoint, you must call `ConfirmTopicRuleDestination` with the token to verify you own or have access to the endpoint\. The token is valid for three days, after which you need to create a new `http` action or call the `UpdateTopicRuleDestination` API to restart the confirmation process\. You can also confirm your topic rule destination by browsing to `enableUrl` or provide the token from the **Destination** page in the AWS IoT console\.

When AWS IoT receives the token sent to your endpoint, the HTTP destination is confirmed\. You must enable the destination before it can be used by rules engine\. A destination can be in one of the following states:

ENABLED  
The destination is enabled\. A destination must be in the `ENABLED` state for it to be used in a rule\. You can only enable a destinations in DISABLED status\.

DISABLED  
The destination has been confirmed but disabled\. This is useful if you want to temporarily prevent traffic to your endpoint without having to go through the confirmation process again\. You can only disable a destinations in ENABLED status\.

IN\_PROGRESS  
Confirmation of the destination is in progress\.

ERROR  
Destination confirmation timed out\.

After they are confirmed and enabled, destinations can be used with any rule in your account\.

## Creating an HTTP topic rule destination<a name="create-destination-http"></a>

A destination is created when you use AWS IoT to create a rule with an `http` action\. You can also create a destination using the `CreateTopicRuleDestination` API or the AWS IoT console\.

When you create a destination, AWS IoT verifies the endpoint URL is valid\. If the URL is valid, a confirmation message is sent to that URL\. The confirmation request has the following format:

```
HTTP POST {confirmationUrl}/?confirmationToken={confirmationToken}
Headers:
x-amz-rules-engine-message-type: DestinationConfirmation
x-amz-rules-engine-destination-arn:"arn:aws:iot:us-east-1:123456789012:ruledestination/http/7a280e37-b9c6-47a2-a751-0703693f46e4"
Content-Type: application/json
Body:
{
    "arn":"arn:aws:iot:us-east-1:123456789012:ruledestination/http/7a280e37-b9c6-47a2-a751-0703693f46e4",  
    "confirmationToken": "AYADeMXLrPrNY2wqJAKsFNn-…NBJndA",
    "enableUrl": "https://iot.us-east-1.amazonaws.com/confirmdestination/AYADeMXLrPrNY2wqJAKsFNn-…NBJndA",
    "messageType": "DestinationConfirmation"
}
```

The fields of this message are defined as follows:

arn  
The Amazon Resource Name \(ARN\) for the topic rule destination to confirm\.

confirmationToken  
The confirmation token\. The token in the example is truncated\. Your token will be longer\.

enableUrl  
The URL to which you browse to confirm a topic rule destination\.

messageType  
The type of message\.

## Creating a VPC topic rule destination<a name="create-destination-vpc"></a>

You create a virtual private cloud \(VPC\) destination by using the [CreateTopicRuleDestination](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateTopicRuleDestination.html) API or the AWS IoT Core console\. 

**Note**  
VPC topic rule destinations that don't receive any traffic for 30 days in a row will be disabled\.

When you create a VPC destination, you must specify the following information\.

vpcId  
The unique ID of the VPC destination\.

subnetIds  
A list of subnets in which the rules engine creates elastic network interfaces\. The rules engine allocates a single network interface for each subnet in the list\.

securityGroups \(optional\)  
A list of security groups to apply to the network interfaces\.

roleArn  
The ARN of a role that has permission to create network interfaces on your behalf\.  
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
            "ec2:CreateNetworkInterfacePermission",
            "ec2:DeleteNetworkInterface",
            "ec2:DescribeSubnets",
            "ec2:DescribeVpcs",
            "ec2:DescribeVpcAttribute"
            ],
            "Resource": "*"
        }
    ]
}
```

### Creating a VPC destination by using the AWS CLI<a name="create-destination-vpc-cli"></a>

The following example shows how to create a VPC destination by using the AWS CLI\.

```
aws --region regions iot create-topic-rule-destination --destination-configuration 'vpcConfiguration={subnetIds=["subnet-123456789101230456"],securityGroups=[],vpcId="vpc-123456789101230456",roleArn="arn:aws:iam::123456789012:role/role-name"}'
```

After you run this command, the VPC destination status will be `IN PROGRESS`\. After a few minutes, its status will change to either `ERROR` \(if the command isn't successful\) or `ENABLED`\. When the destination status is `ENABLED`, it is ready to use\.

You can use the following command to get the status of your VPC destination\.

```
aws --region region iot get-topic-rule-destination --arn "VPCDestinationARN"
```

### Creating a VPC destination by using the AWS IoT Core console<a name="create-destination-vpc-console"></a>

The following steps describe how to create a VPC destination by using the AWS IoT Core console\.

1. Navigate to the AWS IoT Core console\. Choose **Destinations** under the **Act** tab in the left pane\.

1. Enter values for the following fields\.
   + **VPC ID**
   + **Subnet IDs**
   + **Security Group**

1. Select a role that has the permissions required to create ENIs\. The preceding example role contains these permissions\.

When the VPC destination status is **ENABLED**, it is ready to use\.

## Confirming an HTTP topic rule destination<a name="confirm-destination"></a>

You can confirm a destination by making an HTTP GET request to the `enableUrl` that is included in the confirmation message\. You can call `ConfirmTopicRuleDestination` with the token from the confirmation message or you can use the AWS IoT console\.

The token must be valid for the destination to be put in the `ENABLED` state\. If the token has expired, you must call `UpdateTopicRuleDestination` to restart the confirmation process\.

**Note**  
 If you confirm the topic rule destination by calling `ConfirmTopicRuleDestination`, the destination status will be `DISABLED` and you need to explicitly enable your destination by calling `UpdateTopicRuleDestination` before using it\.

## Disabling a topic rule destination<a name="disable-destination"></a>

To disable a destination, call `UpdateTopicRuleDestination` and set the topic rule destination's status to `DISABLED`\.

## Enabling a topic rule destination<a name="enable-destination"></a>

To enable a destination, call `UpdateTopicRuleDestination` and set the topic rule's status to `ENABLED`\. You do not need to re\-validate the URL\.

## Sending a new confirmation message<a name="trigger-confirm"></a>

To trigger a new confirmation message for a destination, call `UpdateTopicRuleDestination` and set the topic rule destination's status to `IN_PROGRESS`\. 

## Deleting a topic rule destination<a name="delete-destination"></a>

To delete a topic rule destination, call `DeleteTopicRuleDestination`\.