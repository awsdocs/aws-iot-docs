# CloudWatch metrics<a name="cloudwatch-metrics-rule-action"></a>

The CloudWatch metric \(`cloudwatchMetric`\) action captures an Amazon CloudWatch metric\. You can specify the metric namespace, name, value, unit, and timestamp\. 

## Requirements<a name="cloudwatch-metrics-rule-action-requirements"></a>

This rule action has the following requirements:
+ An IAM role that AWS IoT can assume to perform the `cloudwatch:PutMetricData` operation\. For more information, see [Granting an AWS IoT rule the access it requires](iot-create-role.md)\.

  In the AWS IoT console, you can choose or create a role to allow AWS IoT to perform this rule action\.

## Parameters<a name="cloudwatch-metrics-rule-action-parameters"></a>

When you create an AWS IoT rule with this action, you must specify the following information:

`metricName`  
The CloudWatch metric name\.  
Supports [substitution templates](iot-substitution-templates.md): Yes

`metricNamespace`  
The CloudWatch metric namespace name\.  
Supports [substitution templates](iot-substitution-templates.md): Yes

`metricUnit`  
The metric unit supported by CloudWatch\.  
Supports [substitution templates](iot-substitution-templates.md): Yes

`metricValue`  
A string that contains the CloudWatch metric value\.  
Supports [substitution templates](iot-substitution-templates.md): Yes

`metricTimestamp`  
\(Optional\) A string that contains the timestamp, expressed in seconds in Unix epoch time\. Defaults to the current Unix epoch time\.  
Supports [substitution templates](iot-substitution-templates.md): Yes

`roleArn`  
The IAM role that allows access to the CloudWatch metric\. For more information, see [Requirements](#cloudwatch-metrics-rule-action-requirements)\.  
Supports [substitution templates](iot-substitution-templates.md): No

## Examples<a name="cloudwatch-metrics-rule-action-examples"></a>

The following JSON example defines a CloudWatch metric action in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic'", 
        "ruleDisabled": false, 
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "cloudwatchMetric": {
                    "metricName": "IotMetric",
                    "metricNamespace": "IotNamespace", 
                    "metricUnit": "Count",
                    "metricValue": "1",
                    "metricTimestamp": "1456821314",
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_cw"
                }
            }
        ]
    }
}
```

The following JSON example defines a CloudWatch metric action with substitution templates in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic'",
        "ruleDisabled": false,
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "cloudwatchMetric": {
                    "metricName": "${topic()}",
                    "metricNamespace": "${namespace}",
                    "metricUnit": "${unit}",
                    "metricValue": "${value}",
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_cw"
                }
            }
        ]
    }
}
```

## See also<a name="cloudwatch-metrics-rule-action-see-also"></a>
+ [What is Amazon CloudWatch?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/) in the *Amazon CloudWatch User Guide*
+ [Using Amazon CloudWatch metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html) in the *Amazon CloudWatch User Guide*