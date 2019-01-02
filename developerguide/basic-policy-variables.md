# Basic Policy Variables<a name="basic-policy-variables"></a>

AWS IoT defines the following basic policy variables:

+ `iot:ClientId`: The client ID used to connect to the AWS IoT message broker\.

+ `aws:SourceIp`: The IP address of the client connected to the AWS IoT message broker\.

The following AWS IoT policy shows the use of policy variables:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Connect"],
        "Resource": [
            "arn:aws:iot:us-east-1:123451234510:client/${iot:ClientId}"
        ]
    },
    {
        "Effect": "Allow",
        "Action": ["iot:Publish"],
        "Resource": [
            "arn:aws:iot:us-east-1:123451234510:topic/foo/bar/${iot:ClientId}"
        ]
    }]
}
```

In these examples, `${iot:ClientId}` is replaced by the ID of the client connected to the AWS IoT message broker when the policy is evaluated\. When you use policy variables like `${iot:ClientId}`, you can inadvertently open access to unintended topics\. For example, if you use a policy that uses `${iot:ClientId}` to specify a topic filter:

```
{
    "Effect": "Allow",
    "Action": ["iot:Subscribe"],
    "Resource": [
        "arn:aws:iot:us-east-1:123456789012:topicfilter/foo/${iot:ClientId}/bar"
    ]
}
```

A client can connect using `+` as the client ID\. This would allow the user to subscribe to any topic matching the topic filter `foo/+/bar`\. To protect against such security gaps, use the `iot:Connect` policy action to control which client IDs are able to connect\. For example, this policy allows only clients whose client ID is `clientid1` to connect:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Connect"],
        "Resource": [
            "arn:aws:iot:us-east-1:123456789012:client/clientid1"
        ]
    }]
}
```