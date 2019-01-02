# AWS IoT Rules Tutorials<a name="iot-rules-tutorial"></a>

The following tutorials show you how to create and test AWS IoT rules\. Before you go through these tutorials, go through the AWS IoT Getting Started Tutorial first\. It shows you how to create an AWS account and connect your device to AWS IoT, which you need to complete these tutorials\.

An AWS IoT rule consists of an SQL SELECT statement, a topic filter, and a rule action\. Devices send information to AWS IoT by publishing messages to MQTT topics\. The SQL SELECT statement allows you to extract data from an incoming MQTT message\. The topic filter of an AWS IoT rule specifies one or more MQTT topics\. The rule is triggered when an MQTT message is received on a topic that matches the topic filter\. Rule actions allow you to take the information extracted from an MQTT message and send it to another AWS service\. Rule actions are defined for AWS services like Amazon DynamoDB, AWS Lambda, Amazon SNS, and Amazon S3\. By using a Lambda rule, you can call other AWS or third\-party web services\. For a complete list of rule actions, see AWS IoT Rule Actions\.

In these tutorials we assume you're using the AWS IoT button and will use `iotbutton/+` as the topic filter in the rules\. If you don't have an AWS IoT button, [you can buy one here](https://www.amazon.com/All-New-AWS-IoT-Button-Generation/dp/B01KW6YCIM)\. 

Alternatively, you can emulate the AWS IoT button by using an MQTT client like the AWS IoT MQTT client in the [AWS IoT console](https://console.aws.amazon.com/iot/home)\. To emulate the AWS IoT button, publish a similar message on the `iotbutton/ABCDEFG12345` topic\. The number after the slash \(/\) is arbitrary\. It's used as the serial number for the button\.

You can also use your own device, but you must know on which MQTT topic your device publishes so you can specify it as the topic filter in the rule\. For more information, see AWS IoT Rules\.

The AWS IoT button sends a JSON payload that looks like this:

```
{
    "serialNumber" : "ABCDEFG12345",
    "batteryVoltage" : "2000mV",
    "clickType" : "SINGLE"
}
```