# AWS IoT Rule Actions<a name="iot-rule-actions"></a>

AWS IoT rule actions are used to specify what to do when a rule is triggered\. You can define actions to write data to a DynamoDB database or a Kinesis stream or to invoke a Lambda function, and more\. The following actions are supported: 

+ `cloudwatchAlarm` to change a CloudWatch alarm\.

+ `cloudwatchMetric` to capture a CloudWatch metric\.

+ `dynamoDB` to write data to a DynamoDB database\. 

+ `dynamoDBv2` to write data to a DynamoDB database\. 

+ `elasticsearch` to write data to an Amazon Elasticsearch Service domain\. 

+ `firehose` to write data to an Amazon Kinesis Firehose stream\.

+ `iotAnalytics` to send data to an AWS IoT Analytics channel\.

+ `iotEvents` to send data to an AWS IoT Events input\.

+ `kinesis` to write data to a Kinesis stream\.

+ `lambda` to invoke a Lambda function\.

+ `republish` to republish the message on another MQTT topic\.

+ `s3` to write data to an Amazon S3 bucket\.

+ `salesforce` to write a message to a Salesforce IoT input stream\.

+ `sns` to write data as a push notification\.

+ `sqs` to write data to an SQS queue\.

+ `stepFunctions` to start execution of a Step Functions state machine\.

**Note**  
The AWS IoT rules engine does not currently retry delivery for messages that fail to be published to another service\.

The following discusses each action in detail\.

## CloudWatch Alarm Action<a name="cloudwatch-alarm-action"></a>

------
#### [ CloudWatch Alarm Action ]

The CloudWatch alarm action allows you to change CloudWatch alarm state\. You can specify the state change reason and value in this call\. 

------
#### [ more info \(1\) ]

When creating an AWS IoT rule with a CloudWatch alarm action, you must specify the following information:

roleArn  
The IAM role that allows access to the CloudWatch alarm\.

alarmName  
The CloudWatch alarm name\.

stateReason  
Reason for the alarm change\.

stateValue  
The value of the alarm state\. Acceptable values are `OK`, `ALARM`, `INSUFFICIENT_DATA`\.

**Note**  
Ensure the role associated with the rule has a policy that grants the `cloudwatch:SetAlarmState` permission\.

The following JSON example shows how to define a CloudWatch alarm action in an AWS IoT rule:

```
{
    "rule": {
        "sql": "SELECT * FROM 'some/topic'", 
        "ruleDisabled": false, 
        "actions": [{
            "cloudwatchAlarm": {
                "roleArn": "arn:aws:iam::123456789012:role/aws_iot_cw", 
                "alarmName": "IotAlarm", 
                "stateReason": "Temperature stabilized.",
                "stateValue": "OK"
            }
        }]
    }
}
```

For more information, see [CloudWatch Alarms](http://alpha-docs-aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/AlarmThatSendsEmail.html)\.

------

## CloudWatch Metric Action<a name="cloudwatch-metric-action"></a>

------
#### [ CloudWatch Metric Action ]

The CloudWatch metric action allows you to capture a CloudWatch metric\. You can specify the metric namespace, name, value, unit, and timestamp\. 

------
#### [ more info \(2\) ]

When creating an AWS IoT rule with a CloudWatch metric action, you must specify the following information:

roleArn  
The IAM role that allows access to the CloudWatch metric\.

metricNamespace  
CloudWatch metric namespace name\.

metricName  
The CloudWatch metric name\.

metricValue  
The CloudWatch metric value\.

metricUnit  
The metric unit supported by CloudWatch\.

metricTimestamp  
An optional Unix timestamp\.

**Note**  
Ensure the role associated with the rule has a policy granting the `cloudwatch:PutMetricData` permission\.

The following JSON example shows how to define a CloudWatch metric action in an AWS IoT rule:

```
{
    "rule": {
        "sql": "SELECT * FROM 'some/topic'", 
        "ruleDisabled": false, 
        "actions": [{
            "cloudwatchMetric": {
                "roleArn": "arn:aws:iam::123456789012:role/aws_iot_cw", 
                "metricNamespace": "IotNamespace", 
                "metricName": "IotMetric",
                "metricValue": "1",
                "metricUnit": "Count",
                "metricTimestamp": "1456821314"
            }
        }]
    }
}
```

For more information, see [CloudWatch Metrics\.](http://alpha-docs-aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/CW_Support_For_AWS.html)

------

## DynamoDB Action<a name="dynamodb-rule"></a>

------
#### [ DynamoDB Action ]

The `dynamoDB` action allows you to write all or part of an MQTT message to a DynamoDB table\. 

------
#### [ more info \(3\) ]

When creating a DynamoDB rule, you must specify the following information: 

hashKeyType  
The data type of the hash key \(also called the partition key\)\. Valid values are: `"STRING"` or `"NUMBER"`\.

hashKeyField  
The name of the hash key \(also called the partition key\)\.

hashKeyValue  
The value of the hash key\.

rangeKeyType  
Optional\. The data type of the range key \(also called the sort key\)\. Valid values are: `"STRING"` or `"NUMBER"`\.

rangeKeyField  
Optional\. The name of the range key \(also called the sort key\)\.

rangeKeyValue  
Optional\. The value of the range key\.

operation  
Optional\. The type of operation to be performed\. This follows the substitution template, so it can be `${operation}`, but the substitution must result in one of the following: `INSERT`, `UPDATE`, or `DELETE`\.

payloadField   
Optional\. The name of the field where the payload is written\. If this value is omitted, the payload is written to the `payload` field\.

table  
The name of the DynamoDB table\.

roleARN  
The IAM role that allows access to the DynamoDB table\. At a minimum, the role must allow the `dynamoDB:PutItem` IAM action\.

The data written to the DynamoDB table is the result from the SQL statement of the rule\. The `hashKeyValue` and `rangeKeyValue` fields are usually composed of expressions \(for example, "$\{topic\(\)\}" or "$\{timestamp\(\)\}"\)\.

**Note**  
Non\-JSON data is written to DynamoDB as binary data\. The DynamoDB console displays the data as Base64\-encoded text\.  
Ensure the role associated with the rule has a policy granting the `dynamodb:PutItem` permission\.

The following JSON example shows how to define a `dynamoDB` action in an AWS IoT rule:

```
{
    "rule": {
        "ruleDisabled": false, 
        "sql": "SELECT * AS message FROM 'some/topic'", 
        "description": "A test Dynamo DB rule", 
        "actions": [{
            "dynamoDB": {
                "hashKeyField": "key", 
                "roleArn": "arn:aws:iam::123456789012:role/aws_iot_dynamoDB", 
                 "tableName": "my_ddb_table", 
                 "hashKeyValue": "${topic()}", 
                 "rangeKeyValue": "${timestamp()}", 
                 "rangeKeyField": "timestamp"
             }
        }]
    }
}
```

For more information, see the [Amazon DynamoDB Getting Started Guide](http://alpha-docs-aws.amazon.com/amazondynamodb/latest/gettingstartedguide/)\.

------

## DynamoDBv2 Action<a name="dynamodb-v2-rule"></a>

------
#### [ DynamoDBv2 Action ]

The `dynamoDBv2` action allows you to write all or part of an MQTT message to a DynamoDB table\. Each attribute in the payload is written to a separate column in the DynamoDB database\.

------
#### [ more info \(4\) ]

 When creating a DynamoDB rule, you must specify the following information: 

roleARN  
The IAM role that allows access to the DynamoDB table\. At a minimum, the role must allow the `dynamoDB:PutItem` IAM action\.

tableName  
The name of the DynamoDB table\.

**Note**  
The MQTT message payload must contain a root\-level key that matches the table's primary partition key and a root\-level key that matches the table's primary sort key, if one is defined\.

The data written to the DynamoDB table is the result from the SQL statement of the rule\.

**Note**  
Ensure the role associated with the rule has a policy granting the `dynamodb:PutItem` permission\.

The following JSON example shows how to define a `dynamoDB` action in an AWS IoT rule:

```
{
    "rule": {
        "ruleDisabled": false, 
        "sql": "SELECT * AS message FROM 'some/topic'", 
        "description": "A test DynamoDBv2 rule", 
        "actions": [{
            "dynamoDBv2": {
                "roleArn": "arn:aws:iam::123456789012:role/aws_iot_dynamoDBv2", 
                "putItem": {
                    "tableName": "my_ddb_table"
                }
             }
        }]
    }
}
```

For more information, see the [Amazon DynamoDB Getting Started Guide](http://alpha-docs-aws.amazon.com/amazondynamodb/latest/gettingstartedguide/)\.

------

## Amazon ES Action<a name="elasticsearch-rule"></a>

------
#### [ Amazon ES Action ]

The `elasticsearch` action allows you to write data from MQTT messages to an Amazon Elasticsearch Service domain\. Data in Amazon ES can then be queried and visualized by using tools like Kibana\. 

------
#### [ more info \(5\) ]

When you create an AWS IoT rule with an `elasticsearch` action, you must specify the following information:

endpoint  
The endpoint of your Amazon ES domain\.

index  
The Amazon ES index where you want to store your data\.

type  
The type of document you are storing\.

id  
The unique identifier for each document\.

**Note**  
Ensure the role associated with the rule has a policy granting the `es:ESHttpPut` permission\.

The following JSON example shows how to define an `elasticsearch` action in an AWS IoT rule:

```
{
  "rule":{
   "sql":"SELECT *, timestamp() as timestamp FROM 'iot/test'",
   "ruleDisabled":false,
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
}
```

For more information, see the [Amazon ES Developer Guide](http://alpha-docs-aws.amazon.com/elasticsearch-service/latest/developerguide/)\.

------

## Firehose Action<a name="firehose-rule"></a>

------
#### [ Firehose Action ]

A `firehose` action sends data from an MQTT message that triggered the rule to a Kinesis Firehose stream\. 

------
#### [ more info \(6\) ]

When creating a rule with a `firehose` action, you must specify the following information:

deliveryStreamName  
The Kinesis Firehose stream to which to write the message data\.

roleArn  
The IAM role that allows access to Kinesis Firehose\.

separator  
A character separator that is used to separate records written to the Firehose stream\. Valid values are: '\\n' \(newline\), '\\t' \(tab\), '\\r\\n' \(Windows newline\), ',' \(comma\)\.

**Note**  
Make sure the role associated with the rule has a policy that grants the `firehose:PutRecord` permission\.

The following JSON example shows how to create an AWS IoT rule with a `firehose` action:

```
{
    "rule": {
        "sql": "SELECT * FROM 'some/topic'", 
        "ruleDisabled": false, 
        "actions": [{
            "firehose": {
                "roleArn": "arn:aws:iam::123456789012:role/aws_iot_firehose", 
                "deliveryStreamName": "my_firehose_stream"
            }
        }] 
    }
}
```

For more information, see the [Kinesis Firehose Developer Guide](http://alpha-docs-aws.amazon.com/firehose/latest/dev/)\.

------

## IoT Analytics Action<a name="iotanalytics-rule"></a>

------
#### [ IoT Analytics Action ]

An `iotAnalytics` action sends data from the MQTT message that triggered the rule to an channel\. 

------
#### [ more info \(7\) ]

When creating a rule with an `iotAnalytics` action, you must specify the following information:

channelName  
The name of the channel to which to write the data\.

roleArn  
The IAM role that allows access to the channel\.

The policy attached to the role you specify should look like this:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iotanalytics:BatchPutMessage",
            "Resource": [
                "arn:aws:iotanalytics:us-west-2:<your-account-number>:channel/mychannel"
            ]
        }
    ]
}
```

and have a trust relationship that looks like this:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "iot.amazonaws.com"
            },
            "Action": "sts:AssumeRole",
        }
    ]
}
```

The following JSON example shows how to create an AWS IoT rule with an `iotAnalytics` action:

```
{
    "rule": {
        "sql": "SELECT * FROM 'some/topic'", 
        "ruleDisabled": false, 
        "awsIotSqlVersion": "2016-03-23",
        "actions": [{
            "iotAnalytics": {
                "channelName": "mychannel", 
                "roleArn": "arn:aws:iam::123456789012:role/analyticsRole", 
            }
        }]
    }
}
```

For more information, see the [ User Guide](http://alpha-docs-aws.amazon.com/iotanalytics/latest/userguide/index.html)\.

------

## Kinesis Action<a name="kinesis-rule"></a>

------
#### [ Kinesis Action ]

The `kinesis` action allows you to write data from MQTT messages into a Kinesis stream\. 

------
#### [ more info \(8\) ]

When creating an AWS IoT rule with a `kinesis` action, you must specify the following information:

stream  
The Kinesis stream to which to write data\.

partitionKey  
The partition key used to determine to which shard the data is written\. The partition key is usually composed of an expression \(for example, "$\{topic\(\)\}" or "$\{timestamp\(\)\}"\)\.

**Note**  
Ensure that the policy associated with the rule has the `kinesis:PutRecord` permission\.

The following JSON example shows how to define a `kinesis` action in an AWS IoT rule:

```
{
    "rule": {
        "sql": "SELECT * FROM 'some/topic'", 
        "ruleDisabled": false, 
        "actions": [{
            "kinesis": {
                "roleArn": "arn:aws:iam::123456789012:role/aws_iot_kinesis", 
                "streamName": "my_kinesis_stream", 
                "partitionKey": "${topic()}"
            }
        }], 
    }
}
```

For more information, see the [Kinesis Developer Guide](http://alpha-docs-aws.amazon.com/streams/latest/dev/introduction.html)\.

------

## Lambda Action<a name="lambda-rule"></a>

------
#### [ Lambda Action ]

 A `lambda` action calls a Lambda function, passing in the MQTT message that triggered the rule\. 

------
#### [ more info \(9\) ]

In order for AWS IoT to call a Lambda function, you must configure a policy granting the `lambda:InvokeFunction` permission to AWS IoT\. Lambda functions use resource\-based policies, so you must attach the policy to the Lambda function itself\. Use the following CLI command to attach a policy granting `lambda:InvokeFunction` permission: 

```
aws lambda add-permission --function-name "function_name" --region "region" --principal iot.amazonaws.com --source-arn arn:aws:iot:us-east-2:account_id:rule/rule_name --source-account "account_id" --statement-id "unique_id" --action "lambda:InvokeFunction"
```

The following are the arguments for the `add-permission` command:

\-\-function\-name   
Name of the Lambda function whose resource policy you are updating by adding a new permission\.

\-\-region  
The AWS region of your account\.

\-\-principal  
The principal who is getting the permission\. This should be `iot.amazonaws.com` to allow AWS IoT permission to call a Lambda function\.

\-\-source\-arn  
The ARN of the rule\. You can use the `get-topic-rule` CLI command to get the ARN of a rule\.

\-\-source\-account  
The AWS account where the rule is defined\.

\-\-statement\-id  
A unique statement identifier\.

\-\-action  
The Lambda action you want to allow in this statement\. In this case, we want to allow AWS IoT to invoke a Lambda function, so we specify `lambda:InvokeFunction`\.

**Note**  
If you add a permission for an AWS IoT principal without providing the source ARN, any AWS account that creates a rule with your Lambda action can trigger rules to invoke your Lambda function from AWS IoT

For more information, see [Lambda Permission Model](http://alpha-docs-aws.amazon.com/lambda/latest/dg/intro-permission-model.html)\.

When creating a rule with a `lambda` action, you must specify the Lambda function to invoke when the rule is triggered\. 

The following JSON example shows a rule that calls a Lambda function:

```
{
    "rule": {
        "sql": "SELECT * FROM 'some/topic'", 
        "ruleDisabled": false, 
        "actions": [{
            "lambda": {
                "functionArn": "arn:aws:lambda:us-east-2:123456789012:function:myLambdaFunction"
             }
        }]
    }
}
```

For more information, see the [AWS Lambda Developer Guide](http://alpha-docs-aws.amazon.com/lambda/latest/dg/)\.

------

## Republish Action<a name="republish-rule"></a>

------
#### [ Republish Action ]

The `republish` action allows you to republish the message that triggered the role to another MQTT topic\. 

------
#### [ more info \(10\) ]

When creating a rule with a `republish` action, you must specify the following information:

topic  
The MQTT topic to which to republish the message\. If you are republishing to a reserved topic, one that begins with `$` use `$$` instead\. For example if you are republishing to a device shadow topic like `$aws/things/MyThing/shadow/update`, specify the topic as `$$aws/things/MyThing/shadow/update`\.

roleArn  
The IAM role that allows publishing to the MQTT topic\.

**Note**  
Make sure the role associated with the rule has a policy granting the `iot:Publish` permission\.

```
{
    "rule": {
        "sql": "SELECT * FROM 'some/topic'", 
        "ruleDisabled": false, 
        "actions": [{
            "republish": {
                "topic": "another/topic", 
                "roleArn": "arn:aws:iam::123456789012:role/aws_iot_republish"
            }
        }]
    }
}
```

------

## S3 Action<a name="s3-rule"></a>

------
#### [ S3 Action ]

An `s3` action writes the data from the MQTT message that triggered the rule to an Amazon S3 bucket\. 

------
#### [ more info \(11\) ]

When creating an AWS IoT rule with an `s3` action, you must specify the following information \(except for `cannedacl`, which is optional\):

bucket  
The Amazon S3 bucket to which to write data\.

cannedacl  
The Amazon S3 canned ACL that controls access to the object identified by the object key\. For more information, see [S3 Canned ACLs](http://alpha-docs-aws.amazon.com/AmazonS3/latest/dev/acl-overview.html#canned-acl) \(which lists the allowed values\)\. Note that `cannedacl` is optional\.

key  
The path to the file where the data is written\. For example, if the value of this argument is "$\{topic\(\)\}/$\{timestamp\(\)\}", the topic the message was sent to is "this/is/my/topic,", and the current timestamp is 1460685389, the data is written to a file called "1460685389" in the "this/is/my/topic" folder on Amazon S3\.  
Using a static key results in a single file in Amazon S3 being overwritten for each invocation of the rule\. More common use cases are to use the message timestamp or another unique message identifier, so that a new file is saved in Amazon S3 for each message received\.

roleArn  
The IAM role that allows access to the Amazon S3 bucket\.

**Note**  
Make sure the role associated with the rule has a policy granting the `s3:PutObject` permission\.

The following JSON example shows how to define an `s3` action in an AWS IoT rule:

```
{
    "rule": {
        "sql": "SELECT * FROM 'some/topic'", 
        "ruleDisabled": false, 
        "actions": [{
            "s3": {
                "roleArn": "arn:aws:iam::123456789012:role/aws_iot_s3", 
                "bucketName": "my-bucket", 
                "key": "${topic()}/${timestamp()}"
                "cannedacl": "public-read"
            }
        }]
    }
}
```

For more information, see the [Amazon S3 Developer Guide](http://alpha-docs-aws.amazon.com/AmazonS3/latest/dev/)\. 

------

## Salesforce Action<a name="salesforce-rule"></a>

------
#### [ Salesforce Action ]

A `salesforce` action sends data from the MQTT message that triggered the rule to a Salesforce IoT Input Stream\. 

------
#### [ more info \(12\) ]

When creating a rule with a `salesforce` action, you must specify the following information:

url  
The URL exposed by the Salesforce IoT Input Stream\. The URL is available from the Salesforce IoT Platform when you create an Input Stream\. Refer to the Salesforce IoT documentation to learn more\.

token  
The token used to authenticate access to the specified Salesforce IoT Input Stream\. The token is available from the Salesforce IoT Platform when you create an Input Stream\. Refer to the Salesforce IoT documentation to learn more\.

**Note**  
These parameters do not support substitution\.

The following JSON example shows how to create an AWS IoT rule with a `salesforce` action:

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

For more information, refer to the Salesforce IoT documentation\.

------

## SNS Action<a name="sns-rule"></a>

------
#### [ SNS Action ]

A `sns` action sends the data from the MQTT message that triggered the rule as an SNS push notification\. 

------
#### [ more info \(13\) ]

When creating a rule with an `sns` action, you must specify the following information:

messageFormat  
The message format\. Accepted values are "JSON" and "RAW\." The default value of the attribute is "RAW\." SNS uses this setting to determine if the payload should be parsed and relevant platform\-specific parts of the payload should be extracted\.

roleArn  
The IAM role that allows access to SNS\.

targetArn  
The SNS topic or individual device to which the push notification is sent\.

**Note**  
Make sure the policy associated with the rule has the `sns:Publish` permission\.

The following JSON example shows how to define an `sns` action in an AWS IoT rule:

```
{
    "rule": {
        "sql": "SELECT * FROM 'some/topic'", 
        "ruleDisabled": false, 
        "actions": [{
            "sns": {
                "targetArn": "arn:aws:sns:us-east-2:123456789012:my_sns_topic", 
                "roleArn": "arn:aws:iam::123456789012:role/aws_iot_sns"
            }
        }]
    }
}
```

For more information, see the [Amazon SNS Developer Guide](http://alpha-docs-aws.amazon.com/sns/latest/dg/)\. 

------

## SQS Action<a name="sqs-rule"></a>

------
#### [ SQS Action ]

A `sqs` action sends data from the MQTT message that triggered the rule to an SQS queue\. 

------
#### [ more info \(14\) ]

When creating a rule with an `sqs` action, you must specify the following information:

queueUrl  
The URL of the SQS queue to which to write the data\.

useBase64  
Set to `true` if you want the MQTT message data to be Base64\-encoded before writing to the SQS queue\. Otherwise, set to `false`\.

roleArn  
The IAM role that allows access to the SQS queue\.

**Note**  
Make sure the role associated with the rule has a policy granting the `sqs:SendMessage` permission\.

The following JSON example shows how to create an AWS IoT rule with an `sqs` action:

```
{
    "rule": {
        "sql": "SELECT * FROM 'some/topic'", 
        "ruleDisabled": false, 
        "actions": [{
            "sqs": {
                "queueUrl": "https://sqs.us-east-2.amazonaws.com/123456789012/my_sqs_queue", 
                "roleArn": "arn:aws:iam::123456789012:role/aws_iot_sqs", 
                "useBase64": false
            }
        }]
    }
}
```

For more information, see the [Amazon SQS Developer Guide](http://alpha-docs-aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/)\.

------

## Step Functions Action<a name="stepfunctions-rule"></a>

------
#### [ Step Functions Action ]

A `stepFunctions` action starts execution of a Step Functions state machine\. 

------
#### [ more info \(15\) ]

When creating a rule with a `stepFunctions` action, you must specify the following information:

executionNamePrefix  
\(Optional\) A name will be given to the state machine execution consisting of this prefix followed by a UUID\. Step Functions automatically creates a unique name for each state machine execution if one is not provided\.

stateMachineName  
The name of the Step Functions state machine whose execution will be started\.

roleArn  
The ARN of the role that grants IoT permission to start execution of a state machine \("Action":"states:StartExecution"\)\.  
Here is an example trust policy that should be attached to the role:  

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Action": "states:StartExecution",
        "Resource": [
            *
        ]
    }
}
```

The following JSON example shows how to create an AWS IoT rule with a `stepFunctions` action:

```
{
    "sql": "expression",
    "ruleDisabled": false,
    "description": "A step functions test rule",
    "awsIotSqlVersion": "2016-03-23",
    "actions": [{
        "stepFunctions": {
            "executionNamePrefix": "myExecution",
            "stateMachineName": "myStateMachine",
            "roleArn": "arn:aws:iam::123456789012:role/aws_iot_step_functions"
        }
    }]
}
```

For more information, see the [ Step Functions Developer Guide](http://alpha-docs-aws.amazon.com/step-functions/latest/dg/)\.

------

## AWS IoT Events Action<a name="iotevents-rule"></a>

------
#### [ AWS IoT Events Action ]

An `iotEvents` action sends data from the MQTT message that triggered the rule to an AWS IoT Events input\. 

------
#### [ more info \(16\) ]

When creating a rule with a `iotEvents` action, you must specify the following information:

inputName  
The name of the AWS IoT Events input\.

messageId  
\(Optional\) Use this to ensure that only one input \(message\) with a given messageId will be processed by an AWS IoT Events detector\.

roleArn  
The ARN of the role that grants AWS IoT permission to send an input to an AWS IoT Events detector\. \("Action":"iotevents:BatchPutMessage"\)\.  
Here is an example trust policy that should be attached to the role:  

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Action": "iotevents:BatchPutMessage",
        "Resource": [
            *
        ]
    }
}
```

The following JSON example shows how to create an AWS IoT rule with a `iotEvents` action:

```
{
    "sql": "expression",
    "ruleDisabled": false,
    "description": "An AWS IoT Events test rule",
    "awsIotSqlVersion": "2016-03-23",
    "actions": [{
        "iotEvents": {
          "inputName": "MyIoTEventsInput",
          "messageId": "1234567890",
          "roleArn": "arn:aws:iam::123456789012:role/aws_iot_events"
        },
    }]
}
```

------