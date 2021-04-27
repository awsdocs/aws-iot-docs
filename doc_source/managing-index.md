# Managing thing indexing<a name="managing-index"></a>

`AWS_Things` is the index created for all of your things\. You can control what to index: registry data, shadow data, and device connectivity status data \(driven by device lifecycle events\)\.

## Enabling thing indexing<a name="enable-index"></a>

You use the update\-indexing\-configuration CLI command or the [UpdateIndexingConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateIndexingConfiguration.html) API to create the `AWS_Things` index and control its configuration\. The `--thing-indexing-configuration` \(`thingIndexingConfiguration`\) parameter allows you to control what kind of data \(for example, registry, shadow, and device connectivity data\) is indexed\. 

The `--thing-indexing-configuration` parameter takes a string with the following structure:

```
{
  "thingIndexingMode": "OFF"|"REGISTRY"|"REGISTRY_AND_SHADOW",
  "thingConnectivityIndexingMode": "OFF"|"STATUS",
  "customFields": [
    { name: field-name, type: String | Number | Boolean },
    ...
  ]
}
```

The `thingIndexingMode` attribute controls what kind of data is indexed\. Valid values are:

OFF  
No indexing\.

REGISTRY  
Index registry data\.

REGISTRY\_AND\_SHADOW  
Index registry and thing shadow data\.

The `thingConnectivityIndexingMode` attribute specifies if thing connectivity data is indexed\. Valid values are:

OFF  
Thing connectivity data is not indexed\.

STATUS  
Thing connectivity data is indexed\.

The `customFields` attribute is a list of field and data type pairs\. Aggregation queries can be performed over these fields based on the data type\. The indexing mode you choose \(REGISTRY or REGISTRY\_AND\_SHADOW\) effects what fields can be specified in `customFields`\. For example, if you specify the `REGISTRY` indexing mode, you cannot specify a field from a thing shadow\. Custom fields must be specified in `customFields` to be indexed\.

If there is a type inconsistency between a custom field in your configuration and the value being indexed, the Fleet Indexing service ignores the inconsistent value for aggregation queries\. CloudWatch logs are helpful when troubleshooting aggregation query problems\. For more information, see [Troubleshooting aggregation queries for the fleet indexing service](aggregation-troubleshooting.md)\. 

Managed fields contain data associated with IoT things, thing groups, and device shadows\. The data type of managed fields are defined by AWS IoT\. You specify the values of each managed field when you create an IoT thing\. For example thing names, thing groups, and thing descriptions are all managed fields\. The Fleet Indexing service indexes managed fields based on the indexing mode you specify:
+ Managed fields for the registry

  ```
  "managedFields" : [
    {name:thingId, type:String},
    {name:thingName, type:String},
    {name:registry.version, type:Number},
    {name:registry.thingTypeName, type:String},
    {name:registry.thingGroupNames, type:String},
  ]
  ```
+ Managed fields for thing shadows

  ```
  "managedFields" : [
    {name:shadow.version, type:Number},
    {name:shadow.hasDelta, type:Boolean}
  ]
  ```
+ Managed fields for thing connectivity

  ```
  "managedFields" : [
    {name:connectivity.timestamp, type:Number},
    {name:connectivity.version, type:Number},
    {name:connectivity.connected, type:Boolean}
  ]
  ```
+ Managed fields for thing groups

  ```
  "managedFields" : [
    {name:description, type:String},
    {name:parentGroupNames, type:String},
    {name:thingGroupId, type:String},
    {name:thingGroupName, type:String},
    {name:version, type:Number},
  ]
  ```

Managed fields cannot be changed or appear in `customFields`\.

The following is an example of how to use update\-indexing\-configuration to configure indexing:

aws iot update\-indexing\-configuration \-\-thing\-indexing\-configuration 'thingIndexingMode=REGISTRY\_AND\_SHADOW,customFields=\[\{name=attributes\.version,type=Number\},\{name=attributes\.color, type=String\},\{name=shadow\.desired\.power, type=Boolean\}\}\]

This command enables indexing for registry and shadow data\. Aggregation queries work with the managed fields and the provided `customFields` based on the data type\.

You can use the get\-indexing\-configuration CLI command or the [GetIndexingConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_GetIndexingConfiguration.html) API to retrieve the current indexing configuration\.

The following command shows how to use the get\-indexing\-configuration CLI command to retrieve the current thing indexing configuration where five custom fields \(three registry custom fields and two shadow custom fields\) are defined\.

aws iot get\-indexing\-configuration

```
{
    "thingGroupIndexingConfiguration": {
        "thingGroupIndexingMode": "OFF"
    },
    "thingIndexingConfiguration": {
        "thingConnectivityIndexingMode": "STATUS",
        "customFields": [
            {
                "name": "attributes.customField_NUM",
                "type": "Number"
            },
            {
                "name": "shadow.desired.customField_STR",
                "type": "String"
            },
            {
                "name": "shadow.desired.customField_NUM",
                "type": "Number"
            },
            {
                "name": "attributes.customField_STR",
                "type": "String"
            },
            {
                "name": "attributes.customField_BOOL",
                "type": "Boolean"
            }
        ],
        "thingIndexingMode": "REGISTRY_AND_SHADOW",
        "managedFields": [
            {
                "name": "shadow.hasDelta",
                "type": "Boolean"
            },
            {
                "name": "registry.thingGroupNames",
                "type": "String"
            },
            {
                "name": "connectivity.version",
                "type": "Number"
            },
            {
                "name": "registry.thingTypeName",
                "type": "String"
            },
            {
                "name": "connectivity.connected",
                "type": "Boolean"
            },
            {
                "name": "registry.version",
                "type": "Number"
            },
            {
                "name": "thingId",
                "type": "String"
            },
            {
                "name": "connectivity.timestamp",
                "type": "Number"
            },
            {
                "name": "thingName",
                "type": "String"
            },
            {
                "name": "shadow.version",
                "type": "Number"
            }
        ]
    }
}
```

The following table provides the allowed combinations of `thingIndexingMode` and `thingConnectivityIndexingMode`, and their associated effects\. The required `thingIndexingMode` parameter specifies if the `AWS_Things` index contains just registry data or registry and shadow data\. The optional `thingConnectivityIndexingMode` parameter specifies whether the index also contains connectivity status data \(when devices have last connected and disconnected to AWS IoT\)\.


****  

| `thingIndexingMode` | `thingConnectivityIndexingMode` | Result | 
| --- | --- | --- | 
| OFF | Not specified\. | No indexing or delete an index\. | 
| OFF | OFF | Equivalent to the previous entry\. | 
| REGISTRY | Not specified\. | Create or configure the AWS\_Things index to index registry data only\. | 
| REGISTRY | OFF | Equivalent to the previous entry\. \(Only registry data is indexed\.\) | 
| REGISTRY\_AND\_SHADOW | Not specified\. | Create or configure the AWS\_Things index to index registry data and shadow data\. | 
| REGISTRY\_AND\_SHADOW | OFF | Equivalent to the previous entry\. \(Registry data and shadow data are indexed\.\) | 
| REGISTRY | STATUS | Create or configure the AWS\_Things index to index registry data and thing connectivity status data \(REGISTRY\_AND\_CONNECTIVITY\_STATUS\)\. | 
| REGISTRY\_AND\_SHADOW | STATUS | Create or configure the AWS\_Things index to index registry data, shadow data, and thing connectivity status data \(REGISTRY\_AND\_SHADOW\_AND\_CONNECTIVITY\_STATUS\)\. | 

You can use the AWS IoT update\-indexing\-configuration CLI command to update your indexing configuration\. The following examples show how to use the update\-indexing\-configuration CLI command\.

Short syntax:

aws iot update\-indexing\-configuration \-\-thing\-indexing\-configuration "thingIndexingMode=REGISTRY\_AND\_SHADOW,thingConnectivityIndexingMode=STATUS,customFields=\[\{name=attributes\.version,type=Number\},\{name=attributes\.color,type=String\},\{name=shadow\.desired\.power,type=Boolean\}\]"



JSON syntax:

aws iot update\-indexing\-configuration \-\-cli\-input\-json \\ '\{ "thingIndexingConfiguration": \{ "thingIndexingMode": "REGISTRY\_AND\_SHADOW", "thingConnectivityIndexingMode": "STATUS", "customFields": \[ \{ "name": "shadow\.desired\.power", "type": "Boolean" \}, \{ "name": "attributes\.color", "type": "String" \}, \{ "name": "attributes\.version", "type": "Number" \} \] \} \}'



The output of these commands is:

```
{
    "thingIndexingConfiguration": {
        "thingConnectivityIndexingMode": "STATUS",
        "customFields": [
            {
                "type": "String",
                "name": "attributes.color"
            },
            {
                "type": "Number",
                "name": "attributes.version"
            },
            {
                "type": "Boolean",
                "name": "shadow.desired.power"
            }
        ],
        "thingIndexingMode": "REGISTRY_AND_SHADOW",
        "managedFields": [
            {
                "type": "Boolean",
                "name": "connectivity.connected"
            },
            {
                "type": "String",
                "name": "registry.thingTypeName"
            },
            {
                "type": "String",
                "name": "thingName"
            },
            {
                "type": "Number",
                "name": "shadow.version"
            },
            {
                "type": "String",
                "name": "thingId"
            },
            {
                "type": "Boolean",
                "name": "shadow.hasDelta"
            },
            {
                "type": "Number",
                "name": "connectivity.timestamp"
            },
            {
                "type": "String",
                "name": "registry.thingGroupNames"
            },
            {
                "type": "Number",
                "name": "connectivity.version"
            },
            {
                "type": "Number",
                "name": "registry.version"
            }
        ]
    },
    "thingGroupIndexingConfiguration": {
        "thingGroupIndexingMode": "OFF"
    }
}
```

In the following example, a new custom field is added to the configuration:

aws iot update\-indexing\-configuration \-\-thing\-indexing\-configuration 'thingIndexingMode=REGISTRY\_AND\_SHADOW,customFields=\[\{name=attributes\.version,type=Number\},\{name=attributes\.color,type=String\},\{name=shadow\.desired\.power,type=Boolean\},\{name=shadow\.desired\.intensity,type=Number\}\]'

This command added `shadow.desired.intensity` to the indexing configuration\.

**Note**  
Updating the custom fields indexing configuration overwrites all existing custom fields\. Make sure to specify all custom fields when calling update\-indexing\-configuration\.

After the index is rebuilt you can, use aggregation query on the newly added fields, search registry data, shadow data, and thing connectivity status data\.

When changing the indexing mode, make sure all of your custom fields are valid using the new indexing mode\. For example, if you start off with `REGISTRY_AND_SHADOW` mode with a custom field called `shadow.desired.temperature` you must delete the `shadow.desired.temperature` custom field before changing the indexing mode to `REGISTRY`\. If your indexing configuration contains custom fields that are not indexed by the indexing mode, the update fails\. 

## Describing a thing index<a name="describe-index"></a>

The following command shows you how to use the describe\-index CLI command to retrieve the current status of the thing index\.

```
aws iot describe-index --index-name "AWS_Things"
{
    "indexName": "AWS_Things", 
    "indexStatus": "BUILDING", 
    "schema": "REGISTRY_AND_SHADOW_AND_CONNECTIVITY_STATUS"
}
```

The first time you enable indexing, AWS IoT builds your index\. You can't query the index if `indexStatus` is in the `BUILDING` state\. The `schema` for the things index indicates which type of data \(`REGISTRY_AND_SHADOW_AND_CONNECTIVITY_STATUS`\) is indexed\.

Changing the configuration of your index causes the index to be rebuilt\. During this process, the `indexStatus` is `REBUILDING`\. You can execute queries on data in the things index while it is being rebuilt\. For example, if you change the index configuration from `REGISTRY` to `REGISTRY_AND_SHADOW` while the index is being rebuilt, you can query registry data, including the latest updates\. However, you can't query the shadow data until the rebuild is complete\. The amount of time it takes to build or rebuild the index depends on the amount of data\.

## Querying a thing index<a name="search-index"></a>

Use the search\-index CLI command to query data in the index\.

aws iot search\-index \-\-index\-name "AWS\_Things" \-\-query\-string "thingName:mything\*"

```
{  
    "things":[{  
         "thingName":"mything1",
         "thingGroupNames":[  
            "mygroup1"
         ],
         "thingId":"a4b9f759-b0f2-4857-8a4b-967745ed9f4e",
         "attributes":{  
            "attribute1":"abc"
         },
         "connectivity": { 
            "connected":false,
            "timestamp":1556649874716
         }         
    },
    {  
        "thingName":"mything2",
        "thingTypeName":"MyThingType",
        "thingGroupNames":[  
            "mygroup1",
            "mygroup2"
        ],
        "thingId":"01014ef9-e97e-44c6-985a-d0b06924f2af",
        "attributes":{  
            "model":"1.2",
            "country":"usa"
        },
        "shadow":{  
            "desired":{  
                "location":"new york",
                "myvalues":[3, 4, 5]
            },
            "reported":{  
                "location":"new york",
                "myvalues":[1, 2, 3],
                "stats":{  
                    "battery":78
                }
            },
            "metadata":{  
                 "desired":{  
                       "location":{  
                            "timestamp":123456789
                        },
                       "myvalues":{  
                             "timestamp":123456789
                       }
                  },
                  "reported":{  
                        "location":{  
                             "timestamp":34535454
                         },
                        "myvalues":{  
                             "timestamp":34535454
                        },
                        "stats":{  
                             "battery":{  
                                   "timestamp":34535454
                             }
                        }
                 }
            },
            "version":10,
            "timestamp":34535454
        },
        "connectivity": { 
            "connected":true,
            "timestamp":1556649855046
        }        
    }],
    "nextToken":"AQFCuvk7zZ3D9pOYMbFCeHbdZ+h=G"
}
```

In the JSON response, `"connectivity"` \(as enabled by the `thingConnectivityIndexingMode=STATUS` setting\) provides a Boolean value and a timestamp that indicates if the device is connected to AWS IoT Core\. The device `"mything1"` disconnected \(`false`\) at POSIX time `1556649874716`:

```
"connectivity": { 
    "connected":false,
    "timestamp":1556649874716
}
```

The device `"mything2"` connected \(`true`\) at POSIX time `1556649855046`:

```
"connectivity": { 
    "connected":true,
    "timestamp":1556649855046
}
```

Timestamps are given in milliseconds since epoch, so `1556649855046` represents 6:44:15\.046 PM on Tuesday, April 30, 2019 \(GMT\)\.

**Important**  
If a device has been disconnected for approximately an hour, the `"timestamp"` value of the connectivity status might be missing\.

## Restrictions and limitations<a name="index-limitations"></a>

These are the restrictions and limitations for `AWS_Things`\.

Shadow fields with complex types  
A shadow field is indexed only if the value of the field is a simple type, a JSON object that does not contain an array, or an array that consists entirely of simple types\. Simple type means a string, number, or one of the literals `true` or `false`\. For example, given the following shadow state, the value of field `"palette"` is not indexed because it's an array that contains items of complex types\. The value of field `"colors"` is indexed because each value in the array is a string\.   

```
{
    "state": {
        "reported": {
            "switched": "ON",
            "colors": [ "RED", "GREEN", "BLUE" ],
            "palette": [
                {
                    "name": "RED", 
                    "intensity": 124
                },
                {
                    "name": "GREEN", 
                    "intensity": 68
                },
                {
                    "name": "BLUE", 
                    "intensity": 201
                }
            ]
        }
    }
}
```

Nested shadow field names  
The names of nested shadow fields are stored as a period \(\.\) delimited string\. For example, given a shadow document:  

```
{
  "state": {
    "desired": {
      "one": {
        "two": {
          "three": "v2"
        }
      }
    }    
  }
}
```
the name of field `three` is stored as `desired.one.two.three`\. If you also have a shadow document like this:  

```
{
  "state": {
    "desired": {
      "one.two.three": "v2"
    }    
  }
}
```
both match a query for `shadow.desired.one.two.three:v2`\. As a best practice, do not use periods in shadow field names\.

Shadow metadata  
A field in a shadow's metadata section is indexed, but only if the corresponding field in the shadow's `"state"` section is indexed\. \(In the previous example, the `"palette"` field in the shadow's metadata section is not indexed either\.\)

Unregistered shadows  
If you use [UpdateThingShadow](https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_UpdateThingShadow.html) to create a shadow using a thing name that hasn't been registered in your AWS IoT account, fields in this shadow are not indexed\.

Numeric values  
If any registry or shadow data is recognized by the service as a numeric value, it's indexed as such\. You can form queries involving ranges and comparison operators on numeric values \(for example, `"attribute.foo<5"` or `"shadow.reported.foo:[75 TO 80]"`\)\. To be recognized as numeric, the value of the data must be a valid JSON number type literal \(an integer in the range \-2^53\.\.\.2^53\-1 or a double\-precision floating point with optional exponential notation\) or part of an array that contains only such values\.

Null values  
Null values are not indexed\.

Maximum number of custom fields for aggregation queries  
5

Maximum number of requested percentiles for aggregation queries\.  
100

## Authorization<a name="query-auth"></a>

You can specify the things index as a resource ARN in an AWS IoT policy action, as follows\.


****  

| Action | Resource | 
| --- | --- | 
|  `iot:SearchIndex`  |  An index ARN \(for example, `arn:aws:iot:your-aws-regionyour-aws-account:index/AWS_Things`\)\.  | 
|  `iot:DescribeIndex`  |  An index ARN \(for example, `arn:aws:iot:your-aws-region:index/AWS_Things`\)\.  | 

**Note**  
If you have permissions to query the fleet index, you can access the data of things across the entire fleet\.