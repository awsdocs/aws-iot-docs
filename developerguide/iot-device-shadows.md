# AWS IoT Device Shadow service<a name="iot-device-shadows"></a>

 The AWS IoT Device Shadow service adds shadows to AWS IoT thing objects\. Shadows can make a device’s state available to apps and other services whether the device is connected to AWS IoT or not\. AWS IoT thing objects can have multiple named shadows so that your IoT solution has more options for connecting your devices to other apps and services\. 

AWS IoT thing objects don't have any named shadows until they are created explicitly; however, an unnamed, classic shadow is created for a thing when the thing is created\. Shadows can be created, updated, and deleted by using the AWS IoT console\. Devices, other web clients, and services can create, update, and delete shadows by using MQTT and the [reserved MQTT topics](reserved-topics.md#reserved-topics-shadow), HTTP using the [Device Shadow REST API](device-shadow-rest-api.md), and the [AWS CLI for AWS IoT](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot-data/index.html)\. Because shadows are stored by AWS in the cloud, they can collect and report device state data from apps and other cloud services whether the device is connected or not\. 

## Using shadows<a name="device-shadow-using"></a>

Shadows provide a reliable data store for devices, apps, and other cloud services to share data\. They enable devices, apps, and other cloud services to connect and disconnect without losing a device's state\. 

While devices, apps, and other cloud services are connected to AWS IoT, they can access and control the current state of a device through its shadows\. For example, an app can request a change in a device's state by updating a shadow\. AWS IoT publishes a message that indicates the change to the device\. The device receives this message, updates its state to match, and publishes a message with its updated state\. The Device Shadow service reflects this updated state in the corresponding shadow\. The app can subscribe to the shadow's update or it can query the shadow for its current state\. 

When a device goes offline, an app can still communicate with AWS IoT and the device's shadows\. When the device reconnects, it receives the current state of its shadows so that it can update its state to match that of its shadows, and then publish a message with its updated state\. Likewise, when an app goes offline and the device state changes while it's offline, the device keeps the shadow updated so the app can query the shadows for its current state when it reconnects\.

If your devices are frequently offline and you would like to configure your devices to receive delta messages after they reconnect, you can use the persistent session feature\. For more information about the persistent session expiry period, see [Persistent session expiry period](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#message-broker-limits)\. 

### Choosing to use named or unnamed shadows<a name="iot-device-shadow-named"></a>

The Device Shadow service supports named and unnamed, classic shadows, as have been used in the past\. A thing object can have multiple named shadows, and no more than one unnamed, classic shadow\. A thing object can have both named and unnamed shadows at the same time; however, the API used to access each is slightly different, so it might be more efficient to decide which type of shadow would work best for your solution and use that type only\. For more information about the API to access the shadows, see [Shadow topics](reserved-topics.md#reserved-topics-shadow)\. 

With named shadows, you can create different views of a thing object’s state\. For example, you could divide a thing object with many properties into shadows with logical groups of properties, each identified by its shadow name\. You could also limit access to properties by grouping them into different shadows and using policies to control access\. For more information about policies to use with device shadows, see [Actions, resources, and condition keys for AWS IoT](https://docs.aws.amazon.com/service-authorization/latest/reference/list_awsiot.html)\.

The classic, unnamed shadows are simpler, but somewhat more limited than the named shadows\. Each AWS IoT thing object can have only one unnamed shadow\. If you expect your IoT solution to have a limited need for shadow data, this might be how you want to get started using shadows\. However, if you think you might want to add additional shadows in the future, consider using named shadows from the start\.

Fleet indexing supports unnamed shadows and named shadows differently\. For more information, see [Fleet indexing](iot-indexing.md)\.

### Accessing shadows<a name="device-shadow-using-access"></a>

Every shadow has a reserved [MQTT topic](reserved-topics.md#reserved-topics-shadow) and [HTTP URL](device-shadow-rest-api.md) that supports the `get`, `update`, and `delete` actions on the shadow\.

Shadows use [JSON shadow documents](device-shadow-document.md) to store and retrieve data\. A shadow’s document contains a state property that describes these aspects of the device’s state:
+ `desired`

  Apps specify the desired states of device properties by updating the `desired` object\.
+ `reported`

  Devices report their current state in the `reported` object\.
+ `delta`

  AWS IoT reports differences between the desired and the reported state in the `delta` object\.

The data stored in a shadow is determined by the state property of the update action's message body\. Subsequent update actions can modify the values of an existing data object, and also add and delete keys and other elements in the shadow’s state object\. For more information about accessing shadows, see [Using shadows in devices](device-shadow-comms-device.md) and [Using shadows in apps and services](device-shadow-comms-app.md)\.

**Important**  
Permission to make update requests should be limited to trusted apps and devices\. This prevents the shadow's state property from being changed unexpectedly; otherwise, the devices and apps that use the shadow should be designed to expect the keys in the state property to change\.

### Using shadows in devices, apps, and other cloud services<a name="device-shadow-implementing"></a>

Using shadows in devices, apps, and other cloud services requires consistency and coordination between all of these\. The AWS IoT Device Shadow service stores the shadow state, sends messages when the shadow state changes, and responds to messages that change its state\. The devices, apps, and other cloud services in your IoT solution must manage their state and keep it consistent with the device shadow's state\.

The shadow state data is dynamic and can be altered by the devices, apps, and other cloud services with permission to access the shadow\. For this reason, it is important to consider how each device, app, and other cloud service will interact with the shadow\. For example:
+ *Devices* should write only to the `reported` property of the shadow state when communicating state data to the shadow\.
+ *Apps and other cloud services* should write only to the `desired` property when communicating state change requests to the device through the shadow\.

**Important**  
The data contained in a shadow data object is independent from that of other shadows and other thing object properties, such as a thing’s attributes and the content of MQTT messages that a thing object's device might publish\. A device can, however, report the same data in different MQTT topics and shadows if necessary\.  
A device that supports multiple shadows must maintain the consistency of the data that it reports in the different shadows\.

### Message order<a name="message-ordering"></a>

There is no guarantee that messages from the AWS IoT service will arrive at the device in any specific order\. The following scenario shows what happens in this case\.

Initial state document:

```
{
  "state": {
    "reported": {
      "color": "blue"
    }
  },
  "version": 9,
  "timestamp": 123456776
}
```

Update 1:

```
{
  "state": {
    "desired": {
      "color": "RED"
    }
  },
  "version": 10,
  "timestamp": 123456777
}
```

Update 2:

```
{
  "state": {
    "desired": {
      "color": "GREEN"
    }
  },
  "version": 11,
  "timestamp": 123456778
}
```

Final state document:

```
{
  "state": {
    "reported": {
      "color": "GREEN"
    }
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
  "state": {
    "color": "GREEN"
  },
  "version": 12,
  "timestamp": 123456779
}
```

The device might receive these messages out of order\. Because the state in these messages is cumulative, a device can safely discard any messages that contain a version number older than the one it is tracking\. If the device receives the delta for version 12 before version 11, it can safely discard the version 11 message\.

### Trim shadow messages<a name="device-shadow-trim-messages"></a>

To reduce the size of shadow messages sent to your device, define a rule that selects only the fields your device needs then republishes the message on an MQTT topic to which your device is listening\.

The rule is specified in JSON and should look like the following: 

```
{
  "sql": "SELECT state, version FROM '$aws/things/+/shadow/update/delta'",
  "ruleDisabled": false,
  "actions": [
    {
      "republish": {
        "topic": "${topic(3)}/delta",
        "roleArn": "arn:aws:iam:123456789012:role/my-iot-role"
      }
    }
  ]
}
```

The SELECT statement determines which fields from the message will be republished to the specified topic\. A "\+" wild card is used to match all shadow names\. The rule specifies that all matching messages should be republished to the specified topic\. In this case, the `"topic()"` function is used to specify the topic on which to republish\. `topic(3)` evaluates to the thing name in the original topic\. For more information about creating rules, see [Rules for AWS IoT](iot-rules.md)\.