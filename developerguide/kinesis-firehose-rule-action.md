# Kinesis Data Firehose<a name="kinesis-firehose-rule-action"></a>

The Kinesis Data Firehose\(`firehose`\) action sends data from an MQTT message to an Amazon Kinesis Data Firehose stream\. 

## Requirements<a name="kinesis-firehose-rule-action-requirements"></a>

This rule action has the following requirements:
+ An IAM role that AWS IoT can assume to perform the `firehose:PutRecord` operation\. For more information, see [Granting AWS IoT the required access](iot-create-role.md)\.

  In the AWS IoT console, you can choose or create a role to allow AWS IoT to perform this rule action\.
+ If you use Kinesis Data Firehose to send data to an Amazon S3 bucket, and you use an AWS Key Management Service \(AWS KMS\) customer managed key \(CMK\) to encrypt data at rest in Amazon S3, Kinesis Data Firehose must have access to your bucket and permission to use the CMK on the caller's behalf\. For more information, see [Grant Kinesis Data Firehose access to an Amazon S3 destination](https://docs.aws.amazon.com/firehose/latest/dev/controlling-access.html#using-iam-s3) in the *Amazon Kinesis Data Firehose Developer Guide*\.

## Parameters<a name="kinesis-firehose-rule-action-parameters"></a>

When you create an AWS IoT rule with this action, you must specify the following information:

`batchMode`  
\(Optional\) Whether to deliver the Kinesis Data Firehose stream as a batch by using [https://docs.aws.amazon.com/firehose/latest/APIReference/API_PutRecordBatch.html](https://docs.aws.amazon.com/firehose/latest/APIReference/API_PutRecordBatch.html) \. The default value is `false`\.  
When `batchMode` is `true` and the rule's SQL statement evaluates to an Array, each Array element forms one record in the `PutRecordBatch` request\. The resulting array can't have more than 500 records\.   
Supports substitution templates: No

`deliveryStreamName`  
The Kinesis Data Firehose stream to which to write the message data\.  
Supports substitution templates: API and AWS CLI only

`separator`  
\(Optional\) A character separator that is used to separate records written to the Kinesis Data Firehose stream\. If you omit this parameter, the stream uses no separator\. Valid values: `,` \(comma\), `\t` \(tab\), `\n` \(newline\), `\r\n` \(Windows newline\)\.  
Supports substitution templates: No

`roleArn`  
The IAM role that allows access to the Kinesis Data Firehose stream\. For more information, see [Requirements](#kinesis-firehose-rule-action-requirements)\.  
Supports substitution templates: No

## Examples<a name="kinesis-firehose-rule-action-examples"></a>

The following JSON example defines a Kinesis Data Firehose action in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic'", 
        "ruleDisabled": false, 
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "firehose": {
                    "deliveryStreamName": "my_firehose_stream",
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_firehose"
                }
            }
        ] 
    }
}
```

The following JSON example defines a Kinesis Data Firehose action with substitution templates in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic'",
        "ruleDisabled": false,
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "firehose": {
                    "deliveryStreamName": "${topic()}",
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_firehose"
                }
            }
        ]
    }
}
```

## See also<a name="kinesis-firehose-rule-action-see-also"></a>
+ [What is Amazon Kinesis Data Firehose?](https://docs.aws.amazon.com/firehose/latest/dev/) in the *Amazon Kinesis Data Firehose Developer Guide*