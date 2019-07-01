# AWS IoT Rules Tutorials<a name="iot-rules-tutorial"></a>

The following tutorials show you how to create and test AWS IoT rules\. Before you begin, be sure to complete the [AWS IoT Getting Started Tutorial](iot-gs.md)\. It shows you how to create an AWS account and register a device in AWS IoT, which are prerequisites for these tutorials\.

The scenario in this tutorial is a greenhouse with rows of plants\. Each plant has a moisture sensor\. At a predetermined interval, the moisture sensor sends its data to AWS IoT\. The AWS IoT rules engine receives this data and writes it to a DynamoDB table\. You create a rule to write data to DynamoDB and emulate the sensors using the AWS IoT MQTT client\.

An AWS IoT rule consists of an SQL SELECT statement, a topic filter, and a rule action\. Devices send information to AWS IoT by publishing messages to MQTT topics\. The SQL SELECT statement allows you to extract data from an incoming MQTT message\. The topic filter of an AWS IoT rule specifies one or more MQTT topics\. The rule is triggered when an MQTT message is received on a topic that matches the topic filter\. Rule actions allow you to take the information extracted from an MQTT message and send it to another AWS service\. Rule actions are defined for AWS services like Amazon DynamoDB, AWS Lambda, Amazon SNS, and Amazon S3\. By using a Lambda rule, you can call other AWS or third\-party web services\. For a complete list of rule actions, see [AWS IoT Rule Actions](iot-rule-actions.md)\.

In these tutorials, we assume that you're using the AWS IoT MQTT client and that you are using `my/greenhouse` as the topic filter in the rules\.

You can also use your own device, but you must know on which MQTT topic your device publishes so you can specify it as the topic filter in the rule\. For more information, see [AWS IoT Rules](iot-rules.md)\.