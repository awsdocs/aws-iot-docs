# Lambda<a name="lambda-rule-action"></a>

A Lambda \(`lambda`\) action invokes an AWS Lambda function, passing in an MQTT message\. AWS IoT invokes Lambda functions asynchronously\.

You can follow a tutorial that shows you how to create and test a rule with a Lambda action\. For more information, see [Tutorial: Formatting a notification by using an AWS Lambda function](iot-lambda-rule.md)\.

## Requirements<a name="lambda-rule-action-requirements"></a>

This rule action has the following requirements:
+ For AWS IoT to invoke a Lambda function, you must configure a policy that grants the `lambda:InvokeFunction` permission to AWS IoT\. You can only invoke a Lambda function defined in the same AWS Region where your Lambda policy exists\. Lambda functions use resource\-based policies, so you must attach the policy to the Lambda function itself\. 

  Use the following AWS CLI command to attach a policy that grants the `lambda:InvokeFunction` permission\.

  ```
  aws lambda add-permission --function-name function_name --region region --principal iot.amazonaws.com --source-arn arn:aws:iot:region:account-id:rule/rule_name --source-account account-id --statement-id unique_id --action "lambda:InvokeFunction"
  ```

  The `add-permission` command expects the following parameters:  
`--function-name`  
Name of the Lambda function\. You add a new permission to update the function's resource policy\.  
`--region`  
The AWS Region of the function\.  
`--principal`  
The principal that gets the permission\. This should be `iot.amazonaws.com` to allow AWS IoT permission to call the Lambda function\.  
`--source-arn`  
The ARN of the rule\. You can use the `get-topic-rule` AWS CLI command to get the ARN of a rule\.  
`--source-account`  
The AWS account where the rule is defined\.  
`--statement-id`  
A unique statement identifier\.  
`--action`  
The Lambda action you want to allow in this statement\. To allow AWS IoT to invoke a Lambda function, specify `lambda:InvokeFunction`\.
**Important**  
If you add a permission for an AWS IoT principal without providing the `source-arn` or `source-account`, any AWS account that creates a rule with your Lambda action can trigger rules to invoke your Lambda function from AWS IoT\.

  For more information, see [AWS Lambda permissions](https://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html)\.
+ If you use an AWS Key Management Service \(AWS KMS\) customer\-managed AWS KMS key \(KMS key\) to encrypt data at rest in Lambda, the service must have permission to use the AWS KMS key on the caller's behalf\. For more information, see [Encryption at rest](https://docs.aws.amazon.com/lambda/latest/dg/security-dataprotection.html#security-privacy-atrest) in the *AWS Lambda Developer Guide*\.

## Parameters<a name="lambda-rule-action-parameters"></a>

When you create an AWS IoT rule with this action, you must specify the following information:

`functionArn`  
The ARN of the Lambda function to invoke\. AWS IoT must have permission to invoke the function\. For more information, see [Requirements](#lambda-rule-action-requirements)\.  
If you don't specify a version or alias for your Lambda function, the most recent version of the function is executed\. You can specify a version or alias if you want to execute a specific version of your Lambda function\. To specify a version or alias, append the version or alias to the ARN of the Lambda function\.  

```
arn:aws:lambda:us-east-2:123456789012:function:myLambdaFunction:someAlias
```
For more information about versioning and aliases see [AWS Lambda function versioning and aliases](https://docs.aws.amazon.com/lambda/latest/dg/versioning-aliases.html)\.  
Supports [substitution templates](iot-substitution-templates.md): API and AWS CLI only

## Examples<a name="lambda-rule-action-examples"></a>

The following JSON example defines a Lambda action in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic'", 
        "ruleDisabled": false, 
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "lambda": {
                    "functionArn": "arn:aws:lambda:us-east-2:123456789012:function:myLambdaFunction"
                 }
            }
        ]
    }
}
```

The following JSON example defines a Lambda action with substitution templates in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic'",
        "ruleDisabled": false,
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "lambda": {
                    "functionArn": "arn:aws:lambda:us-east-1:123456789012:function:${topic()}"
                }
            }
        ]
    }
}
```

## See also<a name="lambda-rule-action-see-also"></a>
+ [What is AWS Lambda?](https://docs.aws.amazon.com/lambda/latest/dg/) in the *AWS Lambda Developer Guide*
+ [Tutorial: Formatting a notification by using an AWS Lambda function](iot-lambda-rule.md)