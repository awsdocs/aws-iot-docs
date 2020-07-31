# Elasticsearch<a name="elasticsearch-rule-action"></a>

The Elasticsearch \(`elasticsearch`\) action writes data from MQTT messages to an Amazon Elasticsearch Service domain\. You can then use tools like Kibana to query and visualize data in Elasticsearch\.

## Requirements<a name="elasticsearch-rule-action-requirements"></a>

This rule action has the following requirements:
+ An IAM role that AWS IoT can assume to perform the `es:ESHttpPut` operation\. For more information, see [Granting AWS IoT the required access](iot-create-role.md)\.

  In the AWS IoT console, you can choose or create a role to allow AWS IoT to perform this rule action\.
+ If you use a customer managed AWS Key Management Service \(AWS KMS\) customer master key \(CMK\) to encrypt data at rest in Elasticsearch, the service must have permission to use the CMK on the caller's behalf\. For more information, see [Encryption of data at rest for Amazon Elasticsearch Service](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/encryption-at-rest.html) in the *Amazon Elasticsearch Service Developer Guide*\.

## Parameters<a name="elasticsearch-rule-action-parameters"></a>

When you create an AWS IoT rule with this action, you must specify the following information:

`endpoint`  
The endpoint of your Amazon Elasticsearch Service domain\.  
Supports substitution templates: API and AWS CLI only

`index`  
The Elasticsearch index where you want to store your data\.  
Supports substitution templates: Yes

`type`  
The type of document you are storing\.  
Supports substitution templates: Yes

`id`  
The unique identifier for each document\.  
Supports substitution templates: Yes

`roleARN`  
The IAM role that allows access to the Elasticsearch domain\. For more information, see [Requirements](#elasticsearch-rule-action-requirements)\.  
Supports substitution templates: No

## Examples<a name="elasticsearch-rule-action-examples"></a>

The following JSON example defines an Elasticsearch action in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT *, timestamp() as timestamp FROM 'iot/test'",
        "ruleDisabled": false,
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "elasticsearch": {
                    "endpoint": "https://my-endpoint",
                    "index": "my-index",
                    "type": "my-type",
                    "id": "${newuuid()}",
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_es"
                }
            }
        ]
    }
}
```

The following JSON example defines an Elasticsearch action with substitution templates in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic'",
        "ruleDisabled": false,
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "elasticsearch": {
                    "endpoint": "https://my-endpoint",
                    "index": "${topic()}",
                    "type": "${type}",
                    "id": "${newuuid()}",
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_es"
                }
            }
        ]
    }
}
```

## See also<a name="elasticsearch-rule-action-see-also"></a>
+ [What is Amazon Elasticsearch Service?](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/) in the *Amazon Elasticsearch Service Developer Guide*