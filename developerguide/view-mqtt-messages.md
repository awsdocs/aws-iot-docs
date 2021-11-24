# View MQTT messages with the AWS IoT MQTT client<a name="view-mqtt-messages"></a>

This section describes how to use the AWS IoT MQTT client in the [AWS IoT console](https://console.aws.amazon.com/iot/home) to watch the MQTT messages sent and received by AWS IoT\. The example used in this section relates to the examples used in [Getting started with AWS IoT Core](iot-gs.md); however, you can replace the *topicName* used in the examples with any [topic name or topic filter](topics.md) used by your IoT solution\. 

Devices publish MQTT messages that are identified by [topics](topics.md) to communicate their state to AWS IoT, and AWS IoT publishes MQTT messages to inform the devices and apps of changes and events\. You can use the MQTT client to subscribe to these topics and watch the messages as they occur\. You can also use the MQTT client to publish MQTT messages to subscribed devices and services in your AWS account\. 

## Viewing MQTT messages in the MQTT client<a name="view-mqtt-subscribe"></a>

**To view MQTT messages in the MQTT client**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot/home), in the left menu, choose **Test** and then choose **MQTT test client**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/choose-test.png)

1. In the **Subscribe to a topic** tab, enter the *topicName* to subscribe to the topic on which your device publishes\. For the getting started sample app, subscribe to **\#**, which subscribes to all message topics\.

   Continuing with the getting started example, on the **Subscribe to a topic** tab, in the **Topic filter** field, enter **\#**, and then choose **Subscribe**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/subscribe-button-topic.png)

   The topic message log page, **\#** opens and **\#** appears in the **Subscriptions** list\. If the device that you configured in [Configure your device](configure-device.md) is running the example program, you should see the messages it sends to AWS IoT in the **\#** message log\. The message log entries will appear below the **Publish** section when messages with the subscribed topic are received by AWS IoT\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/subscribed-button-topic.png)

1. On the **\#** message log page, you can also publish messages to a topic, but you'll need to specify the topic name\. You cannot publish to the **\#** topic\.

   Messages published to subscribed topics appear in the message log as they are received, with the most recent message first\.

### Troubleshooting MQTT messages<a name="view-mqtt-trouble"></a>

**Use the wild card topic filter**  
If your messages are not showing up in the message log as you expect, try subscribing to a wild card topic filter as described in [Topic filters](topics.md#topicfilters)\. The MQTT multi\-level wild card topic filter is the hash or pound sign \( `#` \) and can be used as the topic filter in the **Subscription topic** field\. 

Subscribing to the `#` topic filter subscribes to every topic received by the message broker\. You can narrow the filter down by replacing elements of the topic filter path with a `#` multi\-level wild card character or the '\+' single\-level wild\-card character\. 

**When using wild cards in a topic filter**
+ The multi\-level wild card character must be the last character in the topic filter\.
+ The topic filter path can have only one single\-level wild card character per topic level\. 

For example:


|  Topic filter  |  Displays messages with  | 
| --- | --- | 
|   `#`   |   Any topic name   | 
|   `topic_1/#`   |   A topic name that starts with `topic_1/`  | 
|   `topic_1/level_2/#`   |   A topic name that starts with `topic_1/level_2/`  | 
|   `topic_1/+/level_3`   |   A topic name that starts with `topic_1/`, ends with `/level_3`, and has one element of any value in between\.  | 

For more information on topic filters, see [Topic filters](topics.md#topicfilters)\.

**Check for topic name errors**  
MQTT topic names and topic filters are case sensitive\. If, for example, your device is publishing messages to `Topic_1` \(with a capital *T*\) instead of `topic_1`, the topic to which you subscribed, its messages would not appear in the MQTT client\. Subscribing to the wild card topic filter, however would show that the device is publishing messages and you could see that it was using a topic name that was not the one you expected\.

## Publishing MQTT messages from the MQTT client<a name="view-mqtt-publish"></a>

**To publish a message to an MQTT topic**

1. On the MQTT client page, in the **Publish to a topic** tab, in the **Topic name** field, enter the *topicName* of your message\. In this example, use **my/topic**\. 
**Note**  
Do not use personally identifiable information in topic names, whether using them in the MQTT client or in your system implementation\. Topic names can appear in unencrypted communications and reports\.

1. In the message payload window, enter the following JSON:

   ```
   {
       "message": "Hello, world",
       "clientType": "MQTT client"
   }
   ```

1. Choose **Publish** to publish your message to AWS IoT\.
**Note**  
Make sure you are subscribed to the **my/topic** topic before publishing your message\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/publish-to-topic.png)

1. In the **Subscriptions** list, choose **my/topic** to see the message\. You should see the message appear in the MQTT client below the publish message payload window\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/publish-to-topic-received.png)

You can publish MQTT messages to other topics by changing the *topicName* in the **Topic name** field and choosing the **Publish** button\.