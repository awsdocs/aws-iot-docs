# Device Shadow Service Data Flow<a name="device-shadow-data-flow"></a>

The Device Shadow service acts as an intermediary, allowing devices and applications to retrieve and update a device's shadow\. 

To illustrate how devices and applications communicate with the Device Shadow service, this section walks you through the use of the AWS IoT MQTT client and the AWS CLI to simulate communication between an internet\-connected light bulb, an application, and the Device Shadow service\. 

The Device Shadow service uses MQTT topics to facilitate communication between applications and devices\. To see how this works, use the AWS IoT MQTT client to subscribe to the following MQTT topics with QoS 1:

$aws/things/myLightBulb/shadow/update/accepted  
The Device Shadow service sends messages to this topic when an update is successfully made to the device's shadow\. 

$aws/things/myLightBulb/shadow/update/rejected  
The Device Shadow service sends messages to this topic when an update to the device's shadow is rejected\. 

$aws/things/myLightBulb/shadow/update/delta  
The Device Shadow service sends messages to this topic when a difference is detected between the reported and desired sections of the device's shadow\. For more information, see [/update/delta](device-shadow-mqtt.md#update-delta-pub-sub-topic)\. 

$aws/things/myLightBulb/shadow/get/accepted  
The Device Shadow service sends messages to this topic when a request for the device's shadow is made successfully\. 

$aws/things/myLightBulb/shadow/get/rejected  
The Device Shadow service sends messages to this topic when a request for the device's shadow is rejected\. 

$aws/things/myLightBulb/shadow/delete/accepted  
The Device Shadow service sends messages to this topic when the device's shadow is deleted\. 

$aws/things/myLightBulb/shadow/delete/rejected  
The Device Shadow service sends messages to this topic when a request to delete the device's shadow is rejected\. 

$aws/things/myLightBulb/shadow/update/documents  
The Device Shadow service publishes a state document to this topic whenever an update to the device's shadow is successfully performed\.

To learn more about all of the MQTT topics used by the Device Shadow service, see [Shadow MQTT Topics](device-shadow-mqtt.md)\.

**Note**  
We recommend that you subscribe to the `.../rejected` topics to see any errors sent by the Device Shadow service\.

When the light bulb comes online, it sends its current state to the Device Shadow service by sending an MQTT message to the `$aws/things/myLightBulb/shadow/update` topic\.

 To simulate this, use the AWS IoT MQTT client to publish the following message to the `$aws/things/myLightbulb/shadow/update` topic:

```
{
    "state": {
        "reported": {
            "color": "red"
        }
    }
}
```

This message sets the color of the light bulb to "red\."

The Device Shadow service responds by sending the following message to the `$aws/things/myLightBulb/shadow/update/accepted` topic:

```
{
  "messageNumber": 4,
  "payload": {
    "state": {
      "reported": {
        "color": "red"
      }
    },
    "metadata": {
      "reported": {
        "color": {
          "timestamp": 1469564492
        }
      }
    },
    "version": 1,
    "timestamp": 1469564492
  },
  "qos": 0,
  "timestamp": 1469564492848,
  "topic": "$aws/things/myLightBulb/shadow/update/accepted"
}
```

This message indicates the Device Shadow service received the UPDATE request and updated the device's shadow\. If the shadow doesn't exist, it is created\. Otherwise, the shadow is updated with the data in the message\. If you don't see a message published to `$aws/things/myLightBulb/shadow/update/accepted`, check the subscription to `$aws/things/myLightBulb/shadow/update/rejected` to see any error messages\.

In addition, the Device Shadow service publishes the following message to the `$aws/things/myLightBulb/shadow/update/documents` topic\.

```
{
    "previous":null,
    "current":{
        "state":{
            "reported":{
                "color":"red"
            }
         },
         "metadata":{
            "reported":{
                "color":{
                    "timestamp":1483467764
                }
            }
         },
         "version":1
     },
     "timestamp":1483467764
}
```

Messages are published to the `/update/documents` topic whenever an update to the device's shadow is successfully performed\. For more information of the contents of messages published to this topic, see [Shadow MQTT Topics](device-shadow-mqtt.md)\.

An application that interacts with the light bulb comes online and requests the light bulb's current state\. The application sends an empty message to the `$aws/things/myLightBulb/shadow/get` topic\. To simulate this, use the AWS IoT MQTT client to publish an empty message \(""\) to the `$aws/things/myLightBulb/shadow/get` topic\.

The Device Shadow service responds by publishing the requested shadow to the `$aws/things/myLightBulb/shadow/get/accepted` topic:

```
{
  "messageNumber": 1,
  "payload": {
    "state": {
      "reported": {
        "color": "red"
      }
    },
    "metadata": {
      "reported": {
        "color": {
          "timestamp": 1469564492
        }
      }
    },
    "version": 1,
    "timestamp": 1469564571
  },
  "qos": 0,
  "timestamp": 1469564571533,
  "topic": "$aws/things/myLightBulb/shadow/get/accepted"
}
```

If you don't see a message on the `$aws/things/myLightBulb/shadow/get/accepted` topic, check the `$aws/things/myLightBulb/shadow/get/rejected` topic for any error messages\.

The application displays this information to the user, and the user requests a change to the light bulb's color \(from red to green\)\. To do this, the application publishes a message on the `$aws/things/myLightBulb/shadow/update` topic: 

```
{
    "state": {
        "desired": {
            "color": "green" 
        } 
    }
}
```

To simulate this, use the AWS IoT MQTT client to publish the preceding message to the `$aws/things/myLightBulb/shadow/update` topic\.

The Device Shadow service responds by sending a message to the `$aws/things/myLightBulb/shadow/update/accepted` topic:

```
{
  "messageNumber": 5,
  "payload": {
    "state": {
      "desired": {
        "color": "green"
      }
    },
    "metadata": {
      "desired": {
        "color": {
          "timestamp": 1469564658
        }
      }
    },
    "version": 2,
    "timestamp": 1469564658
  },
  "qos": 0,
  "timestamp": 1469564658286,
  "topic": "$aws/things/myLightBulb/shadow/update/accepted"
}
```

and to the `$aws/things/myLightBulb/shadow/update/delta` topic:

```
{
  "messageNumber": 1,
  "payload": {
    "version": 2,
    "timestamp": 1469564658,
    "state": {
      "color": "green"
    },
    "metadata": {
      "color": {
        "timestamp": 1469564658
      }
    }
  },
  "qos": 0,
  "timestamp": 1469564658309,
  "topic": "$aws/things/myLightBulb/shadow/update/delta"
}
```

The Device Shadow service publishes a message to this topic when it accepts a shadow update and the resulting shadow contains different values for desired and reported states\.

The Device Shadow service also publishes a message to the `$aws/things/myLightBulb/shadow/update/documents` topic:

```
{
    "previous":{
        "state":{
            "reported":{
                "color":"red"
            }
        },
        "metadata":{
            "reported":{
                "color":{
                    "timestamp":1483467764
                }
            }
        },
        "version":1
    },
    "current":{
        "state":{
            "desired":{
                "color":"green"
            },
            "reported":{
                "color":"red"
            }
        },
        "metadata":{
            "desired":{
                "color":{
                    "timestamp":1483468612
                }
            },
            "reported":{
                "color":{
                    "timestamp":1483467764
                }
            }
       },
       "version":2
   },
   "timestamp":1483468612
}
```

The light bulb is subscribed to the `$aws/things/myLightBulb/shadow/update/delta` topic, so it receives the message, changes its color, and publishes its new state\. To simulate this, use the AWS IoT MQTT client to publish the following message to the `$aws/things/myLightbulb/shadow/update` topic to update the shadow state:

```
{
    "state":{
        "reported":{
            "color":"green"
        },
        "desired":null}
    }
}
```

In response, the Device Shadow service sends a message to the `$aws/things/myLightBulb/shadow/update/accepted` topic:

```
{
  "messageNumber": 6,
  "payload": {
    "state": {
      "reported": {
        "color": "green"
      },
      "desired": null
    },
    "metadata": {
      "reported": {
        "color": {
          "timestamp": 1469564801
        }
      },
      "desired": {
        "timestamp": 1469564801
      }
    },
    "version": 3,
    "timestamp": 1469564801
  },
  "qos": 0,
  "timestamp": 1469564801673,
  "topic": "$aws/things/myLightBulb/shadow/update/accepted"
}
```

and to the `$aws/things/myLightBulb/shadow/update/documents` topic:

```
{
    "previous":{
    "state":{
        "reported":{
            "color":"red"
        }
    },
    "metadata":{
         "reported":{
              "color":{
                  "timestamp":1483470355
              }
          }
      },
      "version":3
    },
    "current":{
        "state":{
            "reported":{
                "color":"green"
            }
        },
        "metadata":{
            "reported":{
                "color":{
                    "timestamp":1483470364
                }
            }
        },
        "version":4
    },
    "timestamp":1483470364
}
```

The app requests the current state from the Device Shadow service and displays the most recent state data\. To simulate this, run the following command:

```
aws iot-data get-thing-shadow --thing-name "myLightBulb" "output.txt" && cat "output.txt"
```

**Note**  
On Windows, omit the `&& cat "output.txt"`, which displays the contents of output\.txt to the console\. You can open the file in Notepad or any text editor to see the contents of the shadow\.

The Device Shadow service returns the shadow document:

```
{
    "state":{
        "reported":{
            "color":"green"
        }
    },
    "metadata":{
        "reported":{
            "color":{
                "timestamp":1469564801
            }
        }
    },
    "version":3,
    "timestamp":1469564864}
```

To delete the device's shadow, publish an empty message to the `$aws/things/myLightBulb/shadow/delete` topic\. AWS IoT responds by publishing a message to the `$aws/things/myLightBulb/shadow/delete/accepted` topic:

```
{
  "version" : 1,
  "timestamp" : 1488565234
}
```

## Detecting a Thing Is Connected<a name="thing-connection"></a>

To determine if a device is currently connected, include a connected setting in the shadow and use an MQTT Last Will and Testament \(LWT\) message that sets the connected setting to `false` if a device is disconnected due to error\.

**Note**  
Currently, LWT messages sent to AWS IoT reserved topics \(topics that begin with $\) are ignored by the AWS IoT Device Shadow service, but are still processed by subscribed clients and by the AWS IoT rules engine\. If you want the Device Shadow service to receive LWT messages, register an LWT message to a non\-reserved topic and create a rule that republishes the message on the reserved topic\. The following example shows how to create a republish rule that listens for a messages from the `my/things/myLightBulb/update` topic and republishes it to `$aws/things/myLightBulb/shadow/update`\.  

```
{
    "rule": {
    "ruleDisabled": false,
    "sql": "SELECT * FROM 'my/things/myLightBulb/update'",
    "description": "Turn my/things/ into $aws/things/",
    "actions": [{
        "republish": {
            "topic": "$$aws/things/myLightBulb/shadow/update",
            "roleArn": "arn:aws:iam::123456789012:role/aws_iot_republish"
        }
    }]
    }
}
```

When a device connects, it registers an LWT that sets the connected setting to `false`:

```
{
    "state": {        
        "reported":
        {
            "connected":"false"
        }
    }
}
```

It also publishes a message on its update topic \(`$aws/things/myLightBulb/shadow/update`\), setting its connected state to `true`:

```
{
     "state": {        
        "reported":
        {
            "connected":"true"
        }
    }
}
```

When the device disconnects gracefully, it publishes a message on its update topic and sets its connected state to `false`:

```
{
    "state": {        
        "reported":{
            "connected":"false"
        }
    }
}
```

If the device disconnects due to an error, its LWT message is posted automatically to the update topic\.