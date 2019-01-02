# Thing Policy Examples<a name="thing-policy-examples"></a>

The following policy allows a thing to publish on a specific topic that contains the thing type name and thing name:

```
{  
    "Version":"2012-10-17",
    "Statement":[{  
        "Effect":"Allow",
        "Action":["iot:Publish"],
        "Resource":[  
            "arn:aws:iot:us-east-1:123456789012:topic/${iot:Connection.Thing.ThingTypeName}/${iot:Connection.Thing.ThingName}"
        ]
    }]
}
```

The following policy allows the device to connect if the certificate used to authenticate with AWS IoT is attached to the thing for which the policy is being evaluated\.

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Connect"],
        "Resource": ["*"],  
        "Condition":{  
            "Bool":{  
                "iot:Connection.Thing.IsAttached":["true"]
            }
        }
    }]
}
```

The following policy allows a device to publish on a set of topics \("/foo/bar" and "/foo/baz"\) if:

+ The thing associated with the device has an attribute called "Manufacturer" with a value of "foo," "bar," or "baz\."

+ The thing associated with the device exists in the registry and is attached to the certificate used to connect to AWS IoT\.

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Publish"],
        "Resource": [
            "arn:aws:iot:us-east-1:123456789012:topic/foo/bar",
            "arn:aws:iot:us-east-1:123456789012:topic/foo/baz"
        ],
        "Condition": {
            "ForAnyValue:StringLike": {
                "iot:Connection.Thing.Attributes[Manufacturer]": [
                    "foo",
                    "bar",
                    "baz"
                ]
            }
        }
    }]
}
```

The following policy allows a device to publish to a topic if:

+ The topic is composed of the thing type name, a '/', and the thing name\.

+ The thing exists in the registry\.

+ The thing is attached to the certificate used to connect to AWS IoT\.

```
{  
    "Version":"2012-10-17",
    "Statement":[{  
        "Effect":"Allow",
        "Action":["iot:Publish"],
        "Resource":[  
            "arn:aws:iot:us-east-1:123456789012:topic/${iot:Connection.Thing.ThingTypeName}/${iot:Connection.Thing.ThingName}"
         ]
    }]
}
```

The following policy allows a device to publish only on its own shadow topic, if the corresponding thing exists in the registry\.

```
{
    "Version": "2012-10-17",
    "Statement": [{
       "Effect": "Allow",
        "Action": ["iot:Publish"],
        "Resource": [
            "arn:aws:iot:us-east-1:123456789012:topic/$aws/things/${iot:Connection.Thing.ThingName}/shadow/update"
        ]
    }]
}
```