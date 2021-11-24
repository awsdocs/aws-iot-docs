# Proximity events<a name="iot-sidewalk-proximity-events"></a>

Proximity events publish event notifications when AWS IoT receives a beacon from the Sidewalk device\. When your Sidewalk device approaches Amazon Sidewalk, beacons that are sent from your device are filtered by Amazon Sidewalk at regular intervals and received by AWS IoT Wireless\. AWS IoT Wireless then notifies you of these events when a beacon is received\.



## Enable notifications for proximity events<a name="iot-sidewalk-proximity-events-enable"></a>

Before subscribers to the proximity event topics can receive notifications, you must enable event notifications for them from the AWS Management Console, or by using the API or CLI\. To receive notifications, provide your unique Sidewalk Amazon ID\. All Sidewalk devices that are registered to this Amazon ID will then be notified of any proximity events\.
+ To enable event notifications from the console:

  1. In the [Settings](console.aws.amazon.com/iot/home/settings/) tab of the AWS IoT console, go to the **Sidewalk event notification** section and choose **Manage resource events**\.

  1. Choose your **Amazon ID** and choose whether you want to enable event notifications for only **Sidewalk: proximity** events or for **Sidewalk: device registration state** events as well\. To disable events, choose your **Amazon ID** and clear the check boxes for events you want to disable\. Choose **Confirm**\.

     You'll see the events that you've added in the **Sidewalk event notification** section\.

  1. To receive notifications, choose the events that you've added, and choose **Subscribe**\.

     All Sidewalk devices that are registered to this Sidewalk Amazon ID will receive a notification when an event occurs\.
+ To use the API or CLI to control which event types are published, call the [UpdateResourceEventConfiguration](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateResourceEventConfiguration.html) API or use the [cli/latest/reference/iotwireless/update-resource-event-configuration.html](cli/latest/reference/iotwireless/update-resource-event-configuration.html) CLI command\. For example:

  ```
  aws iotwireless update-resource--event-configuration \ 
      --event-configurations "{\"Proximity\":{\"Sidewalk\":{"AmazonIdEventTopic": "Enabled"}}}"
  ```

  To disable events, use the `UpdateResourceEventConfiguration` API or the update\-resource\-event\-configuration CLI command and set `AmazonIdEventTopic` to `DISABLED`\.

## Format of MQTT topics for proximity events<a name="iot-sidewalk-proximity-events-mqtt"></a>

To notify you of proximity events, you can subscribe to MQTT reserved topics that begin with a dollar \($\) sign\. For more information, see [MQTT topics](topics.md)\. 

Reserved MQTT topics for Sidewalk proximity events use the format:

`$aws/iotwireless/events/proximity/{Possible event Type}/sidewalk/sidewalk_accounts/amazon_id/{id}`

where \{event Type\} can be `beacon_discovered` or `beacon_lost`\.

You can also use the `+` wildcard character to subscribe to multiple topics at the same time\. The `+` wildcard character matches any string in the level that contains the character\. For example, if you want to be notified of all possible event types \(`beacon_discovered` and `beacon_lost`\) and for all devices registered to a particular Amazon ID, you can use the following topic filter:

`$aws/iotwireless/events/proximity/+/sidewalk/sidewalk_accounts /amazon_id/+`

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
    "Sidewalk": {
        "AmazonId": "string"        
    }
}
```

The payload contains the following attributes:

eventId  
A unique event ID, which is a string\.

eventType  
The type of event that occurred\. Can be `beacon_discovered` or `beacon_lost`\.

WirelessDeviceId  
Can be provisioned or registered\.

timestamp  
The UNIX timestamp of when the event occurred\.

sidewalk  
The Sidewalk Amazon ID for which you want to receive event notifications\.

## How proximity events work<a name="iot-sidewalk-proximity-events-work"></a>

Proximity events notify you when AWS IoT receives a beacon, Your Sidewalk devices can emit beacons any time\. When your device is near Amazon Sidewalk, Sidewalk receives the beacons and forwards them to AWS IoT Wireless at regular time intervals\. Amazon Sidewalk has configured this time interval as 10 minutes\. When AWS IoT Wireless receives the beacon from Sidewalk, you'll be notified of the event\.

Proximity events will notify you when a beacon is discovered or when a beacon is lost\. You can configure the intervals at which you're notified of the proximity event\.