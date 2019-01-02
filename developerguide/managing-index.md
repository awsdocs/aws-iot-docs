# Managing Thing Indexing<a name="managing-index"></a>

`AWS_Things` is the index created for all of your things\. You can control what to index \- registry data, shadow data, and device connectivity status data \(driven by device life\-cycle events\)\.

## Enabling Thing Indexing<a name="enable-index"></a>

You can create the `AWS_Things` index and control its configuration by using the `--thing-indexing-configuration` setting in the `update-indexing-configuration` [\(UpdateIndexingConfiguration](http://alpha-docs-aws.amazon.com/iot/latest/apireference/API_UpdateIndexingConfiguration.html)\) API\. You can retrieve the current indexing configuration by using the `get-indexing-configuration` \([GetIndexingConfiguration](http://alpha-docs-aws.amazon.com/iot/latest/apireference/API_GetIndexingConfiguration.html)\) API\. 

The following command shows how to use the `get-indexing-configuration` CLI command to retrieve the current thing indexing configuration \(which, in this example, shows that thing indexing is currently disabled\)\.

```
aws iot get-indexing-configuration
{
    "thingIndexingConfiguration": {
        "thingConnectivityIndexingMode": "OFF"
        "thingIndexingMode": "OFF"
    }
}
```

The following table provides the *allowed* combinations of `thingIndexingMode` and `thingConnectivityIndexingMode`, and their associated effects\. Briefly, the required `thingIndexingMode` parameter specifies if the `AWS_Things` index will contain just registry data or registry and shadow data\. The optional `thingConnectivityIndexingMode` parameter specifies whether the index will also contain connectivity status data \(when devices connected and have disconnected\)\.


****  

| `thingIndexingMode` | `thingConnectivityIndexingMode` | Result | 
| --- | --- | --- | 
| OFF | Not specified\. | No indexing or delete an existing index\. | 
| OFF | OFF | Equivalent to the above\. | 
| REGISTRY | Not specified\. | Create or configure the AWS\_Things index to index registry data only\. | 
| REGISTRY | OFF | Equivalent to the above \(only registry data is indexed\)\. | 
| REGISTRY\_AND\_SHADOW | Not specified\. | Create or configure the AWS\_Things index to index registry data and shadow data\. | 
| REGISTRY\_AND\_SHADOW | OFF | Equivalent to the above \(registry data and shadow data are indexed\)\. | 
| REGISTRY | STATUS | Create or configure the AWS\_Things index to index registry data and thing connectivity status data \(REGISTRY\_AND\_CONNECTIVITY\_STATUS\) | 
| REGISTRY\_AND\_SHADOW | STATUS | Create or configure the AWS\_Things index to index registry data, shadow data, and thing connectivity status data \(REGISTRY\_AND\_SHADOW\_AND\_CONNECTIVITY\_STATUS\) | 

With the above table in mind, you can use the AWS IoT `update-indexing-configuration` CLI command to update the thing indexing configuration\. In the following example, you can search registry data, shadow data, and thing connectivity status data using the `AWS_Things` index \(after it is built, as described in the next section\)\.

```
aws iot update-indexing-configuration --thing-indexing-configuration thingIndexingMode=REGISTRY_AND_SHADOW,thingConnectivityIndexingMode=STATUS
```

The next `describe-index` example shows the result of the prior `update-indexing-configuration` command\.

## Describing a Thing Index<a name="describe-index"></a>

The following command shows you how to use the `describe-index` CLI command to retrieve the current status of the thing index\.

```
aws iot describe-index --index-name "AWS_Things"
{
    "indexName": "AWS_Things", 
    "indexStatus": "BUILDING", 
    "schema": "REGISTRY_AND_SHADOW_AND_CONNECTIVITY_STATUS"
}
```

The first time you enable indexing, AWS IoT builds your index\. You can't query the index if `indexStatus` is in the `BUILDING` state\. The `schema` for the things index indicates which type of data \(`REGISTRY_AND_SHADOW_AND_CONNECTIVITY_STATUS`\) will be indexed\.

Changing the configuration of your index causes the index to be rebuilt\. The `indexStatus` during this process is `REBUILDING`\. You can execute queries on existing data in the things index while a rebuild is in progress\. For example, if you change the index configuration from `REGISTRY` to `REGISTRY_AND_SHADOW`, while the index is being rebuilt, you can query registry data, including the latest updates\. However, you can't query the shadow data until the rebuild is complete\. The amount of time it takes to build or rebuild the index depends on the amount of data\.

## Querying a Thing Index<a name="search-index"></a>

The following command shows how to use the `search-index` CLI command to query data in the index\.

```
aws iot search-index --index-name "AWS_Things" --query-string "thingName:mything*"
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
            "timestamp":1641508937
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
            "timestamp":1540508937
        }        
    }],
    "nextToken":"AQFCuvk7zZ3D9pOYMbFCeHbdZ+h=G"
}
```

In the above JSON response, we see that `"connectivity"` \(as enabled by the `thingConnectivityIndexingMode=STATUS` setting\) provides a Boolean value and a timestamp indicating if the device is connected to AWS IoT Core or not\. Here we see that the device "mything1" disconnected \(`false`\) at POSIX time `1641508937`:

```
"connectivity": { 
    "connected":false,
    "timestamp":1641508937
}
```

For device "mything2", we see that the device connected \(`true`\) at POSIX time `1540508937`:

```
"connectivity": { 
    "connected":true,
    "timestamp":1540508937
}
```

**Important**  
If a device has been disconnected for a few weeks or more, the connectivity status `"timestamp"` value may be absent\. Additionally, the connectivity status data is indexed only for connections where the client ID used has a matching thing name \(the client ID is the value used to connect a device to AWS IoT Core\)\.

## Restrictions and Limitations<a name="what-index"></a>

Note the following restrictions and limitations for the things index\.

Shadow fields with complex types  
A shadow field is indexed only if the value of the field is a simple type or an array consisting entirely of simple types\. \(By "simple type" we mean a string, a number, or one of the literals `true` or `false`\)\. If a field's value is itself a JSON object, or an array containing an object, indexing won't be done on that field\. For example, given the following shadow state, the value of field `"palette"` will NOT be indexed because it's an array whose items are "objects"\. The value of field "colors" WILL be indexed because each value in the array is a string\.   

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

Shadow metadata  
A field in a shadow's metadata section is indexed, but only if the corresponding field in the shadow's `"state"` section is indexed\. \(In the previous example, the "palette" field in the shadow's metadata section won't be indexed either\.\)

Unregistered shadows  
If you create a shadow using a thing name that hasn't been registered in your AWS IoT account \(using [ CreateThing](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/thing-registry.html#create-thing)\), fields in this shadow won't be indexed\.

Numeric values  
If any registry or shadow data that is being indexed is recognized by the service as a numeric value, it's indexed as such\. You can form queries involving ranges and comparison operators on numeric values, for example, `"attribute.foo<5"` or `"shadow.reported.foo:[75 TO 80]"`\. To be recognized as numeric, the value of the data must be a valid JSON "number" type literal \(an integer in the range \-2^53\.\.\.2^53\-1, or a double\-precision floating point with optional exponential notation\) or part of an array containing only such values\.

Null values  
Null values are not indexed\.

## Authorization<a name="query-auth"></a>

You can specify the things index as a resource ARN in an AWS IoT policy action, as follows\.


****  

| Action | Resource | 
| --- | --- | 
|  iot:SearchIndex  |  An index ARN \(for example, arn:aws:iot:*<your\-aws\-region>*:index/AWS\_Things\)\.  | 
|  iot:DescribeIndex  |  An index ARN \(for example, arn:aws:iot:*<your\-aws\-region>*:index/AWS\_Things\)\.  | 

**Note**  
If you have permissions to query the fleet index, you can access the data of things across the entire fleet\.