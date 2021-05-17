# Simulating Device Shadow service communications<a name="using-device-shadows"></a>

This topic demonstrates how the Device Shadow service acts as an intermediary and allows devices and apps to use a shadow to update, store, and retrieve a device's state\.

To demonstrate the interaction described in this topic, and to explore it further, you'll need an AWS account and a system on which you can run the AWS CLI\. If you don't have these, you can still see the interaction in the code examples\.

In this example, the AWS IoT console represents the device\. The AWS CLI represents the app or service that accesses the device by way of the shadow\. The AWS CLI interface is very similar to the API that an app might use to communicate with AWS IoT\. The device in this example is a smart light bulb and the app displays the light bulb's state and can change the light bulb's state\.

## Setting up the simulation<a name="using-device-shadows-setup"></a>

These procedures initialize the simulation by opening the [AWS IoT console](https://console.aws.amazon.com/iot/home), which simulates your device, and the command line window that simulates your app\.

**To set up your simulation environment**

1. Create an AWS account or, if you already have one to use for this simulation, you can skip this step\.

   You'll need an AWS account to run the examples from this topic on your own\. If you don't have an AWS account, create one, as described in [Set up your AWS account](setting-up.md)\.

1. Open the [AWS IoT console](https://console.aws.amazon.com/iot/home), and in the left menu, choose **Test** to open the **MQTT client**\.

1. In another window, open a terminal window on a system that has the AWS CLI installed on it\.

You should have two windows open: one with the AWS IoT console on the **Test** page, and one with a command line prompt\.

## Initialize the device<a name="using-device-shadows-init-device"></a>

In this simulation, we'll be working with a thing object named, *mySimulatedThing*, and its shadow named, *simShadow1*\. 

**Create thing object and its named shadow**  
To create a thing object, in the **AWS IoT Console**:

1. Choose **Manage** and then choose **Things**\. 

1. Click the **Create** button if things are listed otherwise click **Register a single thing>** to create a single AWS IoT thing\. 

1. Enter the name `mySimulatedThing`, leave other settings to default, and then click **Next**\.

1. Use one\-click certificate creation to generate the certificates that will authenticate the device's connection to AWS IoT\. Click **Activate** to activate the certificate\.

1. You can attach the policy `My_IoT_Policy` that would give the device permission to publish and subscribe to the MQTT reserved topics\. For more detailed steps about how to create an AWS IoT thing and how to create this policy, see [Create a thing object](create-iot-resources.md#create-aws-thing)\.

By default, every AWS IoT thing object has a single, classic shadow\. You can create a shadow by publishing an update request to the topic `$aws/things/mySimulatedThing/shadow/name/simShadow1/update` as described below, Alternatively, in the **AWS IoT Console**, choose your thing object in the list of things displayed and then choose **Shadows**\. Choose **Add a shadow**, enter the name `simShadow1`, and then choose **Create** to add the named shadow\.

**Subscribe and publish to reserved MQTT topics**  
In the console, subscribe to the reserved MQTT shadow topics\. These topics are the responses to the `get`, `update`, and `delete` actions so that your device will be ready to receive the responses after it publishes an action\. 

**To subscribe to an MQTT topic in the **MQTT client****

1. In the **MQTT client**, choose **Subscribe to topic**\.

1. Enter the `get`, `update`, and `delete` topics to subscribe to\. Copy one topic at a time from the following list, paste it in the **Topic filter** field, and then click **Subscribe**\. You should see the topics appear under **Subscriptions**\.
   + `$aws/things/mySimulatedThing/shadow/name/simShadow1/delete/accepted`
   + `$aws/things/mySimulatedThing/shadow/name/simShadow1/delete/rejected`
   + `$aws/things/mySimulatedThing/shadow/name/simShadow1/get/accepted`
   + `$aws/things/mySimulatedThing/shadow/name/simShadow1/get/rejected`
   + `$aws/things/mySimulatedThing/shadow/name/simShadow1/update/accepted`
   + `$aws/things/mySimulatedThing/shadow/name/simShadow1/update/rejected`
   + `$aws/things/mySimulatedThing/shadow/name/simShadow1/update/delta`
   + `$aws/things/mySimulatedThing/shadow/name/simShadow1/update/documents`

   At this point, your simulated device is ready to receive the topics as they are published by AWS IoT\.

**To publish to an MQTT topic in the **MQTT client****  
After a device has initialized itself and subscribed to the response topics, it should query for the shadows it supports\. This simulation supports only one shadow, the shadow that supports a thing object named, *mySimulatedThing*, named, *simShadow1*\.

**To get the current shadow state from the **MQTT client****

1. In the **MQTT client**, choose **Publish to a topic**\.

1. Under **Publish**, enter the following topic and delete any content from the message body window below where you entered the topic to get\. You can then choose **Publish to topic** to publish the request\. `$aws/things/mySimulatedThing/shadow/name/simShadow1/get`\.

   If you haven't created the named shadow, `simShadow1`, you receive a message in the `$aws/things/mySimulatedThing/shadow/name/simShadow1/get/rejected` topic and the `code` is `404`, such as in this example as the shadow has not been created, so we'll create it next\.

   ```
   {
     "code": 404,
     "message": "No shadow exists with name: 'simShadow1'"
   }
   ```

**To create a shadow with the current status of the device**

1. In the **MQTT client**, choose **Publish to a topic** and enter this topic:

   ```
   $aws/things/mySimulatedThing/shadow/name/simShadow1/update
   ```

1. In the message body window below where you entered the topic, enter this shadow document to show the device is reporting its ID and its current color in RGB values\. Choose **Publish** to publish the request\.

   ```
   {
     "state": {
       "reported": {
         "ID": "SmartLamp21",
         "ColorRGB": [
           128,
           128,
           128
         ]
       }
     },
     "clientToken": "426bfd96-e720-46d3-95cd-014e3ef12bb6"
   }
   ```

If you receive a message in the topic: 
+ `$aws/things/mySimulatedThing/shadow/name/simShadow1/update/accepted`: It means that the shadow was created and the message body contains the current shadow document\.
+ `$aws/things/mySimulatedThing/shadow/name/simShadow1/update/rejected`: Review the error in the message body\.
+ `$aws/things/mySimulatedThing/shadow/name/simShadow1/get/accepted`: The shadow already exists and the message body has the current shadow state, such as in this example\. With this, you could set your device or confirm that it matches the shadow state\.

  ```
  {
    "state": {
      "reported": {
        "ID": "SmartLamp21",
        "ColorRGB": [
          128,
          128,
          128
        ]
      }
    },
    "metadata": {
      "reported": {
        "ID": {
          "timestamp": 1591140517
        },
        "ColorRGB": [
          {
            "timestamp": 1591140517
          },
          {
            "timestamp": 1591140517
          },
          {
            "timestamp": 1591140517
          }
        ]
      }
    },
    "version": 3,
    "timestamp": 1591140517,
    "clientToken": "426bfd96-e720-46d3-95cd-014e3ef12bb6"
  }
  ```

## Send an update from the app<a name="using-device-shadows-app-update"></a>

This section uses the AWS CLI to demonstrate how an app can interact with a shadow\.

**To get the current state of the shadow using the AWS CLI**  
From the command line, enter this command\.

```
aws iot-data get-thing-shadow --thing-name mySimulatedThing --shadow-name simShadow1 /dev/stdout
```

On Windows platforms, you can use `con` instead of `/dev/stdout`\.

```
aws iot-data get-thing-shadow --thing-name mySimulatedThing --shadow-name simShadow1 con
```

Because the shadow exists and had been initialized by the device to reflect its current state, it should return the following shadow document\.

```
{
  "state": {
    "reported": {
      "ID": "SmartLamp21",
      "ColorRGB": [
        128,
        128,
        128
      ]
    }
  },
  "metadata": {
    "reported": {
      "ID": {
        "timestamp": 1591140517
      },
      "ColorRGB": [
        {
          "timestamp": 1591140517
        },
        {
          "timestamp": 1591140517
        },
        {
          "timestamp": 1591140517
        }
      ]
    }
  },
  "version": 3,
  "timestamp": 1591141111
}
```

The app can use this response to initialize its representation of the device state\.

If the app updates the state, such as when an end user changes the color of our smart light bulb to yellow, the app would send an update\-thing\-shadow command\. This command corresponds to the `UpdateThingShadow` REST API\.

**To update a shadow from an app**  
From the command line, enter this command\.

------
#### [ AWS CLI v2\.x ]

```
aws iot-data update-thing-shadow --thing-name mySimulatedThing --shadow-name simShadow1 \
    --cli-binary-format raw-in-base64-out \
    --payload '{"state":{"desired":{"ColorRGB":[255,255,0]}},"clientToken":"21b21b21-bfd2-4279-8c65-e2f697ff4fab"}' /dev/stdout
```

------
#### [ AWS CLI v1\.x ]

```
aws iot-data update-thing-shadow --thing-name mySimulatedThing --shadow-name simShadow1 \
    --payload '{"state":{"desired":{"ColorRGB":[255,255,0]}},"clientToken":"21b21b21-bfd2-4279-8c65-e2f697ff4fab"}' /dev/stdout
```

------

If successful, this command should return the following shadow document\.

```
{
  "state": {
    "desired": {
      "ColorRGB": [
        255,
        255,
        0
      ]
    }
  },
  "metadata": {
    "desired": {
      "ColorRGB": [
        {
          "timestamp": 1591141596
        },
        {
          "timestamp": 1591141596
        },
        {
          "timestamp": 1591141596
        }
      ]
    }
  },
  "version": 4,
  "timestamp": 1591141596,
  "clientToken": "21b21b21-bfd2-4279-8c65-e2f697ff4fab"
}
```

## Respond to update in device<a name="using-device-shadows-device-update"></a>

Returning to the **MQTT client** in the AWS console, you should see the messages that AWS IoT published to reflect the update command issued in the previous section\.

**To view the update messages in the **MQTT client****  
In the **MQTT client**, choose **$aws/things/mySimulatedThing/shadow/name/simShadow1/update/delta** in the **Subscriptions** column\. If the topic name is truncated, you can pause on it to see the full topic\. In the topic log of this topic, you should see a `/delta` message similar to this one\.

```
{
  "version": 4,
  "timestamp": 1591141596,
  "state": {
    "ColorRGB": [
      255,
      255,
      0
    ]
  },
  "metadata": {
    "ColorRGB": [
      {
        "timestamp": 1591141596
      },
      {
        "timestamp": 1591141596
      },
      {
        "timestamp": 1591141596
      }
    ]
  },
  "clientToken": "21b21b21-bfd2-4279-8c65-e2f697ff4fab"
}
```

Your device would process the contents of this message to set the device state to match the `desired` state in the message\.

After the device updates the state to match the `desired` state in the message, it must send the new reported state back to AWS IoT by publishing an update message\. This procedure simulates this in the **MQTT client**\.

**To update the shadow from the device**

1. In the **MQTT client**, choose **Publish to a topic**\.

1. In the message body window , in the topic field above the message body window, enter the shadow's topic followed by the `/update` action: `$aws/things/mySimulatedThing/shadow/name/simShadow1/update` and in the message body, enter this updated shadow document, which describes the current state of the device\. Click **Publish** to publish the updated device state\.

   ```
   {
     "state": {
       "reported": {
         "ColorRGB": [255,255,0]
         }
     },
     "clientToken": "a4dc2227-9213-4c6a-a6a5-053304f60258"
   }
   ```

   If the message was successfully received by AWS IoT, you should see a new response in the **$aws/things/mySimulatedThing/shadow/name/simShadow1/update/accepted** message log in the **MQTT client** with the current state of the shadow, such as this example\.

   ```
   {
     "state": {
       "reported": {
         "ColorRGB": [
           255,
           255,
           0
         ]
       }
     },
     "metadata": {
       "reported": {
         "ColorRGB": [
           {
             "timestamp": 1591142747
           },
           {
             "timestamp": 1591142747
           },
           {
             "timestamp": 1591142747
           }
         ]
       }
     },
     "version": 5,
     "timestamp": 1591142747,
     "clientToken": "a4dc2227-9213-4c6a-a6a5-053304f60258"
   }
   ```

A successful update to the reported state of the device also causes AWS IoT to send a comprehensive description of the shadow state in a message to the `` topic, such as this message body that resulted from the shadow update performed by the device in the preceding procedure\.

```
{
  "previous": {
    "state": {
      "desired": {
        "ColorRGB": [
          255,
          255,
          0
        ]
      },
      "reported": {
        "ID": "SmartLamp21",
        "ColorRGB": [
          128,
          128,
          128
        ]
      }
    },
    "metadata": {
      "desired": {
        "ColorRGB": [
          {
            "timestamp": 1591141596
          },
          {
            "timestamp": 1591141596
          },
          {
            "timestamp": 1591141596
          }
        ]
      },
      "reported": {
        "ID": {
          "timestamp": 1591140517
        },
        "ColorRGB": [
          {
            "timestamp": 1591140517
          },
          {
            "timestamp": 1591140517
          },
          {
            "timestamp": 1591140517
          }
        ]
      }
    },
    "version": 4
  },
  "current": {
    "state": {
      "desired": {
        "ColorRGB": [
          255,
          255,
          0
        ]
      },
      "reported": {
        "ID": "SmartLamp21",
        "ColorRGB": [
          255,
          255,
          0
        ]
      }
    },
    "metadata": {
      "desired": {
        "ColorRGB": [
          {
            "timestamp": 1591141596
          },
          {
            "timestamp": 1591141596
          },
          {
            "timestamp": 1591141596
          }
        ]
      },
      "reported": {
        "ID": {
          "timestamp": 1591140517
        },
        "ColorRGB": [
          {
            "timestamp": 1591142747
          },
          {
            "timestamp": 1591142747
          },
          {
            "timestamp": 1591142747
          }
        ]
      }
    },
    "version": 5
  },
  "timestamp": 1591142747,
  "clientToken": "a4dc2227-9213-4c6a-a6a5-053304f60258"
}
```

## Observe the update in the app<a name="using-device-shadows-view-result"></a>

The app can now query the shadow for the current state as reported by the device\.

**To get the current state of the shadow using the AWS CLI**

1. From the command line, enter this command\.

   ```
   aws iot-data get-thing-shadow --thing-name mySimulatedThing --shadow-name simShadow1 /dev/stdout
   ```

   On Windows platforms, you can use `con` instead of `/dev/stdout`\.

   ```
   aws iot-data get-thing-shadow --thing-name mySimulatedThing --shadow-name simShadow1 con
   ```

1. Because the shadow has just been updated by the device to reflect its current state, it should return the following shadow document\.

   ```
   {
     "state": {
       "desired": {
         "ColorRGB": [
           255,
           255,
           0
         ]
       },
       "reported": {
         "ID": "SmartLamp21",
         "ColorRGB": [
           255,
           255,
           0
         ]
       }
     },
     "metadata": {
       "desired": {
         "ColorRGB": [
           {
             "timestamp": 1591141596
           },
           {
             "timestamp": 1591141596
           },
           {
             "timestamp": 1591141596
           }
         ]
       },
       "reported": {
         "ID": {
           "timestamp": 1591140517
         },
         "ColorRGB": [
           {
             "timestamp": 1591142747
           },
           {
             "timestamp": 1591142747
           },
           {
             "timestamp": 1591142747
           }
         ]
       }
     },
     "version": 5,
     "timestamp": 1591143269
   }
   ```

## Going beyond the simulation<a name="using-device-shadows-next-steps"></a>

Experiment with the interaction between the AWS CLI \(representing the app\) and the console \(representing the device\) to model your IoT solution\.