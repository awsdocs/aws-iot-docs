# View Device MQTT Messages with the AWS IoT MQTT Client<a name="view-mqtt-messages"></a>

You can use the AWS IoT MQTT client to better understand the MQTT messages sent by a device\.

Devices publish MQTT messages on topics\. You can use the AWS IoT MQTT client to subscribe to these topics to see these messages\.

**To view MQTT messages:**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot/home), in the left navigation pane, choose **Test**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/choose-test.png)

1. Subscribe to the topic on which your thing publishes\. In the case of the AWS IoT button, you can subscribe to **iotbutton/\+** \(note that **\+** is the wildcard character\)\. In **Subscribe to a topic**, in the **Subscription topic** field, type **iotbutton/\+**, and then choose **Subscribe to topic**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/subscribe-button-topic.png)

   Choosing **Subscribe to topic** above, results in the topic **iotbutton/\+** appearing in the **Subscriptions** column\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/subscribed-button-topic.png)

1. Press your AWS IoT button, and then view the resulting message in the AWS IoT MQTT client\. If you do not have a button, you will simulate a button press in the next step\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/receive-mqtt-message.png)
**Note**  
The [AWS IoT Button FAQs](https://aws.amazon.com/iotbutton/faq) contains useful button LED color pattern information\.

1. To use the AWS IoT console to publish a message:

   On the MQTT client page, in the **Publish** section, in the **Specify a topic and a message to publish**… field, type **iotbutton/ABCDEFG12345**\. In the message payload section, type the following JSON:

   ```
   {
       "serialNumber": "ABCDEFG12345",
       "clickType": "SINGLE",
       "batteryVoltage": "2000 mV"
   }
   ```

   Choose **Publish to topic**\. You should see the message in the AWS IoT MQTT client \(choose **iotbutton/\+** in the **Subscription** column to see the message\)\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/publish-to-topic.png)