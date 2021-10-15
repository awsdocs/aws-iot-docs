# Static thing groups<a name="thing-groups"></a>

Static thing groups allow you to manage several things at once by categorizing them into groups\. Static thing groups contain a group of things that are managed by using the console, CLI, or the API\. [Dynamic thing groups](dynamic-thing-groups.md), on the other hand, contain things that match a specified query\. Static thing groups can also contain other static thing groups — you can build a hierarchy of groups\. You can attach a policy to a parent group and it is inherited by its child groups, and by all of the things in the group and in its child groups\. This makes control of permissions easy for large numbers of things\.

Here are the things you can do with static thing groups:
+ Create, describe or delete a group\.
+ Add a thing to a group, or to more than one group\.
+ Remove a thing from a group\.
+ List the groups you have created\.
+ List all child groups of a group \(its direct and indirect descendants\.\)
+ List the things in a group, including all the things in its child groups\.
+ List all ancestor groups of a group \(its direct and indirect parents\.\)
+ Add, delete or update the attributes of a group\. \(Attributes are name\-value pairs you can use to store information about a group\.\)
+ Attach or detach a policy to or from a group\.
+ List the policies attached to a group\.
+ List the policies inherited by a thing \(by virtue of the policies attached to its group, or one of its parent groups\.\)
+ Configure logging options for things in a group\. See [Configure AWS IoT logging](configure-logging.md)\. 
+ Create jobs that are sent to and executed on every thing in a group and its child groups\. See [Jobs](iot-jobs.md)\.

Here are some limitations of static thing groups:
+ A group can have at most one direct parent\.
+ If a group is a child of another group, you must specify this at the time it is created\.
+ You can't change a group's parent later, so be sure to plan your group hierarchy and create a parent group before you create any child groups it contains\.
+ 

  The number of groups to which a thing can belong is [limited](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#thing-limits)\.
+ You cannot add a thing to more than one group in the same hierarchy\. \(In other words, you cannot add a thing to two groups that share a common parent\.\)
+ You cannot rename a group\.
+ Thing group names can't contain international characters, such as û, é and ñ\.
+ You should not use personally identifiable information in your thing group name\. The thing group name can appear in unencrypted communications and reports\. 
+  You should not use a colon character \( : \) in a thing group name\. The colon character is used as a delimiter by other AWS IoT services and this can cause them to parse strings with thing group names incorrectly\. 

Attaching and detaching policies to groups can enhance the security of your AWS IoT operations in a number of significant ways\. The per\-device method of attaching a policy to a certificate, which is then attached to a thing, is time consuming and makes it difficult to quickly update or change policies across a fleet of devices\. Having a policy attached to the thing's group saves steps when it is time to rotate the certificates on a thing\. And policies are dynamically applied to things when they change group membership, so you aren't required to re\-create a complex set of permissions each time a device changes membership in a group\.

## Create a static thing group<a name="create-thing-group"></a>

Use the CreateThingGroup command to create a static thing group:

```
$ aws iot create-thing-group --thing-group-name LightBulbs
```

The CreateThingGroup command returns a response that contains the static thing group's name, ID, and ARN:

```
{
    "thingGroupName": "LightBulbs", 
    "thingGroupId": "abcdefgh12345678ijklmnop12345678qrstuvwx",
    "thingGroupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/LightBulbs"
}
```

**Note**  
We do not recommend using personally identifiable information in your thing group names\.

Here is an example that specifies a parent of the static thing group when it is created:

```
$ aws iot create-thing-group --thing-group-name RedLights --parent-group-name LightBulbs
```

As before, the CreateThingGroup command returns a response that contains the static thing group's name,, ID, and ARN:

```
{
    "thingGroupName": "RedLights", 
    "thingGroupId": "abcdefgh12345678ijklmnop12345678qrstuvwx",
    "thingGroupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/RedLights",
}
```

**Important**  
Keep in mind the following limits when creating thing group hierarchies:  
A thing group can have only one direct parent\.
The number of direct child groups a thing group can have is [limited](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#thing-group-limits)\.
The maximum depth of a group hierarchy is [limited](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#thing-group-limits)\.
The number of attributes a thing group can have is [limited](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#thing-group-limits)\. \(Attributes are name\-value pairs you can use to store information about a group\.\) The lengths of each attribute name and each value are also [limited](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#thing-group-limits)\.

## Describe a thing group<a name="describe-thing-group"></a>

You can use the DescribeThingGroup command to get information about a thing group:

```
$ aws iot describe-thing-group --thing-group-name RedLights
```

The DescribeThingGroup command returns information about the specified group:

```
{
    "thingGroupName": "RedLights", 
    "thingGroupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/RedLights",
    "thingGroupId": "12345678abcdefgh12345678ijklmnop12345678",
    "version": 1,
    "thingGroupMetadata": {
        "creationDate": 1478299948.882
        "parentGroupName": "Lights",
        "rootToParentThingGroups": [
            {
                "groupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/ShinyObjects",
                "groupName": "ShinyObjects"
            },
            {
                "groupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/LightBulbs",
                "groupName": "LightBulbs"
            }
        ]
    },
    "thingGroupProperties": {
        "attributePayload": {
            "attributes": {
                "brightness": "3400_lumens"
            },
        },
        "thingGroupDescription": "string"
    },
}
```

## Add a thing to a static thing group<a name="add-thing-to-group"></a>

You can use the AddThingToThingGroup command to add a thing to a static thing group:

```
$ aws iot add-thing-to-thing-group --thing-name MyLightBulb --thing-group-name RedLights
```

The AddThingToThingGroup command does not produce any output\.

**Important**  
You can add a thing to a maximum of 10 groups\. But you cannot add a thing to more than one group in the same hierarchy\. \(In other words, you cannot add a thing to two groups which share a common parent\.\)  
If a thing belongs to as many thing groups as possible, and one or more of those groups is a dynamic thing group, you can use the [https://docs.aws.amazon.com/iot/latest/apireference/API_AddThingToThingGroup.html#iot-AddThingToThingGroup-request-overrideDynamicGroups](https://docs.aws.amazon.com/iot/latest/apireference/API_AddThingToThingGroup.html#iot-AddThingToThingGroup-request-overrideDynamicGroups) flag to make static groups take priority over dynamic groups\.

## Remove a thing from a static thing group<a name="remove-thing-from-group"></a>

You can use the RemoveThingFromThingGroup command to remove a thing from a group:

```
$ aws iot remove-thing-from-thing-group --thing-name MyLightBulb --thing-group-name RedLights
```

The RemoveThingFromThingGroup command does not produce any output\.

## List things in a thing group<a name="list-things-in-thing-group"></a>

You can use the ListThingsInThingGroup command to list the things that belong to a group:

```
$ aws iot list-things-in-thing-group --thing-group-name LightBulbs
```

The ListThingsInThingGroup command returns a list of the things in the given group:

```
{
    "things":[
        "TestThingA"
    ]
}
```

With the `--recursive` parameter, you can list things belonging to a group and those in any of its child groups:

```
$ aws iot list-things-in-thing-group --thing-group-name LightBulbs --recursive
```

```
{
    "things":[
        "TestThingA",
        "MyLightBulb"
    ]
}
```

**Note**  
This operation is [eventually consistent](https://web.stanford.edu/class/cs345d-01/rl/eventually-consistent.pdf)\. In other words, changes to the thing group might not be reflected immediately\.

## List thing groups<a name="list-thing-groups"></a>

You can use the ListThingGroups command to list your account's thing groups:

```
$ aws iot list-thing-groups
```

The ListThingGroups command returns a list of the thing groups in your AWS account:

```
{
    "thingGroups": [
        {
            "groupName": "LightBulbs", 
            "groupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/LightBulbs"
        },
        {
            "groupName": "RedLights", 
            "groupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/RedLights"
        },
        {
            "groupName": "RedLEDLights", 
            "groupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/RedLEDLights"
        },
        {
            "groupName": "RedIncandescentLights", 
            "groupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/RedIncandescentLights"
        }
        {
            "groupName": "ReplaceableObjects", 
            "groupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/ReplaceableObjects"
        }
    ]
}
```

Use the optional filters to list those groups that have a given group as parent \(`--parent-group`\) or groups whose name begins with a given prefix \(`--name-prefix-filter`\.\) The `--recursive` parameter allows you to list all children groups, not just direct child groups of a thing group:

```
$ aws iot list-thing-groups --parent-group LightBulbs
```

In this case, the ListThingGroups command returns a list of the direct child groups of the thing group defined in your AWS account:

```
{
    "childGroups":[
        {
            "groupName": "RedLights", 
            "groupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/RedLights"
        }
    ]
}
```

Use the `--recursive` parameter with the ListThingGroups command to list all child groups of a thing group, not just direct children:

```
$ aws iot list-thing-groups --parent-group LightBulbs --recursive
```

The ListThingGroups command returns a list of all child groups of the thing group:

```
{
    "childGroups":[
        {
            "groupName": "RedLights", 
            "groupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/RedLights"
        },
        {
            "groupName": "RedLEDLights", 
            "groupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/RedLEDLights"
        },
        {
            "groupName": "RedIncandescentLights", 
            "groupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/RedIncandescentLights"
        }
    ]
}
```

**Note**  
This operation is [eventually consistent](https://web.stanford.edu/class/cs345d-01/rl/eventually-consistent.pdf)\. In other words, changes to the thing group might not be reflected immediately\.

## List groups for a thing<a name="list-thing-groups-for-thing"></a>

You can use the ListThingGroupsForThing command to list the groups a thing belongs to, including any parent groups:

```
$ aws iot list-thing-groups-for-thing --thing-name MyLightBulb
```

The ListThingGroupsForThing command returns a list of the thing groups this thing belongs to, including any parent groups:

```
{
    "thingGroups":[
        {
            "groupName": "LightBulbs", 
            "groupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/LightBulbs"
        },
        {
            "groupName": "RedLights", 
            "groupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/RedLights"
        },
        {
            "groupName": "ReplaceableObjects", 
            "groupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/ReplaceableObjects"
        }
    ]
}
```

## Update a static thing group<a name="update-thing-group"></a>

You can use the UpdateThingGroup command to update the attributes of a static thing group:

```
$ aws iot update-thing-group --thing-group-name "LightBulbs" --thing-group-properties "thingGroupDescription=\"this is a test group\", attributePayload=\"{\"attributes\"={\"Owner\"=\"150\",\"modelNames\"=\"456\"}}"
```

The UpdateThingGroup command returns a response that contains the group's version number after the update:

```
{
    "version": 4
}
```

**Note**  
The number of attributes that a thing can have is [limited](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#thing-limits)\.  


## Delete a thing group<a name="delete-thing-group"></a>

To delete a thing group, use the DeleteThingGroup command:

```
$ aws iot delete-thing-group --thing-group-name "RedLights"
```

The DeleteThingGroup command does not produce any output\.

**Important**  
If you try to delete a thing group that has child thing groups, you receive an error:   

```
A client error (InvalidRequestException) occurred when calling the DeleteThingGroup 
operation: Cannot delete thing group : RedLights when there are still child groups attached to it.
```
You must delete any child groups first before you delete the group\.

You can delete a group that has child things, but any permissions granted to the things by membership in the group no longer apply\. Before deleting a group that has a policy attached, check carefully that removing those permissions would not stop the things in the group from being able to function properly\. Also, note that commands that show which groups a thing belongs to \(for example, ListGroupsForThing\) might continue to show the group while records in the cloud are being updated\.

## Attach a policy to a static thing group<a name="group-attach-policy"></a>

You can use the AttachPolicy command to attach a policy to a static thing group and so, by extension, to all things in that group and things in any of its child groups:

```
$ aws iot attach-policy \
  --target "arn:aws:iot:us-west-2:123456789012:thinggroup/LightBulbs" \
  --policy-name "myLightBulbPolicy"
```

The AttachPolicy command does not produce any output

**Important**  
You can attach a maximum number of two policies to a group\.

**Note**  
We do not recommend using personally identifiable information in your policy names\.

The `--target` parameter can be a thing group ARN \(as above\), a certificate ARN, or an Amazon Cognito Identity\. For more information about policies, certificates and authentication, see [Authentication](authentication.md)\.

## Detach a policy from a static thing group<a name="group-detach-policy"></a>

You can use the DetachPolicy command to detach a policy from a group and so, by extension, to all things in that group and things in any of its child groups:

```
$ aws iot detach-policy --target "arn:aws:iot:us-west-2:123456789012:thinggroup/LightBulbs" --policy-name "myLightBulbPolicy"
```

The DetachPolicy command does not produce any output\.

## List the policies attached to a static thing group<a name="group-list-policies"></a>

You can use the ListAttachedPolicies command to list the policies attached to a static thing group:

```
$ aws iot list-attached-policies --target "arn:aws:iot:us-west-2:123456789012:thinggroup/RedLights"
```

The `--target` parameter can be a thing group ARN \(as above\), a certificate ARN, or an Amazon Cognito identity\.

Add the optional `--recursive` parameter to include all policies attached to the group's parent groups\.

The ListAttachedPolicies command returns a list of policies:

```
{
    "policies": [
        "MyLightBulbPolicy" 
        ...
    ]
}
```

## List the groups for a policy<a name="group-list-targets-for-policy"></a>

You can use the ListTargetsForPolicy command to list the targets, including any groups, that a policy is attached to:

```
$ aws iot list-targets-for-policy --policy-name "MyLightBulbPolicy"
```

Add the optional `--page-size number` parameter to specify the maximum number of results to be returned for each query, and the `--marker string` parameter on subsequent calls to retrieve the next set of results, if any\.

The ListTargetsForPolicy command returns a list of targets and the token to use to retrieve more results:

```
{
    "nextMarker": "string",
    "targets": [ "string" ... ]
}
```

## Get effective policies for a thing<a name="group-get-effective-policies"></a>

You can use the GetEffectivePolicies command to list the policies in effect for a thing, including the policies attached to any groups the thing belongs to \(whether the group is a direct parent or indirect ancestor\):

```
$ aws iot get-effective-policies \
  --thing-name "MyLightBulb" \
  --principal "arn:aws:iot:us-east-1:123456789012:cert/a0c01f5835079de0a7514643d68ef8414ab739a1e94ee4162977b02b12842847"
```

Use the `--principal` parameter to specify the ARN of the certificate attached to the thing\. If you are using Amazon Cognito identity authentication, use the `--cognito-identity-pool-id` parameter and, optionally, add the `--principal` parameter to specify an Amazon Cognito identity\. If you specify only the `--cognito-identity-pool-id`, the policies associated with that identity pool's role for unauthenticated users are returned\. If you use both, the policies associated with that identity pool's role for authenticated users are returned\.

The `--thing-name` parameter is optional and can be used instead of the `--principal` parameter\. When used, the policies attached to any group the thing belongs to, and the policies attached to any parent groups of these groups \(up to the root group in the hierarchy\) are returned\.

The GetEffectivePolicies command returns a list of policies:

```
{
    "effectivePolicies": [
        {
            "policyArn": "string",
            "policyDocument": "string",
            "policyName": "string"
        }
        ...
    ]
}
```

## Test authorization for MQTT actions<a name="group-test-authorization"></a>

You can use the TestAuthorization command to test whether an MQTT action is allowed for a thing:

```
aws iot test-authorization \
    --principal "arn:aws:iot:us-east-1:123456789012:cert/a0c01f5835079de0a7514643d68ef8414ab739a1e94ee4162977b02b12842847" \
    --auth-infos "{\"actionType\": \"PUBLISH\", \"resources\": [ \"arn:aws:iot:us-east-1:123456789012:topic/my/topic\"]}"
```

Use the `--principal` parameter to specify the ARN of the certificate attached to the thing\. If using Amazon Cognito Identity authentication, specify a Cognito Identity as the `--principal` or use the `--cognito-identity-pool-id` parameter, or both\. \(If you specify only the `--cognito-identity-pool-id` then the policies associated with that identity pool's role for unauthenticated users are considered\. If you use both, the policies associated with that identity pool's role for authenticated users are considered\.

Specify one or more MQTT actions you want to test by listing sets of resources and action types following the `--auth-infos` parameter\. The `actionType` field should contain "PUBLISH", "SUBSCRIBE", "RECEIVE", or "CONNECT"\. The `resources` field should contain a list of resource ARNs\. See [AWS IoT Core policies](iot-policies.md) for more information\.

You can test the effects of adding policies by specifying them with the `--policy-names-to-add` parameter\. Or you can test the effects of removing policies by them with the `--policy-names-to-skip` parameter\.

You can use the optional `--client-id` parameter to further refine your results\.

The TestAuthorization command returns details on actions that were allowed or denied for each set of `--auth-infos` queries you specified:

```
{
    "authResults": [
        {
            "allowed": {
                "policies": [
                    {
                        "policyArn": "string",
                        "policyName": "string"
                    }
                ]
            },
            "authDecision": "string",
            "authInfo": {
                "actionType": "string",
                "resources": [ "string" ]
            },
            "denied": {
                "explicitDeny": {
                    "policies": [
                        {
                            "policyArn": "string",
                            "policyName": "string"
                        }
                    ]
                },
                "implicitDeny": {
                    "policies": [
                        {
                            "policyArn": "string",
                            "policyName": "string"
                        }
                    ]
                }
            },
            "missingContextValues": [ "string" ]
        }
    ]
}
```