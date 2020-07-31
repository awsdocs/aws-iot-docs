# Salesforce IoT<a name="salesforce-iot-rule-action"></a>

The Salesforce IoT \(`salesforce`\) action sends data from the MQTT message that triggered the rule to a Salesforce IoT input stream\. 

## Parameters<a name="salesforce-iot-rule-action-parameters"></a>

When you create an AWS IoT rule with this action, you must specify the following information:

`url`  
The URL exposed by the Salesforce IoT input stream\. The URL is available from the Salesforce IoT platform when you create an input stream\. For more information, see the Salesforce IoT documentation\.  
Supports substitution templates: No

`token`  
The token used to authenticate access to the specified Salesforce IoT input stream\. The token is available from the Salesforce IoT platform when you create an input stream\. For more information, see the Salesforce IoT documentation\.  
Supports substitution templates: No

## Examples<a name="salesforce-iot-rule-action-examples"></a>

The following JSON example defines a Salesforce IoT action in an AWS IoT rule\.

```
{
    "topicRulePayload": {
        "sql": "SELECT * FROM 'some/topic'",
        "ruleDisabled": false,
        "awsIotSqlVersion": "2016-03-23",
        "actions": [
            {
                "salesforce": {
                    "token": "ABCDEFGHI123456789abcdefghi123456789",
                    "url": "https://ingestion-cluster-id.my-env.sfdcnow.com/streams/stream-id/connection-id/my-event"
                }
            }
        ]
    }
}
```