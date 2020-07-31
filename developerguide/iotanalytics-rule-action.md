# IoT Analytics<a name="iotanalytics-rule-action"></a>

The IoT Analytics \(`iotAnalytics`\) action sends data from an MQTT message to an AWS IoT Analytics channel\.

## Requirements<a name="iotanalytics-rule-action-requirements"></a>

This rule action has the following requirements:
+ An IAM role that AWS IoT can assume to perform the `iotanalytics:BatchPutMessage` operation\. For more information, see [Granting AWS IoT the required access](iot-create-role.md)\.

  In the AWS IoT console, you can choose or create a role to allow AWS IoT to perform this rule action\.

  The policy attached to the role you specify should look like the following example\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": "iotanalytics:BatchPutMessage",
              "Resource": [
                  "arn:aws:iotanalytics:us-west-2:account-id:channel/mychannel"
              ]
          }
      ]
  }
  ```

## Parameters<a name="iotanalytics-rule-action-parameters"></a>

When you create an AWS IoT rule with this action, you must specify the following information:

`channelName`  
The name of the AWS IoT Analytics channel to which to write the data\.  
Supports substitution templates: API and AWS CLI only

`roleArn`  
The IAM role that allows access to the AWS IoT Analytics channel\. For more information, see [Requirements](#iotanalytics-rule-action-requirements)\.  
Supports substitution templates: No

## Examples<a name="iotanalytics-rule-action-examples"></a>

The following JSON example defines an IoT Analytics action in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic'", 
        "ruleDisabled": false, 
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "iotAnalytics": {
                    "channelName": "mychannel", 
                    "roleArn": "arn:aws:iam::123456789012:role/analyticsRole", 
                }
            }
        ]
    }
}
```

## See also<a name="iotanalytics-rule-action-see-also"></a>
+ [What is AWS IoT Analytics?](https://docs.aws.amazon.com/iotanalytics/latest/userguide/) in the *AWS IoT Analytics User Guide*
+ The AWS IoT Analytics console also has a **Quick start** feature that lets you create a channel, data store, pipeline, and data store with one click\. For more information, see [AWS IoT Analytics console quickstart guide](https://docs.aws.amazon.com/iotanalytics/latest/userguide/quickstart.html) in the *AWS IoT Analytics User Guide*\.  
![\[The quick start feature in the AWS IoT Analytics console.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/iota-console-quickstart.png)