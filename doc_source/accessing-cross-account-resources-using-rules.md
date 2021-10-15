# Accessing cross\-account resources using AWS IoT rules<a name="accessing-cross-account-resources-using-rules"></a>

You can configure AWS IoT rules for cross\-account access so that data ingested on MQTT topics of one account can be routed into the AWS services, such as Amazon SQS and Lambda, of another account\. The following explains how to set up AWS IoT rules for cross\-account data ingestion, from an MQTT topic in one account, to a destination in another account\. 

Cross\-account rules can be configured using [resource\-based permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_controlling.html#TypesPermissions) on the destination resource\. Therefore, only destinations that support resource\-based permissions can be enabled for the cross\-account access with AWS IoT rules\. The supported destinations include Amazon SQS, Amazon SNS, Amazon S3, and AWS Lambda\. 

## Prerequisites<a name="cross-account-prerequisites"></a>
+ Familiarity with [AWS IoT rules](https://docs.aws.amazon.com/iot/latest/developerguide/iot-rules.html)
+ An understanding of IAM [users](http://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_identity-management.html), [roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html), and [resource\-based permission](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_permissions.html#TypesPermissions)
+ Having [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) installed

## Cross\-account setup for Amazon SQS<a name="cross-account-sqs"></a>

Scenario: Account A sends data from an MQTT message to account B's Amazon SQS queue\.


| AWS account | Account referred to as  | Description | 
| --- | --- | --- | 
| 1111\-1111\-1111 | Account A | Rule action: sqs:SendMessage | 
| 2222\-2222\-2222 | Account B | Amazon SQS queue [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/accessing-cross-account-resources-using-rules.html)  | 

**Do the Account A tasks**
**Note**  
To run the following commands, your IAM user should have permissions to `iot:CreateTopicRule` with the rule's Amazon Resource Name \(ARN\) as a resource, and permissions to `iam:PassRole` action with a resource as the role's ARN\.

1. [Configure AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html) using account A’s IAM user\. 

1. Create an IAM role that trusts AWS IoT rules engine, and attaches a policy that allows access to account B's Amazon SQS queue\. See example commands and policy documents in [Granting AWS IoT the required access](https://docs.aws.amazon.com/iot/latest/developerguide/iot-create-role.html)\.

1. To create a rule that is attached to a topic, run the [create\-topic\-rule command](https://docs.aws.amazon.com/cli/latest/reference/iot/create-topic-rule.html)\.

   ```
   aws iot create-topic-rule --rule-name myRule --topic-rule-payload file://./my-rule.json
   ```

   The following is an example payload file with a rule that inserts all messages sent to the `iot/test` topic into the specified Amazon SQS queue\. The SQL statement filters the messages and the role ARN grants AWS IoT permissions to add the message to the Amazon SQS queue\.

   ```
   {
   	"sql": "SELECT * FROM 'iot/test'",
   	"ruleDisabled": false,
   	"actions": [
   		{
   			"sqs": {
   				"queueUrl": "https://sqs.region.amazonaws.com/2222-2222-2222/ExampleQueue",
   				"roleArn": "arn:aws:iam::1111-1111-1111:role/my-iot-role",
   				"useBase64": false
   			}
   		}
   	]
   }
   ```

   For more information about how to define an Amazon SQS action in an AWS IoT rule, see [AWS IoT rule actions \- Amazon SQS](https://docs.aws.amazon.com/iot/latest/developerguide/sqs-rule-action.html)\.

**Do the Account B tasks**

1. [Configure AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html) using account B’s IAM user\. 

1. To give permissions for the Amazon SQS queue resource to account A, run the [add\-permission command](https://docs.aws.amazon.com/cli/latest/reference/sqs/add-permission.html)\.

   ```
   aws sqs add-permission --queue-url https://sqs.region.amazonaws.com/2222-2222-2222/ExampleQueue --label SendMessagesToMyQueue --aws-account-ids 1111-1111-1111 --actions SendMessage
   ```

## Cross\-account setup for Amazon SNS<a name="cross-account-sns"></a>

Scenario: Account A sends data from an MQTT message to an Amazon SNS topic of account B\.


| AWS account | Account referred to as  | Description | 
| --- | --- | --- | 
| 1111\-1111\-1111 | Account A | Rule action: sns:Publish | 
| 2222\-2222\-2222 | Account B | Amazon SNS topic ARN: arn:aws:sns:region:2222\-2222\-2222:ExampleTopic  | 

**Do the Account A tasks**
**Notes**  
 To run the following commands, your IAM user should have permissions to `iot:CreateTopicRule` with rule ARN as a resource and permissions to the `iam:PassRole` action with a resource as role ARN\.

1. [Configure AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html) using account A’s IAM user\. 

1. Create an IAM role that trusts AWS IoT rules engine, and attaches a policy that allows access to account B's Amazon SNS topic\. For example commands and policy documents, see [Granting AWS IoT the required access](https://docs.aws.amazon.com/iot/latest/developerguide/iot-create-role.html)\.

1. To create a rule that is attached to a topic, run the [create\-topic\-rule command](https://docs.aws.amazon.com/cli/latest/reference/iot/create-topic-rule.html)\.

   ```
   aws iot create-topic-rule --rule-name myRule --topic-rule-payload file://./my-rule.json
   ```

   The following is an example payload file with a rule that inserts all messages sent to the `iot/test` topic into the specified Amazon SNS topic\. The SQL statement filters the messages, and the role ARN grants AWS IoT permissions to send the message to the Amazon SNS topic\.

   ```
   {
   	"sql": "SELECT * FROM 'iot/test'",
   	"ruleDisabled": false,
   	"actions": [
   		{
   			"sns": {
   				"targetArn": "arn:aws:sns:region:2222-2222-2222:ExampleTopic",
   				"roleArn": "arn:aws:iam::1111-1111-1111:role/my-iot-role"
   			}
   		}
   	]
   }
   ```

   For more information about how to define an Amazon SNS action in an AWS IoT rule, see [AWS IoT rule actions \- Amazon SNS](https://docs.aws.amazon.com/iot/latest/developerguide/sns-rule-action.html)\.

**Do the Account B tasks**

1. [Configure AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html) using account B’s IAM user\. 

1. To give permission on the Amazon SNS topic resource to account A, run the [add\-permission command](https://docs.aws.amazon.com/cli/latest/reference/sns/add-permission.html)\.

   ```
   aws sns add-permission --topic-arn arn:aws:sns:region:2222-2222-2222:ExampleTopic --label Publish-Permission --aws-account-id 1111-1111-1111 --action-name Publish
   ```

## Cross\-account setup for Amazon S3<a name="cross-account-s3"></a>

Scenario: Account A sends data from an MQTT message to an Amazon S3 bucket of account B\.


| AWS account | Account referred to as  | Description | 
| --- | --- | --- | 
| 1111\-1111\-1111 | Account A | Rule action: s3:PutObject | 
| 2222\-2222\-2222 | Account B | Amazon S3 bucket ARN: arn:aws:s3:::ExampleBucket  | 

**Do the Account A tasks**
**Note**  
To run the following commands, your IAM user should have permissions to `iot:CreateTopicRule` with the rule ARN as a resource and permissions to `iam:PassRole` action with a resource as role ARN\.

1. [Configure AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html) using account A’s IAM user\. 

1. Create an IAM role that trusts AWS IoT rules engine and attaches a policy that allows access to account B's Amazon S3 bucket\. For example commands and policy documents, see [Granting AWS IoT the required access](https://docs.aws.amazon.com/iot/latest/developerguide/iot-create-role.html)\.

1. To create a rule that is attached to your target S3 bucket, run the [create\-topic\-rule command](https://docs.aws.amazon.com/cli/latest/reference/iot/create-topic-rule.html)\.

   ```
   aws iot create-topic-rule --rule-name my-rule --topic-rule-payload file://./my-rule.json
   ```

   The following is an example payload file with a rule that inserts all messages sent to the `iot/test` topic into the specified Amazon S3 bucket\. The SQL statement filters the messages, and the role ARN grants AWS IoT permissions to add the message to the Amazon S3 bucket\.

   ```
   {
   	"sql": "SELECT * FROM 'iot/test'",
   	"ruleDisabled": false,
   	"actions": [
   		{
   			"s3": {
   				"bucketName": "ExampleBucket",
   				"key": "${topic()}/${timestamp()}",
   				"roleArn": "arn:aws:iam::1111-1111-1111:role/my-iot-role"
   			}
   		}
   	]
   }
   ```

   For more information about how to define an Amazon S3 action in an AWS IoT rule, see [AWS IoT rule actions \- Amazon S3](https://docs.aws.amazon.com/iot/latest/developerguide/s3-rule-action.html)\.

**Do the Account B tasks**

1. [Configure AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html) using account B’s IAM user\. 

1. Create a bucket policy that trusts account A's principal\.

   The following is an example payload file that defines a bucket policy that trusts the principal of another account\.

   ```
   {
   	"Version": "2012-10-17",
   	"Statement": [
   		{
   			"Sid": "AddCannedAcl",
   			"Effect": "Allow",
   			"Principal": {
   				"AWS": [
   					"arn:aws:iam::1111-1111-1111:root"
   				]
   			},
   			"Action": "s3:PutObject",
   			"Resource": "arn:aws:s3:::ExampleBucket/*"
   		}
   	]
   }
   ```

   For more information, see [bucket policy examples](https://docs.aws.amazon.com/AmazonS3/latest/userguide/example-bucket-policies.html#example-bucket-policies-use-case-1)\.

1. To attach the bucket policy to the specified bucket, run the [put\-bucket\-policy command](https://docs.aws.amazon.com/cli/latest/reference/s3api/put-bucket-policy.html)\.

   ```
   aws s3api put-bucket-policy --bucket ExampleBucket --policy file://./my-bucket-policy.json
   ```

1. To make the cross\-account access work, make sure you have the correct **Block all public access** settings\. For more information, see [Security Best Practices for Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-best-practices.html)\.

## Cross\-account setup for AWS Lambda<a name="cross-account-lambda"></a>

Scenario: Account A invokes an AWS Lambda function of account B, passing in an MQTT message\.


| AWS account | Account referred to as  | Description | 
| --- | --- | --- | 
| 1111\-1111\-1111 | Account A | Rule action: lambda:InvokeFunction | 
| 2222\-2222\-2222 | Account B | Lambda function ARN:  arn:aws:lambda:region:2222\-2222\-2222:function:example\-function  | 

**Do the Account A tasks**
**Notes**  
 To run the following commands, your IAM user should have permissions to `iot:CreateTopicRule` with rule ARN as a resource, and permissions to `iam:PassRole` action with resource as role ARN\.

1. [Configure AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html) using account A’s IAM user\. 

1. Run the [create\-topic\-rule command ](https://docs.aws.amazon.com/cli/latest/reference/iot/create-topic-rule.html)to create a rule that defines cross\-account access to account B's Lambda function\.

   ```
   aws iot create-topic-rule --rule-name my-rule --topic-rule-payload file://./my-rule.json
   ```

   The following is an example payload file with a rule that inserts all messages sent to the `iot/test` topic into the specified Lambda function\. The SQL statement filters the messages and the role ARN grants AWS IoT permission to pass in the data to the Lambda function\.

   ```
   {
   	"sql": "SELECT * FROM 'iot/test'",
   	"ruleDisabled": false,
   	"actions": [
   		{
   			"lambda": {
   				"functionArn": "arn:aws:lambda:region:2222-2222-2222:function:example-function"
   			}
   		}
   	]
   }
   ```

   For more information about how to define an AWS Lambda action in an AWS IoT rule, read [AWS IoT rule actions \- Lambda](https://docs.aws.amazon.com/iot/latest/developerguide/lambda-rule-action.html)\.

**Do the Account B tasks**

1. [Configure AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html) using account B’s IAM user\. 

1. Run [Lambda's add\-permission command](https://docs.aws.amazon.com/cli/latest/reference/lambda/add-permission.html) to give AWS IoT rules permission to trigger the Lambda function\. To run the following command, your IAM user should have permission to `lambda:AddPermission` action\.

   ```
   aws lambda add-permission --function-name example-function --region us-east-1 --principal iot.amazonaws.com --source-arn arn:aws:iot:region:1111-1111-1111:rule/example-rule --source-account 1111-1111-1111 --statement-id "unique_id" --action "lambda:InvokeFunction"
   ```

   **Options:**

   **\-\-principal**

    This field gives permission to AWS IoT \(represented by `iot.amazonaws.com`\) to call the Lambda function\.

   **\-\-source\-arn**

   This field ensures that only `arn:aws:iot:region:1111-1111-1111:rule/example-rule` in AWS IoT triggers this Lambda function and no other rule in the same or different account can trigger this Lambda function\.

   **\-\-source\-account**

   This field ensures that AWS IoT triggers this Lambda function only on behalf of the `1111-1111-1111` account\.
**Notes**  
If you see an error message "The rule could not be found" from your AWS Lambda function’s console under **Configuration**, ignore the error message and proceed to test the connection\.