# Creating AWS IoT rules to route device data to other services<a name="iot-rules-tutorial"></a>

These tutorials show you how to create and test AWS IoT rules using some of the more common rule actions\.

AWS IoT rules send data from your devices to other AWS services\. They listen for specific MQTT messages, format the data in the message payloads, and send the result to other AWS services\.

We recommend that you try these in the order they are shown here, even if your goal is to create a rule that uses a Lambda function or something more complex\. The tutorials are presented in order from basic to complex\. They present new concepts incrementally to help you learn the concepts you can use to create the rule actions that don't have a specific tutorial\.

**Note**  
AWS IoT rules help you send the data from your IoT devices to other AWS services\. To do that successfully, however, you need a working knowledge of the other services where you want to send data\. While these tutorials provide the necessary information to complete the tasks, you might find it helpful to learn more about the services you want to send data to before you use them in your solution\. A detailed explanation of the other AWS services is outside of the scope of these tutorials\.

**Tutorial scenario overview**  
The scenario for these tutorials is that of a weather sensor device that periodically publishes its data\. There are many such sensor devices in this imaginary system\. The tutorials in this section, however, focus on a single device while showing how you might accommodate multiple sensors\.

The tutorials in this section show you how to use AWS IoT rules to do the following tasks with this imaginary system of weather sensor devices\.
+ 

**[Tutorial: Republishing an MQTT message](iot-repub-rule.md)**  
This tutorial shows how to republish an MQTT message received from the weather sensors as a message that contains only the sensor ID and the temperature value\. It uses only AWS IoT Core services and demonstrates a simple SQL query and how to use the MQTT client to test your rule\.
+ 

**[Tutorial: Sending an Amazon SNS notification](iot-sns-rule.md)**  
This tutorial shows how to send an SNS message when a value from a weather sensor device exceeds a specific value\. It builds on the concepts presented in the previous tutorial and adds how to work with another AWS service, the [Amazon Simple Notification Service](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) \(Amazon SNS\)\.

  If you're new to Amazon SNS, review its [Getting started](https://docs.aws.amazon.com/sns/latest/dg/sns-getting-started.html) exercises before you start this tutorial\. 
+ 

**[Tutorial: Storing device data in a DynamoDB table](iot-ddb-rule.md)**  
This tutorial shows how to store the data from the weather sensor devices in a database table\. It uses the rule query statement and substitution templates to format the message data for the destination service, [Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)\.

  If you're new to DynamoDB, review its [Getting started](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStartedDynamoDB.html) exercises before you start this tutorial\.
+ 

**[Tutorial: Formatting a notification by using an AWS Lambda function](iot-lambda-rule.md)**  
This tutorial shows how to call a Lambda function to reformat the device data and then send it as a text message\. It adds a Python script and AWS SDK functions in an [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) function to format with the message payload data from the weather sensor devices and send a text message\.

  If you're new to Lambda, review its [Getting started](https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html) exercises before you start this tutorial\.

**AWS IoT rule overview**  
All of these tutorials create AWS IoT rules\. 

For an AWS IoT rule to send the data from a device to another AWS service, it uses: 


+ A rule query statement that consists of:
  + A SQL SELECT clause that selects and formats the data from the message payload
  + A topic filter \(the FROM object in the rule query statement\) that identifies the messages to use
  + An optional conditional statement \(a SQL WHERE clause\) that specifies specific conditions on which to act
+ At least one rule action

Devices publish messages to MQTT topics\. The topic filter in the SQL SELECT statement identifies the MQTT topics to apply the rule to\. The fields specified in the SQL SELECT statement format the data from the incoming MQTT message payload for use by the rule's actions\. For a complete list of rule actions, see [AWS IoT Rule Actions](iot-rule-actions.md)\.

**Topics**
+ [Tutorial: Republishing an MQTT message](iot-repub-rule.md)
+ [Tutorial: Sending an Amazon SNS notification](iot-sns-rule.md)
+ [Tutorial: Storing device data in a DynamoDB table](iot-ddb-rule.md)
+ [Tutorial: Formatting a notification by using an AWS Lambda function](iot-lambda-rule.md)