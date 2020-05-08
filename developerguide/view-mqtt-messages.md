# View device MQTT messages with the AWS IoT MQTT client<a name="view-mqtt-messages"></a>

You can use the AWS IoT MQTT client to better understand the MQTT messages sent by a device\.

Devices publish MQTT messages on topics\. You can use the AWS IoT MQTT client to subscribe to these topics to see these messages\.

**To view MQTT messages**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot/home), in the left navigation pane, choose **Test**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/choose-test.png)

1. Subscribe to the topic on which your IoT thing publishes\. Continuing with this example, in **Subscribe to a topic**, in the **Subscription topic** field, enter **my/topic**, and then choose **Subscribe to topic**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/subscribe-button-topic.png)

   The **my/topic** topic appears in the **Subscriptions** column\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/subscribed-button-topic.png)

**To emulate an IoT thing sending a message**
+ On the MQTT client page, in the **Publish** section, in the **Specify a topic and a message to publish** field, enter **my/topic**\. Do not use personally identifiable information in topic names\.

  In the message payload section, enter the following JSON:

  ```
  {
      "message": "Hello, world",
      "clientType": "MQTT client"
  }
  ```

  Choose **Publish to topic**\. You should see the message in the AWS IoT MQTT client\. \(Choose **my/topic** in the **Subscription** column to see the message\.\)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/publish-to-topic.png)