# Publish/Subscribe Policy Examples<a name="pub-sub-policy"></a>

The policy you use depends on how you are connecting to AWS IoT\. You can connect to AWS IoT using an MQTT client, HTTP, or WebSocket\. When you connect with an MQTT client, you are authenticating with an X\.509 certificate\. When you connect over HTTP or the WebSocket protocol, you are authenticating with Signature Version 4 and Amazon Cognito\.

## Policies for MQTT Clients<a name="pub-sub-policy-cert"></a>

When you specify topic filters in AWS IoT policies for MQTT clients, MQTT wildcard characters "\+" and "\#" are treated as literal characters\. Their use might result in unexpected behavior\. For example, the following policy allows a client to subscribe to the topic filter `foo/+/bar` only:

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
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Subscribe"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topicfilter/foo/+/bar"
            ]
        }
    ]
}
```

**Note**  
The MQTT wildcard character '\+' is not treated as a wildcard within a policy\. Attempts to subscribe to topic filters that match the pattern `foo/+/bar` like `foo/baz/bar` or `foo/goo/bar` fails and causes the client to disconnect\.

You can use "\*" as a wildcard in the resource attribute of the policy\. For example, the following policy allows the certificate holder to publish to all topics and subscribe to all topic filters in the AWS account:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iot:*"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

The following policy allows the certificate holder to publish to all topics in the AWS account:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iot:Publish",
                "iot:Connect"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

You can also use the "\*" wildcard at the end of a topic filter\. For example, the following policy allows the certificate holder to subscribe to a topic filter matching the pattern `foo/bar/*`:

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
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Subscribe"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topicfilter/foo/bar/*"
            ]
        }
    ]
}
```

The following policy allows the certificate holder to publish to the `foo/bar` and `foo/baz` topics:

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
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Publish"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/foo/bar",
                "arn:aws:iot:us-east-1:123456789012:topic/foo/baz"
            ]
        }
    ]
}
```

The following policy prevents the certificate holder from publishing to the `foo/bar` topic:

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
                "*"
            ]
        },
        {
            "Effect": "Deny",
            "Action": [
                "iot:Publish"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/foo/bar"
            ]
        }
    ]
}
```

The following policy allows the certificate holder to publish on topic `foo` and prevents the certificate holder from publishing to topic `bar`:

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
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Publish"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/foo"
            ]
        },
        {
            "Effect": "Deny",
            "Action": [
                "iot:Publish"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/bar"
            ]
        }
    ]
}
```

The following policy allows the certificate holder to subscribe to topic filter `foo/bar`:

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
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Subscribe"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topicfilter/foo/bar"
            ]
        }
    ]
}
```

The following policy allows the certificate holder to publish on the `arn:aws:iot:us-east-1:123456789012:topic/iotmonitor/provisioning/8050373158915119971` topic and allows the certificate holder to subscribe to the topic filter `arn:aws:iot:us-east-1:123456789012:topicfilter/iotmonitor/provisioning/8050373158915119971`:

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
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Publish",
                "iot:Receive"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/iotmonitor/provisioning/8050373158915119971"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Subscribe"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topicfilter/iotmonitor/provisioning/8050373158915119971"
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

To allow unauthenticated Amazon Cognito identities to publish messages over HTTP on any topic, attach the following policy to the Amazon Cognito identity pool role:

```
{
    "Version": "2012-10-17",
    "Statement": [
    {
        "Effect": "Allow",
        "Action": [
            "iot:Connect",
            "iot:Publish",
            "iot:Subscribe",
            "iot:Receive",
            "iot:GetThingShadow",
            "iot:UpdateThingShadow",
            "iot:DeleteThingShadow​"
        ],
        "Resource": ["*"]
    }]
}
```

To allow unauthenticated Amazon Cognito identities to publish MQTT messages over HTTP on any topic in your account, attach the following policy to the Amazon Cognito identity pool role:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Publish"],
        "Resource": ["*"]
    }]
}
```

**Note**  
This example is for illustration only\. Unless your service absolutely requires it, we recommend the use of a more restrictive policy, one that does not allow unauthenticated Amazon Cognito identities to publish on any topic\.

To allow unauthenticated Amazon Cognito identities to publish MQTT messages over HTTP on `topic1` in your account, attach the following policy to your Amazon Cognito identity pool role:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Publish"],
        "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/topic1"]
    }]
}
```

For an authenticated Amazon Cognito identity to publish MQTT messages over HTTP on `topic1` in your AWS account, you must specify two policies, as outlined here\. The first policy must be attached to an Amazon Cognito identity pool role\. It allows identities from that pool to make a publish call\. The second policy must be attached to an Amazon Cognito user using the AWS IoT [AttachPolicy](http://alpha-docs-aws.amazon.com//iot/latest/apireference/API_AttachPolicy.html) API\. It allows the specified Amazon Cognito user access to the `topic1` topic\.

Amazon Cognito identity pool policy:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": [ "iot:Publish"],
        "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/topic1"]
    }]
}
```

Amazon Cognito user policy: 

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Publish"],
        "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/topic1"]
    }]
}
```

Similarly, the following example policy allows the Amazon Cognito user to publish MQTT messages over HTTP on the `topic1` and `topic2` topics\. Two policies are required\. The first policy gives the Amazon Cognito identity pool role the ability to make the publish call\. The second policy gives the Amazon Cognito user access to the `topic1` and `topic2` topics\. 

Amazon Cognito identity pool policy:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Publish"],
        "Resource": ["*"]
    }]
}
```

Amazon Cognito user policy:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Publish"],
        "Resource": [
            "arn:aws:iot:us-east-1:123456789012:topic/topic1",
            "arn:aws:iot:us-east-1:123456789012:topic/topic2"
        ]
    }]
}
```

The following policies allow multiple Amazon Cognito users to publish to a topic\. Two policies per Amazon Cognito identity are required\. The first policy gives the Amazon Cognito identity pool role the ability to make the publish call\. The second and third policies give the Amazon Cognito users access to the topics `topic1` and `topic2`, respectively\.

Amazon Cognito identity pool policy:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Publish"],
        "Resource": ["*"]
    }]
}
```

Amazon Cognito user1 policy:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Publish"],
        "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/topic1"]
    }]
}
```

Amazon Cognito user2 policy:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Publish"],
        "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/topic2"]
    }]
}
```

## Receive Policy Examples<a name="receive-policy"></a>

The following policy prevents the certificate holder using any client ID from receiving messages from a topic:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": [
                "iot:Receive"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/foo/restricted"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:*"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

The following policy allows the certificate holder using any client ID to subscribe and receive messages on one topic:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iot:Connect"
            ],
            "Resource": [*]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Subscribe"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topicfilter/foo/bar"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iot:Receive"
            ],
            "Resource": [
                "arn:aws:iot:us-east-1:123456789012:topic/foo/bar"
            ]
        }
    ]
}
```