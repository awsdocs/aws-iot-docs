# Device registration state events<a name="iot-sidewalk-device-events"></a>

Device registration state events publish event notifications when there is a change in the device registration state, such as when a Sidewalk device has been provisioned or registered\. The events provide you information about the different states that the device goes through from when it's provisioned to when it has been registered\.

## Enable notifications for device registration state events<a name="iot-sidewalk-device-events-enable"></a>

Before subscribers to the device registration state reserved topics can receive messages, you must enable event notifications for them from the AWS Management Console, or by using the API or CLI\. To receive notifications, provide your unique Sidewalk Amazon ID\. All Sidewalk devices that are registered to this Amazon ID will then be notified of any device registration state events\.
+ To enable event notifications from the console:

  1. In the [Settings](console.aws.amazon.com/iot/home/settings/) tab of the AWS IoT console, go to the **Sidewalk event notification** section and choose **Manage resource events**\.

  1. Choose your **Amazon ID** and choose whether you want to enable event notifications for only **Sidewalk: device registration state** or for **Sidewalk: proximity** events as well\. To disable events, choose your **Amazon ID** and clear the check boxes for events you want to disable\. Choose **Confirm**\.

     You'll see the events that you've added in the **Sidewalk event notification** section\.

  1. To receive notifications, choose the events that you've added, and choose **Subscribe**\.

     All Sidewalk devices that are registered to this Sidewalk Amazon ID will receive a notification when an event occurs\.
+ To use the API or CLI to control which event types are published, call the [UpdateResourceEventConfiguration](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateResourceEventConfiguration.html) API or use the [cli/latest/reference/iotwireless/update-resource-event-configuration.html](cli/latest/reference/iotwireless/update-resource-event-configuration.html) CLI command\. For example:

  ```
  aws iotwireless update-resource-event-configuration \ 
      --event-configurations "{\"DEVICE_REGISTRATION_STATE\":{\"Sidewalk\":{"AmazonIdEventTopic": "Enabled"}}}"
  ```

  To disable events, use the `UpdateResourceEventConfiguration` API or the update\-resource\-event\-configuration CLI command and set `AmazonIdEventTopic` to `DISABLED`\.

## Format of MQTT topics for device registration state events<a name="iot-sidewalk-device-events-mqtt"></a>

To notify you of device registration state events, you can subscribe to MQTT reserved topics that begin with a dollar \($\) sign\. For more information, see [MQTT topics](topics.md)\. 

Reserved MQTT topics for Sidewalk device registration state events use the format:

`$aws/iotwireless/events/device_registration_state/{Possible event Type}/sidewalk/sidewalk_accounts/amazon_id/{id}`

where \{event Type\} can be `provisioned` or `registered`\.

You can also use the `+` wildcard character to subscribe to multiple topics at the same time\. The `+` wildcard character matches any string in the level that contains the character\. For example, if you want to be notified of all possible event types \(`provisioned` and `registered`\) and for all devices registered to a particular Amazon ID, you can use the following topic filter:

`$aws/iotwireless/events/device_registration_state/+/sidewalk/sidewalk_accounts /amazon_id/+`

**Note**  
You can't use the wildcard character `#` to subscribe to the reserved topics\. For more information about topic filters, see [Topic filters](topics.md#topicfilters)\.

## Message payload for device registration state events<a name="iot-sidewalk-device-events-json"></a>

After you enable notifications for device registration state events, event notifications are published over MQTT with a JSON payload\. These events contain the following example payload:

```
{    
    "eventId": "string", 
    "eventType": "provisioned|registered", 
    "WirelessDeviceId": "string",
    "timestamp": "timestamp",
    "operation": "create|deregister|register", 
    "Sidewalk": {
        "AmazonId": "string",
    }
}
```

The payload contains the following attributes:

eventId  
A unique event ID \(string\)\.

eventType  
The type of event that occurred\. Can be `provisioned` or `registered`\.

wirelessDeviceId  
Can be `provisioned` or `registered`\.

timestamp  
The UNIX timestamp of when the event occurred\.

operation  
The operation that triggered the event\. Valid values are `create`, `register`, and `deregister`\. 

sidewalk  
The Sidewalk Amazon ID for which you want to receive event notifications\.

## How device registration state events work<a name="iot-sidewalk-device-events-work"></a>

When you onboard your Sidewalk device with Amazon Sidewalk and AWS IoT Wireless, AWS IoT Wireless performs a `create` operation and adds your Sidewalk device to your AWS account\. Your device then enters the provisioned state and the `eventType` becomes `provisioned`\. For more information about onboarding your device, see [Getting started with Amazon Sidewalk Integration for AWS IoT Core](iot-sidewalk-getting-started.md)\.

After the device has been `provisioned`, Amazon Sidewalk performs a `register` operation to register your Sidewalk device with AWS IoT Wireless\. The registration process starts, where the encryption and session keys are set up with AWS IoT\. When the device is registered, the `eventType` becomes `registered`, and your device is ready to use\.

After the device has been `registered`, Sidewalk can send a request to `deregister` your device\. AWS IoT Wireless then fulfills the request and changes the device state back to `provisioned`\. For more information about the device states, see [DeviceState](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_SidewalkDeviceMetadata.html#iotwireless-Type-SidewalkDeviceMetadata-DeviceState)\. 