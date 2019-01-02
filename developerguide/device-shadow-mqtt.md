# Shadow MQTT Topics<a name="device-shadow-mqtt"></a>

The Device Shadow service uses reserved MQTT topics to enable applications and devices to get, update, or delete the state information for a device \(shadow\)\. The names of these topics start with $aws/things/*thingName*/shadow\. Publishing and subscribing on shadow topics requires topic\-based authorization\. AWS IoT reserves the right to add new topics to the existing topic structure\. For this reason, we recommend that you avoid wild card subscriptions to shadow topics\. For example, avoid subscribing to topic filters like `$aws/things/thingName/shadow/#` because the number of topics that match this topic filter might increase as AWS IoT introduces new shadow topics\. For examples of the messages published on these topics see [Device Shadow Service Data Flow](device-shadow-data-flow.md)\.

The following are the MQTT topics used for interacting with shadows\.


+ [/update](#update-pub-sub-topic)
+ [/update/accepted](#update-accepted-pub-sub-topic)
+ [/update/documents](#update-documents-pub-sub-topic)
+ [/update/rejected](#update-rejected-pub-sub-topic)
+ [/update/delta](#update-delta-pub-sub-topic)
+ [/get](#get-pub-sub-topic)
+ [/get/accepted](#get-accepted-pub-sub-topic)
+ [/get/rejected](#get-rejected-pub-sub-topic)
+ [/delete](#delete-pub-sub-topic)
+ [/delete/accepted](#delete-accepted-pub-sub-topic)
+ [/delete/rejected](#delete-rejected-pub-sub-topic)

## /update<a name="update-pub-sub-topic"></a>

Publish a request state document to this topic to update the device's shadow:

```
$aws/things/thingName/shadow/update
```

A client attempting to update the state of a thing would send a JSON request state document like this:

```
{
    "state" : {
        "desired" : {
            "color" : "red",
            "power" : "on"
         }
     }
}
```

A device updating its shadow would send a JSON request state document like this:

```
{
    "state" : {
        "reported" : {
            "color" : "red",
            "power" : "on"
         }
     }
}
```

AWS IoT responds by publishing to either [/update/accepted](#update-accepted-pub-sub-topic) or [/update/rejected](#update-rejected-pub-sub-topic)\.

For more information, see [Request State Documents](device-shadow-document-syntax.md#device-shadow-example-request-json)\.

### Example Policy<a name="update-policy"></a>

The following is an example of the required policy:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Publish"],
        "Resource": ["arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/update"]
     }]
}
```

## /update/accepted<a name="update-accepted-pub-sub-topic"></a>

AWS IoT publishes a response state document to this topic when it accepts a change for the device's shadow:

```
$aws/things/thingName/shadow/update/accepted
```

For more information, see [Response State Documents](device-shadow-document-syntax.md#device-shadow-example-response-json)\.

### Example Policy<a name="update-accepted-policy"></a>

The following is an example of the required policy:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Subscribe"],
        "Resource": ["arn:aws:iot:region:account:topicfilter/$aws/things/thingName/shadow/update/accepted"]
    },
    {
        "Effect": "Allow",
        "Action": ["iot:Receive"],
        "Resource": ["arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/update/accepted"]
    }]
}
```

## /update/documents<a name="update-documents-pub-sub-topic"></a>

AWS IoT publishes a state document to this topic whenever an update to the shadow is successfully performed:

```
$aws/things/thingName/shadow/update/documents
```

The JSON document will contain two primary nodes: `previous` and `current`\. The `previous` node will contain the contents of the full shadow document before the update was performed while `current` will contain the full shadow document after the update is successfully applied\. When the shadow is updated \(created\) for the first time, the `previous` node will contain `null`\.

### Example Policy<a name="update-documents-policy"></a>

The following is an example of the required policy:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Subscribe"],
        "Resource": ["arn:aws:iot:region:account:topicfilter/$aws/things/thingName/shadow/update/documents"]
    },
    {
        "Effect": "Allow",
        "Action": ["iot:Receive"],
        "Resource": ["arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/update/accepted"]
    }]
}
```

## /update/rejected<a name="update-rejected-pub-sub-topic"></a>

AWS IoT publishes an error response document to this topic when it rejects a change for the device's shadow:

```
$aws/things/thingName/shadow/update/rejected
```

For more information, see [Error Response Documents](device-shadow-document-syntax.md#device-shadow-example-error-json)\.

### Example Policy<a name="update-rejected-policy"></a>

The following is an example of the required policy:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Subscribe"],
        "Resource": ["arn:aws:iot:region:account:topicfilter/$aws/things/thingName/shadow/update/rejected"]
    },
    {
        "Effect": "Allow",
        "Action": ["iot:Receive"],
        "Resource": ["arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/update/rejected"]
    }]
}
```

## /update/delta<a name="update-delta-pub-sub-topic"></a>

AWS IoT publishes a response state document to this topic when it accepts a change for the device's shadow and the request state document contains different values for `desired` and `reported` states:

```
$aws/things/thingName/shadow/update/delta
```

For more information, see [Response State Documents](device-shadow-document-syntax.md#device-shadow-example-response-json)\.

### Publishing Details<a name="update-delta-rules"></a>

+ A message published on `update/delta` includes only the desired attributes that differ between the `desired` and `reported` sections\. It contains all of these attributes, regardless of whether these attributes were contained in the current update message or were already stored in AWS IoT\. Attributes that do not differ between the `desired` and `reported` sections are not included\.

+ If an attribute is in the `reported` section but has no equivalent in the `desired` section, it is not included\.

+ If an attribute is in the `desired` section but has no equivalent in the `reported` section, it is included\.

+ If an attribute is deleted from the `reported` section but still exists in the `desired` section, it is included\.

### Example Policy<a name="update-delta-policy"></a>

The following is an example of the required policy:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Subscribe"],
        "Resource": ["arn:aws:iot:region:account:topicfilter/$aws/things/thingName/shadow/update/delta"]
    },
    {
        "Effect": "Allow",
        "Action": ["iot:Receive"],
        "Resource": ["arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/update/delta"]
    }]
}
```

## /get<a name="get-pub-sub-topic"></a>

Publish an empty message to this topic to get the device's shadow:

```
$aws/things/thingName/shadow/get
```

AWS IoT responds by publishing to either [/get/accepted](#get-accepted-pub-sub-topic) or [/get/rejected](#get-rejected-pub-sub-topic)\.

### Example Policy<a name="get-policy"></a>

The following is an example of the required policy:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": [
            "iot:Publish"
        ],
        "Resource": ["arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/get"]
    }]
}
```

## /get/accepted<a name="get-accepted-pub-sub-topic"></a>

AWS IoT publishes a response state document to this topic when returning the device's shadow:

```
$aws/things/thingName/shadow/get/accepted
```

For more information, see [Response State Documents](device-shadow-document-syntax.md#device-shadow-example-response-json)\.

### Example Policy<a name="get-accepted-policy"></a>

The following is an example of the required policy:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Subscribe"],
        "Resource": ["arn:aws:iot:region:account:topicfilter/$aws/things/thingName/shadow/get/accepted"]
    },
    {
        "Effect": "Allow",
        "Action": ["iot:Receive"],
        "Resource": ["arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/get/accepted"]
    }]
}
```

## /get/rejected<a name="get-rejected-pub-sub-topic"></a>

AWS IoT publishes an error response document to this topic when it can't return the device's shadow:

```
$aws/things/thingName/shadow/get/rejected
```

For more information, see [Error Response Documents](device-shadow-document-syntax.md#device-shadow-example-error-json)\.

### Example Policy<a name="get-rejected-policy"></a>

The following is an example of the required policy:

```
{
    "Version": "2012-10-17",
    "Statement": [{
    	"Effect": "Allow",
        "Action": ["iot:Subscribe"],
        "Resource": ["arn:aws:iot:region:account:topicfilter/$aws/things/thingName/shadow/get/rejected"]
    },
    {
        "Action": ["iot:Receive"],
        "Resource": ["arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/get/rejected"]
    }]
}
```

## /delete<a name="delete-pub-sub-topic"></a>

To delete a device's shadow, publish an empty message to the delete topic:

```
$aws/things/thingName/shadow/delete
```

The content of the message is ignored\.

AWS IoT responds by publishing to either [/delete/accepted](#delete-accepted-pub-sub-topic) or [/delete/rejected](#delete-rejected-pub-sub-topic)\.

### Example Policy<a name="delete-policy"></a>

The following is an example of the required policy:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Subscribe"],
        "Resource": ["arn:aws:iot:region:account:topicfilter/$aws/things/thingName/shadow/delete"]
    },
    {
        "Effect": "Allow",
        "Action": ["iot:Receive"],
        "Resource": ["arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/delete"]
    }]
}
```

## /delete/accepted<a name="delete-accepted-pub-sub-topic"></a>

AWS IoT publishes a message to this topic when a device's shadow is deleted:

```
$aws/things/thingName/shadow/delete/accepted
```

### Example Policy<a name="delete-accepted-policy"></a>

The following is an example of the required policy:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Subscribe"],
        "Resource": ["arn:aws:iot:region:account:topicfilter/$aws/things/thingName/shadow/delete/accepted"]
    },
     {
        "Effect": "Allow",
        "Action": ["iot:Receive"],
        "Resource": ["arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/delete/accepted"]
    }]
}
```

## /delete/rejected<a name="delete-rejected-pub-sub-topic"></a>

AWS IoT publishes an error response document to this topic when it can't delete the device's shadow:

```
$aws/things/thingName/shadow/delete/rejected
```

For more information, see [Error Response Documents](device-shadow-document-syntax.md#device-shadow-example-error-json)\.

### Example Policy<a name="delete-rejected-policy"></a>

The following is an example of the required policy:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Subscribe"],
        "Resource": ["arn:aws:iot:region:account:topicfilter/$aws/things/thingName/shadow/delete/rejected"]
    },
    {
        "Effect": "Allow",
        "Action": ["iot:Receive"],
        "Resource": ["arn:aws:iot:region:account:topic/$aws/things/thingName/shadow/delete/rejected"]
    }]
}
```