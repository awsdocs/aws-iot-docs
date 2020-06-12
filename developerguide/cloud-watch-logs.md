# Monitor AWS IoT using CloudWatch Logs<a name="cloud-watch-logs"></a>

When [AWS IoT logging is enabled](configure-logging.md), AWS IoT sends progress events about each message as it passes from your devices through the message broker and rules engine\. In the [CloudWatch console](https://console.aws.amazon.com/cloudwatch), CloudWatch logs appear in a log group named **AWSIotLogs**\.

For more information about CloudWatch Logs, see [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatchLogs.html)\. For information about supported AWS IoT CloudWatch Logs, see [CloudWatch AWS IoT log entries](cwl-format.md)\.

## Viewing AWS IoT logs in the CloudWatch console<a name="viewing-logs"></a>

**To view your AWS IoT logs in the CloudWatch console**

1.  Browse to [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\. In the navigation pane, choose **Logs**\.

1. In the **Filter** text box, enter **AWSIotLogsV2** , and then press Enter\.

1. Double\-click the `AWSIotLogsV2` log group\.

1. Choose **Search Log Group**\. A complete list of the AWS IoT logs generated for your account is displayed\.

1. Choose the expand icon to look at an individual stream\.

You can also enter a query in the **Filter events** text box\. Here are some interesting queries to try:
+ `{ $.logLevel = "INFO" }` 

   Find all logs that have a log level of `INFO`\.
+ `{ $.status = "Success" }` 

   Find all logs that have a status of `Success`\.
+ `{ $.status = "Success" && $.eventType = "GetThingShadow" }`

   Find all logs that have a status of `Success` and an event type of `GetThingShadow`\.

For more information about creating filter expressions, see [CloudWatch Logs Queries](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/FilterAndPatternSyntax.html)\.