# Connect and Publish Policy Examples<a name="connect-and-pub"></a>

For devices registered as things in the registry, the following policy grants permission to connect to with a client ID that matches the thing name and restricts the device to publishing on a client\-ID or thing name\-specific MQTT topic\. For a connection to be successful, the thing name must be registered in the registry and be authenticated using an identity or principal attached to the thing:

```
{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action":["iot:Publish"],
        "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/${iot:Connection.Thing.ThingName}"]
      },
      {
        "Effect": "Allow",
        "Action": ["iot:Connect"],
        "Resource": ["arn:aws:iot:us-east-1:123456789012:client/${iot:Connection.Thing.ThingName}"]
      }
    ]
}
```

For devices not registered as things in the registry, the following policy grants permission to connect to with client ID `client1` and restricts the device to publishing on a clientID\-specific MQTT topic:

```
{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action":["iot:Publish"],
        "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/${iot:ClientId}"]
      },
      {
        "Effect": "Allow",
        "Action": ["iot:Connect"],
        "Resource": ["arn:aws:iot:us-east-1:123456789012:client/client1"]
      }
    ]
}
```
