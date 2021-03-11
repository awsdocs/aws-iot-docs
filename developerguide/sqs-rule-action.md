# SQS<a name="sqs-rule-action"></a>

The SQS \(`sqs`\) action sends data from an MQTT message to an Amazon Simple Queue Service \(Amazon SQS\) queue\.

**Note**  
The SQS action doesn't support [Amazon SQS FIFO \(First\-In\-First\-Out\) queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/FIFO-queues.html)\. Because the rules engine is a fully distributed service, there is no guarantee of message order when the SQS action is triggered\.

## Requirements<a name="sqs-rule-action-requirements"></a>

This rule action has the following requirements:
+ An IAM role that AWS IoT can assume to perform the `sqs:SendMessage` operation\. For more information, see [Granting AWS IoT the required access](iot-create-role.md)\.

  In the AWS IoT console, you can choose or create a role to allow AWS IoT to perform this rule action\.
+ If you use an AWS Key Management Service \(AWS KMS\) customer managed key \(CMK\) to encrypt data at rest in Amazon SQS, the service must have permission to use the CMK on the caller's behalf\. For more information, see [Key management](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-key-management.html) in the *Amazon Simple Queue Service Developer Guide*\.

## Parameters<a name="sqs-rule-action-parameters"></a>

When you create an AWS IoT rule with this action, you must specify the following information:

`queueUrl`  
The URL of the Amazon SQS queue to which to write the data\.  
Supports [substitution templates](iot-substitution-templates.md): API and AWS CLI only

`useBase64`  
Set this parameter to `true` to configure the rule action to base64\-encode the message data before it writes the data to the Amazon SQS queue\. Defaults to `false`\.  
Supports [substitution templates](iot-substitution-templates.md): No

`roleArn`  
The IAM role that allows access to the Amazon SQS queue\. For more information, see [Requirements](#sqs-rule-action-requirements)\.  
Supports [substitution templates](iot-substitution-templates.md): No

## Examples<a name="sqs-rule-action-examples"></a>

The following JSON example defines an SQS action in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic'", 
        "ruleDisabled": false, 
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "sqs": {
                    "queueUrl": "https://sqs.us-east-2.amazonaws.com/123456789012/my_sqs_queue", 
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_sqs"
                }
            }
        ]
    }
}
```

The following JSON example defines an SQS action with substitution templates in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic'",
        "ruleDisabled": false,
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "sqs": {
                    "queueUrl": "https://sqs.us-east-2.amazonaws.com/123456789012/${topic()}",
                    "useBase64": true,
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_sqs"
                }
            }
        ]
    }
}
```

## See also<a name="sqs-rule-action-see-also"></a>
+ [What is Amazon Simple Queue Service?](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/) in the *Amazon Simple Queue Service Developer Guide*