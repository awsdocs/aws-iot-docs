# Step 3: Demonstrate subscribing to messages with the AWS IoT Device Client<a name="iot-dc-testconn-subscribe"></a>

In this section, you'll demonstrate two types of message subscriptions:
+ Single topic subscription
+ Wild\-card topic subscription

These policy statements in the policy created for these exercises give the Raspberry Pi permission to perform these actions:
+ 

**`iot:Receive`**  
Gives the AWS IoT Device Client permission to receive MQTT topics that match those named in the `Resource` object\.

  ```
      {
        "Effect": "Allow",
        "Action": [
          "iot:Receive"
        ],
        "Resource": [
          "arn:aws:iot:us-west-2:57EXAMPLE833:topic/test/dc/subtopic"
        ]
      }
  ```
+ 

**`iot:Subscribe`**  
Gives the AWS IoT Device Client permission to subscribe to MQTT topic filters that match those named in the `Resource` object\.

  ```
      {
        "Effect": "Allow",
        "Action": [
          "iot:Subscribe"
        ],
        "Resource": [
          "arn:aws:iot:us-west-2:57EXAMPLE833:topicfilter/test/dc/subtopic"
        ]
      }
  ```

## Subscribe to a single MQTT message topic<a name="iot-dc-testconn-subscribe-simple-topic"></a>

This procedure demonstrates how the AWS IoT Device Client can subscribe to and log MQTT messages\.

In the terminal window on your local host computer that's connected to your Raspberry Pi, list the contents of **\~/dc\-configs/dc\-pubsub\-custom\-config\.json** or open the file in a text editor to review its contents\. Locate the `samples` object, which should look like this\.

```
  "samples": {
    "pub-sub": {
      "enabled": true,
      "publish-topic": "test/dc/pubtopic",
      "publish-file": "~/messages/sample-ws-message.json",
      "subscribe-topic": "test/dc/subtopic",
      "subscribe-file": "~/.aws-iot-device-client/log/pubsub_rx_msgs.log"
```

Notice the `subscribe-topic` value is the MQTT topic to which the AWS IoT Device Client will subscribe when it runs\. The AWS IoT Device Client writes the message payloads that it receives from this subscription to the file named in the `subscribe-file` value\.

**To subscribe to a MQTT message topic from the AWS IoT Device Client**

1. Make sure that both the terminal window and the window with the MQTT test client are visible while you perform this procedure\. Also, make sure that your **MQTT test client** is still subscribed to the **\#** topic filter\. If it isn't, subscribe to the **\#** topic filter again\.

1. In the terminal window, enter these commands to run the AWS IoT Device Client using the config file created in [Create the config file](iot-dc-install-configure.md#iot-dc-install-dc-configure-step1)\.

   ```
   cd ~/aws-iot-device-client/build
   ./aws-iot-device-client --config-file ~/dc-configs/dc-pubsub-custom-config.json
   ```

   In the terminal window, the AWS IoT Device Client displays information messages and any errors that occur when it runs\.

   If no errors are displayed in the terminal window, continue in the AWS IoT console\.

1. In the AWS IoT console, in the **MQTT test client**, choose the **Publish to a topic** tab\.

1. In **Topic name**, enter **test/dc/subtopic**

1. In **Message payload**, review the message contents\.

1. Choose **Publish** to publish the MQTT message\.

1. In the terminal window, watch for the *message received * entry from the AWS IoT Device Client that looks like this\.

   ```
   2021-11-10T16:02:20.890Z [DEBUG] {samples/PubSubFeature.cpp}: Message received on subscribe topic, size: 45 bytes
   ```

1. After you see the *message received * entry that shows the message was received, enter **^C** \(Ctrl\-C\) to stop the AWS IoT Device Client\.

1. Enter this command to view the end of the message log file and see the message you published from the **MQTT test client**\.

   ```
   tail ~/.aws-iot-device-client/log/pubsub_rx_msgs.log
   ```

By viewing the message in the log file, you've demonstrated that the AWS IoT Device Client received the message that you published from the MQTT test client\.

## Subscribe to multiple MQTT message topic using wildcard characters<a name="iot-dc-testconn-subscribe-wild-topic"></a>

These procedures demonstrate how the AWS IoT Device Client can subscribe to and log MQTT messages using wildcard characters\. To do this, you'll:

1. Update the topic filter that the AWS IoT Device Client uses to subscribe to MQTT topics\.

1. Update the policy used by the device to allow the new subscriptions\.

1. Run the AWS IoT Device Client and publish messages from the MQTT test console\.

**To create a config file to subscribe to multiple MQTT message topics by using a wildcard MQTT topic filter**

1. In the terminal window on your local host computer that's connected to your Raspberry Pi, open **\~/dc\-configs/dc\-pubsub\-custom\-config\.json** for editing and locate the `samples` object\.

1. In the text editor, locate the `samples` object and update the `subscribe-topic` value to look like this\. 

   ```
     "samples": {
       "pub-sub": {
         "enabled": true,
         "publish-topic": "test/dc/pubtopic",
         "publish-file": "~/messages/sample-ws-message.json",
         "subscribe-topic": "test/dc/#",
         "subscribe-file": "~/.aws-iot-device-client/log/pubsub_rx_msgs.log"
   ```

   The new `subscribe-topic` value is an [MQTT topic filter](topics.md#topicfilters) with an MQTT wild card character at the end\. This describes a subscription to all MQTT topics that start with `test/dc/`\. The AWS IoT Device Client writes the message payloads that it receives from this subscription to the file named in `subscribe-file`\.

1. Save the modified config file as **\~/dc\-configs/dc\-pubsub\-wild\-config\.json**, and exit the editor\.

**To modify the policy used by your Raspberry Pi to allow subscribing to and receiving multiple MQTT message topics**

1. In the terminal window on your local host computer that's connected to your Raspberry Pi, in your favorite text editor, open **\~/policies/pubsub\_test\_thing\_policy\.json** for editing, and then locate the `iot::Subscribe` and `iot::Receive` policy statements in the file\.

1. In the `iot::Subscribe` policy statement, update the string in the Resource object to replace `subtopic` with `*`, so that it looks like this\.

   ```
       {
         "Effect": "Allow",
         "Action": [
           "iot:Subscribe"
         ],
         "Resource": [
           "arn:aws:iot:us-west-2:57EXAMPLE833:topicfilter/test/dc/*"
         ]
       }
   ```
**Note**  
The [MQTT topic filter wild card characters](topics.md#topicfilters) are the `+` \(plus sign\) and the `#` \(pound sign\)\. A subscription request with a `#` at the end subscribes to all topics that start with the string that precedes the `#` character \(for example, `test/dc/` in this case\)\.   
The resource value in the policy statement that authorizes this subscription, however, must use a `*` \(an asterisk\) in place of the `#` \(a pound sign\) in the topic filter ARN\. This is because the policy processor uses a different wild card character than MQTT uses\.  
For more information about using wild card characters for topics and topic filters in policies, see [Policies for MQTT clients](pub-sub-policy.md#pub-sub-policy-cert)\.

1. In the `iot::Receive` policy statement, update the string in the Resource object to replace `subtopic` with `*`, so that it looks like this\.

   ```
       {
         "Effect": "Allow",
         "Action": [
           "iot:Receive"
         ],
         "Resource": [
           "arn:aws:iot:us-west-2:57EXAMPLE833:topic/test/dc/*"
         ]
       }
   ```

1. Save the updated policy document as **\~/policies/pubsub\_wild\_test\_thing\_policy\.json**, and exit the editor\.

1. Enter this command to update the policy for this tutorial to use the new resource definitions\.

   ```
   aws iot create-policy-version \
   --set-as-default \
   --policy-name "PubSubTestThingPolicy" \
   --policy-document "file://~/policies/pubsub_wild_test_thing_policy.json"
   ```

   If the command succeeds, it returns a response like this\. Notice that `policyVersionId` is now `2`, indicating this is the second version of this policy\. 

   If you successfully updated the policy, you can continue to the next procedure\.

   ```
   {
       "policyArn": "arn:aws:iot:us-west-2:57EXAMPLE833:policy/PubSubTestThingPolicy",
       "policyDocument": "{\n  \"Version\": \"2012-10-17\",\n  \"Statement\": [\n    {\n      \"Effect\": \"Allow\",\n      \"Action\": [\n        \"iot:Connect\"\n      ],\n      \"Resource\": [\n        \"arn:aws:iot:us-west-2:57EXAMPLE833:client/PubSubTestThing\"\n      ]\n    },\n    {\n      \"Effect\": \"Allow\",\n      \"Action\": [\n        \"iot:Publish\"\n      ],\n      \"Resource\": [\n        \"arn:aws:iot:us-west-2:57EXAMPLE833:topic/test/dc/pubtopic\"\n      ]\n    },\n    {\n      \"Effect\": \"Allow\",\n      \"Action\": [\n        \"iot:Subscribe\"\n      ],\n      \"Resource\": [\n        \"arn:aws:iot:us-west-2:57EXAMPLE833:topicfilter/test/dc/*\"\n      ]\n    },\n    {\n      \"Effect\": \"Allow\",\n      \"Action\": [\n        \"iot:Receive\"\n      ],\n      \"Resource\": [\n        \"arn:aws:iot:us-west-2:57EXAMPLE833:topic/test/dc/*\"\n      ]\n    }\n  ]\n}\n",
       "policyVersionId": "2",
       "isDefaultVersion": true
   }
   ```

   If you get an error that there are too many policy versions to save a new one, enter this command to list the current versions of the policy\. Review the list that this command returns to find a policy version that you can delete\.

   ```
   aws iot list-policy-versions --policy-name "PubSubTestThingPolicy"
   ```

   Enter this command to delete a version that you no longer need\. Note that you can't delete the default policy version\. The default policy version is the one with a `isDefaultVersion` value of `true`\.

   ```
   aws iot delete-policy-version \
   --policy-name "PubSubTestThingPolicy" \
   --policy-version-id policyId
   ```

   After deleting a policy version, retry this step\.

With the updated config file and policy, you're ready to demonstrate wild card subscriptions with the AWS IoT Device Client\.

**To demonstrate how the AWS IoT Device Client subscribes to and receives multiple MQTT message topics**

1. In the **MQTT test client**, check the subscriptions\. If the **MQTT test client** is subscribed to the to the in the **\#** topic filter, continue to the next step\. If not, in the **MQTT test client**, in **Subscribe to a topic** tab, in **Topic filter**, enter **\#** \(a pound sign character\), and then choose **Subscribe** to subscribe to it\.

1. In the terminal window on your local host computer that's connected to your Raspberry Pi, enter these commands to start the AWS IoT Device Client\.

   ```
   cd ~/aws-iot-device-client/build
   ./aws-iot-device-client --config-file ~/dc-configs/dc-pubsub-wild-config.json
   ```

1. While watching the AWS IoT Device Client output in the terminal window on the local host computer, return to the **MQTT test client**\. In the **Publish to a topic** tab, in **Topic name**, enter **test/dc/subtopic** , and then choose **Publish**\. 

1. In the terminal window, confirm that the message was received by looking for a message such as:

   ```
   2021-11-10T16:34:20.101Z [DEBUG] {samples/PubSubFeature.cpp}: Message received on subscribe topic, size: 76 bytes
   ```

1. While watching the AWS IoT Device Client output in the terminal window of the local host computer, return to the **MQTT test client**\. In the **Publish to a topic** tab, in **Topic name**, enter **test/dc/subtopic2** , and then choose **Publish**\. 

1. In the terminal window, confirm that the message was received by looking for a message such as:

   ```
   2021-11-10T16:34:32.078Z [DEBUG] {samples/PubSubFeature.cpp}: Message received on subscribe topic, size: 77 bytes
   ```

1. After you see the messages that confirm both messages were received, enter **^C** \(Ctrl\-C\) to stop the AWS IoT Device Client\.

1. Enter this command to view the end of the message log file and see the message you published from the **MQTT test client**\.

   ```
   tail -n 20 ~/.aws-iot-device-client/log/pubsub_rx_msgs.log
   ```
**Note**  
The log file contains only message payloads\. The message topics are not recorded in the received message log file\.  
You might also see the message published by the AWS IoT Device Client in the received log\. This is because the wild card topic filter includes that message topic and, sometimes, the subscription request can be processed by message broker before the published message is sent to subscribers\.

The entries in the log file demonstrate that the messages were received\. You can repeat this procedure using other topic names\. All messages that have a topic name that begins with `test/dc/` should be received and logged\. Messages with topic names that begin with any other text are ignored\.

After demonstrating how the AWS IoT Device Client can publish and subscribe to MQTT messages, continue to [Tutorial: Demonstrate remote actions \(jobs\) with the AWS IoT Device Client](iot-dc-runjobs.md)\.