# Managing thing group indexing<a name="thinggroup-index"></a>

`AWS_ThingGroups` is the index that contains all of your thing groups\. You can use this index to search for groups based on group name, description, attributes, and all parent group names\.

## Enabling thing group indexing<a name="enable-group-index"></a>

You can use the `thing-group-indexing-configuration` setting in the [UpdateIndexingConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateIndexingConfiguration.html) API to create the `AWS_ThingGroups` index and control its configuration\. You can use the [GetIndexingConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_GetIndexingConfiguration.html) API to retrieve the current indexing configuration\. 

Use the get\-indexing\-configuration CLI command to retrieve the current thing and thing group indexing configurations\.

aws iot get\-indexing\-configuration

```
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

## Describing group indexes<a name="describe-group-index"></a>

Use the describe\-index CLI command to retrieve the current status of the `AWS_ThingGroups` index\.

```
aws iot describe-index --index-name "AWS_ThingGroups"
{
   "indexStatus": "ACTIVE", 
   "indexName": "AWS_ThingGroups", 
   "schema": "THING_GROUPS"
}
```

 AWS IoT builds your index the first time you enable indexing\. You can't query the index if the `indexStatus` is `BUILDING`\.

## Querying a thing group index<a name="search-group-index"></a>

Use the search\-index CLI command to query data in the index:

```
aws iot search-index --index-name "AWS_ThingGroups" --query-string "thingGroupName:mythinggroup*"
```

## Authorization<a name="query-thinggroup-auth"></a>

You can specify the thing groups index as a resource ARN in an AWS IoT policy action, as follows\.


****  

| Action | Resource | 
| --- | --- | 
|  `iot:SearchIndex`  |  An index ARN \(for example, `arn:aws:iot:your-aws-region:index/AWS_ThingGroups`\)\.  | 
|  `iot:DescribeIndex`  |  An index ARN \(for example, `arn:aws:iot:your-aws-region:index/AWS_ThingGroups`\)\.  | 