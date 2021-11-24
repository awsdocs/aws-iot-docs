# Registry events<a name="registry-events"></a>

The registry can publish event messages when things, thing types, and thing groups are created, updated, or deleted\. These events, however, are not available by default\. For information about how to turn on these events, see [Enable events for AWS IoT](iot-events.md#iot-events-enable)\.

**Topics**
+ [Thing events](#registry-events-thing)
+ [Thing type events](#registry-events-thingtype)
+ [Thing group events](#registry-events-thinggroup)

## Thing events<a name="registry-events-thing"></a>

**Thing Created/Updated/Deleted**

The registry publishes the following event messages when things are created, updated, or deleted:
+ `$aws/events/thing/thingName/created`
+ `$aws/events/thing/thingName/updated`
+ `$aws/events/thing/thingName/deleted`

The messages contain the following example payload:

```
{
    "eventType" : "THING_EVENT",
    "eventId" : "f5ae9b94-8b8e-4d8e-8c8f-b3266dd89853",
    "timestamp" : 1234567890123,
    "operation" : "CREATED|UPDATED|DELETED",
    "accountId" : "123456789012",
    "thingId" : "b604f69c-aa9a-4d4a-829e-c480e958a0b5",
    "thingName" : "MyThing",
    "versionNumber" : 1,
    "thingTypeName" : null,
    "attributes": {
                "attribute3": "value3",
                "attribute1": "value1",
                "attribute2": "value2"
    }
}
```

The payloads contain the following attributes:

eventType  
Set to "THING\_EVENT"\.

eventId  
A unique event ID \(string\)\.

timestamp  
The UNIX timestamp of when the event occurred\.

operation  
The operation that triggered the event\. Valid values are:  
+ CREATED
+ UPDATED
+ DELETED

accountId  
Your AWS account ID\.

thingId  
The ID of the thing being created, updated, or deleted\.

thingName  
The name of the thing being created, updated, or deleted\.

versionNumber  
The version of the thing being created, updated, or deleted\. This value is set to 1 when a thing is created\. It is incremented by 1 each time the thing is updated\.

thingTypeName  
The thing type associated with the thing, if one exists\. Otherwise, `null`\.

attributes  
A collection of name\-value pairs associated with the thing\.

## Thing type events<a name="registry-events-thingtype"></a>

**Topics**
+ [Thing Type Created/Deprecated/Undeprecated/Deleted](#registry-events-thingtype-crud)
+ [Thing Type Associated or Disassociated with a Thing](#registry-events-thingtype-assoc)

### Thing Type Created/Deprecated/Undeprecated/Deleted<a name="registry-events-thingtype-crud"></a>

The registry publishes the following event messages when thing types are created, deprecated, undeprecated, or deleted:
+ `$aws/events/thingType/thingTypeName/created`
+ `$aws/events/thingType/thingTypeName/updated`
+ `$aws/events/thingType/thingTypeName/deleted`

The message contains the following example payload:

```
{
    "eventType" : "THING_TYPE_EVENT",
    "eventId" : "8827376c-4b05-49a3-9b3b-733729df7ed5",
    "timestamp" : 1234567890123,
    "operation" : "CREATED|UPDATED|DELETED",
    "accountId" : "123456789012",
    "thingTypeId" : "c530ae83-32aa-4592-94d3-da29879d1aac",
    "thingTypeName" : "MyThingType",
    "isDeprecated" : false|true,
    "deprecationDate" : null,
    "searchableAttributes" : [ "attribute1", "attribute2", "attribute3" ],
    "description" : "My thing type"
}
```

The payloads contain the following attributes:

eventType  
Set to "THING\_TYPE\_EVENT"\.

eventId  
A unique event ID \(string\)\.

timestamp  
The UNIX timestamp of when the event occurred\.

operation  
The operation that triggered the event\. Valid values are:  
+ CREATED
+ UPDATED
+ DELETED

accountId  
Your AWS account ID\.

thingTypeId  
The ID of the thing type being created, deprecated, or deleted\.

thingTypeName  
The name of the thing type being created, deprecated, or deleted\.

isDeprecated  
`true` if the thing type is deprecated\. Otherwise, `false`\.

deprecationDate  
The UNIX timestamp for when the thing type was deprecated\.

searchableAttributes  
A collection of name\-value pairs associated with the thing type that can be used for searching\.

description  
A description of the thing type\.

### Thing Type Associated or Disassociated with a Thing<a name="registry-events-thingtype-assoc"></a>

The registry publishes the following event messages when a thing type is associated or disassociated with a thing\.
+ `$aws/events/thingTypeAssociation/thing/thingName/typeName`

The messages contain the following example payload:

```
{
    "eventId" : "87f8e095-531c-47b3-aab5-5171364d138d",
    "eventType" : "THING_TYPE_ASSOCIATION_EVENT",
    "operation" : "CREATED|DELETED",
    "thingId" : "b604f69c-aa9a-4d4a-829e-c480e958a0b5",
    "thingName": "myThing",
    "thingTypeName" : "MyThingType",
    "timestamp" : 1234567890123,
}
```

The payloads contain the following attributes:

eventId  
A unique event ID \(string\)\.

eventType  
Set to "THING\_TYPE\_ASSOCIATION\_EVENT"\.

operation  
The operation that triggered the event\. Valid values are:  
+ CREATED
+ DELETED

thingId  
The ID of the thing whose type association was changed\.

thingName  
The name of the thing whose type association was changed\.

thingTypeName  
The thing type associated with, or no longer associated with, the thing\.

timestamp  
The UNIX timestamp of when the event occurred\.

## Thing group events<a name="registry-events-thinggroup"></a>

**Topics**
+ [Thing Group Created/Updated/Deleted](#registry-events-thinggroup-crud)
+ [Thing Added to or Removed from a Thing Group](#registry-events-thinggroup-addremove)
+ [Thing Group Added to or Deleted from a Thing Group](#registry-events-thinggroup-adddelete)

### Thing Group Created/Updated/Deleted<a name="registry-events-thinggroup-crud"></a>

The registry publishes the following event messages when a thing group is created, updated, or deleted\.
+ `$aws/events/thingGroup/groupName/created`
+ `$aws/events/thingGroup/groupName/updated`
+ `$aws/events/thingGroup/groupName/deleted`

The following is an example of an `updated` payload\. Payloads for `created` and `deleted` messages are similar\.

```
{
  "eventType": "THING_GROUP_EVENT",
  "eventId": "8b9ea8626aeaa1e42100f3f32b975899",
  "timestamp": 1603995417409,
  "operation": "UPDATED",
  "accountId": "571EXAMPLE833",
  "thingGroupId": "8757eec8-bb37-4cca-a6fa-403b003d139f",
  "thingGroupName": "Tg_level5",
  "versionNumber": 3,
  "parentGroupName": "Tg_level4",
  "parentGroupId": "5fce366a-7875-4c0e-870b-79d8d1dce119",
  "description": "New description for Tg_level5",
  "rootToParentThingGroups": [
    {
      "groupArn": "arn:aws:iot:us-west-2:571EXAMPLE833:thinggroup/TgTopLevel",
      "groupId": "36aa0482-f80d-4e13-9bff-1c0a75c055f6"
    },
    {
      "groupArn": "arn:aws:iot:us-west-2:571EXAMPLE833:thinggroup/Tg_level1",
      "groupId": "bc1643e1-5a85-4eac-b45a-92509cbe2a77"
    },
    {
      "groupArn": "arn:aws:iot:us-west-2:571EXAMPLE833:thinggroup/Tg_level2",
      "groupId": "0476f3d2-9beb-48bb-ae2c-ea8bd6458158"
    },
    {
      "groupArn": "arn:aws:iot:us-west-2:571EXAMPLE833:thinggroup/Tg_level3",
      "groupId": "1d9d4ffe-a6b0-48d6-9de6-2e54d1eae78f"
    },
    {
      "groupArn": "arn:aws:iot:us-west-2:571EXAMPLE833:thinggroup/Tg_level4",
      "groupId": "5fce366a-7875-4c0e-870b-79d8d1dce119"
    }
  ],
  "attributes": {
    "attribute1": "value1",
    "attribute3": "value3",
    "attribute2": "value2"
  },
  "dynamicGroupMappingId": null
}
```

The payloads contain the following attributes:

eventType  
Set to "THING\_GROUP\_EVENT"\.

eventId  
A unique event ID \(string\)\.

timestamp  
The UNIX timestamp of when the event occurred\.

operation  
The operation that triggered the event\. Valid values are:  
+ CREATED
+ UPDATED
+ DELETED

accountId  
Your AWS account ID\.

thingGroupId  
The ID of the thing group being created, updated, or deleted\.

thingGroupName  
The name of the thing group being created, updated, or deleted\.

versionNumber  
The version of the thing group\. This value is set to 1 when a thing group is created\. It is incremented by 1 each time the thing group is updated\.

parentGroupName  
The name of the parent thing group, if one exists\.

parentGroupId  
The ID of the parent thing group, if one exists\.

description  
A description of the thing group\.

rootToParentThingGroups  
An array of information about the parent thing group\. There is one element for each parent thing group, starting from the root thing group and continuing to the thing group's parent\. Each entry contains the thing group's `groupArn` and `groupId`\.

attributes  
A collection of name\-value pairs associated with the thing group\.

### Thing Added to or Removed from a Thing Group<a name="registry-events-thinggroup-addremove"></a>

The registry publishes the following event messages when a thing is added to or removed from a thing group\.
+ `$aws/events/thingGroupMembership/thingGroup/thingGroupName/thing/thingName/added`
+ `$aws/events/thingGroupMembership/thingGroup/thingGroupName/thing/thingName/removed`

The messages contain the following example payload:

```
{
    "eventType" : "THING_GROUP_MEMBERSHIP_EVENT",
    "eventId" : "d684bd5f-6f6e-48e1-950c-766ac7f02fd1",
    "timestamp" : 1234567890123,
    "operation" : "ADDED|REMOVED",
    "accountId" : "123456789012",
    "groupArn" : "arn:aws:iot:ap-northeast-2:123456789012:thinggroup/MyChildThingGroup",
    "groupId" : "06838589-373f-4312-b1f2-53f2192291c4",
    "thingArn" : "arn:aws:iot:ap-northeast-2:123456789012:thing/MyThing",
    "thingId" : "b604f69c-aa9a-4d4a-829e-c480e958a0b5",
    "membershipId" : "8505ebf8-4d32-4286-80e9-c23a4a16bbd8"
}
```

The payloads contain the following attributes:

eventType  
Set to "THING\_GROUP\_MEMBERSHIP\_EVENT"\.

eventId  
The event ID\.

timestamp  
The UNIX timestamp for when the event occurred\.

operation  
`ADDED` when a thing is added to a thing group\. `REMOVED` when a thing is removed from a thing group\.

accountId  
Your AWS account ID\.

groupArn  
The ARN of the thing group\.

groupId  
The ID of the group\.

thingArn  
The ARN of the thing that was added or removed from the thing group\.

thingId  
The ID of the thing that was added or removed from the thing group\.

membershipId  
An ID that represents the relationship between the thing and the thing group\. This value is generated when you add a thing to a thing group\.

### Thing Group Added to or Deleted from a Thing Group<a name="registry-events-thinggroup-adddelete"></a>

The registry publishes the following event messages when a thing group is added to or removed from another thing group\.
+ `$aws/events/thingGroupHierarchy/thingGroup/parentThingGroupName/childThingGroup/childThingGroupName/added`
+ `$aws/events/thingGroupHierarchy/thingGroup/parentThingGroupName/childThingGroup/childThingGroupName/removed`

The message contains the following example payload:

```
{
    "eventType" : "THING_GROUP_HIERARCHY_EVENT",
    "eventId" : "264192c7-b573-46ef-ab7b-489fcd47da41",
    "timestamp" : 1234567890123,
    "operation" : "ADDED|REMOVED",
    "accountId" : "123456789012",
    "thingGroupId" : "8f82a106-6b1d-4331-8984-a84db5f6f8cb",
    "thingGroupName" : "MyRootThingGroup",
    "childGroupId" : "06838589-373f-4312-b1f2-53f2192291c4",
    "childGroupName" : "MyChildThingGroup"
}
```

The payloads contain the following attributes:

eventType  
Set to "THING\_GROUP\_HIERARCHY\_EVENT"\.

eventId  
The event ID\.

timestamp  
The UNIX timestamp for when the event occurred\.

operation  
`ADDED` when a thing is added to a thing group\. `REMOVED` when a thing is removed from a thing group\.

accountId  
Your AWS account ID\.

thingGroupId  
The ID of the parent thing group\.

thingGroupName  
The name of the parent thing group\.

childGroupId  
The ID of the child thing group\.

childGroupName  
The name of the child thing group\.