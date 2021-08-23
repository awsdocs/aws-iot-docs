# Error handling \(error action\)<a name="rule-error-handling"></a>

When AWS IoT receives a message from a device, the rules engine checks to see if the message matches a rule\. If so, the rule's query statement is evaluated and the rule's actions are triggered, passing the query statement's result\. 

If a problem occurs when triggering an action, the rules engine triggers an error action, if one is specified for the rule\. This might happen when:
+ A rule doesn't have permission to access an Amazon S3 bucket\.
+ A user error causes DynamoDB provisioned throughput to be exceeded\.

## Error action message format<a name="rule-error-message-format"></a>

A single message is generated per rule and message\. For example, if two rule actions in the same rule fail, the error action receives one message that contains both errors\.

The error action message looks like the following example\.

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
The original message payload Base64\-encoded\.

failures    
failedAction  
The name of the action that failed to complete \(for example, "S3Action"\)\.  
failedResource  
The name of the resource \(for example, the name of an S3 bucket\)\.  
errorMessage  
The description and explanation of the error\.

## Error action example<a name="rule-error-example"></a>

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
    "errorAction" : { 
        "s3" : {
            "roleArn": "arn:aws:iam::123456789012:role/aws_iot_s3",
            "bucketName" : "message-processing-errors",
            "key" : "${replace(topic(), '/', '-') + '-' + timestamp() + '-' + newuuid()}"
        }
    }
}
```

You can use any function or substitution in an error action's SQL statement, except for external functions \(for example, `get_thing_shadow`, `aws_lambda`, and `machinelearning_predict`\.\)

For more information about rules and how to specify an error action, see [Creating an AWS IoT Rule](https://docs.aws.amazon.com/iot/latest/developerguide/iot-create-rule.html)\.

For more information about using CloudWatch to monitor the success or failure of rules, see [AWS IoT metrics and dimensions](metrics_dimensions.md)\.