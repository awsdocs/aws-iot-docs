# Viewing Logs<a name="viewing-logs"></a>

**To view your logs**

1.  Browse to [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\. In the navigation pane, choose **Logs**\.

1. In the **Filter** text box, type **AWSIotLogsV2** , and press Enter\.

1. Double\-click the `AWSIotLogsV2` log group\.

1. Choose **Search Log Group**\. A complete list of the AWS IoT logs generated for your account is displayed\.

1. Choose the expand icon to look at an individual stream\.

You can also type a query in the **Filter events** text box\. Here are some interesting queries to try:

+ `{ $.logLevel = "INFO" }` 

   Find all logs that have a log level of `INFO`\.

+ `{ $.status = "Success" }` 

   Find all logs that have a status of `Success`\.

+ `{ $.status = "Success" && $.eventType = "GetThingShadow" }`

   Find all logs that have a status of `Success` and an event type of `GetThingShadow`\.

For more information about creating filter expressions, see [CloudWatch Logs Queries](http://alpha-docs-aws.amazon.com/AmazonCloudWatch/latest/logs/FilterAndPatternSyntax.html)\.