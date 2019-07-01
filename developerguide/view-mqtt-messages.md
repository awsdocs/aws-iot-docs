# View Device MQTT Messages with the AWS IoT MQTT Client<a name="view-mqtt-messages"></a>

You can use the AWS IoT MQTT client to better understand the MQTT messages sent by a device\.

Devices publish MQTT messages on topics\. You can use the AWS IoT MQTT client to subscribe to these topics to see these messages\.

**To view MQTT messages:**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot/home), in the left navigation pane, choose **Test**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/choose-test.png)

1. Subscribe to the topic on which your IoT thing publishes\. Continuing with this example, in **Subscribe to a topic**, in the **Subscription topic** field, type **my/topic**, and then choose **Subscribe to topic**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/subscribe-button-topic.png)

   Choosing **Subscribe to topic** above, results in the topic **my/topic** appearing in the **Subscriptions** column\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/subscribed-button-topic.png)

**To emulate an IoT thing sending a message**
+ On the MQTT client page, in the **Publish** section, in the **Specify a topic and a message to publish**… field, type **my/topic**\. 
**Note**  
We do not recommend using personally identifiable information in topic names\.

  In the message payload section, type the following JSON:

  ```
  {
      "message": "Hello, world",
      "clientType": "MQTT client"
  }
  ```

  Choose **Publish to topic**\. You should see the message in the AWS IoT MQTT client \(choose **my/topic** in the **Subscription** column to see the message\)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/publish-to-topic.png)