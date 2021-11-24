# Event notifications for Amazon Sidewalk Integration for AWS IoT Core<a name="iot-sidewalk-event-messages"></a>

AWS IoT Wireless can publish messages to notify you of events for Amazon Sidewalk devices that you onboard to AWS IoT Core\. For example, you can be notified of events such as when the Sidewalk devices in your account have been provisioned or registered\.

## How your resources can be notified of events<a name="iot-sidewalk-events-mqtt"></a>

Event notifications are published when certain events occur\. For example, events are generated when your Sidewalk device is provisioned\. Each event causes a single event notification to be sent\. Event notifications are published over MQTT with a JSON payload\. The content of the payload depends on the type of event\.

**Note**  
Event notifications are published at least once\. It's possible for them to be published more than once\. The ordering of event notifications is not guaranteed\.

### Policy for receiving Sidewalk event notifications<a name="iot-sidewalk-events-policy"></a>

To receive event notifications, your device must use an appropriate policy that allows it to connect to the AWS IoT device gateway and subscribe to MQTT event topics\. You must also subscribe to the appropriate topic filters\.

The following is an example of the policy required for receiving Sidewalk events\.

```
{
    "Version":"2012-10-17",
    "Statement":[{
        "Effect":"Allow",
        "Action":[
            "iot:Subscribe",
            "iot:Receive"
        ],
        "Resource":[
            "arn:aws:iotwireless:region:account:/$aws/iotwireless/events/device_registration_state/*", 
            "arn:aws:iotwireless:region:account:/$aws/iotwireless/events/proximity/*"
        ]
    }]
}
```

### Format of MQTT topics for Sidewalk events<a name="iot-sidewalk-message-format"></a>

To send you notifications of events for your Sidewalk devices, AWS IoT uses MQTT reserved topics that begin with a dollar \($\) sign\. You can publish and subscribe to these reserved topics\. However, you can't create new topics that begin with a dollar sign\.

**Note**  
MQTT topics are specific to your AWS account and use the format `arn:aws:iotwireless:aws-region:AWS-account-ID:topic/Topic`\. For more information, see [MQTT topics](topics.md)\. 

Reserved MQTT topics for Sidewalk devices use the format:

`$aws/iotwireless/events/{Event name}/{Possible event Type}/sidewalk/Resource Type/Specific resource identifier/{SomeResourceId}`

For example, the following event can be used to notify you of registration events:

`$aws/iotwireless/events/device_registration_state/registered/sidewalk/sidewalk_accounts/amazon_id/{id}`

If you've subscribed to these topics, you'll be notified when a message is published to one of the event notification topics\. For more information, see [Reserved topics](reserved-topics.md)\.

## Enable events for Sidewalk devices<a name="iot-sidewalk-control-events"></a>

Before subscribers to the reserved topics can receive messages, you must enable event notifications\. To do this, you can use the AWS Management Console, or the API or CLI\.
+ To enable event messages from the console, go to the [Settings](console.aws.amazon.com/iot/home/settings/) tab of the AWS IoT console, and in the **Sidewalk event notification** section, choose **Manage resource events**\. You can then specify the events that you want to manage\.
+ To control which event types are published by using the API or CLI, call the [UpdateResourceEventConfiguration](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateResourceEventConfiguration.html) API or use the [cli/latest/reference/iotwireless/update-resource-event-configuration.html](cli/latest/reference/iotwireless/update-resource-event-configuration.html) CLI command\. For example:

  ```
  aws iotwireless update-resource-event-configuration \ 
     --event-configurations "{\"DeviceRegistrationState\":{\"Sidewalk\":{"AmazonIdEventTopic": "Enabled"}}}"
  ```

**Note**  
All quotation marks \("\) are escaped with a backslash \(\\\)\.

You can get the current event configuration by calling the [GetResourceEventConfiguration](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetResourceEventConfiguration.html) API or by using the [cli/latest/reference/iotwireless/get-resource-event-configuration.html](cli/latest/reference/iotwireless/get-resource-event-configuration.html) CLI command\. For example:

```
aws iotwireless get-resource-event-configuration
```

## Pricing for Sidewalk events<a name="iot-sidewalk-events-pricing"></a>

For information about pricing for subscribing to Sidewalk events and for receiving notifications, see [AWS IoT Core pricing](http://aws.amazon.com/iot-core/pricing/)\.

## Event types for Sidewalk devices<a name="iot-sidewalk-event-types"></a>

You can use the AWS Management Console or AWS IoT Wireless APIs to notify you of events for your Sidewalk devices\. These events can be:
+ Device events that notify you of changes to the state of your Sidewalk device, such as when the device has been registered and is ready to use\.
+ Proximity events that notify you when AWS IoT Wireless receives a notification from Amazon Sidewalk that a beacon has been received\.

The following topics provide more information about these different events\.

**Topics**
+ [How your resources can be notified of events](#iot-sidewalk-events-mqtt)
+ [Enable events for Sidewalk devices](#iot-sidewalk-control-events)
+ [Pricing for Sidewalk events](#iot-sidewalk-events-pricing)
+ [Event types for Sidewalk devices](#iot-sidewalk-event-types)
+ [Device registration state events](iot-sidewalk-device-events.md)
+ [Proximity events](iot-sidewalk-proximity-events.md)