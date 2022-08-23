# Manage thing indexing<a name="managing-index"></a>

The index created for all of your things is `AWS_Things`\. You can control what to index from the following data sources: [AWS IoT registry](thing-registry.md) data, [AWS IoT Device Shadow ](iot-device-shadows.md)data, [AWS IoT connectivity](life-cycle-events.md) data, and [AWS IoT Device Defender](device-defender.md) violations data\. 

## Enabling thing indexing<a name="enable-index"></a>

You use the [update\-indexing\-configuration](https://docs.aws.amazon.com/cli/latest/reference/iot/update-indexing-configuration.html) CLI command or the [UpdateIndexingConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateIndexingConfiguration.html) API operation to create the `AWS_Things` index and control its configuration\. By using the `--thing-indexing-configuration` \(`thingIndexingConfiguration`\) parameter, you control what kind of data \(for example, registry, shadow, device connectivity data, and Device Defender violations data\) is indexed\. 

The `--thing-indexing-configuration` parameter takes a string with the following structure:

```
{
  "thingIndexingMode": "OFF"|"REGISTRY"|"REGISTRY_AND_SHADOW",
  "thingConnectivityIndexingMode": "OFF"|"STATUS",
  "deviceDefenderIndexingMode": "OFF"|"VIOLATIONS",
  "namedShadowIndexingMode": "OFF"|"ON",
  "managedFields": [
    {
      "name": "string",
      "type": "Number"|"String"|"Boolean"
    }, 
    ...
  ], 
  "customFields": [
    { 
      "name": "string",
      "type": "Number"|"String"|"Boolean" 
    },
    ...
  ],
  "filter": {
     "namedShadowNames": [ "string" ]
  }
}
```

### Thing indexing mode<a name="index-mode"></a>

The `thingIndexingMode` attribute controls what kind of data is indexed\. 

**Important**  
To enable thing indexing, the `thingIndexingMode` attribute can't be set to be OFF\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/managing-index.html)

The `thingConnectivityIndexingMode` attribute specifies if thing connectivity data is indexed\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/managing-index.html)

The `deviceDefenderIndexingMode` attribute specifies if Device Defender violations data is indexed\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/managing-index.html)

The `namedShadowIndexingMode` attribute specifies if named shadow data is indexed\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/managing-index.html)

**Note**  
To select named shadows to add to your fleet indexing configuration, set `namedShadowIndexingMode` to be `ON` and specify your named shadow names in [https://docs.aws.amazon.com/iot/latest/apireference/API_IndexingFilter.html](https://docs.aws.amazon.com/iot/latest/apireference/API_IndexingFilter.html)\.

### Managed fields and custom fields<a name="managed-custom-field"></a>

**Managed fields**

Managed fields contain data associated with things, thing groups, device shadows, device connectivity, and Device Defender violations\. AWS IoT defines the data type in managed fields\. You specify the values of each managed field when you create an IoT thing\. For example, thing names, thing groups, and thing descriptions are all managed fields\. Fleet indexing indexes managed fields based on the indexing mode you specify\. Managed fields can't be changed or appear in `customFields`\. 

**Custom fields**

You can aggregate attributes, Device Shadow data, and Device Defender violations data by creating custom fields to index them\. The `customFields` attribute is a list of field name and data type pairs\. You can perform aggregation queries based on data type\. The indexing mode you choose affects what fields can be specified in `customFields`\. For example, if you specify the `REGISTRY` indexing mode, you can't specify a custom field from a thing shadow\. You can use the [update\-indexing\-configuration](https://docs.aws.amazon.com/cli/latest/reference/iot/update-indexing-configuration.html) CLI command to create or update the custom fields \(see an example command in [Updating indexing configuration examples](#update-index-examples)\)\. For more information, see [Custom fields](managing-fleet-index.md#custom-field)\.

### Updating indexing configuration examples<a name="update-index-examples"></a>

You can use the AWS IoT update\-indexing\-configuration CLI command to update your indexing configuration\. The following examples show how to use update\-indexing\-configuration\.

Short syntax:

```
aws iot update-indexing-configuration --thing-indexing-configuration \
'thingIndexingMode=REGISTRY_AND_SHADOW,deviceDefenderIndexingMode=VIOLATIONS,namedShadowIndexingMode=ON,filter={namedShadowNames=[thing1shadow]},thingConnectivityIndexingMode=STATUS,customFields=[{name=attributes.version,type=Number},{name= shadow.name.thing1shadow.desired.DefaultDesired, type=String},{name=shadow.desired.power, type=Boolean}, {name=deviceDefender.securityProfile1.NUMBER_VALUE_BEHAVIOR.lastViolationValue.number, type=Number}]'
```

JSON syntax:

```
aws iot update-indexing-configuration --cli-input-json \ '{
          "thingIndexingConfiguration": { "thingIndexingMode": "REGISTRY_AND_SHADOW",
          "thingConnectivityIndexingMode": "STATUS", 
          "deviceDefenderIndexingMode": "VIOLATIONS",
          "namedShadowIndexingMode": "ON",
          "filter": { "namedShadowNames": ["thing1shadow"]},
          "customFields": [ { "name": "shadow.desired.power", "type": "Boolean" }, 
          {"name": "attributes.version", "type": "Number"}, 
          {"name": "shadow.name.thing1shadow.desired.DefaultDesired", "type": "String"}, 
          {"name": "deviceDefender.securityProfile1.NUMBER_VALUE_BEHAVIOR.lastViolationValue.number", "type": Number} ] } }'
```

This command doesn't produce any output\. 

To check the thing index status, run the `describe-index` CLI command: 

```
aws iot describe-index --index-name "AWS_Things"
```

The output of the `describe-index` command looks like the following:

```
{
    "indexName": "AWS_Things",
    "indexStatus": "ACTIVE",
    "schema": "MULTI_INDEXING_MODE"
}
```

**Note**  
It can take a moment for fleet indexing to update the fleet index\. We recommend waiting until the `indexStatus` shows ACTIVE before using it\. You can have different values in the schema field depending what data sources you've configured\. For more information, see [Describing a thing index](#describe-index)\.

To get your thing indexing configuration details, run the `get-indexing-configuration` CLI command: 

```
aws iot get-indexing-configuration
```

The output of the `get-indexing-configuration` command looks like the following:

```
{
    "thingIndexingConfiguration": {
        "thingIndexingMode": "REGISTRY_AND_SHADOW",
        "thingConnectivityIndexingMode": "STATUS",
        "deviceDefenderIndexingMode": "VIOLATIONS",
        "namedShadowIndexingMode": "ON",
        "managedFields": [
            {
                "name": "connectivity.disconnectReason",
                "type": "String"
            },
            {
                "name": "registry.version",
                "type": "Number"
            },
            {
                "name": "thingName",
                "type": "String"
            },
            {
                "name": "deviceDefender.violationCount",
                "type": "Number"
            },
            {
                "name": "shadow.hasDelta",
                "type": "Boolean"
            },
            {
                "name": "shadow.name.*.version",
                "type": "Number"
            },
            {
                "name": "shadow.version",
                "type": "Number"
            },
            {
                "name": "connectivity.version",
                "type": "Number"
            },
            {
                "name": "connectivity.timestamp",
                "type": "Number"
            },
            {
                "name": "shadow.name.*.hasDelta",
                "type": "Boolean"
            },
            {
                "name": "registry.thingTypeName",
                "type": "String"
            },
            {
                "name": "thingId",
                "type": "String"
            },
            {
                "name": "connectivity.connected",
                "type": "Boolean"
            },
            {
                "name": "registry.thingGroupNames",
                "type": "String"
            }
        ],
        "customFields": [
            {
                "name": "shadow.name.thing1shadow.desired.DefaultDesired",
                "type": "String"
            },

            {
                "name": "deviceDefender.securityProfile1.NUMBER_VALUE_BEHAVIOR.lastViolationValue.number",
                "type": "Number"
            },
            {
                "name": "shadow.desired.power",
                "type": "Boolean"
            },
            {
                "name": "attributes.version",
                "type": "Number"
            }
        ], 
        "filter": {
              "namedShadowNames": [
                  "thing1shadow"
              ]
          }
      },
    "thingGroupIndexingConfiguration": {
        "thingGroupIndexingMode": "OFF"
    }
}
```

To update the custom fields, you can run the update\-indexing\-configuration command\. An example is as follows:

```
aws iot update-indexing-configuration --thing-indexing-configuration
          'thingIndexingMode=REGISTRY_AND_SHADOW,customFields=[{name=attributes.version,type=Number},{name=attributes.color,type=String},{name=shadow.desired.power,type=Boolean},{name=shadow.desired.intensity,type=Number}]'
```

This command added `shadow.desired.intensity` to the indexing configuration\.

**Note**  
Updating the custom field indexing configuration overwrites all existing custom fields\. Make sure to specify all custom fields when calling update\-indexing\-configuration\.

After the index is rebuilt, you can use an aggregation query on the newly added fields, search registry data, shadow data, and thing connectivity status data\.

When changing the indexing mode, make sure all of your custom fields are valid by using the new indexing mode\. For example, if you start off using `REGISTRY_AND_SHADOW` mode with a custom field called `shadow.desired.temperature`, you must delete the `shadow.desired.temperature` custom field before changing the indexing mode to `REGISTRY`\. If your indexing configuration contains custom fields that aren't indexed by the indexing mode, the update fails\. 

## Describing a thing index<a name="describe-index"></a>

The following command shows you how to use the describe\-index CLI command to retrieve the current status of the thing index\.

```
aws iot describe-index --index-name "AWS_Things"
```

The response of the command can look like the following:

```
{
    "indexName": "AWS_Things", 
    "indexStatus": "BUILDING", 
    "schema": "REGISTRY_AND_SHADOW_AND_CONNECTIVITY_STATUS"
}
```

The first time you enable fleet indexing, AWS IoT builds your index\. When `indexStatus` is in the `BUILDING` state, you can't query the index\. The `schema` for the things index indicates which type of data \(`REGISTRY_AND_SHADOW_AND_CONNECTIVITY_STATUS`\) is indexed\.

Changing the configuration of your index causes the index to be rebuilt\. During this process, the `indexStatus` is `REBUILDING`\. You can run queries on data in the things index while it's being rebuilt\. For example, if you change the index configuration from `REGISTRY` to `REGISTRY_AND_SHADOW` while the index is being rebuilt, you can query registry data, including the latest updates\. However, you can't query the shadow data until the rebuild is complete\. The amount of time it takes to build or rebuild the index depends on the amount of data\.

You can see different values in the schema field depending on the data sources you've configured\. The table below shows the different schema values and the corresponding descriptions:


| Schema | Description | 
| --- | --- | 
| OFF | No data sources are configured or indexed\. | 
| REGISTRY | Registry data is indexed\. | 
| REGISTRY\_AND\_SHADOW | Registry data and unnamed \(classic\) shadow data are indexed\. | 
| REGISTRY\_AND\_CONNECTIVITY | Registry data and connectivity data are indexed\. | 
| REGISTRY\_AND\_SHADOW\_AND\_CONNECTIVITY\_STATUS | Registry data, unnamed \(classic\) shadow data, and connectivity data are indexed\. | 
| MULTI\_INDEXING\_MODE | Named shadow or Device Defender violations data is indexed, in addition to registry, unnamed \(classic\) shadow or connectivity data\. | 

## Querying a thing index<a name="search-index"></a>

Use the search\-index CLI command to query data in the index\.

```
aws iot search-index --index-name "AWS_Things" --query-string
          "thingName:mything*"
```

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
            "timestamp":1556649874716,
            "disconnectReason": "CONNECTION_LOST"
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

In the JSON response, `"connectivity"` \(as enabled by the `thingConnectivityIndexingMode=STATUS` setting\) provides a Boolean value, a timestamp, and a disconnectReason that indicates if the device is connected to AWS IoT Core\. The device `"mything1"` disconnected \(`false`\) at POSIX time `1556649874716` due to `CONNECTION_LOST`\. For more information about disconnect reasons, see [Lifecycle events](life-cycle-events.md)\. 

```
"connectivity": { 
    "connected":false,
    "timestamp":1556649874716, 
    "disconnectReason": "CONNECTION_LOST"
}
```

The device `"mything2"` connected \(`true`\) at POSIX time `1556649855046`:

```
"connectivity": { 
    "connected":true,
    "timestamp":1556649855046
}
```

Timestamps are given in milliseconds since epoch, so `1556649855046` represents 6:44:15\.046 PM on Tuesday, April 30, 2019 \(UTC\)\.

**Important**  
If a device has been disconnected for approximately an hour, the `"timestamp"` value and the `"disconnectReason"` value of the connectivity status might be missing\.

## Restrictions and limitations<a name="index-limitations"></a>

These are the restrictions and limitations for `AWS_Things`\.

**Shadow fields with complex types**  
A shadow field is indexed only if the value of the field is a simple type, such as a JSON object that doesn't contain an array, or an array that consists entirely of simple types\. Simple type means a string, number, or one of the literals `true` or `false`\. For example, given the following shadow state, the value of field `"palette"` isn't indexed because it's an array that contains items of complex types\. The value of field `"colors"` is indexed because each value in the array is a string\.   

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

**Nested shadow field names**  
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
The name of field `three` is stored as `desired.one.two.three`\. If you also have a shadow document, it's stored like this:  

```
{
  "state": {
    "desired": {
      "one.two.three": "v2"
    }    
  }
}
```
Both match a query for `shadow.desired.one.two.three:v2`\. As a best practice, don't use periods in shadow field names\.

**Shadow metadata**  
A field in a shadow's metadata section is indexed, but only if the corresponding field in the shadow's `"state"` section is indexed\. \(In the previous example, the `"palette"` field in the shadow's metadata section isn't indexed either\.\)

**Unregistered shadows**  
If you use [UpdateThingShadow](https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_UpdateThingShadow.html) to create a shadow using a thing name that hasn't been registered in your AWS IoT account, fields in this shadow aren't indexed\. This applies to both classic unnamed shadow and named shadow\.

**Numeric values**  
If any registry or shadow data is recognized by the service as a numeric value, it's indexed as such\. You can form queries involving ranges and comparison operators on numeric values \(for example, `"attribute.foo<5"` or `"shadow.reported.foo:[75 TO 80]"`\)\. To be recognized as numeric, the value of the data must be a valid, literal type JSON number\. The value can be an integer in the range \-2^53\.\.\.2^53\-1, a double\-precision floating point with optional exponential notation, or part of an array that contains only these values\. 

**Null values**  
Null values aren't indexed\.

**Maximum values**  
The maximum number of custom fields for aggregation queries is 5\.  
The maximum number of requested percentiles for aggregation queries is 100\.

## Authorization<a name="query-auth"></a>

You can specify the things index as an Amazon Resource Name \(ARN\) in an AWS IoT policy action, as follows\.


****  

| Action | Resource | 
| --- | --- | 
|  `iot:SearchIndex`  |  An index ARN \(for example, `arn:aws:iot:your-aws-regionyour-aws-account:index/AWS_Things`\)\.  | 
|  `iot:DescribeIndex`  |  An index ARN \(for example, `arn:aws:iot:your-aws-region:index/AWS_Things`\)\.  | 

**Note**  
If you have permissions to query the fleet index, you can access the data of things across the entire fleet\.