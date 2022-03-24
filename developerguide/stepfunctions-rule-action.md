# Step Functions<a name="stepfunctions-rule-action"></a>

The Step Functions \(`stepFunctions`\) action starts an AWS Step Functions state machine\.

## Requirements<a name="stepfunctions-rule-action-requirements"></a>

This rule action has the following requirements:
+ An IAM role that AWS IoT can assume to perform the `states:StartExecution` operation\. For more information, see [Granting an AWS IoT rule the access it requires](iot-create-role.md)\.

  In the AWS IoT console, you can choose or create a role to allow AWS IoT to perform this rule action\.

## Parameters<a name="stepfunctions-rule-action-parameters"></a>

When you create an AWS IoT rule with this action, you must specify the following information:

`stateMachineName`  
The name of the Step Functions state machine to start\.  
Supports [substitution templates](iot-substitution-templates.md): API and AWS CLI only

`executionNamePrefix`  
\(Optional\) The name given to the state machine execution consists of this prefix followed by a UUID\. Step Functions creates a unique name for each state machine execution if one is not provided\.  
Supports [substitution templates](iot-substitution-templates.md): Yes

`roleArn`  
The ARN of the role that grants AWS IoT permission to start the state machine\. For more information, see [Requirements](#stepfunctions-rule-action-requirements)\.  
Supports [substitution templates](iot-substitution-templates.md): No

## Examples<a name="stepfunctions-rule-action-examples"></a>

The following JSON example defines a Step Functions action in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic'",
        "ruleDisabled": false,
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "stepFunctions": {
                    "stateMachineName": "myStateMachine",
                    "executionNamePrefix": "myExecution",
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_step_functions"
                }
            }
        ]
    }
}
```

## See also<a name="stepfunctions-rule-action-see-also"></a>
+ [What is AWS Step Functions?](https://docs.aws.amazon.com/step-functions/latest/dg/) in the *AWS Step Functions Developer Guide*