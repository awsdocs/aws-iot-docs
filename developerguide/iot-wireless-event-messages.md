# Event notifications for AWS IoT Wireless<a name="iot-wireless-event-messages"></a>

AWS IoT Wireless can publish messages to notify you of events for LoRaWAN and Sidewalk devices that you onboard to AWS IoT Core\. For example, you can be notified of events such as when the Sidewalk devices in your account have been provisioned or registered\.

## How your resources can be notified of events<a name="iot-wireless-events-mqtt"></a>

Event notifications are published when certain events occur\. For example, events are generated when your Sidewalk device is provisioned\. Each event causes a single event notification to be sent\. Event notifications are published over MQTT with a JSON payload\. The content of the payload depends on the type of event\.

**Note**  
Event notifications are published at least once\. It's possible for them to be published more than once\. The ordering of event notifications is not guaranteed\.

### Events and resource types<a name="iot-wireless-events-types"></a>

The following table shows the different types of events for which you'll receive notifications\. The event types depend on whether the resource type is a wireless device, a wireless gateway, or a Sidewalk account\. You can also enable events for your resources at the resource level, which applies to all resources of a particular type, or for select resources, as described in the following section\. For more information about the different event types, see [Event notifications for LoRaWAN resources](iot-lorawan-events.md) and [Event notifications for Sidewalk resources](iot-sidewalk-events.md)\.


**Event types based on resources**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iot-wireless-event-messages.html)

### Policy for receiving wireless event notifications<a name="iot-wireless-events-policy"></a>

To receive event notifications, your device must use an appropriate policy that allows it to connect to the AWS IoT device gateway and subscribe to MQTT event topics\. You must also subscribe to the appropriate topic filters\.

The following is an example of the policy required for receiving notifications for the various wireless events\.

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
            "arn:aws:iotwireless:region:account:/$aws/iotwireless/events/join/*",
            "arn:aws:iotwireless:region:account:/$aws/iotwireless/events/connection_status/*",
            "arn:aws:iotwireless:region:account:/$aws/iotwireless/events/device_registration_state/*", 
            "arn:aws:iotwireless:region:account:/$aws/iotwireless/events/proximity/*",
            "arn:aws:iotwireless:region:account:/$aws/iotwireless/events/message_delivery_status/*"
        ]
    }]
}
```

### Format of MQTT topics for wireless events<a name="iot-wireless-message-format"></a>

To send you notifications of events for your wireless resources, AWS IoT uses MQTT reserved topics that begin with a dollar sign \($\)\. You can publish and subscribe to these reserved topics\. However, you can't create new topics that begin with a dollar sign\.

**Note**  
MQTT topics are specific to your AWS account and use the format `arn:aws:iotwireless:aws-region:AWS-account-ID:topic/Topic`\. For more information, see [MQTT topics](topics.md)\.

Reserved MQTT topics for wireless devices use the following format:
+ 

**Resource\-level topics**  
These topics apply to all resources of a particular type in your AWS account that you've onboarded to AWS IoT Wireless\.

  `$aws/iotwireless/events/{eventName}/{eventType}/{resourceType}/resources`
+ 

**Identifier\-level topics**  
These topics apply to select resources of a particular type in your AWS account that you've onboarded to AWS IoT Wireless, specified by the resource identifier\.

  `$aws/iotwireless/events/{eventName}/{eventType}/{resourceType}/{resourceIdentifierType}/{resourceID}/{id}`

For more information about topics at resource and identifier levels, see [Event configurations](iot-wireless-control-events.md#iot-wireless-control-events-config)\.

The following table shows examples of MQTT topics for the various events:


**Events and MQTT topics**  

| Event | MQTT topic | Notes | 
| --- | --- | --- | 
| Sidewalk device registration state |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iot-wireless-event-messages.html)  | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iot-wireless-event-messages.html) | 
| Sidewalk proximity |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iot-wireless-event-messages.html)  | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iot-wireless-event-messages.html) | 
| Sidewalk message delivery status |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iot-wireless-event-messages.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iot-wireless-event-messages.html)  | 
| LoRaWAN join |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iot-wireless-event-messages.html)  | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iot-wireless-event-messages.html) | 
| LoRaWAN gateway connection status |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iot-wireless-event-messages.html)  | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iot-wireless-event-messages.html) | 

For more information about the different events, see [Event notifications for LoRaWAN resources](iot-lorawan-events.md) and [Event notifications for Sidewalk resources](iot-sidewalk-events.md)\.

If you've subscribed to these topics, you'll be notified when a message is published to one of the event notification topics\. For more information, see [Reserved topics](reserved-topics.md)\.

## Pricing for wireless events<a name="iot-wireless-events-pricing"></a>

For information about pricing for subscribing to events and for receiving notifications, see [AWS IoT Core pricing](http://aws.amazon.com/iot-core/pricing/)\.