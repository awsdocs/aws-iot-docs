# DynamoDBv2<a name="dynamodb-v2-rule-action"></a>

The DynamoDBv2 \(`dynamoDBv2`\) action writes all or part of an MQTT message to an Amazon DynamoDB table\. Each attribute in the payload is written to a separate column in the DynamoDB database\.

## Requirements<a name="dynamodb-v2-rule-action-requirements"></a>

This rule action has the following requirements:
+ An IAM role that AWS IoT can assume to perform the `dynamodb:PutItem` operation\. For more information, see [Granting AWS IoT the required access](iot-create-role.md)\.

  In the AWS IoT console, you can choose or create a role to allow AWS IoT to perform this rule action\.
+ The MQTT message payload must contain a root\-level key that matches the table's primary partition key and a root\-level key that matches the table's primary sort key, if one is defined\.
+ If you use a customer managed AWS Key Management Service \(AWS KMS\) customer master key \(CMK\) to encrypt data at rest in DynamoDB, the service must have permission to use the CMK on the caller's behalf\. For more information, see [Customer Managed CMK](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/encryption.howitworks.html#managed-cmk-customer-managed) in the *Amazon DynamoDB Getting Started Guide*\.

## Parameters<a name="dynamodb-v2-rule-action-parameters"></a>

When you create an AWS IoT rule with this action, you must specify the following information:

`putItem`  
An object that specifies the DynamoDB table to which the message data will be written\. This object must contain the following information:    
`tableName`  
The name of the DynamoDB table\.  
Supports substitution templates: API and AWS CLI only

`roleARN`  
The IAM role that allows access to the DynamoDB table\. For more information, see [Requirements](#dynamodb-v2-rule-action-requirements)\.  
Supports substitution templates: No

The data written to the DynamoDB table is the result from the SQL statement of the rule\.

## Examples<a name="dynamodb-v2-rule-action-examples"></a>

The following JSON example defines a DynamoDBv2 action in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * AS message FROM 'some/topic'", 
        "ruleDisabled": false,
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "dynamoDBv2": {
                    "putItem": {
                        "tableName": "my_ddb_table"
                    },
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_dynamoDBv2", 
                }
            }
        ]
    }
}
```

The following JSON example defines a DynamoDB action with substitution templates in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic'",
        "ruleDisabled": false,
        "awsIotSqlVersion": "2015-10-08",
        "actions": [
            {
                "dynamoDBv2": {
                    "putItem": {
                        "tableName": "${topic()}"
                    },
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_dynamoDBv2"
                }
            }
        ]
    }
}
```

## See also<a name="dynamodb-v2-rule-action-see-also"></a>
+ [What is Amazon DynamoDB?](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/) in the *Amazon DynamoDB Developer Guide*
+ [Getting started with DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStartedDynamoDB.html) in the *Amazon DynamoDB Developer Guide*