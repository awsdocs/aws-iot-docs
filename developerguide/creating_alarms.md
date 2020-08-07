# Creating CloudWatch alarms to monitor AWS IoT<a name="creating_alarms"></a>

You can create a CloudWatch alarm that sends an Amazon SNS message when the alarm changes state\. An alarm watches a single metric over a time period you specify\. When the value of the metric exceeds a given threshold over a number of time periods, one or more actions are performed\. The action can be a notification sent to an Amazon SNS topic or Auto Scaling policy\. Alarms trigger actions for sustained state changes only\. CloudWatch alarms do not trigger actions simply because they are in a particular state; the state must have changed and been maintained for a specified number of periods\.

**Topics**
+ [How can I be notified if my things do not connect successfully each day?](#how_to_detect_connection_failures)
+ [How can I be notified if my things are not publishing data each day?](#how_to_detect_publish_failures)
+ [How can I be notified if my thing's shadow updates are being rejected each day?](#detect_rejected_updates)
+ [How can I create a CloudWatch alarm for jobs?](#cw-jobs-alarms)

 You can see all the metrics that CloudWatch alarms can monitor at [AWS IoT metrics and dimensions](metrics_dimensions.md)\. 

## How can I be notified if my things do not connect successfully each day?<a name="how_to_detect_connection_failures"></a>

1. Create an Amazon SNS topic named `things-not-connecting-successfully`, and record its Amazon Resource Name \(ARN\)\. This procedure will refer to your topic's ARN as `<sns-topic-arn>`\.

   For more information on how to create an Amazon SNS notification, see [Getting Started with Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/sns-getting-started.html)\.

1. Create the alarm\.

   ```
   aws cloudwatch put-metric-alarm \
       --alarm-name ConnectSuccessAlarm \
       --alarm-description "Alarm when my Things don't connect successfully" \
       --namespace AWS/IoT \
       --metric-name Connect.Success \
       --dimensions Name=Protocol,Value=MQTT \
       --statistic Sum \
       --threshold 10 \
       --comparison-operator LessThanThreshold \
       --period 86400 \
       --evaluation-periods 1 \
       --alarm-actions <sns-topic-arn>
   ```

1. Test the alarm\.

   ```
   aws cloudwatch set-alarm-state --alarm-name ConnectSuccessAlarm --state-reason "initializing" --state-value OK
   ```

   ```
   aws cloudwatch set-alarm-state --alarm-name ConnectSuccessAlarm --state-reason "initializing" --state-value ALARM
   ```

1. Verify that the alarm appears in your [CloudWatch console](https://console.aws.amazon.com/cloudwatch)\.

## How can I be notified if my things are not publishing data each day?<a name="how_to_detect_publish_failures"></a>

1. Create an Amazon SNS topic named `things-not-publishing-data`, and record its Amazon Resource Name \(ARN\)\. This procedure will refer to your topic's ARN as `<sns-topic-arn>`\.

   For more information on how to create an Amazon SNS notification, see [Getting Started with Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/sns-getting-started.html)\.

1. Create the alarm\.

   ```
   aws cloudwatch put-metric-alarm \
       --alarm-name PublishInSuccessAlarm\
       --alarm-description "Alarm when my Things don't publish their data \
       --namespace AWS/IoT \
       --metric-name PublishIn.Success \
       --dimensions Name=Protocol,Value=MQTT \
       --statistic Sum \
       --threshold 10 \
       --comparison-operator LessThanThreshold \
       --period 86400 \
       --evaluation-periods 1 \
       --alarm-actions <sns-topic-arn>
   ```

1. Test the alarm\.

   ```
   aws cloudwatch set-alarm-state --alarm-name PublishInSuccessAlarm --state-reason "initializing" --state-value OK
   ```

   ```
   aws cloudwatch set-alarm-state --alarm-name PublishInSuccessAlarm --state-reason "initializing" --state-value ALARM
   ```

1. Verify that the alarm appears in your [CloudWatch console](https://console.aws.amazon.com/cloudwatch)\.

## How can I be notified if my thing's shadow updates are being rejected each day?<a name="detect_rejected_updates"></a>

1. Create an Amazon SNS topic named `things-shadow-updates-rejected`, and record its Amazon Resource Name \(ARN\)\. This procedure will refer to your topic's ARN as `<sns-topic-arn>`\.

   For more information on how to create an Amazon SNS notification, see [Getting Started with Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/sns-getting-started.html)\.

1. Create the alarm\.

   ```
   aws cloudwatch put-metric-alarm \
       --alarm-name UpdateThingShadowSuccessAlarm \
       --alarm-description "Alarm when my Things Shadow updates are getting rejected" \
       --namespace AWS/IoT \
       --metric-name UpdateThingShadow.Success \
       --dimensions Name=Protocol,Value=MQTT \
       --statistic Sum \
       --threshold 10 \
       --comparison-operator LessThanThreshold \
       --period 86400 \
       --unit Count \
       --evaluation-periods 1 \
       --alarm-actions <sns-topic-arn>
   ```

1. Test the alarm\.

   ```
   aws cloudwatch set-alarm-state --alarm-name UpdateThingShadowSuccessAlarm --state-reason "initializing" --state-value OK
   ```

   ```
   aws cloudwatch set-alarm-state --alarm-name UpdateThingShadowSuccessAlarm --state-reason "initializing" --state-value ALARM
   ```

1. Verify that the alarm appears in your [CloudWatch console](https://console.aws.amazon.com/cloudwatch)\.

## How can I create a CloudWatch alarm for jobs?<a name="cw-jobs-alarms"></a>

The Jobs service provides CloudWatch metrics for you to monitor your jobs\. You can create CloudWatch alarms to monitor any [Jobs metrics](metrics_dimensions.md#jobs-metrics)\.

The following command creates a CloudWatch alarm to monitor the total number of failed job executions for Job *SampleOTAJob* and notifies you when more than 20 job executions have failed\. The alarm monitors the Jobs metric `FailedJobExecutionTotalCount` by checking the reported value every 300 seconds\. It is activated when a single reported value is greater than 20, meaning there were more than 20 failed job executions since the job started\. When the alarm goes off, it sends a notification to the provided Amazon SNS topic\.

```
aws cloudwatch put-metric-alarm \
    --alarm-name TotalFailedJobExecution-SampleOTAJob \
    --alarm-description "Alarm when total number of failed job execution exceeds the threshold for SampleOTAJob" \
    --namespace AWS/IoT \
    --metric-name FailedJobExecutionTotalCount \
    --dimensions Name=JobId,Value=SampleOTAJob \
    --statistic Sum \
    --threshold 20 \
    --comparison-operator GreaterThanThreshold \
    --period 300 \
    --unit Count \
    --evaluation-periods 1 \
    --alarm-actions arn:aws:sns:<AWS_REGION>:<AWS_ACCOUNT_ID>:SampleOTAJob-has-too-many-failed-job-ececutions
```

The following command creates a CloudWatch alarm to monitor the number of failed job executions for Job *SampleOTAJob* in a given period\. It then notifies you when more than five job executions have failed during that period\. The alarm monitors the Jobs metric `FailedJobExecutionCount` by checking the reported value every 3600 seconds\. It is activated when a single reported value is greater than 5, meaning there were more than 5 failed job executions in the past hour\. When the alarm goes off, it sends a notification to the provided Amazon SNS topic\.

```
aws cloudwatch put-metric-alarm \
    --alarm-name FailedJobExecution-SampleOTAJob \
    --alarm-description "Alarm when number of failed job execution per hour exceeds the threshold for SampleOTAJob" \
    --namespace AWS/IoT \
    --metric-name FailedJobExecutionCount \
    --dimensions Name=JobId,Value=SampleOTAJob \
    --statistic Sum \
    --threshold 5 \
    --comparison-operator GreaterThanThreshold \
    --period 3600 \
    --unit Count \
    --evaluation-periods 1 \
    --alarm-actions arn:aws:sns:<AWS_REGION>:<AWS_ACCOUNT_ID>:SampleOTAJob-has-too-many-failed-job-ececutions-per-hour
```