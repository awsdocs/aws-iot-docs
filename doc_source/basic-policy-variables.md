# Basic AWS IoT Core policy variables<a name="basic-policy-variables"></a>

AWS IoT Core defines the following basic policy variables:
+ `iot:ClientId`: The client ID used to connect to the AWS IoT Core message broker\.
+ `aws:SourceIp`: The IP address of the client connected to the AWS IoT Core message broker\.

The following AWS IoT Core policy shows a policy that uses policy variables:

```
{
    "Version": "2012-10-17",
    "Statement": [
      {
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
          "arn:aws:iot:us-east-1:123451234510:topic/my/topic/${iot:ClientId}"
        ]
      }
    ]
}
```

In these examples, `${iot:ClientId}` is replaced by the ID of the client connected to the AWS IoT Core message broker when the policy is evaluated\. When you use policy variables like `${iot:ClientId}`, you can inadvertently open access to unintended topics\. For example, if you use a policy that uses `${iot:ClientId}` to specify a topic filter:

```
{
    "Effect": "Allow",
    "Action": ["iot:Subscribe"],
    "Resource": [
      "arn:aws:iot:us-east-1:123456789012:topicfilter/my/${iot:ClientId}/topic"
    ]
}
```

A client can connect using `+` as the client ID\. This would allow the user to subscribe to any topic that matches the topic filter `my/+/topic`\. To protect against such security gaps, use the `iot:Connect` policy action to control which client IDs can connect\. For example, this policy allows only those clients whose client ID is `clientid1` to connect:

```
{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": ["iot:Connect"],
        "Resource": [
          "arn:aws:iot:us-east-1:123456789012:client/clientid1"
        ]
      }
    ]
}
```