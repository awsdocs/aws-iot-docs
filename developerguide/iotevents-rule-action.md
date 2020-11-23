# IoT Events<a name="iotevents-rule-action"></a>

The IoT Events \(`iotEvents`\) action sends data from an MQTT message to an AWS IoT Events input\. 

## Requirements<a name="iotevents-rule-action-requirements"></a>

This rule action has the following requirements:
+ An IAM role that AWS IoT can assume to perform the `iotevents:BatchPutMessage` operation\. For more information, see [Granting AWS IoT the required access](iot-create-role.md)\.

  In the AWS IoT console, you can choose or create a role to allow AWS IoT to perform this rule action\.

## Parameters<a name="iotevents-rule-action-parameters"></a>

When you create an AWS IoT rule with this action, you must specify the following information:

`batchMode`  
\(Optional\) Whether to process the event actions as a batch\. The default value is `false`\.  
When `batchMode` is `true` and the rule SQL statement evaluates to an Array, each Array element is treated as a separate message when it's sent to AWS IoT Events by calling [https://docs.aws.amazon.com/iotevents/latest/apireference/API_iotevents-data_BatchPutMessage.html](https://docs.aws.amazon.com/iotevents/latest/apireference/API_iotevents-data_BatchPutMessage.html)\. The resulting array can't have more than 10 messages\.  
When `batchMode` is `true`, you can't specify a `messageId`\.   
Supports substitution templates: No

`inputName`  
The name of the AWS IoT Events input\.  
Supports substitution templates: API and AWS CLI only

`messageId`  
\(Optional\) Use this to ensure that only one input \(message\) with a given `messageId` is processed by an AWS IoT Events detector\. You can use the `${newuuid()}` substitution template to generate a unique ID for each request\.  
When `batchMode` is `true`, you can't specify a `messageId`\-\-a new UUID value will be assigned\.  
Supports substitution templates: Yes

`roleArn`  
The IAM role that allows AWS IoT to send an input to an AWS IoT Events detector\. For more information, see [Requirements](#iotevents-rule-action-requirements)\.  
Supports substitution templates: No

## Examples<a name="iotevents-rule-action-examples"></a>

The following JSON example defines an IoT Events action in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic",
        "ruleDisabled": false,
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "iotEvents": {
                    "inputName": "MyIoTEventsInput",
                    "messageId": "${newuuid()}",
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_events"
                }
            }
        ]
    }
}
```

## See also<a name="iotevents-rule-action-see-also"></a>
+ [What is AWS IoT Events?](https://docs.aws.amazon.com/iotevents/latest/developerguide/) in the *AWS IoT Events Developer Guide*