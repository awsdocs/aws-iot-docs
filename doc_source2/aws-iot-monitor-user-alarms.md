# Alarms<a name="aws-iot-monitor-user-alarms"></a>

This section explains how Fleet Hub for AWS IoT Device Management alarms work and walks you through the steps required to create an alarm\.

When you create a Fleet Hub alarm, it applies to all of the devices currently showing in your dashboard\. If you apply no query, the alarm applies to all devices in your fleet\. For information about using your dashboard and creating queries, see [Queries and filters](aws-iot-monitor-user-queries.md)\.

Alarms use Amazon CloudWatch \(CloudWatch\) metrics in combination with searchable fields from the AWS IoT fleet indexing service to create CloudWatch alarms\. For example, you can create an alarm that generates an Amazon Simple Notification Service \(Amazon SNS\) message whenever the average battery level of the devices in your fleet falls below 50%\. 

Fleet Hub alarms use the [GetStatistics](https://docs.aws.amazon.com/iot/latest/developerguide/index-aggregate.html#get-statistics) and [GetPercentiles](https://docs.aws.amazon.com/iot/latest/developerguide/index-aggregate.html#get-percentiles) capabilities of the fleet indexing service to query aggregate data\. For example, when you create an alarm that tracks a custom numerical field, you can create alarming thresholds that apply to the following values of the specified attribute\.
+ Maximum
+ Count
+ Sum
+ Minimum
+ Average
+ Values in the 10th, 50th, 90th, 95th, or 99th percentile

For more information about querying aggregate data in the fleet indexing service, see [Querying for aggregate data](https://docs.aws.amazon.com/iot/latest/developerguide/index-aggregate.html)\.

The following table lists some examples of the aggregation types that are available for AWS\-managed and custom fields\.


| Field | Aggregation type | 
| --- | --- | 
| Thing type \(AWS\-managed string field\) | Count | 
| Thing group \(AWS\-managed string field\) | Count | 
| **Connected** \(AWS\-managed Boolean field\) The value of `true` is 1\. The value of `false` is 0\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/fleethubuserguide/aws-iot-monitor-user-alarms.html)  | 
| shadow\.reported\.batterylevel \(numerical aggregation field created in the fleet indexing service\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/fleethubuserguide/aws-iot-monitor-user-alarms.html)  | 

In addition to specifying aggregation fields and types, you also specify the following values\.
+ The duration of time \(1 minute or 5 minutes\) required for your specified alarming threshold to trigger the alarm\.
+ One of the following comparison operators to apply to your specified aggregation field and type\.
  + Greater
  + Greater/Equal
  + Lower
  + Lower/Equal
+ The value to use with your specified comparison operator\.
+ A list of email addresses of people in your organization who receive Amazon SNS messages whenever your alarm is triggered\.
+ An alarm name\.

To create a Fleet Hub alarm, see [Creating alarms](aws-iot-monitor-user-alarms-create.md)\.