# Managing Thing Group Indexing<a name="thinggroup-index"></a>

`AWS_ThingGroups` is the index that contains all of your thing groups\. You can use this index to search for groups based on group name, description, attributes, and all parent group names\.

## Enabling Thing Group Indexing<a name="enable-group-index"></a>

You can create the `AWS_ThingGroups` index and control its configuration by using the `thing-group-indexing-configuration` setting in the [UpdateIndexingConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateIndexingConfiguration.html) API\. You can retrieve the current indexing configuration by using the [GetIndexingConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_GetIndexingConfiguration.html) API\. 

Use the get\-indexing\-configuration CLI command to retrieve the current thing and thing group indexing configurations\.

```
aws iot get-indexing-configuration
{
   "thingGroupIndexingConfiguration": {
        "thingGroupIndexingMode": "ON"
    }
}
```

Use the update\-indexing\-configuration CLI command to update the thing group indexing configurations\.

```
aws iot update-indexing-configuration --thing-group-indexing-configuration thingGroupIndexingMode=ON
```

**Note**  
You can also update configurations for both thing and thing group indexing in a single command, as follows\.  

```
aws iot update-indexing-configuration --thing-indexing-configuration thingIndexingMode=REGISTRY --thing-group-indexing-configuration thingGroupIndexingMode=ON
```

The following are valid values for `thingGroupIndexingMode`\.

OFF  
No indexing/delete index\.

ON  
Create or configure the `AWS_ThingGroups` index\.

## Describing Group Indexes<a name="describe-group-index"></a>

Use the describe\-index CLI command to retrieve the current status of the `AWS_ThingGroups` index\.

```
aws iot describe-index --index-name "AWS_ThingGroups"
{
   "indexStatus": "ACTIVE", 
   "indexName": "AWS_ThingGroups", 
   "schema": "THING_GROUPS"
}
```

 The first time you enable indexing, AWS IoT builds your index\. You can't query the index if the `indexStatus` is `BUILDING`\.

## Querying a Thing Group Index<a name="search-group-index"></a>

Use the search\-index CLI command to query data in the index:

```
aws iot search-index --index-name "AWS_ThingGroups" --query-string "thingGroupName:mythinggroup*"
```

## Authorization<a name="query-thinggroup-auth"></a>

You can specify the thing groups index as a resource ARN in an AWS IoT policy action, as follows\.


****  

| Action | Resource | 
| --- | --- | 
|  `iot:SearchIndex`  |  An index ARN \(for example, arn:aws:iot:*<your\-aws\-region>*:index/AWS\_ThingGroups\)\.  | 
|  `iot:DescribeIndex`  |  An index ARN \(for example, arn:aws:iot:*<your\-aws\-region>*:index/AWS\_ThingGroups\)\.  | 