# Monitor AWS IoT Wireless using CloudWatch Logs<a name="connect-iot-lorawan-cloud-watch-logs"></a>

AWS IoT Wireless has more than 50 CloudWatch log entries that are enabled by default\. Each log entry describes the event type, log level, and the resource type\. For more information, see [AWS IoT Wireless resources and log levels](connect-iot-lorawan-configure-resource-logging.md#connect-iot-lorawan-log-levels-resources)\.

**How to monitor your AWS IoT Wireless resources**  
When logging is enabled for AWS IoT Wireless, AWS IoT Wireless sends progress events about each message as it passes from your devices through AWS IoT and back\. By default, AWS IoT Wireless log entries have a default log level of error\. When you enable logging as described in [Create logging role and policy for AWS IoT Wireless](connect-iot-lorawan-create-logging-role-policy.md), you'll see messages in the CloudWatch console that have a default log level of `ERROR`\. By using this log level, the messages will show only error information for all wireless devices and gateway resources that you're using\.

If you want the logs to show additional information, such as those that have a log level of `INFO`, or disable logs for some of your devices and show log messages for only some of your devices, you can use the AWS IoT Wireless logging API\. For more information, see [Configure log levels of resources using the CLI](connect-iot-lorawan-configure-resource-logging.md#connect-iot-lorawan-configure-logging-api)\.

You can also create filter expressions to display only the required messages\.

**Before you can view AWS IoT Wireless logs in the console**  
To make the **/aws/iotwireless** log group appear in the CloudWatch console, you must have done the following\.
+ Enabled logging in AWS IoT Wireless\. For more information about how to enable logging in AWS IoT Wireless, see [Configure Logging for AWS IoT Wireless](connect-iot-lorawan-configure-logging.md)\.
+ Written some log entries by performing AWS IoT Wireless operations\.

To create and use filter expressions more effectively, we recommend that you try using CloudWatch insights as described in the following topics\. We also recommend that you follow the topics in the order they're presented here\. This will help you use CloudWatch **Log groups** first to learn about the different types of resources, its event types, and log levels that you can use to view log entries in the console\. You can then learn how to create filter expressions by using CloudWatch Insights to get more helpful information from your resources\. 

**Topics**
+ [View CloudWatch AWS IoT Wireless log entries](connect-iot-lorawan-cwl-format.md)
+ [Use CloudWatch Insights to filter logs for AWS IoT Wireless](connect-iot-lorawan-cwl-insights.md)