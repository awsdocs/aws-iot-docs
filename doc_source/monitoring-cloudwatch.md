# Monitor AWS IoT alarms and metrics using Amazon CloudWatch<a name="monitoring-cloudwatch"></a>

You can monitor AWS IoT using CloudWatch, which collects and processes raw data from AWS IoT into readable, near real\-time metrics\. These statistics are recorded for a period of two weeks, so that you can access historical information and gain a better perspective on how your web application or service is performing\. By default, AWS IoT metric data is sent automatically to CloudWatch in one minute intervals\. For more information, see [What Are Amazon CloudWatch, Amazon CloudWatch Events, and Amazon CloudWatch Logs?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatch.html) in the *Amazon CloudWatch User Guide*\.

## Using AWS IoT metrics<a name="how_to_use_metrics"></a>

The metrics reported by AWS IoT provide information that you can analyze in different ways\. The following use cases are based on a scenario where you have ten things that connect to the internet once a day\. Each day:
+ Ten things connect to AWS IoT at roughly the same time\.
+ Each thing subscribes to a topic filter, and then waits for an hour before disconnecting\. During this period, things communicate with one another and learn more about the state of the world\.
+ Each thing publishes some perception it has based on its newly found data using `UpdateThingShadow`\.
+ Each thing disconnects from AWS IoT\.

To help you get started, these topics explore some of the questions that you might have\.
+ [How can I be notified if my things do not connect successfully each day?](creating_alarms.md#how_to_detect_connection_failures)
+ [How can I be notified if my things are not publishing data each day?](creating_alarms.md#how_to_detect_publish_failures)
+ [How can I be notified if my thing's shadow updates are being rejected each day?](creating_alarms.md#detect_rejected_updates)
+ [How can I create a CloudWatch alarm for Jobs?](creating_alarms.md#cw-jobs-alarms)

**Topics**
+ [Using AWS IoT metrics](#how_to_use_metrics)
+ [Creating CloudWatch alarms to monitor AWS IoT](creating_alarms.md)
+ [AWS IoT metrics and dimensions](metrics_dimensions.md)