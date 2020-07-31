# IoT SiteWise<a name="iotsitewise-rule-action"></a>

The IoT SiteWise \(`iotSiteWise`\) action sends data from an MQTT message to asset properties in AWS IoT SiteWise\.

You can follow a tutorial that shows you how to ingest data from AWS IoT things\. For more information, see [Ingesting data to AWS IoT SiteWise from AWS IoT things](https://docs.aws.amazon.com/iot-sitewise/latest/userguide/ingest-data-from-iot-things.html) in the *AWS IoT SiteWise User Guide*\.

## Requirements<a name="iotsitewise-rule-action-requirements"></a>

This rule action has the following requirements:
+ An IAM role that AWS IoT can assume to perform the `iotsitewise:BatchPutAssetPropertyValue` operation\. For more information, see [Granting AWS IoT the required access](iot-create-role.md)\.

  You can attach the following example trust policy to the role\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": "iotsitewise:BatchPutAssetPropertyValue",
              "Resource": "*"
          }
      ]
  }
  ```

  To improve security, you can specify an AWS IoT SiteWise asset hierarchy path in the `Condition` property\. The following example is a trust policy that specifies an asset hierarchy path\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": "iotsitewise:BatchPutAssetPropertyValue",
              "Resource": "*",
              "Condition": {
                  "StringLike": {
                      "iotsitewise:assetHierarchyPath": [
                          "/root node asset ID",
                          "/root node asset ID/*"
                      ]
                  }
              }
          }
      ]
  }
  ```
+ When you send data to AWS IoT SiteWise with this action, your data must meet the requirements of the `BatchPutAssetPropertyValue` operation\. For more information, see [BatchPutAssetPropertyValue](https://docs.aws.amazon.com/iot-sitewise/latest/APIReference/API_BatchPutAssetPropertyValue.html) in the *AWS IoT SiteWise API Reference*\.

## Parameters<a name="iotsitewise-rule-action-parameters"></a>

When you create an AWS IoT rule with this action, you must specify the following information:

`putAssetPropertyValueEntries`  
A list of asset property value entries that each contain the following information:    
`propertyAlias`  
\(Optional\) The property alias associated with your asset property\. You must specify either a `propertyAlias` or both an `assetId` and a `propertyId`\. For more information about property aliases, see [Mapping industrial data streams to asset properties](https://docs.aws.amazon.com/iot-sitewise/latest/userguide/connect-data-streams.html) in the *AWS IoT SiteWise User Guide*\.  
Supports substitution templates: Yes  
`assetId`  
\(Optional\) The ID of the AWS IoT SiteWise asset\. You must specify either a `propertyAlias` or both an `assetId` and a `propertyId`\.  
Supports substitution templates: Yes  
`propertyId`  
\(Optional\) The ID of the asset's property\. You must specify either a `propertyAlias` or both an `assetId` and a `propertyId`\.  
Supports substitution templates: API and AWS CLI only  
`entryId`  
\(Optional\) A unique identifier for this entry\. You can define the `entryId` to better track which message caused an error in case of failure\. Defaults to a new UUID\.  
Supports substitution templates: Yes  
`propertyValues`  
A list of property values to insert that each contain timestamp, quality, and value \(TQV\) in the following format:    
`timestamp`  
A timestamp structure that contains the following information:    
`timeInSeconds`  
A string that contains the time in seconds in Unix epoch time\. If your message payload doesn't have a timestamp, you can use [timestamp\(\)](iot-sql-functions.md#iot-function-timestamp), which returns the current time in milliseconds\. To convert that time to seconds, you can use the following substitution template: **$\{floor\(timestamp\(\) / 1E3\)\}**\.  
Supports substitution templates: Yes  
`offsetInNanos`  
\(Optional\) A string that contains the nanosecond time offset from the time in seconds\. If your message payload doesn't have a timestamp, you can use [timestamp\(\)](iot-sql-functions.md#iot-function-timestamp), which returns the current time in milliseconds\. To calculate the nanosecond offset from that time, you can use the following substitution template: **$\{\(timestamp\(\) % 1E3\) \* 1E6\}**\.  
Supports substitution templates: Yes
With respect to Unix epoch time, AWS IoT SiteWise accepts only entries that have a timestamp of up to 15 minutes in the past and up to 5 minutes in the future\.  
`quality`  
\(Optional\) A string that describes the quality of the value\. Valid values: `GOOD`, `BAD`, `UNCERTAIN`\.  
Supports substitution templates: Yes  
`value`  
A value structure that contains one of the following value fields, depending on the asset property's data type:    
`booleanValue`  
\(Optional\) A string that contains the boolean value of the value entry\.  
Supports substitution templates: Yes  
`doubleValue`  
\(Optional\) A string that contains the double value of the value entry\.  
Supports substitution templates: Yes  
`integerValue`  
\(Optional\) A string that contains the integer value of the value entry\.  
Supports substitution templates: Yes  
`stringValue`  
\(Optional\) The string value of the value entry\.  
Supports substitution templates: Yes

`roleArn`  
The ARN of the IAM role that grants AWS IoT permission to send an asset property value to AWS IoT SiteWise\. For more information, see [Requirements](#iotsitewise-rule-action-requirements)\.  
Supports substitution templates: No

## Examples<a name="iotsitewise-rule-action-examples"></a>

The following JSON example defines a basic IoT SiteWise action in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic'",
        "ruleDisabled": false,
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "iotSiteWise": {
                    "putAssetPropertyValueEntries": [
                        {
                            "propertyAlias": "/some/property/alias",
                            "propertyValues": [
                                {
                                    "timestamp": {
                                        "timeInSeconds": "${my.payload.timeInSeconds}"
                                    },
                                    "value": {
                                        "integerValue": "${my.payload.value}"
                                    }
                                }
                            ]
                        }
                    ],
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_sitewise"
                }
            }
        ]
    }
}
```

The following JSON example defines an IoT SiteWise action in an AWS IoT rule\. This example uses the topic as the property alias and the `timestamp()` function\. For example, if you publish data to `/company/windfarm/3/turbine/7/rpm`, this action sends the data to the asset property with a property alias that is the same as the topic that you specified\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM '/company/windfarm/+/turbine/+/+'",
        "ruleDisabled": false,
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "iotSiteWise": {
                    "putAssetPropertyValueEntries": [
                        {
                            "propertyAlias": "${topic()}",
                            "propertyValues": [
                                {
                                    "timestamp": {
                                        "timeInSeconds": "${floor(timestamp() / 1E3)}",
                                        "offsetInNanos": "${(timestamp() % 1E3) * 1E6}"
                                    },
                                    "value": {
                                        "doubleValue": "${my.payload.value}"
                                    }
                                }
                            ]
                        }
                    ],
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_sitewise"
                }
            }
        ]
    }
}
```

## See also<a name="iotsitewise-rule-action-see-also"></a>
+ [What is AWS IoT SiteWise?](https://docs.aws.amazon.com/iot-sitewise/latest/userguide/what-is-sitewise.html) in the *AWS IoT SiteWise User Guide*
+ [Ingesting data using AWS IoT Core rules](https://docs.aws.amazon.com/iot-sitewise/latest/userguide/iot-rules.html) in the *AWS IoT SiteWise User Guide*
+ [Ingesting data to AWS IoT SiteWise from AWS IoT things](https://docs.aws.amazon.com/iot-sitewise/latest/userguide/ingest-data-from-iot-things.html) in the *AWS IoT SiteWise User Guide*
+ [Troubleshooting an AWS IoT SiteWise rule action](https://docs.aws.amazon.com/iot-sitewise/latest/userguide/troubleshoot-rule.html) in the *AWS IoT SiteWise User Guide*