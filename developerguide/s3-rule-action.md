# S3<a name="s3-rule-action"></a>

The S3 \(`s3`\) action writes the data from an MQTT message to an Amazon Simple Storage Service \(Amazon S3\) bucket\. 

## Requirements<a name="s3-rule-action-requirements"></a>

This rule action has the following requirements:
+ An IAM role that AWS IoT can assume to perform the `s3:PutObject` operation\. For more information, see [Granting an AWS IoT rule the access it requires](iot-create-role.md)\.

  In the AWS IoT console, you can choose or create a role to allow AWS IoT to perform this rule action\.
+ If you use an AWS Key Management Service \(AWS KMS\) customer\-managed AWS KMS key \(KMS key\) to encrypt data at rest in Amazon S3, the service must have permission to use the AWS KMS key on the caller's behalf\. For more information, see [AWS managed AWS KMS keys and customer managed AWS KMS keys](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingKMSEncryption.html#aws-managed-customer-managed-cmks) in the *Amazon Simple Storage Service Developer Guide*\.

## Parameters<a name="s3-rule-action-parameters"></a>

When you create an AWS IoT rule with this action, you must specify the following information:

`bucket`  
The Amazon S3 bucket to which to write data\.  
Supports [substitution templates](iot-substitution-templates.md): API and AWS CLI only

`cannedacl`  
\(Optional\) The Amazon S3 canned ACL that controls access to the object identified by the object key\. For more information, including allowed values, see [Canned ACL](https://docs.aws.amazon.com/AmazonS3/latest/dev/acl-overview.html#canned-acl)\.   
Supports [substitution templates](iot-substitution-templates.md): No

`key`  
The path to the file where the data is written\.  
Consider an example where this parameter is `${topic()}/${timestamp()}` and the rule receives a message where the topic is `some/topic`\. If the current timestamp is `1460685389`, then this action writes the data to a file called `1460685389` in the `some/topic` folder of the S3 bucket\.  
If you use a static key, AWS IoT overwrites a single file each time the rule invokes\. We recommend that you use the message timestamp or another unique message identifier so that a new file is saved in Amazon S3 for each message received\.
Supports [substitution templates](iot-substitution-templates.md): Yes

`roleArn`  
The IAM role that allows access to the Amazon S3 bucket\. For more information, see [Requirements](#s3-rule-action-requirements)\.  
Supports [substitution templates](iot-substitution-templates.md): No

## Examples<a name="s3-rule-action-examples"></a>

The following JSON example defines an S3 action in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic'", 
        "ruleDisabled": false, 
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "s3": {
                    "bucketName": "my-bucket", 
                    "cannedacl": "public-read",
                    "key": "${topic()}/${timestamp()}",
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_s3"
                }
            }
        ]
    }
}
```

## See also<a name="s3-rule-action-see-also"></a>
+ [What is Amazon S3?](https://docs.aws.amazon.com/AmazonS3/latest/dev/) in the *Amazon Simple Storage Service User Guide*