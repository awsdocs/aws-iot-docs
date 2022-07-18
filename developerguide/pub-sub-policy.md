# Publish/Subscribe policy examples<a name="pub-sub-policy"></a>

The policy you use depends on how you're connecting to AWS IoT Core\. You can connect to AWS IoT Core by using an MQTT client, HTTP, or WebSocket\. When you connect with an MQTT client, you're authenticating with an X\.509 certificate\. When you connect over HTTP or the WebSocket protocol, you're authenticating with Signature Version 4 and Amazon Cognito\.

## Policies for MQTT clients<a name="pub-sub-policy-cert"></a>

To describe multiple topic names and topic filters in the `resource` attribute of a policy, use the `*` and `?` wildcard characters in place of the MQTT wildcard characters\.

MQTT and AWS IoT Core policies have different wildcard characters and they should be chosen after careful consideration\. In MQTT, the wildcard characters `+` and `#` are used in [MQTT topic filters](https://docs.aws.amazon.com/iot/latest/developerguide/topics.html#topicfilters) to subscribe to multiple topic names\. AWS IoT Core policies use `*` and `?` as wildcard characters and follow the conventions of [IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_grammar.html#policies-grammar-json)\. In a policy document, the `*` represents any combination of characters and a question mark `?` represents any single character\. In policy documents, the MQTT wildcard characters, `+` and `#` are treated as those characters with no special meaning\.

When choosing wildcard characters to use in a policy document, consider that the `*` character is not confined to a single topic level as the `+` character is in an MQTT topic filter\. To help constrain a wildcard specification to a single MQTT topic filter level, consider using multiple `?` characters\. For more information about using wildcard characters in a policy resource and more examples of what they match, see [Using wildcards in resource ARNs](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_resource.html#reference_policies_elements_resource_wildcards)\.

The table below shows the different wildcard characters used in MQTT and AWS IoT Core policies for MQTT clients\.


| Wildcard character | Is MQTT wildcard character | Example in MQTT | Is AWS IoT Core policy wildcard character | Example in AWS IoT Core policies for MQTT clients | 
| --- | --- | --- | --- | --- | 
| \# | Yes | some/\# | No | N/A | 
| \+ | Yes | some/\+/topic | No | N/A | 
| \* | No | N/A | Yes | topicfilter/some/\*/topic  | 
| ? | No | N/A | Yes |  `topic/some/?????/topic` `topicfilter/some/sensor???/topic`  | 

The following policy enables a device to publish to all subtopics that start with the same thing name\.

------
#### [ Registered devices \(5\) ]

For devices registered as things in the AWS IoT Core registry, the following policy grants permission to connect to AWS IoT Core using a client ID that matches the thing name and to publish to any topic prefixed by the thing name:

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
                "arn:aws:iot:us-east-1:123456789012:topic/${iot:Connection.Thing.ThingName}/*"
            ]
        }
    ]
}
```

------
#### [ Unregistered devices \(5\) ]

For devices not registered as things in the AWS IoT Core registry, the following policy grants permission to connect to AWS IoT Core using client ID `client1`, `client2`, or `client3` and to publish to any topic prefixed by the client ID:

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

You can also use the \* wildcard character at the end of a topic filter\. Using wildcard characters might lead to granting unintended privileges, so they should only be used after careful consideration\. One situation in which they might be useful is when devices must subscribe to messages with many different topics \(for example, if a device must subscribe to reports from temperature sensors in multiple locations\)\. 

------
#### [ Registered devices \(6\) ]

For devices registered as things in the AWS IoT Core registry, the following policy grants permission to connect to AWS IoT Core using the device's thing name as the client ID, and to subscribe to a topic prefixed by the thing name, followed by `room`, followed by any string\. \(It is expected that these topics are, for example, `thing1/room1`, `thing1/room2`, and so on\):

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
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Receive"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/${iot:Connection.Thing.ThingName}/room*"
            ]
        }
    ]
}
```

------
#### [ Unregistered devices \(6\) ]

For devices not registered as things in the AWS IoT Core registry, the following policy grants permission to connect to AWS IoT Core using client IDs `client1`, `client2`, `client3`, and to subscribe to a topic prefixed by the client ID, followed by `room`, followed by any string\. \(It is expected that these topics are, for example, `client1/room1`, `client1/room2`, and so on\):

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
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Receive"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/${iot:ClientId}/room*"
            ]
        }
    ]
}
```

------

When you specify topic filters in AWS IoT Core policies for MQTT clients, MQTT wildcard characters `+` and `#` are treated as literal strings\. Using these characters might result in unexpected behavior\. Because these characters are treated as literal strings at the time of enforcement, there is a possibility of unintentionally receiving data through a wildcard topic even if a Deny policy is present\. For example, if an application wants to allow a device to receive messages from all topics under `a/`except `a/restricted/...`, the following policy example will not work: 

```
{
     "Effect": "Allow",
     "Action": "iot:Subscribe",
     "Resource": "arn:aws:iot:us-east-1:123456789012:topicfilter/a/*"
 },
{
     "Effect": "Deny",
     "Action": "iot:Subscribe",
     "Resource": "arn:aws:iot:us-east-1:123456789012:topicfilter/a/restricted/*"
}
```

Based on the MQTT protocol, subscribing to the topic filter `a/#` permits clients to receive topics that include those starting with `a/restricted/[any string]`\. 

To allow a device to receive messages from all topics under `a/` except `a/restricted/...`, you can add another statement:

```
{
   "Effect": "Allow",
   "Action": "iot:Receive",
   "NotResource": "arn:aws:iot:us-east-1:123456789012:topic/a/restricted/*"
}
```

------
#### [ Registered devices \(4\) ]

For devices registered as things in the AWS IoT Core registry, the following policy grants permission to connect to AWS IoT Core with the client ID that matches the thing name, and to subscribe to the topic filter `some/+/topic` only\. Note in `topicfilter/some/+/topic` of the resource ARN, \+ is treated as a literal string in AWS IoT Core policies for MQTT clients, meaning that only the string `some/+/topic` matches the topic filter\.

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

For devices registered as things in the AWS IoT Core registry, the following policy grants permission to connect to AWS IoT Core with the client ID that matches the thing name, and to subscribe to the topic filter `some/*/topic`\. Note in `topicfilter/some/*/topic` of the resource ARN, \* is treated as a wildcard character in AWS IoT Core policies for MQTT clients, meaning that any string in the level that contains the character matches the topic filter\. \(It is expected that these topics are, for example, `some/string1/topic`, `some/string2/topic`, and so on\)

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
                "arn:aws:iot:us-east-1:123456789012:topicfilter/some/*/topic"
            ]
        }
    ]
}
```

------
#### [ Unregistered devices \(4\) ]

For devices not registered as things in the AWS IoT Core registry, the following policy grants permission to connect to AWS IoT Core with client ID `client1` and subscribe to the topic filter `some/+/topic` only\. Note in `topicfilter/some/+/topic` of the resource ARN, \+ is treated as a literal string in AWS IoT Core policies for MQTT clients, meaning that only the string `some/+/topic` matches the topic filter\.

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

For devices not registered as things in the AWS IoT Core registry, the following policy grants permission to connect to AWS IoT Core with client ID `client1` and subscribe to the topic filter `some/+/topic`\. Note in `topicfilter/some/*/topic` of the resource ARN, \* is treated as a wildcard character in AWS IoT Core policies for MQTT clients, meaning that any string in the level that contains the character matches the topic filter\. \(It is expected that these topics are, for example, `some/string1/topic`, `some/string2/topic`, and so on\)

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
          "arn:aws:iot:us-east-1:123456789012:topicfilter/some/*/topic"
        ]
      }
    ]
}
```

------

**Note**  
The MQTT wildcards `+` and `#` are treated as literal strings in an AWS IoT Core policy for MQTT clients\. To specify wildcard characters in topic names and topic filters in AWS IoT Core policies for MQTT clients, you must use `*`\. 

------
#### [ Registered devices \(7\) ]

For devices registered as things in the AWS IoT Core registry, the following policy grants permission to connect to AWS IoT Core using the device's thing name as the client ID, and to subscribe to the topics `my/topic` and `my/othertopic`:

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
#### [ Unregistered devices \(7\) ]

For devices not registered as things in the AWS IoT Core registry, the following policy grants permission to connect to AWS IoT Core using client ID `client1`, and to subscribe to the topics `my/topic` and `my/othertopic`:

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
#### [ Registered devices \(8\) ]

For devices registered as things in the AWS IoT Core registry, the following policy grants permission to connect to AWS IoT Core using the device's thing name as the client ID and to subscribe to a topic unique to that thing name/client ID:

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
#### [ Unregistered devices \(8\) ]

For devices not registered as things in the AWS IoT Core registry, the following policy grants permission to connect to AWS IoT Core using client ID `client1`, and to publish to a topic unique to that client ID:

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
#### [ Registered devices \(9\) ]

For devices registered as things in the AWS IoT Core registry, the following policy grants permission to connect to AWS IoT Core using the device's thing name as the client ID and to publish to any topic prefixed by that thing name or client except for one topic ending with `bar`:

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
#### [ Unregistered devices \(9\) ]

For devices not registered as things in the AWS IoT Core registry, the following policy grants permission to connect to AWS IoT Core using client IDs `client1` and `client1` and to publish to any topic prefixed by the client ID used to connect, except for one topic ending with `bar`:

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
#### [ Registered devices \(10\) ]

For devices registered as things in the AWS IoT Core registry, the following policy grants permission to connect to AWS IoT Core using the device's thing name as the client ID\. The device can subscribe to the topic `my/topic`, but cannot publish to the `thing-name /bar` where *thing\-name* is the name of the IoT thing connecting to AWS IoT Core:

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
#### [ Unregistered devices \(10\) ]

For devices not registered as things in the AWS IoT Core registry, the following policy grants permission to connect to AWS IoT Core using client ID `client1` and to subscribe to the topic `my/topic`:

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

Thing policy variables are also replaced when a certificate or authenticated Amazon Cognito Identity is attached to a thing\. The following policy grants permission to connect to AWS IoT Core with client ID `client1` and to publish and receive topic `iotmonitor/provisioning/987654321098`\. It also allows the certificate holder to subscribe to this topic\. 

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

## Policies for HTTP and WebSocket clients<a name="pub-sub-policy-cognito"></a>

Amazon Cognito identities can be authenticated or unauthenticated\. Authenticated identities belong to users who are authenticated by any supported identity provider\. Unauthenticated identities typically belong to guest users who do not authenticate with an identity provider\. Amazon Cognito provides a unique identifier and AWS credentials to support unauthenticated identities\. 

For the following operations, AWS IoT Core uses AWS IoT Core policies attached to Amazon Cognito identities \(through the `AttachPolicy` API\) to scope down the permissions attached to the Amazon Cognito Identity pool with authenticated identities\.
+ `iot:Connect`
+ `iot:Publish`
+ `iot:Subscribe`
+ `iot:Receive`
+ `iot:GetThingShadow`
+ `iot:UpdateThingShadow`
+ `iot:DeleteThingShadow`

That means an Amazon Cognito Identity needs permission from the IAM role policy attached to the pool and the AWS IoT Core policy attached to the Amazon Cognito Identity through the AWS IoT Core `AttachPolicy` API\.

Authenticated and unauthenticated users are different identity types\. If you don't attach an AWS IoT policy to the Amazon Cognito Identity, an authenticated user fails authorization in AWS IoT and doesn't have access to AWS IoT resources and actions\.

**Note**  
For other AWS IoT Core operations or for unauthenticated identities, AWS IoT Core does not scope down the permissions attached to the Amazon Cognito identity pool role\. For both authenticated and unauthenticated identities, this is the most permissive policy that we recommend you attach to the Amazon Cognito pool role\.

**HTTP**

To allow unauthenticated Amazon Cognito identities to publish messages over HTTP on a topic specific to the Amazon Cognito Identity, attach the following IAM policy to the Amazon Cognito Identity pool role:

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

To allow authenticated users, attach the preceding policy to the Amazon Cognito Identity pool role and to the Amazon Cognito Identity using the AWS IoT Core [AttachPolicy](https://docs.aws.amazon.com/iot/latest/apireference/API_AttachPolicy.html) API\.

**Note**  
When authorizing Amazon Cognito identities, AWS IoT Core considers both policies and grants the least privileges specified\. An action is allowed only if both policies allow the requested action\. If either policy disallows an action, that action is unauthorized\.

**MQTT**

To allow unauthenticated Amazon Cognito identities to publish MQTT messages over WebSocket on a topic specific to the Amazon Cognito Identity in your account, attach the following IAM policy to the Amazon Cognito Identity pool role:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iot:Publish"
            ],
            "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/${cognito-identity.amazonaws.com:sub}"]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Connect"
            ],
            "Resource": ["arn:aws:iot:us-east-1:123456789012:client/${cognito-identity.amazonaws.com:sub}"]
        }
    ]
}
```

To allow authenticated users, attach the preceding policy to the Amazon Cognito Identity pool role and to the Amazon Cognito Identity using the AWS IoT Core [AttachPolicy](https://docs.aws.amazon.com/iot/latest/apireference/API_AttachPolicy.html) API\.

**Note**  
When authorizing Amazon Cognito identities, AWS IoT Core considers both and grants the least privileges specified\. An action is allowed only if both policies allow the requested action\. If either policy disallows an action, that action is unauthorized\.

## Receive policy examples<a name="receive-policy"></a>

------
#### [ Registered devices \(11\) ]

For devices registered in AWS IoT Core registry, the following policy grants permission to connect to AWS IoT Core with a client ID that matches the thing name and to subscribe to and receive messages on the `my/topic` topic:

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
#### [ Unregistered devices \(11\) ]

For devices not registered in AWS IoT Core registry, the following policy grants permission to connect to AWS IoT Core with client ID `client1` and to subscribe to and receive messages on one topic:

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