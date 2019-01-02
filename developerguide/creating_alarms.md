# Creating CloudWatch Alarms to Monitor AWS IoT<a name="creating_alarms"></a>

You can create a CloudWatch alarm that sends an Amazon SNS message when the alarm changes state\. An alarm watches a single metric over a time period you specify and performs one or more actions based on the value of the metric relative to a given threshold over a number of time periods\. The action is a notification sent to an Amazon SNS topic or Auto Scaling policy\. Alarms trigger actions for sustained state changes only\. CloudWatch alarms do not trigger actions simply because they are in a particular state; the state must have changed and been maintained for a specified number of periods\.

## How can I be notified if my things do not connect successfully each day?<a name="how_to_detect_connection_failures"></a>

1. Create an Amazon SNS topic, arn:aws:sns:us\-east\-1:123456789012:things\-not\-connecting\-successfully\.

   For more information, see [Set Up Amazon Simple Notification Service](Amazon Simple Notification Service Developer GuideUS_SetupSNS.html)\.

1. Create the alarm\.

   ```
   Prompt>aws cloudwatch put-metric-alarm \
       --alarm-name ConnectSuccessAlarm \
       --alarm-description "Alarm when my Things don't connect successfully" \
       --namespace AWS/IoT \
       --metric-name Connect.Success \
       --dimensions Name=Protocol,Value=MQTT \
       --statistic Sum \
       --threshold 10 \
       --comparison-operator LessThanThreshold \
       --period 86400 \
       --unit Count \
       --evaluation-periods 1 \
       --alarm-actions arn:aws:sns:us-east-1:1234567890:things-not-connecting-successfully
   ```

1. Test the alarm\.

   ```
   Prompt>aws cloudwatch set-alarm-state --alarm-name ConnectSuccessAlarm --state-reason "initializing" --state-value OK
   ```

   ```
   Prompt>aws cloudwatch set-alarm-state --alarm-name ConnectSuccessAlarm --state-reason "initializing" --state-value ALARM
   ```

## How can I be notified if my things are not publishing data each day?<a name="how_to_detect_publish_failures"></a>

1. Create an Amazon SNS topic, `arn:aws:sns:us-east-1:123456789012:things-not-publishing-data`\.

   For more information, see [Set Up Amazon Simple Notification Service](Amazon Simple Notification Service Developer GuideUS_SetupSNS.html)\.

1. Create the alarm\.

   ```
   Prompt>aws cloudwatch put-metric-alarm \
       --alarm-name PublishInSuccessAlarm\
       --alarm-description "Alarm when my Things don't publish their data \
       --namespace AWS/IoT \
       --metric-name PublishIn.Success \
       --dimensions Name=Protocol,Value=MQTT \
       --statistic Sum \
       --threshold 10 \
       --comparison-operator LessThanThreshold \
       --period 86400 \
       --unit Count \
       --evaluation-periods 1 \
       --alarm-actions arn:aws:sns:us-east-1:1234567890:things-not-publishing-data
   ```

1. Test the alarm\.

   ```
   Prompt>aws cloudwatch set-alarm-state --alarm-name PublishInSuccessAlarm --state-reason "initializing" --state-value OK
   ```

   ```
   Prompt>aws cloudwatch set-alarm-state --alarm-name PublishInSuccessAlarm --state-reason "initializing" --state-value ALARM
   ```

## How can I be notified if my thing's shadow updates are being rejected each day?<a name="detect_rejected_updates"></a>

1. Create an Amazon SNS topic, arn:aws:sns:us\-east\-1:1234567890:things\-shadow\-updates\-rejected\.

   For more information, see [Set Up Amazon Simple Notification Service](Amazon Simple Notification Service Developer GuideUS_SetupSNS.html)\.

1. Create the alarm\.

   ```
   Prompt>aws cloudwatch put-metric-alarm \
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
       --alarm-actions arn:aws:sns:us-east-1:1234567890:things-shadow-updates-rejected
   ```

1. Test the alarm\.

   ```
   Prompt>aws cloudwatch set-alarm-state --alarm-name UpdateThingShadowSuccessAlarm --state-reason "initializing" --state-value OK
   ```

   ```
   Prompt>aws cloudwatch set-alarm-state --alarm-name UpdateThingShadowSuccessAlarm --state-reason "initializing" --state-value ALARM
   ```