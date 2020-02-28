# Publish/Subscribe Policy Examples<a name="pub-sub-policy"></a>

The policy you use depends on how you are connecting to \. You can connect to using an MQTT client, HTTP, or WebSocket\. When you connect with an MQTT client, you are authenticating with an X\.509 certificate\. When you connect over HTTP or the WebSocket protocol, you are authenticating with Signature Version 4 and Amazon Cognito\.

## Policies for MQTT Clients<a name="pub-sub-policy-cert"></a>

To specify wildcards in topic names, use \* in the `resource` attribute of the policy when the device publishes and subscribes to multiple topics\. The following policy enables a device to publish to all subtopics that start with the same thing name\.

------
#### [ Registered Devices \(5\) ]

For devices registered as things in the registry, the following policy grants permission to connect to using a client ID that matches the thing name and to publish to any topic prefixed by the thing name:

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
                "arn:aws:iot:us-east-1:123456789012:client/${iot:Connection.Thing.ThingName}",
            ]
        }
        {
            "Effect": "Allow",
            "Action": [
                "iot:Publish"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/${iot:Connection.Thing.ThingName}/*"
            ]
        }
    ]
}
```

------
#### [ Unregistered Devices \(5\) ]

For devices not registered as things in the registry, the following policy grants permission to connect to using client ID `client1`, `client2`, or `client3` and to publish to any topic prefixed by the client ID:

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
                "arn:aws:iot:us-east-1:123456789012:client/client1",
                "arn:aws:iot:us-east-1:123456789012:client/client2",
                "arn:aws:iot:us-east-1:123456789012:client/client3"
            ]
        }
        {
            "Effect": "Allow",
            "Action": [
                "iot:Publish"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/${iot:ClientId}/*"
            ]
        }
    ]
}
```

------

You can also use the \* wildcard at the end of a topic filter\. Using wildcard characters might lead to granting unintended privileges, so they should only be used after careful consideration\. One situation in which they might be useful is when devices must subscribe to messages with many different topics \(for example, if a device must subscribe to reports from temperature sensors in multiple locations\)\. 

------
#### [ Registered Devices \(6\) ]

For devices registered as things in the registry, the following policy grants permission to connect to using the device's thing name as the client ID, and to subscribe to a topic prefixed by the thing name, followed by `room`, followed by any string\. \(It is expected that these topics are, for example, `thing1/room1`, `thing1/room2`, and so on\):

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
                "arn:aws:iot:us-east-1:123456789012:client/${iot:Connection.Thing.ThingName}"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Subscribe"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topicfilter/${iot:Connection.Thing.ThingName}/room*"
            ]
        }
    ]
}
```

------
#### [ Unregistered Devices \(6\) ]

For devices not registered as things in the registry, the following policy grants permission to connect to using client IDs `client1`, `client2`, `client3`, and to subscribe to a topic prefixed by the client ID, followed by `room`, followed by any string\. \(It is expected that these topics are, for example, `client1/room1`, `client1/room2`, and so on\):

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
                "arn:aws:iot:us-east-1:123456789012:client/client1",
                "arn:aws:iot:us-east-1:123456789012:client/client2",
                "arn:aws:iot:us-east-1:123456789012:client/client3"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Subscribe"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topicfilter/${iot:ClientId}/room*"
            ]
        }
    ]
}
```

------

When you specify topic filters in policies for MQTT clients, MQTT wildcard characters "\+" and "\#" are treated as literal characters\. Their use might result in unexpected behavior\.

------
#### [ Registered Devices \(4\) ]

For devices registered as things in the registry, the following policy grants permission to connect to with the client ID that matches the thing name, and to subscribe to the topic filter `some/+/topic` only:

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
                "arn:aws:iot:us-east-1:123456789012:client/${iot:Connection.Thing.ThingName}"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Subscribe"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topicfilter/some/+/topic"
            ]
        }
    ]
}
```

------
#### [ Unregistered Devices \(4\) ]

For devices not registered as things in the registry, the following policy grants permission to connect to with client ID `client1` and subscribe to the topic filter `some/+/topic` only:

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
          "arn:aws:iot:us-east-1:123456789012:client/client1"
        ]
      },
      {
        "Effect": "Allow",
        "Action": [
          "iot:Subscribe"
        ],
        "Resource": [
          "arn:aws:iot:us-east-1:123456789012:topicfilter/some/+/topic"
        ]
      }
    ]
}
```

------

**Note**  
In a policy, the MQTT wildcard character \+ is treated as a literal, not a wildcard\. Attempts to subscribe to topic filters that match the pattern `some/+/topic` fail and cause the client to disconnect\.

------
#### [ Registered Devices \(7\) ]

For devices registered as things in the registry, the following policy grants permission to connect to using the device's thing name as the client ID, and to subscribe to the topics `my/topic` and `my/othertopic`:

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
                "arn:aws:iot:us-east-1:123456789012:client/${iot:Connection.Thing.ThingName}"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Subscribe"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topicfilter/my/topic",
                "arn:aws:iot:us-east-1:123456789012:topicfilter/my/othertopic"
            ]
        }
    ]
}
```

------
#### [ Unregistered Devices \(7\) ]

For devices not registered as things in the registry, the following policy grants permission to connect to using client ID `client1`, and to subscribe to the topics `my/topic` and `my/othertopic`:

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
                "arn:aws:iot:us-east-1:123456789012:client/client1"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Subscribe"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topicfilter/my/topic",
                "arn:aws:iot:us-east-1:123456789012:topicfilter/my/othertopic"
            ]
        }
    ]
}
```

------

------
#### [ Registered Devices \(8\) ]

For devices registered as things in the registry, the following policy grants permission to connect to using the device's thing name as the client ID and to subscribe to a topic unique to that thing name/client ID:

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
                "arn:aws:iot:us-east-1:123456789012:client/${iot:Connection.Thing.ThingName}"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Publish"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/my/topic/${iot:Thing.ThingName}"
            ]
        }
    ]
}
```

------
#### [ Unregistered Devices \(8\) ]

For devices not registered as things in the registry, the following policy grants permission to connect to using client ID `client1`, and to publish to a topic unique to that client ID:

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
                "arn:aws:iot:us-east-1:123456789012:client/client1"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Publish"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/my/topic/${iot:ClientId}"
            ]
        }
    ]
}
```

------

------
#### [ Registered Devices \(9\) ]

For devices registered as things in the registry, the following policy grants permission to connect to using the device's thing name as the client ID and to publish to any topic prefixed by that thing name or client except for one topic ending with `bar`:

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
                "arn:aws:iot:us-east-1:123456789012:client/${iot:Connection.Thing.ThingName}"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Publish"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/${iot:Thing.ThingName}/*"
            ]
        },
        {
            "Effect": "Deny",
            "Action": [
                "iot:Publish"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/${iot:Thing.ThingName}/bar"
            ]
        }
    ]
}
```

------
#### [ Unregistered Devices \(9\) ]

For devices not registered as things in the registry, the following policy grants permission to connect to using client IDs `client1` and `client1` and to publish to any topic prefixed by the client ID used to connect, except for one topic ending with `bar`:

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
                "arn:aws:iot:us-east-1:123456789012:client/client1",
                "arn:aws:iot:us-east-1:123456789012:client/client2"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Publish"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/${iot:ClientId}/*"
            ]
        },
        {
            "Effect": "Deny",
            "Action": [
                "iot:Publish"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/${iot:ClientId}/bar"
            ]
        }
    ]
}
```

------

------
#### [ Registered Devices \(10\) ]

For devices registered as things in the registry, the following policy grants permission to connect to using the device's thing name as the client ID\. The device can subscribe to the topic `my/topic`, but cannot publish to the `<thing-name> /bar` where *<thing\-name>* is the name of the IoT thing connecting to :

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
                "arn:aws:iot:us-east-1:123456789012:client/${iot:Connection.Thing.ThingName}"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Subscribe"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topicfilter/my/topic"
            ]
        },
        {
            "Effect": "Deny",
            "Action": [
                "iot:Publish"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/${iot:Thing.ThingName}/bar"
            ]
        }
    ]
}
```

------
#### [ Unregistered Devices \(10\) ]

For devices not registered as things in the registry, the following policy grants permission to connect to using client ID `client1` and to subscribe to the topic `my/topic`:

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
                "arn:aws:iot:us-east-1:123456789012:client/client1"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Subscribe"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topicfilter/my/topic"
            ]
        }
    ]
}
```

------

Thing policy variables are also replaced when a certificate or authenticated Amazon Cognito Identity is attached to a thing\. The following policy grants permission to connect to with client ID `client1` and to publish and receive topic `iotmonitor/provisioning/987654321098`\. It also allows the certificate holder to subscribe to this topic\. 

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
                "arn:aws:iot:us-east-1:123456789012:client/client1"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Publish",
                "iot:Receive"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/iotmonitor/provisioning/987654321098"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Subscribe"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topicfilter/iotmonitor/provisioning/987654321098"
            ]
        }
    ]
}
```

## Policies for HTTP and WebSocket Clients<a name="pub-sub-policy-cognito"></a>

For the following operations, uses policies attached to Amazon Cognito identities \(through the `AttachPolicy` API\) to scope down the permissions attached to the Amazon Cognito Identity pool with authenticated identities\. That means an Amazon Cognito Identity needs permission from the IAM role policy attached to the pool and the policy attached to the Amazon Cognito Identity through the `AttachPolicy` API\.
+ `iot:Connect`
+ `iot:Publish`
+ `iot:Subscribe`
+ `iot:Receive`
+ `iot:GetThingShadow`
+ `iot:UpdateThingShadow`
+ `iot:DeleteThingShadow`​

**Note**  
For other operations or for unauthenticated identities, does not scope down the permissions attached to the Amazon Cognito identity pool role\. For both authenticated and unauthenticated identities, this is the most permissive policy that we recommend you attach to the Amazon Cognito pool role\.

**HTTP**

To allow unauthenticated Amazon Cognito identities to publish messages over HTTP on a topic specific to the Amazon Cognito Identity, attach the following policy to the Amazon Cognito Identity pool role:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iot:Publish",
            ],
            "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/${cognito-identity.amazonaws.com:sub}"]
        }
    ]
}
```

To allow authenticated users, attach the preceding policy to the Amazon Cognito Identity pool role and to the Amazon Cognito Identity using the [AttachPolicy](https://docs.aws.amazon.com/iot/latest/apireference/API_AttachPolicy.html) API\.

**Note**  
When authorizing Amazon Cognito identities, considers both policies and grants the least privileges specified\. An action is allowed only if both policies allow the requested action\. If either policy disallows an action, that action is unauthorized\.

**MQTT**

To allow unauthenticated Amazon Cognito identities to publish MQTT messages over WebSocket on a topic specific to the Amazon Cognito Identity in your account, attach the following policy to the Amazon Cognito Identity pool role:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iot:Publish",
            ],
            "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/${cognito-identity.amazonaws.com:sub}"]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Connect",
            ],
            "Resource": ["arn:aws:iot:us-east-1:123456789012:client/${cognito-identity.amazonaws.com:sub}"]
        }
    ]
}
```

To allow authenticated users, attach the preceding policy to the Amazon Cognito Identity pool role and to the Amazon Cognito Identity using the [AttachPolicy](https://docs.aws.amazon.com/iot/latest/apireference/API_AttachPolicy.html) API\.

**Note**  
When authorizing Amazon Cognito identities, considers both and grants the least privileges specified\. An action is allowed only if both policies allow the requested action\. If either policy disallows an action, that action is unauthorized\.

## Receive Policy Examples<a name="receive-policy"></a>

------
#### [ Registered Devices \(11\) ]

For devices registered in registry, the following policy grants permission to connect to with a client ID that matches the thing name and to subscribe to and receive messages on the `my/topic` topic:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iot:Connect"
            ],
            "Resource": ["arn:aws:iot:us-east-1:123456789012:client/${iot:Connection.Thing.ThingName}"]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Subscribe"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topicfilter/my/topic"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Receive"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/my/topic"
            ]
        }
    ]
}
```

------
#### [ Unregistered Devices \(11\) ]

For devices not registered in registry, the following policy grants permission to connect to with client ID `client1` and to subscribe to and receive messages on one topic:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iot:Connect"
            ],
            "Resource": ["arn:aws:iot:us-east-1:123456789012:client/client1"]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Subscribe"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topicfilter/my/topic"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Receive"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/my/topic"
            ]
        }
    ]
}
```

------