# MQTT message payload<a name="topicdata"></a>

 The message payload sent in your MQTT messages is not specified by AWS IoT, unless the message payload is for one of the [Reserved topics](reserved-topics.md)\. Rather, you define the message payload for your topics to best accommodate your application's needs, within the constraints of the [ AWS IoT Core Service Quotas for Protocols](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#iot-protocol-limits)\. 

Using a JSON format for your message payload enables the AWS IoT rules engine to parse your messages and apply SQL queries to it\. If your application doesn't require the Rules engine to apply SQL queries to your message payloads, you can use any data format that your application requires\. For information about limitations and reserved characters in a JSON document used in SQL queries, see [JSON extensions](iot-sql-json.md)\. 

For more information about designing your MQTT topics and their corresponding message payloads, see [Designing MQTT Topics for AWS IoT Core](https://d1.awsstatic.com/whitepapers/Designing_MQTT_Topics_for_AWS_IoT_Core.pdf)\.