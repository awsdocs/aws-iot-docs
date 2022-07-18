# Proximity events<a name="iot-sidewalk-proximity-events"></a>

Proximity events publish event notifications when AWS IoT receives a beacon from the Sidewalk device\. When your Sidewalk device approaches Amazon Sidewalk, beacons that are sent from your device are filtered by Amazon Sidewalk at regular intervals and received by AWS IoT Wireless\. AWS IoT Wireless then notifies you of these events when a beacon is received\.

## How proximity events work<a name="iot-sidewalk-proximity-events-work"></a>

Proximity events notify you when AWS IoT receives a beacon, Your Sidewalk devices can emit beacons any time\. When your device is near Amazon Sidewalk, Sidewalk receives the beacons and forwards them to AWS IoT Wireless at regular time intervals\. Amazon Sidewalk has configured this time interval as 10 minutes\. When AWS IoT Wireless receives the beacon from Sidewalk, you'll be notified of the event\.

Proximity events will notify you when a beacon is discovered or when a beacon is lost\. You can configure the intervals at which you're notified of the proximity event\.

## Enable notifications for proximity events<a name="iot-sidewalk-proximity-events-enable"></a>

Before subscribers to the Sidewalk proximity reserved topics can receive messages, you must enable event notifications for them from the AWS Management Console, or by using the API or CLI\. You can enable these events for all Sidewalk resources in your AWS account or for select resources\. For information about how to enable these events, see [Enable events for wireless resources](iot-wireless-control-events.md)\. 

## Format of MQTT topics for proximity events<a name="iot-sidewalk-proximity-events-mqtt"></a>

To notify you of proximity events, you can subscribe to MQTT reserved topics that begin with a dollar \($\) sign\. For more information, see [MQTT topics](topics.md)\.

Reserved MQTT topics for Sidewalk proximity events use the format:
+ For resource\-level topics:

  `$aws/iotwireless/events/{eventName}/{eventType}/sidewalk/wireless_devices`
+ For identifier topics:

  `$aws/iotwireless/events/{eventName}/{eventType}/sidewalk/{resourceType}/{resourceID}/{id}`

Where:

**\{eventName\}**  
\{eventName\} must be `proximity`\.

**\{eventType\}**  
\{eventType\} can be `beacon_discovered` or `beacon_lost`\.

**\{resourceType\}**  
\{resourceType\} can be `sidewalk_accounts` or `wireless_devices`\.

**\{resourceID\}**  
\{resourceID\} is `amazon_id` for \{resourceType\} of `sidewalk_accounts` and `wireless_device_id` for \{resourceType\} of `wireless_devices`\.

You can also use the `+` wildcard character to subscribe to multiple topics at the same time\. The `+` wildcard character matches any string in the level that contains the character\. For example, if you want to be notified of all possible event types \(`beacon_discovered` and `beacon_lost`\) and for all devices registered to a particular Amazon ID, you can use the following topic filter:

`$aws/iotwireless/events/proximity/+/sidewalk/sidewalk_accounts/amazon_id/+`

**Note**  
You can't use the wildcard character `#` to subscribe to the reserved topics\. For more information about topic filters, see [Topic filters](topics.md#topicfilters)\.

## Message payload for proximity events<a name="iot-sidewalk-proximity-events-json"></a>

After you enable notifications for proximity events, event messages are published over MQTT with a JSON payload\. These events contain the following example payload:

```
{    
    "eventId": "string", 
    "eventType": "beacon_discovered|beacon_lost",
    "WirelessDeviceId": "string",
    "timestamp": "1234567890123",

    // Event-specific fields
    "Sidewalk": {
        "AmazonId": "string",
        "SidewalkManufacturingSn": "string"        
    }
}
```

The payload contains the following attributes:

**eventId**  
A unique event ID, which is a string\.

**eventType**  
The type of event that occurred\. Can be `beacon_discovered` or `beacon_lost`\.

**WirelessDeviceId**  
The identifier of the wireless device\.

**timestamp**  
The Unix timestamp of when the event occurred\.

**sidewalk**  
The Sidewalk Amazon ID or `SidewalkManufacturingSn` for which you want to receive event notifications\.