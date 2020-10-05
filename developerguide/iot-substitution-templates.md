# Substitution templates<a name="iot-substitution-templates"></a>

You can use a substitution template to augment the JSON data returned when a rule is triggered and AWS IoT performs an action\. The syntax for a substitution template is `${`*expression*`}`, where *expression* can be any expression supported by AWS IoT in SELECT clauses, WHERE clauses, and [AWS IoT rule actions](iot-rule-actions.md)\. This expression can be plugged into an action field on a rule, allowing you to dynamically configure an action\. In effect, this feature substitutes a piece of information in an action\. This includes functions, operators, and information present in the original message payload\.

**Important**  
Because an expression in a substitution template is evaluated separately from the "SELECT \.\.\." statement, you cannot reference an alias created using the AS clause\. You can reference only information present in the original payload, [functions](iot-sql-functions.md), and [operators](iot-sql-operators.md)\.

For more information about supported expressions, see [AWS IoT SQL reference](iot-sql-reference.md)\.

The following rule actions support substitution templates\. Each action supports different fields that can be substituted\.
+ [CloudWatch alarms](cloudwatch-alarms-rule-action.md)
+ [CloudWatch Logs](cloudwatch-logs-rule-action.md)
+ [CloudWatch metrics](cloudwatch-metrics-rule-action.md)
+ [DynamoDB](dynamodb-rule-action.md)
+ [DynamoDBv2](dynamodb-v2-rule-action.md)
+ [Elasticsearch](elasticsearch-rule-action.md)
+ [HTTPS](https-rule-action.md)
+ [IoT Analytics](iotanalytics-rule-action.md)
+ [IoT Events](iotevents-rule-action.md)
+ [IoT SiteWise](iotsitewise-rule-action.md)
+ [Kinesis Data Streams](kinesis-rule-action.md)
+ [Kinesis Data Firehose](kinesis-firehose-rule-action.md)
+ [Lambda](lambda-rule-action.md)
+ [Republish](republish-rule-action.md)
+ [S3](s3-rule-action.md)
+ [SNS](sns-rule-action.md)
+ [SQS](sqs-rule-action.md)
+ [Step Functions](stepfunctions-rule-action.md)
+ [Timestream](timestream-rule-action.md)

Substitution templates appear in the action parameters within a rule: 

```
{
    "sql": "SELECT *, timestamp() AS timestamp FROM 'my/iot/topic'",
    "ruleDisabled": false,
    "actions": [{
        "republish": {
            "topic": "${topic()}/republish",
            "roleArn": "arn:aws:iam::123456789012:role/my-iot-role"
        }
    }]
}
```

If this rule is triggered by the following JSON published to `my/iot/topic`:

```
{
    "deviceid": "iot123",
    "temp": 54.98,
    "humidity": 32.43,
    "coords": {
        "latitude": 47.615694,
        "longitude": -122.3359976
    }
}
```

Then this rule publishes the following JSON to `my/iot/topic/republish`, which AWS IoT substitutes from `${topic()}/republish`:

```
{
    "deviceid": "iot123",
    "temp": 54.98,
    "humidity": 32.43,
    "coords": {
        "latitude": 47.615694,
        "longitude": -122.3359976
    },
    "timestamp": 1579637878451
}
```