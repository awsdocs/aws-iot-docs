# Basic Ingest<a name="iot-basic-ingest"></a>

Basic Ingest enables you to securely send device data to the AWS services supported by [AWS IoT Rule Actions](iot-rule-actions.md) without incurring [messaging costs](https://aws.amazon.com/iot-core/pricing/)\. Basic Ingest optimizes data flow by removing the publish/subscribe message broker from the ingestion path, so it's more cost effective while keeping the security and data processing features of AWS IoT\.

To use Basic Ingest, you send messages from your devices or applications with topic names that start with `$aws/rules/rule-name` as their first three levels, where `rule-name` is the name of your AWS IoT Rule to trigger\.

You can continue to use an existing rule with Basic Ingest by just adding the Basic Ingest prefix \(`$aws/rules/rule-name`\) to the message topic by which you normally trigger the rule\. For example, if you have a rule named "BuildingManager" that is triggered by messages with topics like "Buildings/Building5/Floor2/Room201/Lights" \(`"sql": "SELECT * FROM 'Buildings/#'"`\), you can trigger the same rule with Basic Ingest by sending a message with topic `$aws/rules/BuildingManager/Buildings/Building5/Floor2/Room201/Lights`\.

Be aware that:

+ Your devices and rules cannot subscribe to Basic Ingest reserved topics\. \(See [Reserved Topics](topics.md#reserved-topics) for details\.\)

+ If you need a publish/subscribe broker to distribute messages to multiple subscribers \(for example, to deliver messages to other devices as well as the Rules Engine\), you should continue to use the AWS IoT message broker to handle the message distribution\. Just publish your messages on topics other than Basic Ingest topics\.

## To use Basic Ingest<a name="iot-basic-ingest-use"></a>

Make sure your device or application is using a policy that has publish permissions on `$aws/rules/*`\. Or you can specify permission for individual rules with `$aws/rules/rule-name/*` in the policy\. Otherwise, your devices and applications can continue to use their existing connections with AWS IoT Core\.

When the message reaches the Rules Engine, there is no difference in execution or error handling between rules triggered from Basic Ingest and those triggered through message broker subscriptions\.

You can, of course, create rules for use with Basic Ingest\. Keep in mind the following:

+ The initial prefix of a Basic Ingest topic \(`$aws/rules/rule-name`\) isn't available to the [topic\(Decimal\)](iot-sql-functions.md#iot-function-topic) function\.

+ If you define a rule that is triggered only with Basic Ingest, the `FROM` clause is optional in the `sql` field of the `rule` definition\. It's still required if the rule will also be triggered by other messages that must be sent through the message broker \(for example, because those other messages must be distributed to multiple subscribers\)\. See [AWS IoT SQL Reference](iot-sql-reference.md)\.

+ The first three levels of the Basic Ingest topic \(`$aws/rules/rule-name`\) are not counted toward the eight segment length limit or toward the 256 total character limit for a topic\. Otherwise, the same restrictions apply as documented in [ AWS IoT Limits](http://alpha-docs-aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_iot)\.

+ If a message is received with a Basic Ingest topic that specifies an inactive rule or a rule that doesn't exist, an error log is created in a CloudWatch log to help you with debugging \(See [Rules Engine Logs](cloud-watch-logs.md#rule-engine-logs)\)\. A "RuleNotFound" metric is indicated and you can create alarms on this metric\. See Rule Metrics in [AWS IoT Metrics](metrics_dimensions.md#aws-iot-metrics)\.

+ You can still publish with QoS 1 on Basic Ingest topics\. You will receive a PUBACK once the message is successfully delivered to the Rules Engine\. Receiving a PUBACK does not mean that your rule actions completed successfully\. You can configure an error action to handle errors during action execution\. See [Rule Error Handling](iot-rules.md#rule-error-handling)\.