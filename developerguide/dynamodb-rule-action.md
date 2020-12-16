# DynamoDB<a name="dynamodb-rule-action"></a>

The DynamoDB \(`dynamoDB`\) action writes all or part of an MQTT message to an Amazon DynamoDB table\. 

You can follow a tutorial that shows you how to create and test a rule with a DynamoDB action\. For more information, see [Creating a rule with a DynamoDB action](iot-ddb-rule.md)\.

**Note**  
This rule writes non\-JSON data to DynamoDB as binary data\. The DynamoDB console displays the data as base64\-encoded text\.

## Requirements<a name="dynamodb-rule-action-requirements"></a>

This rule action has the following requirements:
+ An IAM role that AWS IoT can assume to perform the `dynamodb:PutItem` operation\. For more information, see [Granting AWS IoT the required access](iot-create-role.md)\.

  In the AWS IoT console, you can choose or create a role to allow AWS IoT to perform this rule action\.
+  If you use a customer\-managed AWS Key Management Service \(AWS KMS\) customer master key \(CMK\) to encrypt data at rest in DynamoDB, the service must have permission to use the CMK on the caller's behalf\. For more information, see [Customer Managed CMK](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/encryption.howitworks.html#managed-cmk-customer-managed) in the *Amazon DynamoDB Getting Started Guide*\.

## Parameters<a name="dynamodb-rule-action-parameters"></a>

When you create an AWS IoT rule with this action, you must specify the following information:

`tableName`  
The name of the DynamoDB table\.  
Supports substitution templates: API and AWS CLI only

`hashKeyField`  
The name of the hash key \(also called the partition key\)\.  
Supports substitution templates: API and AWS CLI only

`hashKeyType`  
\(Optional\) The data type of the hash key \(also called the partition key\)\. Valid values: `STRING`, `NUMBER`\.  
Supports substitution templates: API and AWS CLI only

`hashKeyValue`  
The value of the hash key\. Consider using a substitution template such as `${topic()}` or `${timestamp()}`\.  
Supports substitution templates: Yes

`rangeKeyField`  
\(Optional\) The name of the range key \(also called the sort key\)\.  
Supports substitution templates: API and AWS CLI only

`rangeKeyType`  
\(Optional\) The data type of the range key \(also called the sort key\)\. Valid values: `STRING`, `NUMBER`\.  
Supports substitution templates: API and AWS CLI only

`rangeKeyValue`  
\(Optional\) The value of the range key\. Consider using a substitution template such as `${topic()}` or `${timestamp()}`\.  
Supports substitution templates: Yes

`payloadField`  
\(Optional\) The name of the column where the payload is written\. If you omit this value, the payload is written to the column named `payload`\.  
Supports substitution templates: Yes

`operation`  
\(Optional\) The type of operation to be performed\. Valid values: `INSERT`, `UPDATE`, `DELETE`\.  
Supports substitution templates: Yes

`roleARN`  
The IAM role that allows access to the DynamoDB table\. For more information, see [Requirements](#dynamodb-rule-action-requirements)\.  
Supports substitution templates: No

The data written to the DynamoDB table is the result from the SQL statement of the rule\.

## Examples<a name="dynamodb-rule-action-examples"></a>

The following JSON example defines a DynamoDB action in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * AS message FROM 'some/topic'", 
        "ruleDisabled": false,
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "dynamoDB": {
                    "tableName": "my_ddb_table",
                    "hashKeyField": "key",
                    "hashKeyValue": "${topic()}",
                    "rangeKeyField": "timestamp",
                    "rangeKeyValue": "${timestamp()}",
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_dynamoDB"
                }
            }
        ]
    }
}
```

## See also<a name="dynamodb-rule-action-see-also"></a>
+ [What is Amazon DynamoDB?](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/) in the *Amazon DynamoDB Developer Guide*
+ [Getting started with DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStartedDynamoDB.html) in the *Amazon DynamoDB Developer Guide*
+ [Creating a rule with a DynamoDB action](iot-ddb-rule.md)