# Create an Amazon SNS topic and subscription<a name="iot-moisture-create-sns-topic"></a>

Create an Amazon SNS topic and subscription\.

1. From the AWS IoT console, choose **Services**, type **SNS**, and then choose **Simple Notification Service**\.

1. In the navigation pane, choose **Topics**, and then choose **Create topic**\.

1. Enter a name for the topic \(for example, **MoistureSensorTopic**\)\.

1. Enter a display name for the topic \(for example, **Moisture Sensor Topic**\)\. This is the name displayed for your topic in the Amazon SNS console\.

1. Choose **Create topic**\.

1. In the Amazon SNS topic detail page, choose **Create subscription**\.

1. For **Protocol**, choose **Email**\.

1. For **Endpoint**, enter your email address\.

1. Choose **Create subscription**\.

1. Open your email client and look for a message with the subject **MoistureSensorTopic**\. Open the email and click the **Confirm subscription** link\.
**Important**  
You won't receive any email alerts from this Amazon SNS topic until you confirm the subscription\.

You should receive an email message with the text you typed\.