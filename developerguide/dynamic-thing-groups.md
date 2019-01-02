# Dynamic Thing Groups<a name="dynamic-thing-groups"></a>

Dynamic thing groups update group membership through search queries\. Using dynamic thing groups, you can change the way you interact with things depending on their connectivity, registry, or shadow data\.

Because dynamic thing groups are tied to your fleet index, you must enable the fleet indexing service to use them\. You can preview the things in a dynamic thing group before you create the group with a fleet indexing search query\. For more information, see [Fleet Indexing Service](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/iot-indexing.html) and [Fleet Indexing Service: Query Syntax](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/query-syntax.html)\.

With dynamic thing groups:

+ You can specify a dynamic thing group as a target for a job\. Only things that meet the criteria that define the dynamic thing group perform the job\.

  For example, suppose that you want to update the firmware on your devices, but, to minimize the chance that the update is interrupted, you only want to update firmware on devices with battery life greater than 80%\. You can create a dynamic thing group that only includes devices with a reported battery life above 80%, and you can use that dynamic thing group as the target for your firmware update job\. Only devices that meet your battery life criteria receive the firmware update\. As devices reach the 80% battery life criteria, they are automatically added to the dynamic thing group and receive the firmware update\.

  For more information about specifying thing groups as job targets, see [Using the AWS IoT Jobs APIs](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/jobs-api.html#jobs-CreateJob)\.

+ You can attach a policy to a dynamic thing group\. The policy applies only to those things that meet the criteria that defines the dynamic thing group\.

  For example, suppose that you want to prevent the misuse of some leased devices that you are tracking\. If you have a dynamic thing group that contains all of your leased devices, and you have a policy that restricts permissions to a set of limited topics, you can attach that policy to your dynamic group of leased items\.

Dynamic thing groups differ from static thing groups in the following ways:

+ Thing membership is not explicitly defined\. To create a dynamic thing group, you must define a query string that defines group membership\.

+ Dynamic thing groups cannot be part of a hierarchy\.

+ You use a different set of commands to create, update, and delete dynamic thing groups\. For all other operations, the same commands that you use to interact with static thing groups can be used to interact with dynamic thing groups\.

+ A single account can have no more than 100 dynamic thing groups defined\.

For more information about static thing groups, see \.

## Create a Dynamic Thing Group<a name="create-dynamic-thing-group"></a>

Use the CreateDynamicThingGroup command to create a dynamic thing group:

```
$ aws iot create-dynamic-thing-group --thing-group-name "RedLights" --query-string "attributes.color:red"
```

The CreateDynamicThingGroup command returns a response that contains the index name, query string, query version, thing group name, thing group ID, and thing group ARN:

```
{
    "indexName": "AWS_Things",
    "queryString": "attributes.color:red",
    "queryVersion": "2017-09-30",
    "thingGroupName": "RedLights", 
    "thingGroupId": "abcdefgh12345678ijklmnop12345678qrstuvwx",
    "thingGroupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/RedLights"
}
```

Dynamic thing group creation is not instantaneous\. The dynamic thing group backfill takes time to complete\. When a dynamic thing group is created, the status of the group is set to `BUILDING`\. When the backfill is complete, the status changes to `ACTIVE`\. To check the status of your dynamic thing group, use the [DescribeThingGroup](http://alpha-docs-aws.amazon.com/iot/latest/apireference/API_DescribeThingGroup.html) command\.

## Describe a Dynamic Thing Group<a name="describe-dynamic-thing-group"></a>

Use the DescribeThingGroup command to get information about a dynamic thing group:

```
$ aws iot describe-thing-group --thing-group-name "RedLights"
```

The DescribeThingGroup command returns information about the specified group:

```
{
    "indexName": "AWS_Things",
    "queryString": "attributes.color:red",
    "queryVersion": "2017-09-30",
    "status": "ACTIVE",
    "thingGroupName": "RedLights", 
    "thingGroupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/RedLights",
    "thingGroupId": "12345678abcdefgh12345678ijklmnop12345678",
    "version": 1,
    "thingGroupMetadata": {
        "creationDate": 1478299948.882
    },
    "thingGroupProperties": {
        "attributePayload": {
            "attributes": {
                "color": "red"
            },
        },
        "thingGroupDescription": "This thing group contains red lights."
    },
}
```

Running DescribeThingGroup on a dynamic thing group returns attributes that are specific to dynamic thing groups, such as the queryString and the status\.

The status of a dynamic thing group can take the following values:

`ACTIVE`  
The dynamic thing group is ready for use\.

`BUILDING`  
The dynamic thing group is being created, and thing membership is being processed\.

`REBUILDING`  
The dynamic thing group's membership is being updated, following the adjustment of the group's search query\.

**Note**  
After you create a dynamic thing group, you can use the group, regardless of its status\. Only dynamic thing groups with an `ACTIVE` status include all of the things that match the search query for that dynamic thing group\. Dynamic thing groups with `BUILDING` and `REBUILDING` statuses might not include all of the things that match the search query\. 

## Update a Dynamic Thing Group<a name="update-dynamic-thing-group"></a>

Use the UpdateDynamicThingGroup command to update the attributes of a dynamic thing group, including the group's search query:

```
$ aws iot update-dynamic-thing-group --thing-group-name "RedLights" --thing-group-properties "thingGroupDescription=\"This thing group contains red, Red, and RED lights.\" --query-string "attributes.color:(red OR Red OR RED)"
```

The UpdateDynamicThingGroup command returns a response that contains the group's version number after the update:

```
{
    "version": 2
}
```

Dynamic thing group updates are not instantaneous\. The dynamic thing group backfill takes time to complete\. When a dynamic thing group is updated, the status of the group changes to `REBUILDING` while the group updates its membership\. When the backfill is complete, the status changes to `ACTIVE`\. To check the status of your dynamic thing group, use the [DescribeThingGroup](http://alpha-docs-aws.amazon.com/iot/latest/apireference/API_DescribeThingGroup.html) command\.

## Delete a Dynamic Thing Group<a name="delete-dynamic-thing-group"></a>

Use the DeleteDynamicThingGroup command to delete a dynamic thing group:

```
$ aws iot delete-dynamic-thing-group --thing-group-name "RedLights"
```

The DeleteDynamicThingGroup command does not produce any output\.

Before you delete a group that has a policy attached, be sure that removing those permissions does not stop the things in the group from functioning properly\. Commands that show which groups a thing belongs to \(for example, ListGroupsForThing\) might continue to show the group while records in the cloud are being updated\.

## Limitations and Conflicts<a name="dynamic-thing-group-limitations"></a>

Dynamic thing groups share some of the same limitations as static thing groups:

+ A thing group can have up to 50 attributes\.

+ A thing can belong to up to 10 thing groups total\.

+ Thing groups cannot be renamed\.

+ Thing group names cannot contain international characters, such as û, é, and ñ\.

When using dynamic thing groups, keep the following in mind\.

### Older dynamic thing groups take priority over newer ones<a name="group-priorities"></a>

By default, if a thing belongs to 10 thing groups, you cannot add it to additional groups\. If a conflict in membership arises among dynamic thing groups when you create or update a dynamic thing group, older dynamic thing groups take priority over newer ones\.

### With `overrideDynamicGroups` enabled, static groups take priority over dynamic groups<a name="membership-limit"></a>

By default, if a thing belongs to 10 thing groups, you cannot add the thing to additional groups\. If you are updating thing membership with the [AddThingToThingGroup](http://alpha-docs-aws.amazon.com/iot/latest/apireference/API_AddThingToThingGroup.html) or [UpdateThingGroupsForThing](http://alpha-docs-aws.amazon.com/iot/latest/apireference/API_UpdateThingGroupsForThing.html) commands, you can use the `overrideDynamicGroups` flag to make static thing groups take priority over dynamic thing groups\. With `overrideDynamicGroups` enabled, if a thing belongs to 10 thing groups, and one or more of those groups is dynamic, adding the thing to a static thing group removes it from the newest dynamic thing group\.

For example, suppose that you create a dynamic thing group named DynamicGroup1, and then you create nine more dynamic thing groups, with DynamicGroup10 being the last group that you created\. If Thing1 belongs to all 10 dynamic thing groups, manually adding Thing1 to a static group with `OverrideDynamicGroups` enabled removes the thing from DynamicGroup10\.

### Dynamic thing group membership is eventually consistent<a name="update-conflict"></a>

Only the final state of a thing is evaluated for the registry\. Intermediary states can be skipped if states are updated rapidly\. Avoid associating a rule, job, or policy with a dynamic thing group whose membership depends on an intermediary state\.

### The fleet indexing service must be enabled<a name="indexing-backfill-conflict"></a>

The fleet indexing service must be enabled and the fleet indexing backfill must be complete before you can create and use dynamic thing groups\. Expect a delay after you enable the fleet indexing service\. The backfill can take some time to complete\. The more things that you have registered, the longer the backfill process takes\. After you enable the fleet indexing service for dynamic thing groups, you cannot disable it until you delete all of your dynamic thing groups\.

**Note**  
If you have permissions to query the fleet index, you can access the data of things across the entire fleet\.