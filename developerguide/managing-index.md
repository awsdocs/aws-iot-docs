# Managing Thing Indexing<a name="managing-index"></a>

`AWS_Things` is the index created for all of your things\. You can control what to index: registry data, shadow data, and device connectivity status data \(driven by device lifecycle events\)\.

## Enabling Thing Indexing<a name="enable-index"></a>

You can create the `AWS_Things` index and control its configuration by using the `--thing-indexing-configuration` parameter in the [UpdateIndexingConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateIndexingConfiguration.html) API\. You can retrieve the current indexing configuration by using the [GetIndexingConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_GetIndexingConfiguration.html) API\. 

The following command shows how to use the get\-indexing\-configuration CLI command to retrieve the current thing indexing configuration\. \(In this example, thing indexing is currently disabled\.\)

```
aws iot get-indexing-configuration
{
    "thingIndexingConfiguration": {
        "thingConnectivityIndexingMode": "OFF"
        "thingIndexingMode": "OFF"
    }
}
```

The following table lists the allowed combinations of `thingIndexingMode` and `thingConnectivityIndexingMode`, and their associated effects\. The required `thingIndexingMode` parameter specifies if the `AWS_Things` index contains just registry data or registry and shadow data\. The optional `thingConnectivityIndexingMode` parameter specifies whether the index also contains connectivity status data \(that is, when devices connected and disconnected\)\.


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

Use the update\-indexing\-configuration CLI command to update the thing indexing configuration\. In the following example, you can search registry data, shadow data, and thing connectivity status data using the `AWS_Things` index \(after it is built, as described in the next section\)\.

```
aws iot update-indexing-configuration --thing-indexing-configuration thingIndexingMode=REGISTRY_AND_SHADOW,thingConnectivityIndexingMode=STATUS
```

## Describing a Thing Index<a name="describe-index"></a>

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

Changing the configuration of your index causes the index to be rebuilt\. During this process, the `indexStatus` is `REBUILDING`\. You can execute queries on data in the things index while it is being rebuilt\. For example, if you change the index configuration from `REGISTRY` to `REGISTRY_AND_SHADOW`, while the index is being rebuilt, you can query registry data, including the latest updates\. However, you can't query the shadow data until the rebuild is complete\. The amount of time it takes to build or rebuild the index depends on the amount of data\.

## Querying a Thing Index<a name="search-index"></a>

Use the search\-index CLI command to query data in the index\.

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
If a device has been disconnected for approximately an hour, the connectivity status `"timestamp"` value might be absent\. For persistent sessions, the value might be absent after a client has been disconnected longer than the configured time\-to\-live \(TTL\) for the persistent session\. The connectivity status data is indexed only for connections where the client ID has a matching thing name\. \(The client ID is the value used to connect a device to AWS IoT Core\.\)

## Restrictions and Limitations<a name="index-limitations"></a>

These are the restrictions and limitations for `AWS_Things`\.

Shadow fields with complex types  
A shadow field is indexed only if the value of the field is a simple type or an array that consists entirely of simple types\. \(Simple type means a string, number, or one of the literals `true` or `false`\)\. If a field's value is itself a JSON object, or an array that contains an object, indexing is not performed on that field\. For example, given the following shadow state, the value of field `"palette"` is not indexed because it's an array whose items are objects\. The value of field `"colors"` is indexed because each value in the array is a string\.   

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
A field in a shadow's metadata section is indexed, but only if the corresponding field in the shadow's `"state"` section is indexed\. \(In the previous example, the `"palette"` field in the shadow's metadata section is not indexed either\.\)

Unregistered shadows  
If you use [CreateThing](https://docs.aws.amazon.com/iot/latest/developerguide/thing-registry.html#create-thing) to create a shadow using a thing name that hasn't been registered in your AWS IoT account, fields in this shadow are not indexed\.

Numeric values  
If any registry or shadow data is recognized by the service as a numeric value, it's indexed as such\. You can form queries involving ranges and comparison operators on numeric values \(for example, `"attribute.foo<5"` or `"shadow.reported.foo:[75 TO 80]"`\)\. To be recognized as numeric, the value of the data must be a valid JSON "number" type literal \(an integer in the range \-2^53\.\.\.2^53\-1 or a double\-precision floating point with optional exponential notation\) or part of an array that contains only such values\.

Null values  
Null values are not indexed\.

## Authorization<a name="query-auth"></a>

You can specify the things index as a resource ARN in an AWS IoT policy action, as follows\.


****  

| Action | Resource | 
| --- | --- | 
|  `iot:SearchIndex`  |  An index ARN \(for example, `arn:aws:iot:<your-aws-region>:index/AWS_Things`\)\.  | 
|  `iot:DescribeIndex`  |  An index ARN \(for example, `arn:aws:iot:<your-aws-region>:index/AWS_Things`\)\.  | 

**Note**  
If you have permissions to query the fleet index, you can access the data of things across the entire fleet\.