# Use CloudWatch Insights to filter logs for AWS IoT Wireless<a name="connect-iot-lorawan-cwl-insights"></a>

While you can use CloudWatch Logs to create filter expressions, we recommend that you use CloudWatch insights to more effectively create and use filter expressions depending on your application\.

We recommend that you first use CloudWatch **Log groups** to learn about the different types of resources, its event types, and log levels that you can use to view log entries in the console\. You can then use the examples of some filter expressions on this page as a reference to create your own filters for your AWS IoT Wireless resources\.

## Viewing AWS IoT logs in the CloudWatch Logs insights console<a name="connect-iot-lorawan-viewing-logs"></a>

In the [CloudWatch console](https://console.aws.amazon.com/cloudwatch), CloudWatch logs appear in a log group named **/aws/iotwireless**\. For more information about CloudWatch Logs, see [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatchLogs.html)\.

**To view your AWS IoT logs in the CloudWatch console**

Navigate to the [CloudWatch console](https://console.aws.amazon.com/cloudwatch) and choose **Logs Insights** in the navigation pane\.

1. In the **Filter** text box, enter **/aws/iotwireless** , and then choose the `/aws/iotwireless` Logs Insights\.

1. To see a complete list of log groups, choose **Select log group\(s\)**\. To look at log groups for AWS IoT Wireless, choose `/aws/iotwireless`\.

You can now start entering queries to filter the log groups\. The following sections contain some useful queries that'll help you gain insights about your resource metrics\.

## Create useful queries to filter and gain insights for AWS IoT Wireless<a name="connect-iot-lorawan-insights-resource-filter"></a>

You can use filter expressions to show additional helpful log information with CloudWatch Insights\. Following shows some sample queries:

### Show only logs for specific resource types<a name="connect-iot-lorawan-insights-gateway-filter"></a>

You can create a query that'll help you show logs for only specific resource types, such as a LoRaWAN gateway or a Sidewalk device\. For example, to filter logs to show only messages for Sidewalk devices, you can enter the following query and choose **Run query**\. To save this query, choose **Save**\.

```
fields @message
| filter @message like /Sidewalk/
```

After the query runs, you'll see the results in the **Logs** tab, which shows the timestamps for logs related to Sidewalk devices in your account\. You'll also see a bar graph, which will show the time at which the events occurred, if there were such events that occurred previously related to your Sidewalk device\. Following shows an example if you expand one of the results in the **Logs** tab\. Alternatively, if you want to troubleshoot errors related to Sidewalk devices, you can add another filter that sets the log level to `ERROR` and show only error information\. 

```
Field	          Value
@ingestionTime      1623894967640
@log	             954314929104:/aws/iotwireless
@logStream	   WirelessDevice-Downlink_Data-715adccfb34170214ec2f6667ddfa13cb5af2c3ddfc52fbeee0e554a2e780bed
@message	     {                    
                    "resource": "WirelessDevice",
                    "wirelessDeviceId": "3b058d05-4e84-4e1a-b026-4932bddf978d",
                    "wirelessDeviceType": "Sidewalk",
                    "devEui": "feffff000000011a",
                    "event": "Downlink_Data",
                    "logLevel": "INFO",
                    "messageId": "7e752a10-28f5-45a5-923f-6fa7133fedda",
                    "message": "Successfully sent downlink message. Amazon SidewalkId = 2000000006, Sequence number = 0"
                    }
@timestamp          1623894967640
devEui	           feffff000000011a
event	        Downlink_Data
logLevel            INFO
message	          Successfully sent downlink message. Amazon SidewalkId = 2000000006, Sequence number = 0
messageId	    7e752a10-28f5-45a5-923f-6fa7133fedda
resource	     WirelessDevice
wirelessDeviceId    3b058d05-4e84-4e1a-b026-4932bddf978d
wirelessDeviceType  Sidewalk
```

### Show specific messages or events<a name="connect-iot-lorawan-insights-gateway-filter"></a>

You can create a query that'll help you show specific messages and observe when the events occurred\. For example, if you want to see when your downlink message was sent from your LoRaWAN wireless device, you can enter the following query and choose **Run query**\. To save this query, choose **Save**\.

```
filter @message like /Downlink message sent/
```

After the query runs, you'll see the results in the **Logs** tab, which shows the timestamps when the downlink message was successfully sent to your wireless device\. You'll also see a bar graph, which will show the time at which a downlink message was sent, if there were downlink messages were previously sent to your wireless device\. Following shows an example if you expand one of the results in the **Logs** tab\. Alternatively, if a downlink message wasn't sent, you can modify the query to display only results for when the message wasn't sent so that you can debug the issue\.

```
Field	          Value
@ingestionTime      1623884043676
@log	             954314929104:/aws/iotwireless
@logStream	   WirelessDevice-Downlink_Data-42d0e6d09ba4d7015f4e9756fcdc616d401cd85fe3ac19854d9fbd866153c872
@message	     {
                    "timestamp": "2021-06-16T22:54:00.770493863Z",
                    "resource": "WirelessDevice",
                    "wirelessDeviceId": "3b058d05-4e84-4e1a-b026-4932bddf978d",
                    "wirelessDeviceType": "LoRaWAN",
                    "devEui": "feffff000000011a",
                    "event": "Downlink_Data",
                    "logLevel": "INFO",
                    "messageId": "7e752a10-28f5-45a5-923f-6fa7133fedda",
                    "message": "Downlink message sent. MessageId: 7e752a10-28f5-45a5-923f-6fa7133fedda"
                    }
@timestamp          1623884040858
devEui	           feffff000000011a
event	        Downlink_Data
logLevel            INFO
message	          Downlink message sent. MessageId: 7e752a10-28f5-45a5-923f-6fa7133fedda
messageId	    7e752a10-28f5-45a5-923f-6fa7133fedda
resource	     WirelessDevice
timestamp	    2021-06-16T22:54:00.770493863Z
wirelessDeviceId    3b058d05-4e84-4e1a-b026-4932bddf978d
wirelessDeviceType  LoRaWAN
```

## Next steps<a name="connect-iot-lorawan-insights-next-steps"></a>

You've learned how to use CloudWatch Insights to gain more helpful information by creating queries to filter log messages\. You can combine some of the filters described previously and design your own filters depending on the resource you're monitoring\. For more information about using CloudWatch Insights, see [Analyzing log data with CloudWatch Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData)\.

After you've created queries with CloudWatch Insights, if you've saved them, you can load and run the saved queries as needed\. Alternatively, if you click the **History** button in the CloudWatch **Logs Insights** console, you can view the previously run queries and rerun them as needed, or further modify them by creating additional queries\.