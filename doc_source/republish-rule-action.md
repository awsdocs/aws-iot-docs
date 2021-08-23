# Republish<a name="republish-rule-action"></a>

The republish \(`republish`\) action republishes an MQTT message to another MQTT topic\.

## Requirements<a name="republish-rule-action-requirements"></a>

This rule action has the following requirements:
+ An IAM role that AWS IoT can assume to perform the `iot:Publish` operation\. For more information, see [Granting AWS IoT the required access](iot-create-role.md)\.

  In the AWS IoT console, you can choose or create a role to allow AWS IoT to perform this rule action\.

## Parameters<a name="republish-rule-action-parameters"></a>

When you create an AWS IoT rule with this action, you must specify the following information:

`topic`  
The MQTT topic to which to republish the message\.  
To republish to a reserved topic, which begins with `$`, use `$$` instead\. For example, to republish to the device shadow topic `$aws/things/MyThing/shadow/update`, specify the topic as `$$aws/things/MyThing/shadow/update`\.  
Republishing to [reserved job topics](reserved-topics.md#reserved-topics-job) is not supported\.
Supports [substitution templates](iot-substitution-templates.md): Yes

`qos`  
\(Optional\) The Quality of Service \(QoS\) level to use when republishing messages\. Valid values: `0`, `1`\. The default value is `0`\. For more information about MQTT QoS, see [MQTT](mqtt.md)\.  
Supports [substitution templates](iot-substitution-templates.md): No

`roleArn`  
The IAM role that allows AWS IoT to publish to the MQTT topic\. For more information, see [Requirements](#republish-rule-action-requirements)\.  
Supports [substitution templates](iot-substitution-templates.md): No

## Examples<a name="republish-rule-action-examples"></a>

The following JSON example defines a republish action in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic'", 
        "ruleDisabled": false, 
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "republish": {
                    "topic": "another/topic",
                    "qos": 1,
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_republish"
                }
            }
        ]
    }
}
```

The following JSON example defines a republish action with substitution templates in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic'",
        "ruleDisabled": false,
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "republish": {
                    "topic": "${topic()}/republish",
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_republish"
                }
            }
        ]
    }
}
```