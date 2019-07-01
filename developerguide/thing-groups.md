# Thing Groups<a name="thing-groups"></a>

Thing groups allow you to manage several things at once by categorizing them into groups\. Groups can also contain groups — you can build a hierarchy of groups\. You can attach a policy to a parent group and it will be inherited by its child groups, and by all of the things in the group and in its child groups as well\. This makes control of permissions easy for large numbers of things\.

Here are the things you can do with thing groups:
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
+ Configure logging options for things in a group \(see [Configure AWS IoT Logging](cloud-watch-logs.md#configure-logging)\.\) 
+ Create jobs that will be sent to and executed on every thing in a group and its child groups \(see [Jobs](iot-jobs.md)\.\)

Here are some limitations of thing groups:
+ A group can have at most one direct parent\.
+ If a group will be a child of another group, this must be specified at the time it is created\.
+ You can't change a group's parent later\. \(So be sure to plan your group hierarchy and create a parent group before creating any child groups it will contain\.
+ You cannot add a thing to more than than 10 groups\.
+ You cannot add a thing to more than one group in the same hierarchy\. \(In other words, you cannot add a thing to two groups which share a common parent\.\)
+ You cannot rename a group\.
+ Thing group names can't contain international characters, such as û, é and ñ\.

Attaching and detaching policies to groups can enhance the security of your AWS IoT operations in a number of significant ways\. The per device method of attaching a policy to a certificate, which is then attached to a thing, is time consuming and makes it difficult to quickly update or change policies across a fleet of devices\. Having a policy attached to the thing's group saves steps when it is time to rotate the certificates on a thing\. And policies are dynamically applied to things when they change group membership, so you aren't required to re\-create a complex set of permissions each time a device changes membership in a group\.

## Create a Thing Group<a name="create-group"></a>

You can use the CreateThingGroup command to create a thing group:

```
$ aws iot create-thing-group --thing-group-name LightBulbs
```

The CreateThingGroup command returns a response that contains the thing group, its ID, and ARN:

```
{
    "thingGroupName": "LightBulbs", 
    "thingGroupId": "abcdefgh12345678ijklmnop12345678qrstuvwx",
    "thingGroupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/LightBulbs"
}
```

**Note**  
We do not recommend using personally identifiable information in your thing group names\.

Here is an example that specifies a parent of the thing group when it is created:

```
$ aws iot create-thing-group --thing-group-name RedLights --parent-group-name LightBulbs
```

As before, the CreateThingGroup command returns a response that contains the thing group, its ID, and ARN:

```
{
    "thingGroupName": "RedLights", 
    "thingGroupId": "abcdefgh12345678ijklmnop12345678qrstuvwx",
    "thingGroupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/RedLights",
}
```

**Important**  
Keep in mind the following limits when creating group hierarchies:  
A group can have at most one direct parent\.
A group may have no more than 100 direct child groups\.
The maximum depth of a group hierarchy is 7\.
A group can have up to 50 attributes\. \(Attributes are name\-value pairs you can use to store information about a group\.\) Each attribute name can contain up to 128 characters and each value up to 800 characters\.

## Describe a Thing Group<a name="describe-thing-group"></a>

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

## Add a Thing to a Thing Group<a name="add-thing-to-group"></a>

You can use the AddThingToThingGroup command to add a thing to a group:

```
$ aws iot add-thing-to-thing-group --thing-name MyLightBulb --thing-group-name RedLights
```

The AddThingToThingGroup command does not produce any output\.

**Important**  
You can add a thing to a maximum of 10 groups\. But you cannot add a thing to more than one group in the same hierarchy\. \(In other words, you cannot add a thing to two groups which share a common parent\.\)  
If a thing belongs to 10 thing groups, and one or more of those groups is a dynamic thing group, you can use the [overrideDynamicGroups](https://docs.aws.amazon.com/iot/latest/apireference/API_AddThingToThingGroup.html#iot-AddThingToThingGroup-request-overrideDynamicGroups) flag to make static groups take priority over dynamic groups\.

## Remove a Thing from a Thing Group<a name="remove-thing-from-group"></a>

You can use the RemoveThingFromThingGroup command to remove a thing from a group:

```
$ aws iot remove-thing-from-thing-group --thing-name MyLightBulb --thing-group-name RedLights
```

The RemoveThingFromThingGroup command does not produce any output\.

## List Things in a Thing Group<a name="list-things-in-thing-group"></a>

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

With the `--recursive` parameter, you can list things belonging to a group and those in any of its child groups as well:

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

## List Thing Groups<a name="list-thing-groups"></a>

You can use the ListThingGroups command to list groups you have created:

```
$ aws iot list-thing-groups
```

The ListThingGroups command returns a list of the groups defined in your AWS account:

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

## List Groups for a Thing<a name="list-thing-groups-for-thing"></a>

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

## Update a Thing Group<a name="update-group"></a>

You can use the UpdateThingGroup command to update the attributes of a thing group:

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
A group can have up to 50 attributes\.

## Delete a Thing Group<a name="delete-thing-group"></a>

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

## Attach a Policy to a Thing Group<a name="group-attach-policy"></a>

You can use the AttachPolicy command to attach a policy to a thing group and so, by extension, to all things in that group and things in any of its child groups:

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

The `--target` parameter can be a thing group ARN \(as above\), a certificate ARN, or an Amazon Cognito Identity\. For more information about policies, certificates and authentication, see [Security and Identity for AWS IoT ](iot-security-identity.md)\.

## Detach a Policy from a Thing Group<a name="group-detach-policy"></a>

You can use the DetachPolicy command to detach a policy from a group and so, by extension, to all things in that group and things in any of its child groups:

```
$ aws iot detach-policy --target "arn:aws:iot:us-west-2:123456789012:thinggroup/LightBulbs" --policy-name "myLightBulbPolicy"
```

The DetachPolicy command does not produce any output\.

## List the Policies Attached to a Thing Group<a name="group-list-policies"></a>

You can use the ListAttachedPolicies command to list the policies attached to a group:

```
$ aws iot list-attached-policies --target "arn:aws:iot:us-west-2:123456789012:thinggroup/RedLights"
```

The `--target` parameter can be a thing group ARN \(as above\), a certificate ARN, or an Amazon Cognito Identity\.

Add the optional `--recursive` parameter to include all policies attached to the group's parent groups as well\.

The ListAttachedPolicies command returns a list of policies:

```
{
    "policies": [
        "MyLightBulbPolicy" 
        ...
    ]
}
```

## List the Groups for a Policy<a name="group-list-targets-for-policy"></a>

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

## Get Effective Policies for a Thing<a name="group-get-effective-policies"></a>

You can use the GetEffectivePolicies command to list the policies in effect for a thing, including the policies attached to any groups the thing belongs to \(whether the group is a direct parent or indirect ancestor\):

```
$ aws iot get-effective-policies \
  --thing-name "MyLightBulb" \
  --principal "arn:aws:iot:us-east-1:123456789012:cert/a0c01f5835079de0a7514643d68ef8414ab739a1e94ee4162977b02b12842847"
```

Use the `--principal` parameter to specify the ARN of the certificate attached to the thing\. If you are using Amazon Cognito Identity authentication, use the `--cognito-identity-pool-id` parameter and, optionally, add the `--principal` parameter to specify a specific Cognito Identity\. \(If you specify only the `--cognito-identity-pool-id` then the policies associated with that identity pool's role for unauthenticated users are returned\. If you use both, the policies associated with that identity pool's role for authenticated users are returned\.

The `--thing-name` parameter is optional and may be used instead of the `--principal` parameter\. When used, the policies attached to any group the thing belongs to, and the policies attached to any parent groups of these groups \(up to the root group in the hierarchy\) will be returned\.

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

## Test Authorization for MQTT Actions<a name="group-test-authorization"></a>

You can use the TestAuthorization command to test whether an MQTT action is allowed for a thing:

```
aws iot test-authorization \
    --principal "arn:aws:iot:us-east-1:123456789012:cert/a0c01f5835079de0a7514643d68ef8414ab739a1e94ee4162977b02b12842847" \
    --auth-infos "{\"actionType\": \"PUBLISH\", \"resources\": [ \"arn:aws:iot:us-east-1:123456789012:topic/my/topic\"]}"
```

Use the `--principal` parameter to specify the ARN of the certificate attached to the thing\. If using Amazon Cognito Identity authentication, specify a Cognito Identity as the `--principal` or use the `--cognito-identity-pool-id` parameter, or both\. \(If you specify only the `--cognito-identity-pool-id` then the policies associated with that identity pool's role for unauthenticated users are considered\. If you use both, the policies associated with that identity pool's role for authenticated users are considered\.

Specify one or more MQTT actions you want to test by listing sets of resources and action types following the `--auth-infos` parameter\. The `actionType` field should contain "PUBLISH", "SUBSCRIBE", "RECEIVE", or "CONNECT"\. The `resources` field should contain a list of resource ARNs\. See [AWS IoT Policies](iot-policies.md) for more information\.

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