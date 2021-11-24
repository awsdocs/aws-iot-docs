# Monitoring your wireless resource fleet in real time using network analyzer<a name="connect-iot-lorawan-network-analyzer-overview"></a>

Network analyzer uses a default WebSocket connection to receive real\-time trace message logs for your wireless connectivity resources\. By using network analyzer, you can add the resources you want to monitor, activate a trace messaging session, and start receiving trace messages in real time\.

To monitor your resources, you can also use Amazon CloudWatch\. To use CloudWatch, you set up an IAM role to configure logging and then wait for the log entries to be displayed in the console\. Network analyzer significantly reduces the time that it takes to set up a connection and start receiving trace messages, providing you with just\-in\-time log information for your fleet of resources\. For information about monitoring by using CloudWatch, see [Monitoring and logging for AWS IoT Core for LoRaWAN using Amazon CloudWatch](connect-iot-lorawan-logging-monitoring.md)\.

By reducing your setup time and using the information from the trace messages, you can monitor your resources more effectively, get meaningful insights, and troubleshoot errors\. You can monitor both LoRaWAN devices and LoRaWAN gateways\. For example, you can quickly identify a join error when onboarding one of your LoRaWAN devices\. To debug the error, use the information in the provided trace message log\.

**How to use network analyzer**  
To monitor your resource fleet and start receiving trace messages, perform the following steps

1. 

**Add resources and specify configuration settings for your configuration**  
Add resources to your network analyzer configuration by entering the wireless gateway and wireless device identifiers\. You can also edit the configuration settings, log levels and wireless device frame information, for your network analyzer configuration\.

1. 

**Stream network analyzer trace messages with WebSockets**  
You can generate a presigned request URL using the credentials for your IAM role to stream network analyzer trace messages by using the WebSocket protocol\.

1. 

**Activate your trace messaging session and monitor trace messages**  
To start receiving trace messages, activate your trace messaging session\. To avoid incurring additional costs, you can either deactivate or close your network analyzer trace messaging session\.

The following shows how to add resources and activate your trace messaging session\.

**Topics**
+ [Add resources and update the network analyzer configuration](connect-iot-lorawan-network-analyzer-resources.md)
+ [Stream network analyzer trace messages with WebSockets](connect-iot-lorawan-network-analyzer-api.md)
+ [View and monitor network analyzer trace message logs in real time](connect-iot-lorawan-network-analyzer-logs.md)