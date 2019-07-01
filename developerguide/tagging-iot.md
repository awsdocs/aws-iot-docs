# Tagging Your AWS IoT Resources<a name="tagging-iot"></a>

To help you manage and organize your thing groups, thing types, topic rules, jobs, scheduled audits and security profiles you can optionally assign your own metadata to each of these resources in the form of tags\. This section describes tags and shows you how to create them\.

To help you manage your costs related to things, you can create [billing groups](tagging-iot-billing-groups.md) that contain things\. You can then assign tags containing your metadata to each of these billing groups\. This section also discusses billing groups and the commands available to create and manage them\.

## Tag Basics<a name="tagging-iot-basics"></a>

Tags enable you to categorize your AWS IoT resources in different ways, for example, by purpose, owner, or environment\. This is useful when you have many resources of the same type — you can quickly identify a specific resource based on the tags you've assigned to it\. Each tag consists of a key and optional value, both of which you define\. For example, you could define a set of tags for your thing types that helps you track devices by type\. We recommend that you create a set of tag keys that meets your needs for each kind of resource\. Using a consistent set of tag keys makes it easier for you to manage your resources\.

You can search for and filter resources based on the tags you add or apply\. You can also use billing group tags to categorize and track your costs\. You can also use tags to control access to your resources as described in [Using Tags with IAM Policies](tagging-iot-iam.md#tagging-iot-iam.title)\.

For ease of use, the Tag Editor in the AWS Management Console provides a central, unified way to create and manage your tags\. For more information, see [Working with Tag Editor](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/tag-editor.html) in [ Working with the AWS Management Console](http://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/getting-started.html)\.

You can also work with tags using the AWS CLI and the AWS IoT API\. You can associate tags with thing groups, thing types, topic rules, jobs, security profiles, and billing groups when you create them by using the "Tags" field in the following commands: 
+ [ CreateBillingGroup](iot-commands.md#api-iot-CreateBillingGroup)
+ [ CreateDynamicThingGroup](iot-commands.md#api-iot-CreateDynamicThingGroup)
+ [ CreateJob](iot-commands.md#api-iot-CreateJob)
+ [ CreateOTAUpdate](iot-commands.md#api-iot-CreateOTAUpdate)
+ [ CreateScheduledAudit](iot-commands.md#api-iot-CreateScheduledAudit)
+ [ CreateSecurityProfile](iot-commands.md#api-iot-CreateSecurityProfile)
+ [ CreateStream](iot-commands.md#api-iot-CreateStream)
+ [ CreateThingGroup](iot-commands.md#api-iot-CreateThingGroup)
+ [ CreateThingType](iot-commands.md#api-iot-CreateThingType)
+ [ CreateTopicRule](iot-commands.md#api-iot-CreateTopicRule)

You can add, modify, or delete tags for existing resources that support tagging by using the following commands:
+ [ TagResource](iot-commands.md#api-iot-TagResource)
+ [ ListTagsForResource](iot-commands.md#api-iot-ListTagsForResource)
+ [ UntagResource](iot-commands.md#api-iot-UntagResource)

You can edit tag keys and values, and you can remove tags from a resource at any time\. You can set the value of a tag to an empty string, but you can't set the value of a tag to null\. If you add a tag that has the same key as an existing tag on that resource, the new value overwrites the old value\. If you delete a resource, any tags associated with the resource are also deleted\.

Additional information is available in [AWS Tagging Strategies](https://aws.amazon.com/answers/account-management/aws-tagging-strategies/)\.

### Tag Restrictions and Limitations<a name="tagging-iot-restrict"></a>

The following basic restrictions apply to tags:
+ Maximum number of tags per resource — 50
+ Maximum key length — 127 Unicode characters in UTF\-8
+ Maximum value length — 255 Unicode characters in UTF\-8
+ Tag keys and values are case\-sensitive\.
+ Do not use the "aws:" prefix in your tag names or values because it's reserved for AWS use\. You can't edit or delete tag names or values with this prefix\. Tags with this prefix don't count against your tags per resource limit\.
+ If your tagging schema is used across multiple services and resources, remember that other services may have restrictions on allowed characters\. Generally, allowed characters are: letters, spaces, and numbers representable in UTF\-8, and the following special characters: \+ \- = \. \_ : / @\. 