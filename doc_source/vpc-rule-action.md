# VPC<a name="vpc-rule-action"></a>

The virtual private cloud \(VPC\) action routes traffic to your Amazon Virtual Private Cloud \(Amazon VPC\)\. Unlike other rule actions, the VPC action doesn't require any configuration and is automatically enabled when you specify a VPC destination for your rule action\. A VPC destination contains a list of subnets inside the VPC\. The rules engine creates an elastic network interface in each subnet that you specify in this list\.

**Note**  
For more information about network interfaces, see [Elastic network interfaces](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html) in the Amazon EC2 User Guide\.

Currently the VPC action works with the following rule actions:
+ [Apache Kafka](apache-kafka-rule-action.md)

For pricing purposes, a VPC rule action is metered in addition to the action that sends a message to a resource when the resource is in your VPC\. For pricing information, see [AWS IoT Core pricing](https://aws.amazon.com/iot-core/pricing/)\.

For information about creating VPC destinations, see [Creating a VPC topic rule destination](rule-destination.md#create-destination-vpc)\.