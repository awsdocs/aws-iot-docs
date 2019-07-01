# Billing Groups<a name="tagging-iot-billing-groups"></a>

AWS IoT doesn't allow you to directly apply tags to individual things, but it does allow you to place things in billing groups and to apply tags to these\. For AWS IoT, allocation of cost and usage data based on tags is limited to billing groups\. 

The following commands are available:
+ [ AddThingToBillingGroup](iot-commands.md#api-iot-AddThingToBillingGroup) adds a thing to a billing group\.
+ [ CreateBillingGroup](iot-commands.md#api-iot-CreateBillingGroup) creates a billing group\.
+ [ DeleteBillingGroup](iot-commands.md#api-iot-DeleteBillingGroup) deletes the billing group\.
+ [ DescribeBillingGroup](iot-commands.md#api-iot-DescribeBillingGroup) returns information about a billing group\.
+ [ ListBillingGroups](iot-commands.md#api-iot-ListBillingGroups) lists the billing groups you have created\.
+ [ ListThingsInBillingGroup](iot-commands.md#api-iot-ListThingsInBillingGroup) lists the things you have added to the given billing group\.
+ [ RemoveThingFromBillingGroup](iot-commands.md#api-iot-RemoveThingFromBillingGroup) removes the given thing from the billing group\.
+ [ UpdateBillingGroup](iot-commands.md#api-iot-UpdateBillingGroup) updates information about the billing group\.
+ [ CreateThing](iot-commands.md#api-iot-CreateThing) allows you to specify a billing group for the thing when you create it\.
+ [ DescribeThing](iot-commands.md#api-iot-DescribeThing) returns the description of a thing including the billing group the thing belongs to, if any\.

## Viewing Cost Allocation and Usage Data<a name="tagging-iot-billing-groups-costs"></a>

You can use billing group tags to categorize and track your costs\. When you apply tags to billing groups \(and so to the things they include\), AWS generates a cost allocation report as a comma\-separated value \(CSV\) file with your usage and costs aggregated by your tags\. You can apply tags that represent business categories \(such as cost centers, application names, or owners\) to organize your costs across multiple services\. For more information about using tags for cost allocation, see [ Use Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) in the [ AWS Billing and Cost Management User Guide](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/)\.

**Note**  
To accurately associate usage and cost data with those things you have placed in billing groups, each device or application must:  
Be registered as a thing in AWS IoT \(see [Managing Devices with AWS IoT ](iot-thing-management.md)\)\.
Connect to the AWS IoT message broker via MQTT using only the thing's name as the client ID \(see [Message Broker for AWS IoT](iot-message-broker.md)\)\.
Authenticate using a client certificate associated with the thing\.

The following pricing dimensions are available for billing groups \(based on the activity of things associated with the billing group\):
+ Connectivity \(based on the thing name used as the client ID to connect\)
+ Messaging \(based on messages inbound from, and outbound to, a thing; MQTT only\)
+ Shadow operations \(based on the thing whose message triggered a shadow update\)
+ Rules triggered \(based on the thing whose inbound message triggered the rule; does not apply to those rules triggered by MQTT lifecycle events\)
+ Thing index updates \(based on the thing that was added to the index\) 
+ Remote actions \(based on the thing updated\)
+ [Detect](device-defender-detect.md) reports \(based on the thing whose activity is reported\)

Cost and usage data based on tags \(and reported for a billing group\) doesn't reflect the following activities:
+ Device registry operations \(including updates to things, thing groups, and thing types; see [Managing Devices with AWS IoT ](iot-thing-management.md)\)
+ Thing group index updates \(when adding a thing group\)
+ Index search queries
+ [Device Provisioning](iot-provision.md)
+ [Audit](device-defender-audit.md) reports 

## Limits<a name="tagging-iot-billing-groups-limits"></a>
+ A thing can belong to exactly one billing group\.
+ Unlike thing groups, billing groups cannot be organized into hierarchies\.
+ In order for its usage to be registered for tagging or billing purposes, a device must:
  + Be registered as a thing in AWS IoT\.
  + Communicate with AWS IoT using MQTT only\.
  + Authenticate with AWS IoT using only its thing name as the clientID\.
  + Use only an X\.509 certificate or Amazon Cognito Identity to authenticate\.

  Additional information can be found in [Managing Devices with AWS IoT ](iot-thing-management.md), [Security and Identity for AWS IoT ](iot-security-identity.md), and [Device Provisioning](iot-provision.md)\. The API command [ AttachThingPrincipal](iot-commands.md#api-iot-AttachThingPrincipal), can be used to attach a certificate or other credential to a thing\. 
+ The maximum number of billing groups per account is 20,000\.