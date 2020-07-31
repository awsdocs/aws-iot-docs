# CloudWatch alarms<a name="cloudwatch-alarms-rule-action"></a>

The CloudWatch alarm \(`cloudWatchAlarm`\) action changes the state of an Amazon CloudWatch alarm\. You can specify the state change reason and value in this call\. 

## Requirements<a name="cloudwatch-alarms-rule-action-requirements"></a>

This rule action has the following requirements:
+ An IAM role that AWS IoT can assume to perform the `cloudwatch:SetAlarmState` operation\. For more information, see [Granting AWS IoT the required access](iot-create-role.md)\.

  In the AWS IoT console, you can choose or create a role to allow AWS IoT to perform this rule action\.

## Parameters<a name="cloudwatch-alarms-rule-action-parameters"></a>

When you create an AWS IoT rule with this action, you must specify the following information:

`alarmName`  
The CloudWatch alarm name\.  
Supports substitution templates: API and AWS CLI only

`stateReason`  
Reason for the alarm change\.  
Supports substitution templates: Yes

`stateValue`  
The value of the alarm state\. Valid values: `OK`, `ALARM`, `INSUFFICIENT_DATA`\.  
Supports substitution templates: Yes

`roleArn`  
The IAM role that allows access to the CloudWatch alarm\. For more information, see [Requirements](#cloudwatch-alarms-rule-action-requirements)\.  
Supports substitution templates: No

## Examples<a name="cloudwatch-alarms-rule-action-examples"></a>

The following JSON example defines a CloudWatch alarm action in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic'", 
        "ruleDisabled": false, 
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "cloudwatchAlarm": {
                    "alarmName": "IotAlarm", 
                    "stateReason": "Temperature stabilized.",
                    "stateValue": "OK",
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_cw"
                }
            }
        ]
    }
}
```

## See also<a name="cloudwatch-alarms-rule-action-see-also"></a>
+ [What is Amazon CloudWatch?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/) in the *Amazon CloudWatch User Guide*
+ [Using Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the *Amazon CloudWatch User Guide*