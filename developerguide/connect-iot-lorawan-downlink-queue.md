# Queue downlink messages to send to LoRaWAN devices<a name="connect-iot-lorawan-downlink-queue"></a>

Cloud\-hosted applications and other AWS services can send downlink messages to your wireless devices\. Downlink messages are messages that are sent from AWS IoT Core for LoRaWAN to your wireless device\. You can schedule and send downlink messages for each device that you've onboarded to AWS IoT Core for LoRaWAN\.

If you have multiple devices for which you want to send a downlink message, you can use a multicast group\. Devices in a multicast group share the same multicast address, which is then distributed to an entire group of recipient devices\. For more information, see [Create multicast groups to send a downlink payload to multiple devices](connect-iot-lorawan-multicast-groups.md)\. 

## How a downlink message queue works<a name="connect-iot-lorawan-how-downlink-works"></a>

The device class of your LoRaWAN device determines how the messages in your queue are sent to the device\. Class A devices send an uplink message to AWS IoT Core for LoRaWAN to indicate that the device is available to receive downlink messages\. Class B devices can receive messages at regular downlink slots\. Class C devices can receive downlink messages at any time\. For more information about device classes, see [Device classes](connect-iot-lorawan-manage-end-devices.md#connect-iot-lorawan-device-classes)\.

The following shows how messages are queued and sent to your class A devices\.

1. AWS IoT Core for LoRaWAN buffers the downlink message that you added to the queue with the frame port, payload data, and the acknowledge mode parameters that you specified by using the AWS IoT console or the AWS IoT Wireless API\.

1. Your LoRaWAN device sends an uplink message to indicate that it's online and can start receiving downlink messages\.

1. If you added more than one downlink message to the queue, AWS IoT Core for LoRaWAN sends the first downlink message in the queue to your device with the acknowledge \(ACK\) flag set\.

1. Your device either sends an uplink message to AWS IoT Core for LoRaWAN immediately, or it sleeps until the next uplink message and includes the ACK flag in the message\.

1. When AWS IoT Core for LoRaWAN receives the uplink message with the ACK flag, it clears the downlink message from the queue, indicating that your device has successfully received the downlink message\. If the ACK flag is missing from the uplink message after checking three times, the message is discarded\.

## Perform downlink queue operations by using the console<a name="connect-iot-lorawan-downlink-queue-console"></a>

You can use the AWS Management Console to queue downlink messages and clear individual messages, or the entire queue, as needed\. For class A devices, after an uplink is received from the device to indicate that it's online, the queued messages are then sent to the device\. After the message is sent, it's automatically cleared from the queue\.

**Queue downlink messages**  
To create a downlink message queue

1. Go to the [Devices hub of the AWS IoT console](https://console.aws.amazon.com/iot/home#/wireless/devices) and choose the device for which you want to queue downlink messages\.

1. In the **Downlink messages** section of the device details page, choose **Queue downlink messages**\.

1. Specify the following parameters to configure your downlink message:
   + **FPort**: Choose the frame port for the device to communicate with AWS IoT Core for LoRaWAN\.
   + **Payload**: Specify the payload message that you want to send to your device\. The maximum payload size is 242 bytes\. If adaptive data rate \(ADR\) is enabled, AWS IoT Core for LoRaWAN uses it to choose the optimal data rate for your payload size\. You can further optimize the data rate as needed\.
   + **Acknowledge mode**: Confirm whether your device has received the downlink message\. If a message requires this mode, you'll see an uplink message with the ACK flag in your data stream, and the message will be cleared from the queue\.

1. To add your downlink message to the queue, choose **Submit**\.

Your downlink message has now been added to the queue\. If you don't see your message or you receive an error, you can troubleshoot the error as described in [Troubleshoot downlink message queue errors](#connect-iot-lorawan-downlink-queue-troubleshoot)\. 

**Note**  
After your downlink message has been added to the queue, you can no longer edit the parameters **FPort**, **Payload**, and **Acknowledge mode**\. To send a downlink message with different values for these parameters, you can delete this message and queue a new downlink message with the updated parameter values\.

The queue lists the downlink messages you've added\. To see the payload for the uplink and downlink messages that are exchanged between your devices and AWS IoT Core for LoRaWAN, you can use network analyzer\. For more information, see [Monitoring your wireless resource fleet in real time using network analyzer](connect-iot-lorawan-network-analyzer-overview.md)\.

**List downlink message queue**  
The downlink message that you created is added to the queue\. Each subsequent downlink message is added to the queue after this message\. You can see a list of downlink messages in the **Downlink messages** section of the device details page\. After an uplink is received, the messages are sent to the device\. After a downlink message has been received by your device, it will be removed from the queue\. The next message then moves up in the queue to be sent to your device\.

**Delete individual downlink messages or clear entire queue**  
Each downlink message is cleared from the queue automatically after it's sent to your device\. You can also delete individual messages or clear the entire downlink queue\. These actions can't be undone\.
+ If you find messages in the queue that you don't want to send, choose the messages and choose **Delete**\.
+ If you don't want to send any messages from the queue to your device, you can clear the entire queue by choosing **Clear downlink queue**\.

## Perform downlink queue operations by using the API<a name="connect-iot-lorawan-downlink-queue-api"></a>

You can use the AWS IoT Wireless API to queue downlink messages and clear individual messages, or the entire queue, as needed\.

**Queue downlink messages**  
To create a downlink message queue, use the [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_SendDataToWirelessDevice.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_SendDataToWirelessDevice.html) API operation or the [cli/latest/reference/iotwireless/send-data-to-wireless-device.html](cli/latest/reference/iotwireless/send-data-to-wireless-device.html) CLI command\.

```
aws iotwireless send-data-to-wireless-device \
    --id "11aa5eae-2f56-4b8e-a023-b28d98494e49" \
    --transmit-mode "1" \
    --payload-data "SGVsbG8gVG8gRGV2c2lt" \
    --wireless-metadata LoRaWAN={FPort=1}
```

The output of running this command generates a `MessageId` for the downlink message\. In some cases, even if you receive the `MessageId`, packets can get dropped\. For more information about how you can resolve the error, see [Troubleshoot downlink message queue errors](#connect-iot-lorawan-downlink-queue-troubleshoot)\.

```
{
    MessageId: "6011dd36-0043d6eb-0072-0008"
}
```

**List downlink messages in the queue**  
To list all downlink messages in the queue, use the [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListQueuedMessages.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListQueuedMessages.html) API operation or the [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/list-queued-messages.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/list-queued-messages.html) CLI command\.

```
aws iotwireless list-queued-messages
```

By default, a maximum of 10 downlink messages are displayed when running this command\.

**Remove individual downlink messages or clear entire queue**  
To remove individual messages from the queue or to clear the entire queue, use the [https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_DeleteQueuedMessages.html](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_DeleteQueuedMessages.html) API operation or the [https://docs.aws.amazon.com/cli/latest/reference/iotwireless/delete-queued-messages.html](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/delete-queued-messages.html) CLI command\.
+ To remove individual messages, provide the `messageID` for messages you want to remove for your wireless device, specified by the `wirelessDeviceId`\.
+ To clear the entire downlink queue, specify `messageID` as `*` for your wireless device, specified by the `wirelessDeviceId`\.

## Troubleshoot downlink message queue errors<a name="connect-iot-lorawan-downlink-queue-troubleshoot"></a>

Here are some things to check if you're not seeing the expected results:
+ 

**Downlink messages don't appear in the AWS IoT console**  
If you don't see your downlink message in the queue after adding it as described in [Perform downlink queue operations by using the console](#connect-iot-lorawan-downlink-queue-console), it might be because your device hasn't completed a process called *activation* or *join procedure*\. This procedure is completed when your device onboards with AWS IoT Core for LoRaWAN\. For more information, see [Add your wireless device specification to AWS IoT Core for LoRaWAN using the console](connect-iot-lorawan-end-devices-add.md#connect-iot-lorawan-end-device-spec-console)\.

  After onboarding your device to AWS IoT Core for LoRaWAN, you can monitor your device to check whether join and rejoin succeeded by using the network analyzer or Amazon CloudWatch\. For more information, see [Monitoring and logging for AWS IoT Wireless using Amazon CloudWatch](connect-iot-lorawan-logging-monitoring.md)\.
+ 

**Missing downlink message packets when using the API**  
When you use the `SendDataToWirelessDevice` API operation, the API returns a unique `MessageId`\. However, it can't confirm whether your LoRaWAN device has received the downlink message\. The downlink packets can get dropped in cases such as when your device hasn't completed the join procedure\. For more information about how to resolve this error, see the previous section\.
+ 

**Missing ARN error when sending downlink message**  
When sending a downlink message to your device from the queue, you can receive a missing Amazon Resource Name \(ARN\) error\. This error might occur because the destination hasn't been specified correctly for the device that's receiving the downlink message\. To resolve this error, check the destination details for your device\.