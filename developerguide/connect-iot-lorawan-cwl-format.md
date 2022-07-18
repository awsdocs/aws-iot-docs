# View CloudWatch AWS IoT Wireless log entries<a name="connect-iot-lorawan-cwl-format"></a>

After you've configured logging for AWS IoT Wireless as described in [Create logging role and policy for AWS IoT Wireless](connect-iot-lorawan-create-logging-role-policy.md) and written some log entries, you can view the log entries in the CloudWatch console by performing the following steps\.

## Viewing AWS IoT logs in the CloudWatch Log groups console<a name="connect-iot-lorawan-viewing-logs"></a>

In the [CloudWatch console](https://console.aws.amazon.com/cloudwatch), CloudWatch logs appear in a log group named **/aws/iotwireless**\. For more information about CloudWatch Logs, see [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatchLogs.html)\.

**To view your AWS IoT logs in the CloudWatch console**

Navigate to the [CloudWatch console](https://console.aws.amazon.com/cloudwatch) and choose **Log groups** in the navigation pane\.

1. In the **Filter** text box, enter **/aws/iotwireless**, and then choose the `/aws/iotwireless` Log group\.

1. To see a complete list of the AWS IoT Wireless logs generated for your account, choose **Search all**\. To look at an individual log stream, choose the expand icon\.

1. To filter the log streams, you can also enter a query in the **Filter events** text box\. Here are some queries to try:
   + `{ $.logLevel = "ERROR" }` 

      Use this filter to find all logs that have a log level of `ERROR` and you can expand the individual error streams to read the error messages, which will help you resolve them\.
   + `{ $.resource = "WirelessGateway" }` 

      Find all logs for the `WirelessGateway` resource regardless of the log level\.
   + `{ $.event = "CUPS_Request" && $.logLevel = "ERROR" }`

      Find all logs that have an event type of `CUPS_Request` and a log level of `ERROR`\.

## Events and resource types<a name="connect-iot-lorawan-cwl-format-events-resources"></a>

The following table shows the different types of events for which you'll see log entries\. The event types also depend on whether the resource type is a wireless device or a wireless gateway\. You can use the default log level for the resources and event types or override the default log level by specifying a log level for each of them\.


**Event types based on resources used**  

| Resource | Resource type | Event type | 
| --- | --- | --- | 
| Wireless gateway | LoRaWAN |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/connect-iot-lorawan-cwl-format.html)  | 
| Wireless device | LoRaWAN |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/connect-iot-lorawan-cwl-format.html)  | 
| Wireless device | Sidewalk |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/connect-iot-lorawan-cwl-format.html)  | 

The following topic contains more information about these event types and the log entries for wireless gateways and wireless devices\.

**Topics**
+ [Viewing AWS IoT logs in the CloudWatch Log groups console](#connect-iot-lorawan-viewing-logs)
+ [Events and resource types](#connect-iot-lorawan-cwl-format-events-resources)
+ [Log entries for wireless gateways and wireless device resources](connect-iot-lorawan-wireless-log-entries.md)