# Monitoring AWS IoT<a name="monitoring_overview"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of AWS IoT and your AWS solutions\.

We strongly encourage you to collect monitoring data from all parts of your AWS solution to make it easier to debug a multi\-point failure, if one occurs\. Start by creating a monitoring plan that answers the following questions\. If you're not sure how to answer these, you can still continue to [enable logging](configure-logging.md) and establish your performance baselines\.
+ What are your monitoring goals?
+ Which resources will you monitor?
+ How often will you monitor these resources?
+ Which monitoring tools will you use?
+ Who will perform the monitoring tasks?
+ Who should be notified when something goes wrong?

Your next step is to [enable logging](configure-logging.md) and establish a baseline of normal AWS IoT performance in your environment by measuring performance at various times and under different load conditions\. As you monitor AWS IoT, keep historical monitoring data so that you can compare it with current performance data\. This will help you identify normal performance patterns and performance anomalies, and devise methods to address issues\.

To establish your baseline performance for AWS IoT, you should monitor these metrics to start\. You can always monitor more metrics later\.
+ [`PublishIn.Success`](metrics_dimensions.md#message-broker-metrics)
+ [`PublishOut.Success`](metrics_dimensions.md#message-broker-metrics)
+ [`Subscribe.Success`](metrics_dimensions.md#message-broker-metrics)
+ [`Ping.Success`](metrics_dimensions.md#message-broker-metrics)
+ [`Connect.Success`](metrics_dimensions.md#message-broker-metrics)
+ [`GetThingShadow.Accepted`](metrics_dimensions.md#shadow-metrics)
+ [`UpdateThingShadow.Accepted`](metrics_dimensions.md#shadow-metrics)
+ [`DeleteThingShadow.Accepted`](metrics_dimensions.md#shadow-metrics)
+ [`RulesExecuted`](metrics_dimensions.md#iot-metrics)

The topics in this section can help you start logging and monitoring AWS IoT\.

**Topics**
+ [Configure AWS IoT logging](configure-logging.md)
+ [Monitor AWS IoT alarms and metrics using Amazon CloudWatch](monitoring-cloudwatch.md)
+ [Monitor AWS IoT using CloudWatch Logs](cloud-watch-logs.md)
+ [Log AWS IoT API calls using AWS CloudTrail](iot-using-cloudtrail.md)