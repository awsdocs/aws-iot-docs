# Rules for AWS IoT<a name="iot-rules"></a>

Rules give your devices the ability to interact with AWS services\. Rules are analyzed and actions are performed based on the MQTT topic stream\. You can use rules to support tasks like these:

+ Augment or filter data received from a device\.

+ Write data received from a device to an Amazon DynamoDB database\.

+ Save a file to Amazon S3\.

+ Send a push notification to all users using Amazon SNS\.

+ Publish data to an Amazon SQS queue\.

+ Invoke a Lambda function to extract data\.

+ Process messages from a large number of devices using Amazon Kinesis\.

+ Send data to the Amazon Elasticsearch Service\.

+ Capture a CloudWatch metric\. 

+ Change a CloudWatch alarm\.

+ Send the data from an MQTT message to Amazon Machine Learning to make predictions based on an Amazon ML model\. 

+ Send a message to a Salesforce IoT Input Stream\.

+ Send message data to an channel\.

+ Start execution of a Step Functions state machine\.

+ Send message data to an AWS IoT Events input\.

Your rules can use MQTT messages that pass through the publish/subscribe [Message Broker for AWS IoT](iot-message-broker.md) or, using the [Basic Ingest](iot-basic-ingest.md) feature, you can securely send device data to the AWS services listed above without incurring [messaging costs](https://aws.amazon.com/iot-core/pricing/)\. \(The [Basic Ingest](iot-basic-ingest.md) feature optimizes data flow by removing the publish/subscribe message broker from the ingestion path, so it is more cost effective while keeping the security and data processing features of AWS IoT\.\)

Before AWS IoT can perform these actions, you must grant it permission to access your AWS resources on your behalf\. When the actions are performed, you incur the standard charges for the AWS services you use\.


+ [Granting AWS IoT the Required Access](#iot-create-role)
+ [Pass Role Permissions](#pass-role)
+ [Creating an AWS IoT Rule](#iot-create-rule)
+ [Viewing Your Rules](#iot-view-rules)
+ [SQL Versions](#iot-rule-sql-version)
+ [Troubleshooting a Rule](#iot-troubleshoot-rule)
+ [Rule Error Handling](#rule-error-handling)
+ [Deleting a Rule](#iot-delete-rule)
+ [AWS IoT Rule Actions](iot-rule-actions.md)
+ [AWS IoT SQL Reference](iot-sql-reference.md)

## Granting AWS IoT the Required Access<a name="iot-create-role"></a>

You use IAM roles to control the AWS resources to which each rule has access\. Before you create a rule, you must create an IAM role with a policy that allows access to the required AWS resources\. AWS IoT assumes this role when executing a rule\.

**To create an IAM role \(AWS CLI\)**

1. Save the following trust policy document, which grants AWS IoT permission to assume the role, to a file called iot\-role\-trust\.json:

   ```
   {
       "Version":"2012-10-17",
       "Statement":[{
           "Effect": "Allow",
           "Principal": {
               "Service": "iot.amazonaws.com"
           },
           "Action": "sts:AssumeRole"
       }]
   }
   ```

   Use the [create\-role](http://alpha-docs-aws.amazon.com/cli/latest/reference/iam/create-role.html) command to create an IAM role specifying the iot\-role\-trust\.json file:

   ```
   aws iam create-role --role-name my-iot-role --assume-role-policy-document file://iot-role-trust.json
   ```

   The output of this command looks like the following:

   ```
   {
     "Role": {
         "AssumeRolePolicyDocument": "url-encoded-json",
         "RoleId": "AKIAIOSFODNN7EXAMPLE",
         "CreateDate": "2015-09-30T18:43:32.821Z",
         "RoleName": "my-iot-role",
         "Path": "/",
         "Arn": "arn:aws:iam::123456789012:role/my-iot-role"
     }
   }
   ```

1. Save the following JSON into a file named iot\-policy\.json\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [{
           "Effect": "Allow",
           "Action": "dynamodb:*",
           "Resource": "*"
       }]
   }
   ```

   This JSON is an example policy document that grants AWS IoT administrator access to DynamoDB\.

   Use the [create\-policy](http://alpha-docs-aws.amazon.com/cli/latest/reference/iam/create-policy.html) command to grant AWS IoT access to your AWS resources upon assuming the role, passing in the iot\-policy\.json file:

   ```
   aws iam create-policy --policy-name my-iot-policy --policy-document file://my-iot-policy.json
   ```

   For more information about how to grant access to AWS services in policies for AWS IoT, see [Creating an AWS IoT Rule](#iot-create-rule)\.

   The output of the [create\-policy](http://alpha-docs-aws.amazon.com/cli/latest/reference/iam/create-policy.html) command contains the ARN of the policy\. You need to attach the policy to a role\.

   ```
   {
       "Policy": {
           "PolicyName": "my-iot-policy",
           "CreateDate": "2015-09-30T19:31:18.620Z",
           "AttachmentCount": 0,
           "IsAttachable": true,
           "PolicyId": "ZXR6A36LTYANPAI7NJ5UV",
           "DefaultVersionId": "v1",
           "Path": "/",
           "Arn": "arn:aws:iam::123456789012:policy/my-iot-policy",
           "UpdateDate": "2015-09-30T19:31:18.620Z"
       }
   }
   ```

1. Use the [attach\-role\-policy](http://alpha-docs-aws.amazon.com/cli/latest/reference/iam/attach-role-policy.html) command to attach your policy to your role:

   ```
   aws iam attach-role-policy --role-name my-iot-role --policy-arn "arn:aws:iam::123456789012:policy/my-iot-policy"
   ```

## Pass Role Permissions<a name="pass-role"></a>

Part of a rule definition is an IAM role that grants permission to access resources specified in the rule's action\. The rules engine assumes that role when the rule's action is triggered\. The role must be defined in the same AWS account as the rule\.

When creating or replacing a rule you are, in effect, passing a role to the rules engine\. The user performing this operation requires the `iam:PassRole` permission\. To ensure you have this permission, create a policy that grants the `iam:PassRole` permission and attach it to your IAM user\. The following policy shows how to allow `iam:PassRole` permission for a role\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1",
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": [
                "arn:aws:iam::123456789012:role/myRole"
            ]
        }
    ]
}
```

In this policy example, the `iam:PassRole` permission is granted for the role `myRole`\. The role is specified using the role's ARN\. You must attach this policy to your IAM user or role to which your user belongs\. For more information, see [Working with Managed Policies](http://alpha-docs-aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html)\.

**Note**  
Lambda functions use resource\-based policy, where the policy is attached directly to the Lambda function itself\. When creating a rule that invokes a Lambda function, you do not pass a role, so the user creating the rule does not need the `iam:PassRole` permission\. For more information about Lambda function authorization, see [Granting Permissions Using a Resource Policy](http://alpha-docs-aws.amazon.com/lambda/latest/dg/intro-permission-model.html#intro-permission-model-access-policy)\. 

## Creating an AWS IoT Rule<a name="iot-create-rule"></a>

You configure rules to route data from your connected things\. Rules consist of the following:

Rule name  
The name of the rule\.

Optional description  
A textual description of the rule\.

SQL statement  
A simplified SQL syntax to filter messages received on an MQTT topic and push the data elsewhere\. For more information, see [AWS IoT SQL Reference](iot-sql-reference.md)\.

SQL version  
The version of the SQL rules engine to use when evaluating the rule\. Although this property is optional, we strongly recommend that you specify the SQL version\. If this property is not set, the default, `2015-10-08`, is used\.

One or more actions  
The actions AWS IoT performs when executing the rule\. For example, you can insert data into a DynamoDB table, write data to an Amazon S3 bucket, publish to an Amazon SNS topic, or invoke a Lambda function\.

An error action  
The action AWS IoT performs when it is unable to perform a rule's action\.

When you create a rule, be aware of how much data you are publishing on topics\. If you create rules that include a wildcard topic pattern, they might match a large percentage of your messages, and you might need to increase the capacity of the AWS resources used by the target actions\. Also, if you create a republish rule that includes a wildcard topic pattern, you can end up with a circular rule that causes an infinite loop\.

**Note**  
Creating and updating rules are administrator\-level actions\. Any user who has permission to create or update rules is able to access data processed by the rules\.

**To create a rule \(AWS CLI\)**  
Use the [create\-topic\-rule](http://alpha-docs-aws.amazon.com/cli/latest/reference/iot/create-topic-rule.html) command to create a rule:

```
aws iot create-topic-rule --rule-name my-rule --topic-rule-payload file://my-rule.json
```

The following is an example payload file with a rule that inserts all messages sent to the `iot/test` topic into the specified DynamoDB table\. The SQL statement filters the messages and the role ARN grants AWS IoT permission to write to the DynamoDB table\.

```
{
  "sql": "SELECT * FROM 'iot/test'",
  "ruleDisabled": false,
  "awsIotSqlVersion": "2016-03-23",
  "actions": [{
      "dynamoDB": {
          "tableName": "my-dynamodb-table",
          "roleArn": "arn:aws:iam::123456789012:role/my-iot-role",
          "hashKeyField": "topic",
          "hashKeyValue": "${topic(2)}",
          "rangeKeyField": "timestamp",
          "rangeKeyValue": "${timestamp()}"
      }
  }]
}
```

The following is an example payload file with a rule that inserts all messages sent to the `iot/test` topic into the specified S3 bucket\. The SQL statement filters the messages, and the role ARN grants AWS IoT permission to write to the Amazon S3 bucket\.

```
{
    "awsIotSqlVersion": "2016-03-23",
    "sql": "SELECT * FROM 'iot/test'",
    "ruleDisabled": false,
    "actions": [
    {
        "s3": {
            "roleArn": "arn:aws:iam::123456789012:role/aws_iot_s3",
            "bucketName": "my-bucket",
            "key": "myS3Key"
        }
    }
    ]
}
```

The following is an example payload file with a rule that pushes data to Amazon ES:

```
{
   "sql":"SELECT *, timestamp() as timestamp FROM 'iot/test'",
   "ruleDisabled":false,
  "awsIotSqlVersion": "2016-03-23",
    "actions":[
      {
        "elasticsearch":{
          "roleArn":"arn:aws:iam::123456789012:role/aws_iot_es",
          "endpoint":"https://my-endpoint",
         "index":"my-index",
          "type":"my-type",
          "id":"${newuuid()}"
        }
      }
    ]
}
```

The following is an example payload file with a rule that invokes a Lambda function:

```
{
  "sql": "expression",
  "ruleDisabled": false,
  "awsIotSqlVersion": "2016-03-23",
  "actions": [{
      "lambda": {
          "functionArn": "arn:aws:lambda:us-west-2:123456789012:function:my-lambda-function"
      }
  }]
}
```

The following is an example payload file with a rule that publishes to an Amazon SNS topic:

```
{
  "sql": "expression",
  "ruleDisabled": false,
  "awsIotSqlVersion": "2016-03-23",
  "actions": [{
      "sns": {
          "targetArn": "arn:aws:sns:us-west-2:123456789012:my-sns-topic",
          "roleArn": "arn:aws:iam::123456789012:role/my-iot-role"
      }
  }]
}
```

The following is an example payload file with a rule that republishes on a different MQTT topic:

```
{
  "sql": "expression",
  "ruleDisabled": false,
  "awsIotSqlVersion": "2016-03-23",
  "actions": [{
      "republish": {
          "topic": "my-mqtt-topic",
          "roleArn": "arn:aws:iam::123456789012:role/my-iot-role"
      }
  }]
}
```

The following is an example payload file with a rule that pushes data to an Amazon Kinesis Firehose stream:

```
{
    "sql": "SELECT * FROM 'my-topic'",
    "ruleDisabled": false,
  "awsIotSqlVersion": "2016-03-23",
    "actions": [{
        "firehose": {
            "roleArn": ""arn:aws:iam::123456789012:role/my-iot-role",
            "deliveryStreamName": "my-stream-name"
        }
    }]
}
```

The following is an example payload file with a rule that uses the Amazon Machine Learning `machinelearning_predict` function to republish to a topic if the data in the MQTT payload is classified as a 1\.

```
{
    "sql": "SELECT * FROM 'iot/test' where machinelearning_predict('my-model', 'arn:aws:iam::123456789012:role/my-iot-aml-role', *).predictedLabel=1",
    "ruleDisabled": false,
  "awsIotSqlVersion": "2016-03-23",
    "actions": [{
        "republish": {
            "roleArn": "arn:aws:iam::123456789012:role/my-iot-role",
            "topic": "my-mqtt-topic"
        }
    }]
}
```

The following is an example payload file with a rule that publishes messages to a Salesforce IoT Cloud input stream\.

```
{
    "sql": "expression",
    "ruleDisabled": false,
    "awsIotSqlVersion": "2016-03-23",
    "actions": [{
        "salesforce": {
            "token": "ABCDEFGHI123456789abcdefghi123456789",
            "url": "https://ingestion-cluster-id.my-env.sfdcnow.com/streams/stream-id/connection-id/my-event"
        }
    }]
}
```

The following is an example payload file with a rule that starts an execution of a Step Functions state machine\.

```
{
  "sql": "expression",
  "ruleDisabled": false,
  "awsIotSqlVersion": "2016-03-23",
  "actions": [{
      "stepFunctions": {
          "stateMachineName": "myCoolStateMachine",
          "executionNamePrefix": "coolRunning",
          "roleArn": "arn:aws:iam::123456789012:role/my-iot-role"
      }
  }]
}
```

## Viewing Your Rules<a name="iot-view-rules"></a>

Use the [list\-topic\-rules](http://alpha-docs-aws.amazon.com/cli/latest/reference/iot/list-topic-rules.html) command to list your rules:

```
aws iot list-topic-rules
```

Use the [get\-topic\-rule](http://alpha-docs-aws.amazon.com/cli/latest/reference/iot/get-topic-rule.html) command to get information about a rule:

```
aws iot get-topic-rule --rule-name my-rule
```

## SQL Versions<a name="iot-rule-sql-version"></a>

The AWS IoT rules engine uses an SQL\-like syntax to select data from MQTT messages\. The SQL statements are interpreted based on an SQL version specified with the `awsIotSqlVersion` property in a JSON document that describes the rule\. For more information about the structure of JSON rule documents, see Creating a Rule\. The `awsIotSqlVersion` property allows you to specify which version of the AWS IoT SQL rules engine you want to use\. When a new version is deployed, you can continue to use an older version or change your rule to use the new version\. Your current rules continue to use the version with which they were created\. 

The following JSON example shows how to specify the SQL version using the `awsIotSqlVersion` property:

```
{
    "sql": "expression",
    "ruleDisabled": false,
    "awsIotSqlVersion": "2016-03-23",
    "actions": [{
        "republish": {
            "topic": "my-mqtt-topic",
            "roleArn": "arn:aws:iam::123456789012:role/my-iot-role"
        }
     }]
}
```

Current supported versions are:

+ `2015-10-08`, the original SQL version built on 2015\-10\-08\.

+ `2016-03-23`, the SQL version built on 2016\-03\-23\.

+ `beta`, the most recent beta SQL version\. The use of this version might introduce breaking changes to your rules\.

### What's New in the 2016\-03\-23 SQL Rules Engine Version<a name="sql-2016-03-23-beta"></a>

+ Fixes for selecting nested JSON objects\.

+ Fixes for array queries\.

+ Inter\-object query support\.

+ Support to output an array as a top\-level object\.

+ Adds the encode \(*value*, *encodingScheme*\) function, which can be applied on both JSON and non\-JSON format data\.

#### Inter\-Object Queries<a name="inter-obj-query"></a>

This feature allows you to query for an attribute in a JSON object\. For example, given the following MQTT message:

```
{ 
    "e": [
       { "n": "temperature", "u": "Cel", "t": 1234, "v":22.5 },
       { "n": "light", "u": "lm", "t": 1235, "v":135 },
       { "n": "acidity", "u": "pH", "t": 1235, "v":7 }
    ]
}
```

And the following rule:

```
SELECT (SELECT v FROM e WHERE n = 'temperature') as temperature FROM 'my/topic'
```

The rule generates the following output:

```
{"temperature": [{"v":22.5}]}
```

Using the same MQTT message, given a slightly more complicated rule such as: 

```
SELECT get((SELECT v FROM e WHERE n = 'temperature'),1).v as temperature FROM 'topic'
```

The rule generates the following output:

```
{"temperature":22.5}
```

#### Output an `Array` as a Top\-Level Object<a name="return-array-rule"></a>

This feature allows a rule to return an array as a top\-level object\. For example, given the following MQTT message:

```
{
    "a": {"b":"c"},
    "arr":[1,2,3,4]
}
```

And the following rule:

```
SELECT VALUE arr FROM 'topic'
```

The rule generates the following output:

```
[1,2,3,4]
```

#### Encode Function<a name="encode-function"></a>

Encodes the payload, which potentially might be non\-JSON data, into its string representation based on the specified encoding scheme\.

## Troubleshooting a Rule<a name="iot-troubleshoot-rule"></a>

If you are having an issue with your rules, you should enable CloudWatch Logs\. By analyzing your logs, you can determine whether the issue is authorization or whether, for example, a WHERE clause condition did not match\. For more information about using Amazon CloudWatch Logs, see [Setting Up CloudWatchLogs](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/cloud-watch-logs.html)\.

## Rule Error Handling<a name="rule-error-handling"></a>

When AWS IoT receives a message from a device, the Rules Engine checks to see if the message matches a rule\. If so, the rule's SQL statement is evaluated and the rule's actions are triggered, passing the SQL statement's result\. 

If a problem occurs when triggering an action, the Rules Engine will trigger an error action, if one is specified for the rule\. This may happen when, for example:

+ A rule doesn't have permission to access an Amazon S3 bucket\.

+ A user error causes DynamoDB provisioned throughput to be exceeded\.

### Error Action Message Format<a name="rule-error-message-format"></a>

A single message is generated per rule and message\. For example, if two rule actions in the same rule fail, the error action will receive one message containing both errors\.

The error action message will look like this:

```
{
      "ruleName": "TestAction",
      "topic": "testme/action",
      "cloudwatchTraceId": "7e146a2c-95b5-6caf-98b9-50e3969734c7",
      "clientId": "iotconsole-1511213971966-0",
      "base64OriginalPayload": "ewogICJtZXNzYWdlIjogIkhlbGxvIHZyb20gQVdTIElvVCBjb25zb2xlIgp9",
      "failures": [
        {
          "failedAction": "S3Action",
          "failedResource": "us-east-1-s3-verify-user",
          "errorMessage": "Failed to put S3 object. The error received was The specified bucket does not exist (Service: Amazon S3; Status Code: 404; Error Code: NoSuchBucket; Request ID: 9DF5416B9B47B9AF; S3 Extended Request ID: yMah1cwPhqTH267QLPhTKeVPKJB8BO5ndBHzOmWtxLTM6uAvwYYuqieAKyb6qRPTxP1tHXCoR4Y=). Message arrived on: error/action, Action: s3, Bucket: us-east-1-s3-verify-user, Key: \"aaa\". Value of x-amz-id-2: yMah1cwPhqTH267QLPhTKeVPKJB8BO5ndBHzOmWtxLTM6uAvwYYuqieAKyb6qRPTxP1tHXCoR4Y="
        }
      ]
    }
```

ruleName  
The name of the rule that triggered the error action\.

topic  
The topic on which the original message was received\.

cloudwatchTraceId  
A unique identity referring to the error logs in CloudWatch\.

clientId  
The client ID of the message publisher\.

base64OriginalPayload  
The original message payload base64 encoded\.

failures    
failedAction  
The name of the action that failed to complete, for example "S3Action"\.  
failedResource  
The name of the resource, for example the name of an S3 bucket\.  
errorMessage  
The description and explanation of the error\.

### Error Action Example<a name="rule-error-example"></a>

Here is an example of a rule with an added error action\. The following rule has an action that writes message data to a DynamoDB table and an error action that writes data to an Amazon S3 bucket:

```
{
    "sql" : "SELECT * FROM ..."
    "actions" : [{ 
        "dynamoDB" : {
            "table" : "PoorlyConfiguredTable",
            "hashKeyField" : "AConstantString",
            "hashKeyValue" : "AHashKey"}}
    ],
    "errorAction" : { "s3" : {
            "roleArn": "arn:aws:iam::123456789012:role/aws_iot_s3",
            "bucket" : "message-processing-errors",
            "key" : "${replace(topic(), '/', '-') + '-' + timestamp() + '-' + newuuid()}"
    }}
}
```

You can use any function or substitution in an error action's SQL statement, except for external functions \(for example, `get_thing_shadow`, `aws_lambda`, and `machinelearning_predict`\.\)

For more information about rules and how to specify an error action, see [Creating an AWS IoT Rule](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/iot-create-rule.html)\.

For more information on using CloudWatch to monitor the success or failure of rules, see \.

## Deleting a Rule<a name="iot-delete-rule"></a>

When you are finished with a rule, you can delete it\.

**To delete a rule \(AWS CLI\)**  
Use the [delete\-topic\-rule](http://alpha-docs-aws.amazon.com/cli/latest/reference/iot/delete-topic-rule.html) command to delete a rule:

```
aws iot delete-topic-rule --rule-name my-rule
```