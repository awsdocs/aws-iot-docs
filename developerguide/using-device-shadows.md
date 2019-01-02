# Using Shadows<a name="using-device-shadows"></a>

AWS IoT provides three methods for working with a device's shadow:

`UPDATE`  
Creates a device's shadow if it doesn't exist, or updates the contents of a device's shadow with the data provided in the request\. The data is stored with timestamp information to indicate when it was last updated\. Messages are sent to all subscribers with the difference between `desired` or `reported` state \(delta\)\. Things or apps that receive a message can perform an action based on the difference between `desired` or `reported` states\. For example, a device can update its state to the desired state, or an app can update its UI to show the change in the device's state\.

`GET`  
Retrieves the latest state stored in the device's shadow \(for example, during start\-up of a device to retrieve configuration and the last state of operation\)\. This method returns the full JSON document, including metadata\.

`DELETE`  
Deletes a device's shadow, including all of its content\. This removes the JSON document from the data store\. You can't restore a device's shadow you deleted, but you can create a new shadow with the same name\.

## Protocol Support<a name="protocol-support"></a>

These methods are supported through both [MQTT](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.html) and a RESTful API over HTTPS\. Because MQTT is a publish/subscribe communication model, AWS IoT implements a set of reserved topics\. Things or applications subscribe to these topics before publishing on a request topic in order to implement a request–response behavior\. For more information, see [Shadow MQTT Topics](device-shadow-mqtt.md) and [Device Shadow RESTful API](device-shadow-rest-api.md)\.

## Updating a Shadow<a name="update-device-shadow"></a>

You can update a device's shadow by using the [UpdateThingShadow](API_UpdateThingShadow.md) RESTful API or by publishing to the [/update](device-shadow-mqtt.md#update-pub-sub-topic) topic\. Updates affect only the fields specified in the request\.

Initial state:

```
{
    "state": {
        "reported" : {
            "color" : { "r" :255, "g": 255, "b": 0 }    
        }
    }
}
```

An update message is sent:

```
{
    "state": {
        "desired" : {
            "color" : { "r" : 10 },
            "engine" : "ON"
        }
    }
}
```

The device receives the `desired` state on the `/update/delta` topic that is triggered by the previous `/update` message and then executes the desired changes\. When finished, the device should confirm its updated state through the `reported` section in the shadow JSON document\.

Final state:

```
{
    "state": {
        "reported" : {
            "color" : {  "r" : 10, "g" : 255, "b": 0 },
            "engine" : "ON"
        }
    }
}
```

## Retrieving a Shadow Document<a name="retrieving-device-shadow"></a>

You can retrieve a device's shadow by using the [GetThingShadow](API_GetThingShadow.md) RESTful API or by subscribing and publishing to the [/get](device-shadow-mqtt.md#get-pub-sub-topic) topic\. This retrieves the entire document plus the delta between the `desired` or `reported` states\.

Example document:

```
{
    "state": {
        "desired": {
            "lights": {
                "color": "RED"
            },
            "engine": "ON"
        },
        "reported": {
            "lights": {
                "color": "GREEN"
            },
            "engine": "ON"
        }
    },
    "metadata": {
        "desired": {
            "lights": {
                "color": {
                    "timestamp": 123456
                },
                "engine": {
                    "timestamp": 123456
                }
            }
        },
        "reported": {
            "lights": {
                "color": {
                    "timestamp": 789012
                }
            },
            "engine": {
                "timestamp": 789012
            }
        },
        "version": 10,
        "timestamp": 123456789
    }
}
```

Response:

```
{
    "state": {
        "desired": {
            "lights": {
                "color": "RED"
            },
            "engine": "ON"
        },
        "reported": {
            "lights": {
                "color": "GREEN"
            },
            "engine": "ON"
        },
        "delta": {
            "lights": {
                "color": "RED"
            }
        }
    },
    "metadata": {
        "desired": {
            "lights": {
                "color": {
                    "timestamp": 123456
                },
             },
             "engine": {
                "timestamp": 123456
            }
        },
        "reported": {
            "lights": {
                "color": {
                    "timestamp": 789012
                }
            },
            "engine": {
                "timestamp": 789012
            }
        },
        "delta": {
            "lights": {
                "color": {
                    "timestamp": 123456
                }
            }
        }
    },
    "version": 10,
    "timestamp": 123456789
}
```

### Optimistic Locking<a name="optimistic-locking"></a>

You can use the state document version to ensure you are updating the most recent version of a device's shadow document\. When you supply a version with an update request, the service rejects the request with an HTTP 409 conflict response code if the current version of the state document does not match the version supplied\.

For example:

Initial document:

```
{
    "state" : {
        "desired" : { "colors" : ["RED", "GREEN", "BLUE" ] }
    },
    "version" : 10
}
```

Update: \(version doesn't match; request will be rejected\)

```
{
    "state": {
        "desired": {
            "colors": [
                "BLUE"
            ]
        }
    },
    "version": 9
}
```

Result:

```
409 Conflict
```

Update: \(version matches; this request will be accepted\)

```
{
    "state": {
        "desired": {
            "colors": [
                "BLUE"
            ]
        }
    },
    "version": 10
}
```

Final state:

```
{
    "state": {
        "desired": {
            "colors": [
                "BLUE"
            ]
        }
    },
    "version": 11
}
```

## Deleting Data<a name="deleting-thing-data"></a>

You can delete data from a device's shadow by publishing to the [/update](device-shadow-mqtt.md#update-pub-sub-topic) topic, setting the fields to be deleted to null\. Any field with a value of `null` is removed from the document\.

Initial state:

```
{
    "state": {
        "desired" : {
            "lights": { "color": "RED" },
            "engine" : "ON"
        },
        "reported" : {
            "lights" : { "color": "GREEN"  },
            "engine" : "OFF"
        }
    }
}
```

An update message is sent:

```
{
    "state": {
        "desired": null,
        "reported": {
            "engine": null
        }
    }
}
```

Final state:

```
{
    "state": {
        "reported" : {
            "lights" : { "color" : "GREEN" }
        }
    }
}
```

You can delete all data from a device's shadow by setting its state to `null`\. For example, sending the following message deletes all of the state data, but the device's shadow remains\.

```
{
    "state": null
}
```

The device's shadow still exists even if its state is `null`\. The version of the shadow is incremented when the next update occurs\.

## Deleting a Shadow<a name="deleting-device-shadow"></a>

You can delete a device's shadow document by using the [DeleteThingShadow](API_DeleteThingShadow.md) RESTful API or by publishing to the [/delete](device-shadow-mqtt.md#delete-pub-sub-topic) topic\. 

**Note**  
Deleting a device's shadow does not delete the thing\. Deleting a thing does not delete the corresponding device's shadow\.

Initial state:

```
{
    "state": {
        "desired" : {
            "lights": { "color": "RED" },
            "engine" : "ON"
        },
        "reported" : {
            "lights" : { "color": "GREEN"  },
            "engine" : "OFF"
        }
    }
}
```

An empty message is published to the /delete topic\.

Final state:

```
 HTTP 404 - resource not found
```

## Delta State<a name="delta-state"></a>

Delta state is a virtual type of state that contains the difference between the `desired` and `reported` states\. Fields in the `desired` section that are not in the `reported` section are included in the delta\. Fields that are in the `reported` section and not in the `desired` section are not included in the delta\. The delta contains metadata, and its values are equal to the metadata in the `desired` field\. For example:

```
{
    "state": {
        "desired": {
            "color": "RED",
            "state": "STOP"
        },
        "reported": {
            "color": "GREEN",
            "engine": "ON"
        },
        "delta": {
            "color": "RED",
            "state": "STOP"
        }
    },
    "metadata": {
        "desired": {
            "color": {
                "timestamp": 12345
            },
            "state": {
                "timestamp": 12345
            },
            "reported": {
                "color": {
                    "timestamp": 12345
                },
                "engine": {
                    "timestamp": 12345
                }
            },
            "delta": {
                "color": {
                    "timestamp": 12345
                },
                "state": {
                    "timestamp": 12345
                }
            }
        },
        "version": 17,
        "timestamp": 123456789
    }
}
```

When nested objects differ, the delta contains the path all the way to the root\.

```
{
    "state": {
        "desired": {
            "lights": {
                "color": {
                    "r": 255,
                    "g": 255,
                    "b": 255
                }
            }
        },
        "reported": {
            "lights": {
                "color": {
                    "r": 255,
                    "g": 0,
                    "b": 255
                }
            }
        },
        "delta": {
            "lights": {
                "color": {
                    "g": 255
                }
            }
        }
    },
    "version": 18,
    "timestamp": 123456789
}
```

The Device Shadow service calculates the delta by iterating through each field in the `desired` state and comparing it to the `reported` state\.

Arrays are treated like values\. If an array in the `desired` section doesn't match the array in the `reported` section, then the entire desired array is copied into the delta\.

## Observing State Changes<a name="observing-state-changes"></a>

 When a device's shadow is updated, messages are published on two MQTT topics: 

+ $aws/things/*thing\-name*/shadow/update/accepted

+ $aws/things/*thing\-name*/shadow/update/delta

The message sent to the `update/delta` topic is intended for the thing whose state is being updated\. This message contains only the difference between the `desired` and `reported` sections of the device's shadow document\. Upon receiving this message, the device should decide whether to make the requested change\. If the device's state is changed, it should publish its new current state to the `$aws/things/thing-name/shadow/update` topic\.

Devices and applications can subscribe to either of these topics to be notified when the state of the document has changed\.

Here is an example of that flow:

1. A device reports its state\.

1. The system updates the state document in its persistent data store\.

1. The system publishes a delta message, which contains only the delta and is targeted at the subscribed devices\. Devices should subscribe to this topic to receive updates\.

1. The device's shadow publishes an accepted message, which contains the entire received document, including metadata\. Applications should subscribe to this topic to receive updates\.

## Message Order<a name="message-ordering"></a>

There is no guarantee that messages from the AWS IoT service will arrive at the device in any specific order\.

Initial state document:

```
{
    "state" : {
        "reported" : { "color" : "blue" }
    },
    "version" : 10,
    "timestamp": 123456777
}
```

Update 1:

```
{
    "state": { "desired" : { "color" : "RED" } },
    "version": 10,
    "timestamp": 123456777
}
```

Update 2:

```
{
    "state": { "desired" : { "color" : "GREEN" } },
    "version": 11,  
    "timestamp": 123456778
}
```

Final state document:

```
{
    "state": {
        "reported": { "color" : "GREEN" }
    },
    "version": 12,
    "timestamp": 123456779
}
```

This results in two delta messages:

```
{
    "state": {
        "color": "RED"
    },
    "version": 11,
    "timestamp": 123456778
}
```

```
{
    "state": { "color" : "GREEN" },
    "version": 12,
    "timestamp": 123456779
}
```

The device might receive these messages out of order\. Because the state in these messages is cumulative, a device can safely discard any messages that contain a version number older than the one it is tracking\. If the device receives the delta for version 12 before version 11, it can safely discard the version 11 message\.

## Trim Shadow Messages<a name="device-shadow-trim-messages"></a>

To reduce the size of shadow messages sent to your device, define a rule that selects only the fields your device needs then republishes the message on an MQTT topic to which your device is listening\.

The rule is specified in JSON and should look like the following: 

```
{
    "sql": "SELECT state, version FROM '$aws/things/+/shadow/update/delta'",
    "ruleDisabled": false,
    "actions": [{
        "republish": {
            "topic": "${topic(2)}/delta",
            "roleArn": "arn:aws:iam::123456789012:role/my-iot-role"
        }
    }]
}
```

The SELECT statement determines which fields from the message will be republished to the specified topic\. A "\+" wild card is used to match all shadow names\. The rule specifies that all matching messages should be republished to the specified topic\. In this case, the `"topic()"` function is used to specify the topic on which to republish\. `topic(2)` evaluates to the thing name in the original topic\. For more information about creating rules, see [Rules](http://alpha-docs-aws.amazon.com/iot/latest/developerguide//iot-rules.html)\.