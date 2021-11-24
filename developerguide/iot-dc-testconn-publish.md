# Step 2: Demonstrate publishing messages with the AWS IoT Device Client<a name="iot-dc-testconn-publish"></a>

The procedures in this section demonstrate how the AWS IoT Device Client can send default and custom MQTT messages\.

These policy statements in the policy that you created in the previous step for these exercises give the Raspberry Pi permission to perform these actions:
+ 

**`iot:Connect`**  
Gives the client named `PubSubTestThing`, your Raspberry Pi running the AWS IoT Device Client, to connect\.

  ```
      {
        "Effect": "Allow",
        "Action": [
          "iot:Connect"
        ],
        "Resource": [
          "arn:aws:iot:us-west-2:57EXAMPLE833:client/PubSubTestThing"
        ]
      }
  ```
+ 

**`iot:Publish`**  
Gives the Raspberry Pi permission to publish messages with an MQTT topic of `test/dc/pubtopic`\.

  ```
      {
        "Effect": "Allow",
        "Action": [
          "iot:Publish"
        ],
        "Resource": [
          "arn:aws:iot:us-west-2:57EXAMPLE833:topic/test/dc/pubtopic"
        ]
      }
  ```

  The `iot:Publish` action gives permission to publish to the MQTT topics listed in the Resource array\. The *content* of those messages is not controlled by the policy statement\.

## Publish the default message using the AWS IoT Device Client<a name="iot-dc-testconn-publish-default"></a>

This procedure runs the AWS IoT Device Client so that it publishes a single default MQTT message that the **MQTT test client** receives and displays\.

**To send the default MQTT message from the AWS IoT Device Client**

1. Make sure that both the terminal window on your local host computer that's connected to your Raspberry Pi and the window with the **MQTT test client** are visible while you perform this procedure\.

1. In the terminal window, enter these commands to run the AWS IoT Device Client using the config file created in [Create the config file](iot-dc-install-configure.md#iot-dc-install-dc-configure-step1)\.

   ```
   cd ~/aws-iot-device-client/build
   ./aws-iot-device-client --config-file ~/dc-configs/dc-pubsub-config.json
   ```

   In the terminal window, the AWS IoT Device Client displays information messages and any errors that occur when it runs\.

   If no errors are displayed in the terminal window, review the **MQTT test client**\.

1. In the **MQTT test client**, in the **Subscriptions** window, see the *Hello World\!* message sent to the `test/dc/pubtopic` message topic\.

1. If the AWS IoT Device Client displays no errors and you see *Hello World\!* sent to the `test/dc/pubtopic` message in the **MQTT test client**, you've demonstrated a successful connection\.

1. In the terminal window, enter **^C** \(Ctrl\-C\) to stop the AWS IoT Device Client\.

After you've demonstrated that the AWS IoT Device Client published the default MQTT message, you can continue to the [Publish a custom message using the AWS IoT Device Client\.](#iot-dc-testconn-publish-custom)\.

## Publish a custom message using the AWS IoT Device Client\.<a name="iot-dc-testconn-publish-custom"></a>

The procedures in this section create a custom MQTT message and then runs the AWS IoT Device Client so that it publishes the custom MQTT message one time for the **MQTT test client** to receive and display\.

### Create a custom MQTT message for the AWS IoT Device Client<a name="iot-dc-testconn-publish-custom-create"></a>

Perform these steps in the terminal window on the local host computer that's connected to your Raspberry Pi\.

**To create a custom message for the AWS IoT Device Client to publish**

1. In the terminal window, open a text editor, such as `nano`\.

1. Into the text editor, copy and paste the following JSON document\. This will be the MQTT message payload that the AWS IoT Device Client publishes\.

   ```
   {
     "temperature": 28,
     "humidity": 80,
     "barometer": 1013,
     "wind": {
       "velocity": 22,
       "bearing": 255
     }
   }
   ```

1. Save the contents of the text editor as **\~/messages/sample\-ws\-message\.json**\. 

1. Enter the following command to set the permissions of the message file that you just created\.

   ```
   chmod 600 ~/messages/*
   ```

**To create a config file for the AWS IoT Device Client to use to send the custom message**

1. In the terminal window, in a text editor such as `nano`, open the existing AWS IoT Device Client config file: **\~/dc\-configs/dc\-pubsub\-config\.json**\. 

1. Edit the `samples` object to look like this\. No other part of this file needs to be changed\.

   ```
     "samples": {
       "pub-sub": {
         "enabled": true,
         "publish-topic": "test/dc/pubtopic",
         "publish-file": "~/messages/sample-ws-message.json",
         "subscribe-topic": "test/dc/subtopic",
         "subscribe-file": "~/.aws-iot-device-client/log/pubsub_rx_msgs.log"
   ```

1. Save the contents of the text editor as **\~/dc\-configs/dc\-pubsub\-custom\-config\.json**\. 

1. Run this command to set the permissions on the new config file\.

   ```
   chmod 644 ~/dc-configs/dc-pubsub-custom-config.json
   ```

### Publish the custom MQTT message by using the AWS IoT Device Client<a name="iot-dc-testconn-publish-custom-publish"></a>

This change affects only the *contents* of the MQTT message payload, so the current policy will continue to work\. However, if the *MQTT topic* \(as defined by the `publish-topic` value in `~/dc-configs/dc-pubsub-custom-config.json`\) was changed, the `iot::Publish` policy statement would also need to be modified to allow the Raspberry Pi to publish to the new MQTT topic\.

**To send the MQTT message from the AWS IoT Device Client**

1. Make sure that both the terminal window and the window with the **MQTT test client** are visible while you perform this procedure\. Also, make sure that your **MQTT test client** is still subscribed to the **\#** topic filter\. If it isn't, subscribe to the **\#** topic filter again\.

1. In the terminal window, enter these commands to run the AWS IoT Device Client using the config file created in [Create the config file](iot-dc-install-configure.md#iot-dc-install-dc-configure-step1)\.

   ```
   cd ~/aws-iot-device-client/build
   ./aws-iot-device-client --config-file ~/dc-configs/dc-pubsub-custom-config.json
   ```

   In the terminal window, the AWS IoT Device Client displays information messages and any errors that occur when it runs\.

   If no errors are displayed in the terminal window, review the MQTT test client\.

1. In the **MQTT test client**, in the **Subscriptions** window, see the custom message payload sent to the `test/dc/pubtopic` message topic\.

1. If the AWS IoT Device Client displays no errors and you see the custom message payload that you published to the `test/dc/pubtopic` message in the **MQTT test client**, you've published a custom message successfully\.

1. In the terminal window, enter **^C** \(Ctrl\-C\) to stop the AWS IoT Device Client\.

After you've demonstrated that the AWS IoT Device Client published a custom message payload, you can continue to [Step 3: Demonstrate subscribing to messages with the AWS IoT Device Client](iot-dc-testconn-subscribe.md)\.