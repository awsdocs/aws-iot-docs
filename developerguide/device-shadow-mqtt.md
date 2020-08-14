# Device Shadow MQTT topics<a name="device-shadow-mqtt"></a>

The Device Shadow service uses reserved MQTT topics to enable devices and apps to get, update, or delete the state information for a device \(shadow\)\. 

Publishing and subscribing on shadow topics requires topic\-based authorization\. AWS IoT reserves the right to add new topics to the existing topic structure\. For this reason, we recommend that you avoid wild card subscriptions to shadow topics\. For example, avoid subscribing to topic filters like `$aws/things/thingName/shadow/#` because the number of topics that match this topic filter might increase as AWS IoT introduces new shadow topics\. For examples of the messages published on these topics see [Interacting with shadows](device-shadow-data-flow.md)\.

Shadows can be named or unnamed \(classic\)\. The topics used by each differ only in the topic prefix\. This table shows the topic prefix used by each shadow type\.


| *ShadowTopicPrefix* value | Shadow type | 
| --- | --- | 
| $aws/things/thingName/shadow | Unnamed \(classic\) shadow | 
| $aws/things/thingName/shadow/name/shadowName | Named shadow | 

To create a complete topic, select the `ShadowTopicPrefix` for the type of shadow to which you want to refer, replace `thingName`, and `shadowName` if applicable, with their corresponding values, and then append that with the topic stub as shown in the following sections\. Topics are case sensitive\.

The following are the MQTT topics used for interacting with shadows\.

**Topics**
+ [/get](#get-pub-sub-topic)
+ [/get/accepted](#get-accepted-pub-sub-topic)
+ [/get/rejected](#get-rejected-pub-sub-topic)
+ [/update](#update-pub-sub-topic)
+ [/update/delta](#update-delta-pub-sub-topic)
+ [/update/accepted](#update-accepted-pub-sub-topic)
+ [/update/documents](#update-documents-pub-sub-topic)
+ [/update/rejected](#update-rejected-pub-sub-topic)
+ [/delete](#delete-pub-sub-topic)
+ [/delete/accepted](#delete-accepted-pub-sub-topic)
+ [/delete/rejected](#delete-rejected-pub-sub-topic)

## /get<a name="get-pub-sub-topic"></a>

Publish an empty message to this topic to get the device's shadow:

```
ShadowTopicPrefix/get
```

AWS IoT responds by publishing to either [/get/accepted](#get-accepted-pub-sub-topic) or [/get/rejected](#get-rejected-pub-sub-topic)\.

### Example policy<a name="get-policy"></a>

The following is an example of the required policy:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Publish"
      ],
      "Resource": [
        "arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/get"
      ]
    }
  ]
}
```

## /get/accepted<a name="get-accepted-pub-sub-topic"></a>

AWS IoT publishes a response shadow document to this topic when returning the device's shadow:

```
ShadowTopicPrefix/get/accepted
```

For more information, see [Response state documents](device-shadow-document.md#device-shadow-example-response-json)\.

### Example policy<a name="get-accepted-policy"></a>

The following is an example of the required policy:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Subscribe"
      ],
      "Resource": [
        "arn:aws:iot:region:account:topicfilter/$aws/things/thingName/shadow/get/accepted"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iot:Receive"
      ],
      "Resource": [
        "arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/get/accepted"
      ]
    }
  ]
}
```

## /get/rejected<a name="get-rejected-pub-sub-topic"></a>

AWS IoT publishes an error response document to this topic when it can't return the device's shadow:

```
ShadowTopicPrefix/get/rejected
```

For more information, see [Error response document](device-shadow-document.md#device-shadow-example-error-json)\.

### Example policy<a name="get-rejected-policy"></a>

The following is an example of the required policy:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Subscribe"
      ],
      "Resource": [
        "arn:aws:iot:region:account:topicfilter/$aws/things/thingName/shadow/get/rejected"
      ]
    },
    {
      "Action": [
        "iot:Receive"
      ],
      "Resource": [
        "arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/get/rejected"
      ]
    }
  ]
}
```

## /update<a name="update-pub-sub-topic"></a>

Publish a request state document to this topic to update the device's shadow:

```
ShadowTopicPrefix/update
```

The message body contains a [partial request state document](device-shadow-document.md#device-shadow-example-request-json)\.

A client attempting to update the state of a device would send a JSON request state document with the `desired` property such as this:

```
{
  "state": {
    "desired": {
      "color": "red",
      "power": "on"
    }
  }
}
```

A device updating its shadow would send a JSON request state document with the `reported` property, such as this:

```
{
  "state": {
    "reported": {
      "color": "red",
      "power": "on"
    }
  }
}
```

AWS IoT responds by publishing to either [/update/accepted](#update-accepted-pub-sub-topic) or [/update/rejected](#update-rejected-pub-sub-topic)\.

### Example policy<a name="update-policy"></a>

The following is an example of the required policy:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Publish"
      ],
      "Resource": [
        "arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/update"
      ]
    }
  ]
}
```

## /update/delta<a name="update-delta-pub-sub-topic"></a>

AWS IoT publishes a response state document to this topic when it accepts a change for the device's shadow, and the request state document contains different values for `desired` and `reported` states:

```
ShadowTopicPrefix/update/delta
```

The message buffer contains a [/delta response state document](device-shadow-document.md#device-shadow-example-response-json-delta)\.

### Message body details<a name="update-delta-rules"></a>
+ A message published on `update/delta` includes only the desired attributes that differ between the `desired` and `reported` sections\. It contains all of these attributes, regardless of whether these attributes were contained in the current update message or were already stored in AWS IoT\. Attributes that do not differ between the `desired` and `reported` sections are not included\.
+ If an attribute is in the `reported` section but has no equivalent in the `desired` section, it is not included\.
+ If an attribute is in the `desired` section but has no equivalent in the `reported` section, it is included\.
+ If an attribute is deleted from the `reported` section but still exists in the `desired` section, it is included\.

### Example policy<a name="update-delta-policy"></a>

The following is an example of the required policy:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Subscribe"
      ],
      "Resource": [
        "arn:aws:iot:region:account:topicfilter/$aws/things/thingName/shadow/update/delta"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iot:Receive"
      ],
      "Resource": [
        "arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/update/delta"
      ]
    }
  ]
}
```

## /update/accepted<a name="update-accepted-pub-sub-topic"></a>

AWS IoT publishes a response state document to this topic when it accepts a change for the device's shadow:

```
ShadowTopicPrefix/update/accepted
```

The message buffer contains a [/accepted response state document](device-shadow-document.md#device-shadow-example-response-json-accepted)\.

### Example policy<a name="update-accepted-policy"></a>

The following is an example of the required policy:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Subscribe"
      ],
      "Resource": [
        "arn:aws:iot:region:account:topicfilter/$aws/things/thingName/shadow/update/accepted"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iot:Receive"
      ],
      "Resource": [
        "arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/update/accepted"
      ]
    }
  ]
}
```

## /update/documents<a name="update-documents-pub-sub-topic"></a>

AWS IoT publishes a state document to this topic whenever an update to the shadow is successfully performed:

```
ShadowTopicPrefix/update/documents
```

The message body contains a [/documents response state document](device-shadow-document.md#device-shadow-example-response-json-documents)\.

### Example policy<a name="update-documents-policy"></a>

The following is an example of the required policy:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Subscribe"
      ],
      "Resource": [
        "arn:aws:iot:region:account:topicfilter/$aws/things/thingName/shadow/update/documents"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iot:Receive"
      ],
      "Resource": [
        "arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/update/accepted"
      ]
    }
  ]
}
```

## /update/rejected<a name="update-rejected-pub-sub-topic"></a>

AWS IoT publishes an error response document to this topic when it rejects a change for the device's shadow:

```
ShadowTopicPrefix/update/rejected
```

The message body contains an [Error response document](device-shadow-document.md#device-shadow-example-error-json)\.

### Example policy<a name="update-rejected-policy"></a>

The following is an example of the required policy:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Subscribe"
      ],
      "Resource": [
        "arn:aws:iot:region:account:topicfilter/$aws/things/thingName/shadow/update/rejected"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iot:Receive"
      ],
      "Resource": [
        "arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/update/rejected"
      ]
    }
  ]
}
```

## /delete<a name="delete-pub-sub-topic"></a>

To delete a device's shadow, publish an empty message to the delete topic:

```
ShadowTopicPrefix/shadow/delete
```

The content of the message is ignored\.

AWS IoT responds by publishing to either [/delete/accepted](#delete-accepted-pub-sub-topic) or [/delete/rejected](#delete-rejected-pub-sub-topic)\.

### Example policy<a name="delete-policy"></a>

The following is an example of the required policy:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Publish"
      ],
      "Resource": [
        "arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/delete"
      ]
    }
  ]
}
```

## /delete/accepted<a name="delete-accepted-pub-sub-topic"></a>

AWS IoT publishes a message to this topic when a device's shadow is deleted:

```
ShadowTopicPrefix/delete/accepted
```

### Example policy<a name="delete-accepted-policy"></a>

The following is an example of the required policy:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Subscribe"
      ],
      "Resource": [
        "arn:aws:iot:region:account:topicfilter/$aws/things/thingName/shadow/delete/accepted"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iot:Receive"
      ],
      "Resource": [
        "arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/delete/accepted"
      ]
    }
  ]
}
```

## /delete/rejected<a name="delete-rejected-pub-sub-topic"></a>

AWS IoT publishes an error response document to this topic when it can't delete the device's shadow:

```
ShadowTopicPrefix/delete/rejected
```

The message body contains an [Error response document](device-shadow-document.md#device-shadow-example-error-json)\.

### Example policy<a name="delete-rejected-policy"></a>

The following is an example of the required policy:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Subscribe"
      ],
      "Resource": [
        "arn:aws:iot:region:account:topicfilter/$aws/things/thingName/shadow/delete/rejected"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iot:Receive"
      ],
      "Resource": [
        "arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/delete/rejected"
      ]
    }
  ]
}
```
