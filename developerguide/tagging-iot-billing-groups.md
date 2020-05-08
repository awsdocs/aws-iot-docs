# Billing groups<a name="tagging-iot-billing-groups"></a>

AWS IoT doesn't allow you to directly apply tags to individual things, but it does allow you to place things in billing groups and to apply tags to these\. For AWS IoT, allocation of cost and usage data based on tags is limited to billing groups\. 

The following commands are available:
+ [AddThingToBillingGroup](https://docs.aws.amazon.com/iot/latest/apireference/API_AddThingToBillingGroup) adds a thing to a billing group\.
+ [CreateBillingGroup](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateBillingGroup) creates a billing group\.
+ [DeleteBillingGroup](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteBillingGroup) deletes the billing group\.
+ [DescribeBillingGroup](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeBillingGroup) returns information about a billing group\.
+ [ListBillingGroups](https://docs.aws.amazon.com/iot/latest/apireference/API_ListBillingGroups) lists the billing groups you have created\.
+ [ListThingsInBillingGroup](https://docs.aws.amazon.com/iot/latest/apireference/API_ListThingsInBillingGroup) lists the things you have added to the given billing group\.
+ [RemoveThingFromBillingGroup](https://docs.aws.amazon.com/iot/latest/apireference/API_RemoveThingFromBillingGroup) removes the given thing from the billing group\.
+ [UpdateBillingGroup](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateBillingGroup) updates information about the billing group\.
+ [CreateThing](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateThing) allows you to specify a billing group for the thing when you create it\.
+ [DescribeThing](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeThing) returns the description of a thing including the billing group the thing belongs to, if any\.

## Viewing cost allocation and usage data<a name="tagging-iot-billing-groups-costs"></a>

You can use billing group tags to categorize and track your costs\. When you apply tags to billing groups \(and so to the things they include\), AWS generates a cost allocation report as a comma\-separated value \(CSV\) file with your usage and costs aggregated by your tags\. You can apply tags that represent business categories \(such as cost centers, application names, or owners\) to organize your costs across multiple services\. For more information about using tags for cost allocation, see [ Use Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) in the [ AWS Billing and Cost Management User Guide](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/)\.

**Note**  
To accurately associate usage and cost data with those things you have placed in billing groups, each device or application must:  
Be registered as a thing in AWS IoT\. For more information, see [Managing devices with AWS IoT ](iot-thing-management.md)\.
Connect to the AWS IoT message broker through MQTT using only the thing's name as the client ID\. For more information, see [Message broker for AWS IoT](iot-message-broker.md)\.
Authenticate using a client certificate associated with the thing\.

The following pricing dimensions are available for billing groups \(based on the activity of things associated with the billing group\):
+ Connectivity \(based on the thing name used as the client ID to connect\)\.
+ Messaging \(based on messages inbound from, and outbound to, a thing; MQTT only\)\.
+ Shadow operations \(based on the thing whose message triggered a shadow update\)\.
+ Rules triggered \(based on the thing whose inbound message triggered the rule; does not apply to those rules triggered by MQTT lifecycle events\)\.
+ Thing index updates \(based on the thing that was added to the index\)\. 
+ Remote actions \(based on the thing updated\)\.
+ [Detect](device-defender-detect.md) reports \(based on the thing whose activity is reported\)\.

Cost and usage data based on tags \(and reported for a billing group\) doesn't reflect the following activities:
+ Device registry operations \(including updates to things, thing groups, and thing types\)\. For more information, see [Managing devices with AWS IoT ](iot-thing-management.md)\)\.
+ Thing group index updates \(when adding a thing group\)\.
+ Index search queries\.
+ [Device provisioning](iot-provision.md)\.
+ [Audit](device-defender-audit.md) reports\. 