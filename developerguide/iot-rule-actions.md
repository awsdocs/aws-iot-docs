# AWS IoT rule actions<a name="iot-rule-actions"></a>

AWS IoT rule actions specify what to do when a rule is triggered\. You can define actions to send data to an Amazon DynamoDB database, send data to Amazon Kinesis Data Streams, invoke an AWS Lambda function, and so on\. AWS IoT supports the following actions in AWS Regions where the action's service is available\.


| Rule action | Description | Name in API | 
| --- | --- | --- | 
| [Apache Kafka](apache-kafka-rule-action.md) | Sends a message to an Apache Kafka cluster\. | kafka | 
| [CloudWatch alarms](cloudwatch-alarms-rule-action.md) | Changes the state of an Amazon CloudWatch alarm\. | cloudwatchAlarm | 
| [CloudWatch Logs](cloudwatch-logs-rule-action.md) | Sends a message to Amazon CloudWatch Logs\. | cloudwatchLogs | 
| [CloudWatch metrics](cloudwatch-metrics-rule-action.md) | Sends a message to a CloudWatch metric\. | cloudwatchMetric | 
| [DynamoDB](dynamodb-rule-action.md) | Sends a message to a DynamoDB table\. | dynamoDB | 
| [DynamoDBv2](dynamodb-v2-rule-action.md) | Sends message data to multiple columns in a DynamoDB table\. | dynamoDBv2 | 
| [Elasticsearch](elasticsearch-rule-action.md) | Sends a message to an Amazon Elasticsearch Service endpoint\. | elasticsearch | 
| [HTTPS](https-rule-action.md) | Posts a message to an HTTPS endpoint\. | http | 
| [IoT Analytics](iotanalytics-rule-action.md) | Sends a message to an AWS IoT Analytics channel\. | iotAnalytics | 
| [IoT Events](iotevents-rule-action.md) | Sends a message to an AWS IoT Events input\. | iotEvents | 
| [IoT SiteWise](iotsitewise-rule-action.md) | Sends message data to AWS IoT SiteWise asset properties\. | iotSiteWise | 
| [Kinesis Data Firehose](kinesis-firehose-rule-action.md) | Sends a message to a Kinesis Data Firehose delivery stream\. | firehose | 
| [Kinesis Data Streams](kinesis-rule-action.md) | Sends a message to a Kinesis data stream\. | kinesis | 
| [Lambda](lambda-rule-action.md) | Invokes a Lambda function with message data as input\. | lambda | 
| [Republish](republish-rule-action.md) | Republishes a message to another MQTT topic\. | republish | 
| [S3](s3-rule-action.md) | Stores a message in an Amazon Simple Storage Service \(Amazon S3\) bucket\. | s3 | 
| [Salesforce IoT](salesforce-iot-rule-action.md) | Sends a message to a Salesforce IoT input stream\. | salesforce | 
| [SNS](sns-rule-action.md) | Publishes a message as an Amazon Simple Notification Service \(Amazon SNS\) push notification\. | sns | 
| [SQS](sqs-rule-action.md) | Sends a message to an Amazon Simple Queue Service \(Amazon SQS\) queue\. | sqs | 
| [Step Functions](stepfunctions-rule-action.md) | Starts an AWS Step Functions state machine\. | stepFunctions | 
| [Timestream](timestream-rule-action.md) | Sends a message to an Amazon Timestream database table\. | timestream | 
| [VPC](vpc-rule-action.md) | Sends data to an Amazon Virtual Private Cloud \(Amazon VPC\)\. | VPC | 

**Notes**  
You must define the rule in the same AWS Region as another service's resource, so that the rule action can interact with that resource\.
The AWS IoT rules engine might make multiple attempts to perform an action in case of intermittent errors\. If all attempts fail, the message is discarded and the error is available in your CloudWatch logs\. You can specify an error action for each rule that is invoked after a failure occurs\. For more information, see [Error handling \(error action\)](rule-error-handling.md)\.
Some rule actions trigger actions in services that integrate with AWS Key Management Service \(AWS KMS\) to support data encryption at rest\. If you use a customer\-managed AWS KMS customer master key \(CMK\) to encrypt data at rest, the service must have permission to use the CMK on the caller's behalf\. See the data encryption topics in the appropriate service guide to learn how to manage permissions for your customer\-managed CMK\. For more information about CMKs and customer\-managed CMKs, see [AWS Key Management Service concepts](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html) in the *AWS Key Management Service Developer Guide*\.