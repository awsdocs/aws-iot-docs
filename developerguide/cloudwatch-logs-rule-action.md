# CloudWatch Logs<a name="cloudwatch-logs-rule-action"></a>

The CloudWatch Logs \(`cloudwatchLogs`\) action sends data to Amazon CloudWatch Logs\. You can specify the log group to which the action sends data\.

## Requirements<a name="cloudwatch-logs-rule-action-requirements"></a>

This rule action has the following requirements:
+ An IAM role that AWS IoT can assume to perform the `logs:CreateLogStream`, `logs:DescribeLogStreams`, and `logs:PutLogEvents` operations\. For more information, see [Granting AWS IoT the required access](iot-create-role.md)\.

  In the AWS IoT console, you can choose or create a role to allow AWS IoT to perform this rule action\.
+ If you use a customer managed AWS Key Management Service \(AWS KMS\) customer master key \(CMK\) to encrypt log data in CloudWatch Logs, the service must have permission to use the CMK on the caller's behalf\. For more information, see [Encrypt log data in CloudWatch Logs using AWS KMS](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/encrypt-log-data-kms.html) in the *Amazon CloudWatch Logs User Guide*\.

## Parameters<a name="cloudwatch-logs-rule-action-parameters"></a>

When you create an AWS IoT rule with this action, you must specify the following information:

`logGroupName`  
The CloudWatch log group to which the action sends data\.  
Supports substitution templates: API and AWS CLI only

`roleArn`  
The IAM role that allows access to the CloudWatch log group\. For more information, see [Requirements](#cloudwatch-logs-rule-action-requirements)\.  
Supports substitution templates: No

## Examples<a name="cloudwatch-logs-rule-action-examples"></a>

The following JSON example defines a CloudWatch Logs action in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic'", 
        "ruleDisabled": false, 
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "cloudwatchLogs": {
                    "logGroupName": "IotLogs",
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_cw"
                }
            }
        ]
    }
}
```

## See also<a name="cloudwatch-logs-rule-action-see-also"></a>
+ [What is Amazon CloudWatch Logs?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/) in the *Amazon CloudWatch Logs User Guide*