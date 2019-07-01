# Publish/Subscribe Policy Examples<a name="pub-sub-policy"></a>

The policy you use depends on how you are connecting to AWS IoT\. You can connect to AWS IoT using an MQTT client, HTTP, or WebSocket\. When you connect with an MQTT client, you are authenticating with an X\.509 certificate\. When you connect over HTTP or the WebSocket protocol, you are authenticating with Signature Version 4 and Amazon Cognito\.

## Policies for MQTT Clients<a name="pub-sub-policy-cert"></a>

When you specify topic filters in AWS IoT policies for MQTT clients, MQTT wildcard characters "\+" and "\#" are treated as literal characters\. Their use might result in unexpected behavior\.

------
#### [ Registered devices \(4\) ]

For devices registered as things in the AWS IoT registry, the following policy grants permission to connect to AWS IoT with the client id that matches the thing name, and to subscribe to the topic filter `some/+/topic` only:

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
#### [ Unregistered devices \(4\) ]

For devices not registered as things in the AWS IoT registry, the following policy grants permission to connect to AWS IoT with client ID `client1` and subscribe to the topic filter `some/+/topic` only:

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
Within a policy, the MQTT wildcard character '\+' is treated as a literal, not a wildcard\. Attempts to subscribe to topic filters that match the pattern `some/+/topic` fail and cause the client to disconnect\.

You can use "\*" as a wildcard in the resource attribute of the policy\. For example, if each device in your account must publish on a unique topic reserved for it alone, use the following policy:

------
#### [ Registered devices \(5\) ]

For devices registered as things in the AWS IoT registry, the following policy grants permission to connect to AWS IoT using a client ID that matches the thing name and to publish to any topic prefixed by the thing name:

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
#### [ Unregistered devices \(5\) ]

For devices not registered as things in the AWS IoT registry, the following policy grants permission to connect to AWS IoT using client id `client1`, `client2`, or `client3` and to publish to any topic prefixed by the client id:

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

You can also use the "\*" wildcard at the end of a topic filter\. Using wildcard characters could lead to granting unintended privileges, so they should only be used after careful consideration\. One situation in which they might be useful is when devices must subscribe to messages with many different topics, for example if a device must subscribe to reports from temperature sensors in multiple locations\. 

------
#### [ registered devices \(6\) ]

For devices registered as things in the AWS IoT registry, the following policy grants permission to connect to AWS IoT using the device's thing name as the client id, and to subscribe to a topic prefixed by the thing name, followed by `room`, followed by any string\. \(It is expected that these topics will be, for example, `thing1/room1`, `thing1/room2`\.\.\.\):

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
#### [ unregistered devices \(6\) ]

For devices not registered as things in the AWS IoT registry, the following policy grants permission to connect to AWS IoT using client ids `client1`, `client2`, `client3`, and to subscribe to a topic prefixed by the client id, followed by `room`, followed by any string\. \(It is expected that these topics will be, for example, `client1/room1`, `client1/room2`\.\.\.\):

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

------
#### [ registered devices \(7\) ]

For devices registered as things in the AWS IoT registry, the following policy grants permission to connect to AWS IoT using the device's thing name as the client id, and to subscribe to the topics `my/topic` and `my/othertopic`:

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
#### [ unregistered devices \(7\) ]

For devices not registered as things in the AWS IoT registry, the following policy grants permission to connect to AWS IoT using client id `client1`, and to subscribe to the topics `my/topic` and `my/othertopic`:

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
#### [ registered devices \(8\) ]

For devices registered as things in the AWS IoT registry, the following policy grants permission to connect to AWS IoT using the device's thing name as the client id and to subscribe to a topic unique to that thing name/client id:

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
#### [ unregistered devices \(8\) ]

For devices not registered as things in the AWS IoT registry, the following policy grants permission to connect to AWS IoT using client id `client1`, and to publish to a topic unique to that client ID:

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
#### [ registered devices \(9\) ]

For devices registered as things in the AWS IoT registry, the following policy grants permission to connect to AWS IoT using the device's thing name as the client id and to publish to any topic prefixed by that thing name/client except for one topic ending with `bar`:

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
#### [ unregistered devices \(9\) ]

For devices not registered as things in the AWS IoT registry, the following policy grants permission to connect to AWS IoT using client ids `client1` and `client1` and to publish to any topic prefixed by the client id used to connect, except for one topic ending with `bar`:

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
#### [ registered devices \(10\) ]

For devices registered as things in the AWS IoT registry, the following policy grants permission to connect to AWS IoT using the device's thing name as the client id\. The device can subscribe to the topic `my/topic`, but cannot publish to the `<thing-name> /bar` where *<thing\-name>* is the name of the IoT thing connecting to AWS IoT:

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
#### [ unregistered devices \(10\) ]

For devices not registered as things in the AWS IoT registry, the following policy grants permission to connect to AWS IoT using client id `client1` and to subscribe to the topic `my/topic`:

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

Thing policy variables are also replaced when a certificate or authenticated Amazon Cognito identity is attached to a thing\. The following policy grants permission to connect to AWS IoT with client id `client1` and to publish and receive to topic `iotmonitor/provisioning/987654321098`\. It also allows the certificate holder to subscribe to this same topic\. 

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

For the following operations, AWS IoT uses AWS IoT policies attached to Amazon Cognito identities \(through the `AttachPolicy` API\) to scope down the permissions attached to the Amazon Cognito identity pool with authenticated identities\. That means an Amazon Cognito identity needs permission from the IAM role policy attached to the pool and the AWS IoT policy attached to the Amazon Cognito identity through the AWS IoT `AttachPolicy` API\.
+ `iot:Connect`
+ `iot:Publish`
+ `iot:Subscribe`
+ `iot:Receive`
+ `iot:GetThingShadow`
+ `iot:UpdateThingShadow`
+ `iot:DeleteThingShadow`​

**Note**  
For other AWS IoT operations or for unauthenticated identities, AWS IoT does not scope down the permissions attached to the Amazon Cognito identity pool role\. For both authenticated and unauthenticated identities, this is the most permissive policy that we recommend attaching to the Amazon Cognito pool role\.

**HTTP**

To allow unauthenticated Amazon Cognito identities to publish messages over HTTP on a topic specific to the Cognito identity, attach the following policy to the Amazon Cognito identity pool role:

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

To allow authenticated users, attach the preceding policy to the Amazon Cognito identity pool role and to the Cognito identity using the AWS IoT [AttachPrincipalPolicy](https://docs.aws.amazon.com//iot/latest/apireference/API_AttachPrincipalPolicy.html) API\.

**Note**  
When authorizing Cognito identities, AWS IoT will consider both these policies and grant the least privileges specified\. An action is only allowed if both the policies allow the requested action, and if either one of these policies disallow an action, that action will be unauthorized\.

**MQTT**

To allow unauthenticated Amazon Cognito identities to publish MQTT messages over WebSockets on a topic specific to the Cognito identity in your account, attach the following policy to the Amazon Cognito identity pool role:

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
            "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/${cognito-identity.amazonaws.com:sub}"]
        }
    ]
}
```

To allow authenticated users, attach the preceding policy to the Amazon Cognito identity pool role and to the Cognito identity using the AWS IoT [AttachPrincipalPolicy](https://docs.aws.amazon.com//iot/latest/apireference/API_AttachPrincipalPolicy.html) API\.

**Note**  
When authorizing Cognito identities, AWS IoT will consider both these policies and grant the least privileges specified\. An action is only allowed if both the policies allow the requested action, and if either one of these policies disallow an action, that action will be unauthorized\.

## Receive Policy Examples<a name="receive-policy"></a>

------
#### [ registered devices \(11\) ]

For devices registered in AWS IoT registry, the following policy grants permission to connect to AWS IoT with a client id that matches the thing name and to subscribe to and receive messages on the `my/topic` topic:

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
#### [ unregistered devices \(11\) ]

For devices not registered in AWS IoT registry, the following policy grants permission to connect to AWS IoT with client id `client1` and to subscribe to and receive messages on one topic:

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