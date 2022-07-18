# View and monitor network analyzer trace message logs in real time<a name="connect-iot-lorawan-network-analyzer-logs"></a>

If you've added resources to your network analyzer configuration, you can activate trace messaging to start receiving trace messages for your resources\. You can use either the AWS Management Console, the AWS IoT Wireless API, or the AWS CLI\.

## Prerequisites<a name="connect-iot-lorawan-network-analyzer-logs-prereq"></a>

Before you can activate trace messaging by using network analyzer, you must have:
+ Added the resources that you want to monitor to your default network analyzer configuration\. For more information, see [Add resources and update the network analyzer configuration](connect-iot-lorawan-network-analyzer-resources.md)\.
+ Generated a presigned request by using the `StartNetworkAnalyzerStream` request URL\. The request will be signed using the credentials for the AWS Identity and Access Management role that makes this request\. For more information, see [Create a presigned URL](connect-iot-lorawan-network-analyzer-generate-request.md#connect-iot-lorawan-network-analyzer-presigned-url)

## Activate trace messaging by using the console<a name="connect-iot-lorawan-network-analyzer-activate-console"></a>

To activate trace messaging

1. Open the [Network Analyzer hub of the AWS IoT console](https://console.aws.amazon.com/iot/home#/wireless/networkAnalyzer) and choose your network analyzer configuration, **NetworkAnalyzerConfig\_Default**\.

1. In the details page of your network analyzer configuration, choose **Activate trace messaging** and then choose **Activate**\.

   You'll start receiving trace messages where the newest trace message appears first in the console\.
**Note**  
After the messaging session starts, receiving trace messages can incur additional costs until you deactivate the session or leave the trace session\. For more information about pricing, see [AWS IoT Core pricing](http://aws.amazon.com/iot-core/pricing/)\.

## View and monitor trace messages<a name="connect-iot-lorawan-network-analyzer-view-trace"></a>

After you activate trace messaging, the WebSocket connection is established and trace messages start appearing in real time, newest first\. You can customize the preferences to specify the number of trace messages to be displayed in each page and to display only the relevant fields for each message\. For example, you can customize the trace message log to show only logs for wireless gateway resources that have **Log level** set to `ERROR`, so that you can quickly identify and debug errors with your gateways\. The trace messages contain the following information\. 
+ **Message Number**: A unique number that shows the last message received first\.
+ **Resource ID**: The wireless gateway or wireless device ID of the resource\.
+ **Timestamp**: The time when the message was received\.
+ **Message ID**: An identifier that AWS IoT Core for LoRaWAN assigns to each received message\.
+ **FPort**: The frequency port for communicating with the device by using the WebSocket connection\.
+ **DevEui**: The extended unique identifier \(EUI\) for your wireless device\.
+ **Resource**: Whether the monitored resource is a wireless device or a wireless gateway\.
+ **Event**: The event for a log message for a wireless device, which can be **Join**, **Rejoin**, **Uplink\_Data**, **Downlink\_Data**, or **Registration**\.
+ **Log level**:Information about `INFO` or `ERROR` log streams for your device\.

## Network analyzer JSON log message<a name="connect-iot-network-analyzer-trace-logs"></a>

You can also choose one trace message at a time to view the JSON payload for that message\. Depending on the message that you select in the trace message logs, you'll see information in the JSON payload that indicates contains 2 parts: **CustomerLog** and **LoRaFrame**\.

**CustomerLog**  
The **CustomerLog** part of the JSON displays the type and identifier of the resource that received the message, the log level, and the message content\. The following example shows a **CustomerLog** log message\. You can use the `message` field in the JSON to get more information about the error and how it can be resolved\.

**LoRaFrame**  
The **LoRaFrame** part of the JSON has a **Message ID** and contains information about the physical payload for the device and the wireless metadata\.

The following shows the structure of the trace message\.

**Note**  
If your devices send an uplink message without a value for `Fport`, AWS IoT Core for LoRaWAN network analyzer will display an `fPort` value of 225 in the trace message received\.

```
export type TraceMessage = {
  ResourceId: string;
  Timestamp: string;
  LoRaFrame: 
  {
    MessageId: string;
    PhysicalPayload: any;
    WirelessMetadata: 
    {
      fPort: number;
      dataRate: number;
      devEui: string;
      frequency: number,
      timestamp: string;
    },
  }
  CustomerLog: 
  {
    resource: string;
    wirelessDeviceId: string;
    wirelessDeviceType: string;
    event: string;
    logLevel: string;
    messageId: string;
    message: string;
  },
};
```

## Review and next steps<a name="connect-iot-lorawan-network-analyzer-review"></a>

In this section, you've viewed trace messages and learned how you can use the information to debug errors\. After you've viewed all messages, you can:
+ 

**Deactivate trace messaging**  
To avoid incurring any additional costs, you can deactivate the trace messaging session\. Deactivating the session disconnects your WebSocket connection so you won't receive any additional trace messages\. You can still continue viewing the existing messages in the console\.
+ 

**Edit frame info for your configuration**  
You can edit the network analyzer configuration and choose whether to deactivate frame info and choose the log levels for your messages\. Before you update your configuration, consider deactivating your trace messaging session\. To make these edits, open the [Network Analyzer details page in the AWS IoT console](https://console.aws.amazon.com/iot/home#/wireless/networkAnalyzer/details/NetworkAnalyzerConfig_Default) and choose **Edit**\. You can then update your configuration with the new configuration settings and activate trace messaging to see the updated messages\.
+ 

**Add resources to your configuration**  
You can also add more resources to your network analyzer configuration and monitor them in real time\. You can add up to a combined total of 250 wireless gateway and wireless device resources\. To add resources, on the [Network Analyzer details page of the AWS IoT console](https://console.aws.amazon.com/iot/home#/wireless/networkAnalyzer/details/NetworkAnalyzerConfig_Default), choose the **Resources** tab and choose **Add resources**\. You can then update your configuration with the new resources and activate trace messaging to see the updated messages for the additional resources\.

For more information about updating your network analyzer configuration by editing the configuration settings and adding resources, see [Add resources and update the network analyzer configuration](connect-iot-lorawan-network-analyzer-resources.md)\.