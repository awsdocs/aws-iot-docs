# OpenSearch<a name="opensearch-rule-action"></a>

The OpenSearch \(`openSearch`\) action writes data from MQTT messages to an Amazon OpenSearch Service domain\. You can then use tools like OpenSearch Dashboards to query and visualize data in OpenSearch Service\.

## Requirements<a name="opensearch-rule-action-requirements"></a>

This rule action has the following requirements:
+ An IAM role that AWS IoT can assume to perform the `es:ESHttpPut` operation\. For more information, see [Granting AWS IoT the required access](iot-create-role.md)\.

  In the AWS IoT console, you can choose or create a role to allow AWS IoT to perform this rule action\.
+ If you use a customer\-managed AWS KMS key \(KMS key\) to encrypt data at rest in OpenSearch Service, the service must have permission to use the KMS key on the caller's behalf\. For more information, see [Encryption of data at rest for Amazon OpenSearch Service](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/encryption-at-rest.html) in the *Amazon OpenSearch Service Developer Guide*\.

## Parameters<a name="opensearch-rule-action-parameters"></a>

When you create an AWS IoT rule with this action, you must specify the following information:

`endpoint`  
The endpoint of your Amazon OpenSearch Service domain\.  
Supports [substitution templates](iot-substitution-templates.md): API and AWS CLI only

`index`  
The OpenSearch index where you want to store your data\.  
Supports [substitution templates](iot-substitution-templates.md): Yes

`type`  
The type of document you are storing\.  
Supports [substitution templates](iot-substitution-templates.md): Yes

`id`  
The unique identifier for each document\.  
Supports [substitution templates](iot-substitution-templates.md): Yes

`roleARN`  
The IAM role that allows access to the OpenSearch Service domain\. For more information, see [Requirements](#opensearch-rule-action-requirements)\.  
Supports [substitution templates](iot-substitution-templates.md): No

## Examples<a name="opensearch-rule-action-examples"></a>

The following JSON example defines an OpenSearch action in an AWS IoT rule and how you can specify the fields for the `OpenSearch` action\. For more information, see [OpenSearchAction](https://docs.aws.amazon.com/iot/latest/apireference/API_OpenSearchAction.html)\.

```
{
    "topicRulePayload": {
        "sql": "SELECT *, timestamp() as timestamp FROM 'iot/test'",
        "ruleDisabled": false,
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "openSearch": {
                    "endpoint": "https://my-endpoint",
                    "index": "my-index",
                    "type": "my-type",
                    "id": "${newuuid()}",
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_os"
                }
            }
        ]
    }
}
```

The following JSON example defines an OpenSearch action with substitution templates in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic'",
        "ruleDisabled": false,
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "openSearch": {
                    "endpoint": "https://my-endpoint",
                    "index": "${topic()}",
                    "type": "${type}",
                    "id": "${newuuid()}",
                    "roleArn": "arn:aws:iam::123456789012:role/aws_iot_os"
                }
            }
        ]
    }
}
```

## See also<a name="opensearch-rule-action-see-also"></a>

[What is Amazon OpenSearch Service?](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/) in the *Amazon OpenSearch Service Developer Guide*