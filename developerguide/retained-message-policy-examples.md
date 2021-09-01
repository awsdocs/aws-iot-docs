# Retained message policy examples<a name="retained-message-policy-examples"></a>

Using [retained messages](mqtt.md#mqtt-retain) requires specific policies\. Retained messages are MQTT messages published with the RETAIN flag set and stored by AWS IoT Core\. This section presents examples of policies that allow common uses of retained messages\.

**Topics**
+ [Policy to connect and publish retained messages](#retained-message-policy-examples-publish)
+ [Policy to connect and publish retained Will messages](#retained-message-policy-examples-publish-lwt)
+ [Policy to list and get retained messages](#retained-message-policy-examples-list-get)

## Policy to connect and publish retained messages<a name="retained-message-policy-examples-publish"></a>

For a device to publish retained messages, the device must be able to connect, publish \(any MQTT message\), and publish MQTT retained messages\. The following policy grants these permissions for the topic: `device/sample/configuration` to client **device1**\. For another example that grants permission to connect, see [Connect and publish policy examples](connect-and-pub.md)\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
    {
      "Effect": "Allow",
      "Action": [
        "iot:Connect"
      ],
      "Resource": [
        "arn:aws:iot:us-east-1:123456789012:client/device1"
      ]
    },
      "Effect": "Allow",
      "Action": [
        "iot:Publish",
        "iot:RetainPublish"
      ],
      "Resource": [
        "arn:aws:iot:us-east-1:123456789012:topic/device/sample/configuration"
      ]
    }
  ]
}
```

## Policy to connect and publish retained Will messages<a name="retained-message-policy-examples-publish-lwt"></a>

Clients can configure a message that AWS IoT Core will publish when the client disconnects unexpectedly\. MQTT calls such a message a [*Will* message](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/errata01/os/mqtt-v3.1.1-errata01-os-complete.html#_Will_Flag)\. A client must have an additional condition added to its connect permission to include them\. 

The following policy document grants all clients permission to connect and publish a Will message, identified by its topic, `will`, that AWS IoT Core will also retain\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Connect"
      ],
      "Resource": [
        "arn:aws:iot:us-east-1:123456789012:client/*"
      ],
      "Condition": {
        "ForAllValues:StringEquals": {
          "iot:ConnectAttributes": [
            "LastWill"
          ]
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "iot:Publish",
        "iot:RetainPublish"
      ],
      "Resource": [
        "arn:aws:iot:us-east-1:123456789012:topic/will"
      ]
    }
  ]
}
```

## Policy to list and get retained messages<a name="retained-message-policy-examples-list-get"></a>

Services and applications can access retained messages without the need to support an MQTT client by calling [https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_ListRetainedMessages.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_ListRetainedMessages.html) and [https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_GetRetainedMessage.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_GetRetainedMessage.html)\. The services and applications that call these actions must be authorized by using a policy such as the following example\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:ListRetainedMessages"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iot:GetRetainedMessage"
      ],
      "Resource": [
        "arn:aws:iot:us-east-1:123456789012:topic/foo"
      ]
    }
  ]
}
```