# Step 3: Send and Receive Test Data for the Thing<a name="iot-plant-step3"></a>

In this step, you practice sending test data to the device shadow for the Raspberry Pi \(or your development computer as a device simulator\)\. In a later step, you either send real data from the soil moisture sensor kit through the Raspberry Pi to its device shadow, or you send simulated data from your development computer to its device shadow\.

A device's shadow is a JSON document, stored in AWS IoT , that AWS IoT uses to save and retrieve current state information for a device\. The [Device Shadow Service for AWS IoT](iot-device-shadows.md) maintains a shadow for each device you connect to AWS IoT \. You can use the shadow to get and set the state of a device, regardless of whether the device is connected to the internet\.

1. In the [ AWS IoT console](https://console.aws.amazon.com/iot/home), on the **Things** page, choose **MyRPi**\.  
![\[AWS IoT console Manage, Things page, with the thing named MyRPi highlighted.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-open-thing.png)

1. Choose **Interact**\.  
![\[Interact page for the thing named MyRPi.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-interact.png)

1. For MQTT, make a note of the value for each of the following MQTT topics, which enable you to set and get updates to the shadow:
   + **Update to this device shadow** \(for example, `$aws/things/MyRPi/shadow/update`\)
   + **Get this device shadow** \(for example, `$aws/things/MyRPi/shadow/get`\)
   + **Get this device shadow accepted** \(for example, `$aws/things/MyRPi/shadow/get/accepted`\)  
![\[Interact page for the thing named MyRPi, with several MQTT topics highlighted.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-mqtt-topic-list.png)

1. Choose the back button\.

1. If an **Introducing AWS IoT Device Management** dialog box is displayed, choose **Show me later**, or press **Esc**\.

1. In the service navigation pane, choose **Test**\.  
![\[AWS IoT navigation pane with Test highlighted.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-test.png)

1. For **Subscription topic**, enter the MQTT topic value that you noted in step 3 of this procedure for **Update to this device shadow** \(for example, **$aws/things/MyRPi/shadow/update**\), and then choose **Subscribe to topic**\.  
![\[Test page showing the MQTT client details, with $aws/things/MyRPi/shadow/update and Subscribe to topic highlighted.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-subscribe-topic.png)
**Important**  
If you named your thing something other than MyRPi, be sure to substitute your thing’s name for MyRPi in the preceding MQTT topic name and other MQTT topic names throughout Step 3\. Otherwise, the subscribed MQTT topic won’t display any activity\.

1. Choose **Subscribe to a topic**\.  
![\[Test page showing the MQTT client details, with Subscribe to a topic highlighted.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-subscribe-topic.png)

1. Repeat steps 7 and 8 in this procedure for the MQTT topic values that you noted for **Get this device shadow** \(for example, **$aws/things/MyRPi/shadow/get**\) and **Get this device shadow accepted** \(for example, **$aws/things/MyRPi/shadow/get/accepted**\)\.

1. Now push some test data into the shadow\. To do this, in the MQTT client navigation pane, choose the MQTT topic value for **Update to this device shadow** \(for example, **$aws/things/MyRPi/shadow/update**\)\. You might need to pause your mouse over a truncated topic value to see its full value\.

1. 11\. In the message payload area, replace the current payload with the following payload:

   ```
   {
     "state": {
       "desired": {
         "welcome": null
       },
       "reported": {
         "welcome": null,
         "moisture": "low"
       }
     }
   }
   ```

   The preceding payload removes the default welcome value for the shadow and adds a moisture value with the value `low` to the shadow\.

1. Choose **Publish to topic**\.  
![\[Test page showing the MQTT client details, with Subscribe to a topic highlighted.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-update-shadow.png)

1. To get that data from the shadow, choose the MQTT topic value for **Get this device shadow** \(for example, **$aws/things/MyRPi/shadow/get**\)\.

1. In the message payload area, replace the current payload with the following payload:

   ```
   {}
   ```

   You specify empty curly braces here because the **Get this thing shadow** MQTT topic takes only an empty payload\.

1. Choose **Publish to topic**\.  
![\[Test page showing the MQTT client details for $aws/things/MyRPi/shadow/get, with Publish to topic highlighted.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-get-shadow.png)

   A green dot is displayed next to the MQTT value for **Get this device shadow accepted**\. This means that there is new information displayed for that MQTT topic\.  
![\[Test page showing the MQTT client with a dot next to $aws/things/MyRPi/shadow/get/accepted to indicate this MQTT topic has new information.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-get-shadow-received.png)

1. Choose the MQTT topic value for **Get this device shadow accepted** \(for example, `$aws/things/MyRPi/shadow/get/accepted`\), and note the output, for example:

   ```
   {
     "state": {
       "reported": {
         "moisture": "low"
       }
     },
     "metadata": {
       "reported": {
         "moisture": {
           "timestamp": 1539272338
         }
       }
     },
     "version": 19,
     "timestamp": 1539272436
   }
   ```

   In the preceding output, the `moisture` value that was reported earlier is shown, along with the time each corresponding event happened and the current shadow document version\.

1. Make another update to the shadow\. To do this, in the MQTT client navigation pane, choose the MQTT topic value for **Update to this device shadow** \(for example, **$aws/things/MyRPi/shadow/update**\)\.

1. In the message payload area, replace the current payload with the following payload to change the current moisture value:

   ```
   {
     "state": {
       "reported": {
         "moisture": "okay"
       }
     }
   }
   ```

1. Choose **Publish to topic**\.

1. Choose the MQTT topic value for **Get this device shadow** \(for example, **$aws/things/MyRPi/shadow/get**\)\.

1. In the message payload area, replace the current payload with the following payload:

   ```
   {}
   ```

1. Choose **Publish to topic**\. A green dot is displayed next to the MQTT value for **Get this device shadow accepted**\.

1. Choose the MQTT topic value for **Get this device shadow accepted** \(for example, **$aws/things/MyRPi/shadow/get/accepted**\), and note the output, for example:

   ```
   {
     "state": {
       "reported": {
         "moisture": "okay"
       }
     },
     "metadata": {
       "reported": {
         "moisture": {
           "timestamp": 1539272823
         }
       }
     },
     "version": 20,
     "timestamp": 1539272827
   }
   ```

   In the preceding output, the `moisture` value that was just changed is shown, along with the time that each corresponding event happened and the new current shadow document version\.