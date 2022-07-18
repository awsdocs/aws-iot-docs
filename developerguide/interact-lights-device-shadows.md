# Tutorial: Interacting with Device Shadow using the sample app and the MQTT test client<a name="interact-lights-device-shadows"></a>

To interact with the `shadow.py` sample app, enter a value in the terminal for the `desired` value\. For example, you can specify colors that resemble the traffic lights and AWS IoT responds to the request and updates the reported values\.

**In this tutorial, you'll learn how to:**
+ Use the `shadow.py` sample app to specify desired states and update the shadow's current state\.
+ Edit the Shadow document to observe delta events and how the `shadow.py` sample app responds to it\.
+ Use the MQTT test client to subscribe to shadow topics and observe updates when you run the sample program\.

**Before you run this tutorial, you must have:**  
Set up your AWS account, configured your Raspberry Pi device, and created an AWS IoT thing and policy\. You must have also installed the required software, Device SDK, certificate files, and run the sample program in the terminal\. For more information, see the previous tutorials [Tutorial: Preparing your Raspberry Pi to run the shadow application](create-resources-shadow.md) and [Step 1: Run the shadow\.py sample app](lightbulb-shadow-application.md#run-sample-application-shadows)\. You must complete these tutorials if you haven't already\.

**Topics**
+ [Step 1: Update desired and reported values using `shadow.py` sample app](#update-desired-shadow-sample)
+ [Step 2: View messages from the `shadow.py` sample app in the MQTT test client](#shadow-sample-view-msg)
+ [Step 3: Troubleshoot errors with Device Shadow interactions](#shadow-observe-messages-troubleshoot)
+ [Step 4: Review the results and next steps](#sample-shadow-review)

This tutorial takes about 45 minutes to complete\.

## Step 1: Update desired and reported values using `shadow.py` sample app<a name="update-desired-shadow-sample"></a>

In the previous tutorial [Step 1: Run the shadow\.py sample app](lightbulb-shadow-application.md#run-sample-application-shadows), you learned how to observe a message published to the Shadow document in the AWS IoT console when you enter a desired value as described in the section [Tutorial: Installing the Device SDK and running the sample application for Device Shadows](lightbulb-shadow-application.md)\.

In the previous example, we set the desired color to `yellow`\. After you enter each value, the terminal prompts you to enter another `desired` value\. If you again enter the same value \(`yellow`\), the app recognizes this and prompts you to enter a new `desired` value\.

```
Enter desired value:
yellow
Local value is already 'yellow'.
Enter desired value:
```

Now, say that you enter the color `green`\. AWS IoT responds to the request and updates the `reported` value to `green`\. This is how the update happens when the `desired` state is different from the `reported` state, causing a delta\.

**How the `shadow.py` sample app simulates Device Shadow interactions:**

1. Enter a `desired` value \(say `yellow`\) in the terminal to publish the desired state\.

1. As the `desired` state is different from the `reported` state \(say the color `green`\), a delta occurs, and the app that is subscribed to the delta receives this message\.

1. The app responds to the message and updates its state to the `desired` value, `yellow`\.

1. The app then publishes an update message with the new reported value of the device's state, `yellow`\.

Following shows the messages displayed in the terminal that shows how the update request is published\.

```
Enter desired value:
green
Changed local shadow value to 'green'.
Updating reported shadow value to 'green'...
Update request published.
Finished updating reported shadow value to 'green'.
```

In the AWS IoT console, the Shadow document reflects the updated value to `green` for both the `reported` and `desired` fields, and the version number is incremented by 1\. For example, if the previous version number was displayed as 10, the current version number will display as 11\.

**Note**  
Deleting a shadow doesn't reset the version number to 0\. You'll see that the shadow version is incremented by 1 when you publish an update request or create another shadow with the same name\.

**Edit the Shadow document to observe delta events**  
The `shadow.py` sample app is also subscribed to `delta` events, and responds when there is a change to the `desired` value\. For example, you can change the `desired` value to the color `red`\. To do this, in the AWS IoT console, edit the Shadow document by clicking **Edit** and then set the `desired` value to `red` in the JSON, while keeping the `reported` value to `green`\. Before you save the changes, keep the terminal on the Raspberry Pi open as you'll see messages displayed in the terminal when the change occurs\.

```
{
"desired": {
  "welcome": "aws-iot",
  "color": "red"
},
"reported": {
  "welcome": "aws-iot",
  "color": "green"
}
}
```

After you save the new value, the `shadow.py` sample app responds to this change and displays messages in the terminal indicating the delta\. You should then see the following messages appear below the prompt for entering the `desired` value\.

```
Enter desired value:
Received shadow delta event.
Delta reports that desired value is 'red'. Changing local value...
Changed local shadow value to 'red'.
Updating reported shadow value to 'red'...
Finished updating reported shadow value to 'red'.
Enter desired value:
Update request published.
Finished updating reported shadow value to 'red'.
```

## Step 2: View messages from the `shadow.py` sample app in the MQTT test client<a name="shadow-sample-view-msg"></a>

You can use the **MQTT test client** in the **AWS IoT console** to monitor MQTT messages that are passed in your AWS account\. By subscribing to reserved MQTT topics used by the Device Shadow service, you can observe the messages received by the topics when running the sample app\.

If you haven't already used the MQTT test client, you can review [View MQTT messages with the AWS IoT MQTT client](view-mqtt-messages.md)\. This helps you learn how to use the **MQTT test client** in the **AWS IoT console** to view MQTT messages as they pass through the message broker\.

1. 

**Open the MQTT test client**

   Open the [MQTT test client in the AWS IoT console](https://console.aws.amazon.com/iot/home#/test) in a new window so that you can observe the messages received by the MQTT topics without losing the configuration of your MQTT test client\. The MQTT test client doesn't retain any subscriptions or message logs if you leave it to go to another page in the console\. For this section of the tutorial, you can have the Shadow document of your AWS IoT thing and the MQTT test client open in separate windows to more easily observe the interaction with Device Shadows\.

1. 

**Subscribe to the MQTT reserved Shadow topics**

   You can use the MQTT test client to enter the names of the Device Shadow's MQTT reserved topics and subscribe to them to receive updates when running the `shadow.py` sample app\. To subscribe to the topics:

   1. In the **MQTT test client** in the **AWS IoT console**, choose **Subscribe to a topic**\.

   1.  In the **Topic filter** section, enter: **$aws/things/*thingname*/shadow/update/\#**\. Here, `thingname` is the name of the thing resource that you created earlier \(for example, `My_light_bulb`\)\.

   1. Keep the default values for the additional configuration settings, and then choose **Subscribe**\.

   By using the **\#** wildcard in the topic subscription, you can subscribe to multiple MQTT topics at the same time and observe all the messages that are exchanged between the device and its Shadow in a single window\. For more information about the wildcard characters and their use, see [MQTT topics](topics.md)\.

1. 

**Run `shadow.py` sample program and observe messages**

   In your command line window of the Raspberry Pi, if you've disconnected the program, run the sample app again and watch the messages in the **MQTT test client** in the **AWS IoT console**\.

   1. Run the following command to restart the sample program\. Replace *your\-iot\-thing\-name* and *your\-iot\-endpoint* with the names of the AWS IoT thing that you created earlier \(for example, `My_light_bulb`\), and the endpoint to interact with the device\. 

      ```
      cd ~/aws-iot-device-sdk-python-v2/samples
      python3 shadow.py --ca_file ~/certs/Amazon-root-CA-1.pem --cert ~/certs/device.pem.crt --key ~/certs/private.pem.key --endpoint your-iot-endpoint --thing_name your-iot-thing-name
      ```

      The `shadow.py` sample app then runs and retrieves the current shadow state\. If you've deleted the shadow or cleared the current states, the program sets the current value to `off` and then prompts you to enter a `desired` value\.

      ```
      Connecting to a3qEXAMPLEffp-ats.iot.us-west-2.amazonaws.com with client ID 'test-0c8ae2ff-cc87-49d2-a82a-ae7ba1d0ca5a'...
      Connected!
      Subscribing to Delta events...
      Subscribing to Update responses...
      Subscribing to Get responses...
      Requesting current shadow state...
      Launching thread to read user input...
      Finished getting initial shadow state.
      Shadow document lacks 'color' property. Setting defaults...
      Changed local shadow value to 'off'.
      Updating reported shadow value to 'off'...
      Update request published.
      Finished updating reported shadow value to 'off'...
      Enter desired value:
      ```

      On the other hand, if the program was running and you restarted it, you'll see the latest color value reported in the terminal\. In the MQTT test client, you'll see an update to the topics **$aws/things/*thingname*/shadow/get** and **$aws/things/*thingname*/shadow/get/accepted**\.

      Suppose that the latest color reported was `green`\. Following shows the contents of the **$aws/things/*thingname*/shadow/get/accepted** JSON file\.

      ```
      {
      "state": {
        "desired": {
          "welcome": "aws-iot",
          "color": "green"
        },
        "reported": {
          "welcome": "aws-iot",
          "color": "green"
        }
      },
      "metadata": {
        "desired": {
          "welcome": {
            "timestamp": 1620156892
          },
          "color": {
            "timestamp": 1620161643
          }
        },
        "reported": {
          "welcome": {
            "timestamp": 1620156892
          },
          "color": {
            "timestamp": 1620161643
          }
        }
      },
      "version": 10,
      "timestamp": 1620173908
      }
      ```

   1. Enter a `desired` value in the terminal, such as `yellow`\. The `shadow.py` sample app responds and displays the following messages in the terminal that show the change in the `reported` value to `yellow`\.

      ```
      Enter desired value:
      yellow
      Changed local shadow value to 'yellow'.
      Updating reported shadow value to 'yellow'...
      Update request published.
      Finished updating reported shadow value to 'yellow'.
      ```

      In the **MQTT test client** in the **AWS IoT console**, under **Subscriptions**, you see that the following topics received a message:
      + **$aws/things/*thingname*/shadow/update**: shows that both `desired` and `updated` values change to the color `yellow`\.
      + **$aws/things/*thingname*/shadow/update/accepted**: shows the current values of the `desired` and `reported` states and their metadata and version information\.
      + **$aws/things/*thingname*/shadow/update/documents**: shows the previous and current values of the `desired` and `reported` states and their metadata and version information\.

      As the document **$aws/things/*thingname*/shadow/update/documents** also contains information that is contained in the other two topics, we can review it to see the state information\. The previous state shows the reported value set to `green`, its metadata and version information, and the current state that shows the reported value updated to `yellow`\.

      ```
      {
      "previous": {
        "state": {
          "desired": {
            "welcome": "aws-iot",
            "color": "green"
          },
          "reported": {
            "welcome": "aws-iot",
            "color": "green"
          }
        },
        "metadata": {
          "desired": {
            "welcome": {
              "timestamp": 1617297888
            },
            "color": {
              "timestamp": 1617297898
            }
          },
          "reported": {
            "welcome": {
              "timestamp": 1617297888
            },
            "color": {
              "timestamp": 1617297898
            }
          }
        },
        "version": 10
      },
      "current": {
        "state": {
          "desired": {
            "welcome": "aws-iot",
            "color": "yellow"
          },
          "reported": {
            "welcome": "aws-iot",
            "color": "yellow"
          }
        },
        "metadata": {
          "desired": {
            "welcome": {
              "timestamp": 1617297888
            },
            "color": {
              "timestamp": 1617297904
            }
          },
          "reported": {
            "welcome": {
              "timestamp": 1617297888
            },
            "color": {
              "timestamp": 1617297904
            }
          }
        },
        "version": 11
      },
      "timestamp": 1617297904
      }
      ```

   1. Now, if you enter another `desired` value, you see further changes to the `reported` values and message updates received by these topics\. The version number also increments by 1\. For example, if you enter the value `green`, the previous state reports the value `yellow` and the current state reports the value `green`\.

1. 

**Edit Shadow document to observe delta events**

   To observe changes to the delta topic, edit the Shadow document in the AWS IoT console\. For example, you can change the `desired` value to the color `red`\. To do this, in the AWS IoT console, choose **Edit** and then set the `desired` value to red in the JSON, while keeping the `reported` value set to `green`\. Before you save the change, keep the terminal open as you'll see the delta message reported in the terminal\.

   ```
   {
   "desired": {
     "welcome": "aws-iot",
     "color": "red"
   },
   "reported": {
     "welcome": "aws-iot",
     "color": "green"
   }
   }
   ```

   The `shadow.py` sample app responds to this change and displays messages in the terminal indicating the delta\. In the MQTT test client, the `update` topics will have received a message showing changes to the `desired` and `reported` values\.

   You also see that the topic **$aws/things/*thingname*/shadow/update/delta** received a message\. To see the message, choose this topic, which is listed under **Subscriptions**\.

   ```
   {
   "version": 13,
   "timestamp": 1617318480,
   "state": {
     "color": "red"
   },
   "metadata": {
     "color": {
       "timestamp": 1617318480
     }
   }
   }
   ```

## Step 3: Troubleshoot errors with Device Shadow interactions<a name="shadow-observe-messages-troubleshoot"></a>

When you run the Shadow sample app, you might encounter issues with observing interactions with the Device Shadow service\. 

If the program runs successfully and prompts you to enter a `desired` value, you should be able to observe the Device Shadow interactions by using the Shadow document and the MQTT test client as described previously\. However, if you're unable to see the interactions, here are some things you can check:
+ 

**Check the thing name and its shadow in the AWS IoT console**  
If you don't see the messages in the Shadow document, review the command and make sure it matches the thing name in the **AWS IoT console**\. You can also check whether you have a classic shadow by choosing your thing resource and then choosing **Shadows**\. This tutorial focuses primarily on interactions with the classic shadow\.

   You can also confirm that the device you used is connected to the internet\. In the **AWS IoT console**, choose the thing you created earlier, and then choose **Interact**\. On the thing details page, you should see a message here that says: `This thing already appears to be connected.` 
+ 

**Check the MQTT reserved topics you subscribed to**  
If you don't see the messages appear in the MQTT test client, check whether the topics you subscribed to are formatted correctly\. MQTT Device Shadow topics have a format **$aws/things/*thingname*/shadow/** and might have `update`, `get`, or `delete` following it depending on actions you want to perform on the shadow\. This tutorial uses the topic **$aws/things/*thingname*/shadow/\#** so make sure you entered it correctly when subscribing to the topic in the **Topic filter** section of the test client\.

  As you enter the topic name, make sure that the *thingname* is the same as the name of the AWS IoT thing that you created earlier\. You can also subscribe to additional MQTT topics to see if an update has been successfully performed\. For example, you can subscribe to the topic **$aws/things/*thingname*/shadow/update/rejected** to receive a message whenever an update request failed so that you can debug connection issues\. For more information about the reserved topics, see [Shadow topics](reserved-topics.md#reserved-topics-shadow) and [Device Shadow MQTT topics](device-shadow-mqtt.md)\.

## Step 4: Review the results and next steps<a name="sample-shadow-review"></a>

**In this tutorial, you learned how to:**
+ Use the `shadow.py` sample app to specify desired states and update the shadow's current state\.
+ Edit the Shadow document to observe delta events and how the `shadow.py` sample app responds to it\.
+ Use the MQTT test client to subscribe to shadow topics and observe updates when you run the sample program\.

**Next steps**  
You can subscribe to additional MQTT reserved topics to observe updates to the shadow application\. For example, if you only subscribe to the topic **$aws/things/*thingname*/shadow/update/accepted**, you'll see only the current state information when an update is successfully performed\.

You can also subscribe to additional shadow topics to debug issues or learn more about the Device Shadow interactions and also debug any issues with the Device Shadow interactions\. For more information, see [Shadow topics](reserved-topics.md#reserved-topics-shadow) and [Device Shadow MQTT topics](device-shadow-mqtt.md)\.

You can also choose to extend your application by using named shadows or by using additional hardware connected with the Raspberry Pi for the LEDs and observe changes to their state using messages sent from the terminal\.

For more information about the Device Shadow service and using the service in devices, apps, and services, see [AWS IoT Device Shadow service](iot-device-shadows.md), [Using shadows in devices](device-shadow-comms-device.md), and [Using shadows in apps and services](device-shadow-comms-app.md)\.