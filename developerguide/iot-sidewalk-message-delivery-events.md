# Message delivery status events<a name="iot-sidewalk-message-delivery-events"></a>

Message delivery status events publish event notifications about the status of messages that are exchanged between your Sidewalk devices and AWS IoT Wireless\. Event notifications are published for both downlink messages that are sent from AWS IoT Wireless to the Sidewalk device, and uplink messages that are sent from your device to AWS IoT Wireless\.

## How message delivery status events work<a name="iot-sidewalk-message-delivery-events-work"></a>

After you've onboarded your Sidewalk device to AWS IoT Wireless and connected your device, messages can be exchanged between your device and AWS IoT Wireless\. The events publish notifications about the message delivery status that indicate whether these messages were successfully delivered to your device or to AWS IoT Wireless\.

For example, if an uplink message is received from the device with an acknowledge \(ACK\) flag, a notification is published indicating that the message was delivered successfully\. When you send downlink messages from AWS IoT Wireless to the Sidewalk device, the `SendDataToWirelessDevice` API returns a `MessageId` for the downlink message even if packets have dropped or the message wasn't delivered\. In this case, the message delivery status events return an error indicating that the message failed to deliver to the device\.

## Enable notifications for message delivery status events<a name="iot-sidewalk-message-delivery-events-enable"></a>

Before subscribers to the Sidewalk message delivery status reserved topics can receive messages, you must enable event notifications for them using the AWS IoT Wireless API or the AWS CLI\. You can enable these events for all Sidewalk resources in your AWS account or for select resources\.

**Note**  
The Sidewalk message delivery status event configuration isn't available in the console\.

For information about how to enable these events, see [Enable notifications using the AWS CLI](iot-wireless-control-events.md#iot-wireless-control-events-cli)\. 

## Format of MQTT topics for message delivery status events<a name="iot-sidewalk-message-delivery-events-mqtt"></a>

To receive notifications about message delivery status events, you can subscribe to MQTT reserved topics that begin with a dollar \($\) sign\. For more information, see [MQTT topics](topics.md)\.

Reserved MQTT topics for Sidewalk proximity events use the format:
+ For resource\-level topics:

  `$aws/iotwireless/events/{eventName}/{eventType}/sidewalk/wireless_devices`
+ For identifier topics:

  `$aws/iotwireless/events/{eventName}/{eventType}/sidewalk/{resourceType}/{resourceID}/{id}`

Where:

**\{eventName\}**  
\{eventName\} must be `message_delivery_status`\.

**\{eventType\}**  
\{eventType\} can be `success` or `error`\.

**\{resourceType\}**  
\{resourceType\} can be `sidewalk_accounts` or `wireless_devices`\.

**\{resourceID\}**  
\{resourceID\} is `amazon_id` for \{resourceType\} of `sidewalk_accounts` and `wireless_device_id` for \{resourceType\} of `wireless_devices`\.

You can also use the `+` wildcard character to subscribe to multiple topics at the same time\. The `+` wildcard character matches any string in the level that contains the character\. For example, if you want to be notified of all possible event types \(`success` and `error`\) and for all devices registered to a particular Amazon ID, you can use the following topic filter:

`$aws/iotwireless/events/message_delivery_status/+/sidewalk/sidewalk_accounts/amazon_id/+`

**Note**  
You can't use the wildcard character `#` to subscribe to the reserved topics\. For more information about topic filters, see [Topic filters](topics.md#topicfilters)\.

## Message payload for message delivery status events<a name="iot-sidewalk-proximity-events-json"></a>

After you enable notifications for message delivery status events, event messages are published over MQTT with a JSON payload\. These events contain the following example payload depending on whether the event is a success, indicating that the device successfully received the message, or an error\.

**Success events**  
The following shows the payload format when the event is a success\.

```
{
    "eventId": "string",
    "eventType": "success",
    "WirelessDeviceId": "string",
    "timestamp": "timestamp",
    "Sidewalk": {
        "Seq": "Integer",
        "MsgType": "CUSTOM_COMMAND_ID_RESP",
        "CmdExStatus": "COMMAND_EXEC_STATUS_SUCCESS"   
    }
}
```

The payload contains the following attributes:

**eventId**  
A unique event ID, which is a string\.

**eventType**  
The type of event that occurred\. Can be `success` or `error`\. In this case, the `eventType` is `error`\.

**WirelessDeviceId**  
The identifier of the wireless device\.

**timestamp**  
The Unix timestamp of when the event occurred\.

**sidewalk**  
The Sidewalk wrapper that contains the status code for success messages, sequence number of message, and the message type\.

**Error events**  
The following shows the payload format when the event indicates that an error occurred\.

```
{
    "eventId": "string",
    "eventType": "error" ,
    "WirelessDeviceId": "string",
    "timestamp": "timestamp",
    "Sidewalk": {
        "Seq": "Integer",
        "Status": "DeviceNotReachable" | "RADIO_TX_ERROR" | "MEMORY_ERROR"        
    }
}
```

The payload contains similar attributes as when the `eventType` is a `success`\. Following are some differences or additional attributes:

**eventType**  
The type of event that occurred\. In this case, the `eventType` is an `error`\.

**sidewalk**  
The Sidewalk wrapper that contains the sequence number and the status code that indicates why the downlink message wasn't sent successfully\.