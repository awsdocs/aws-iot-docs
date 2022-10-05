# Thing policy examples<a name="thing-policy-examples"></a>

The following policy allows a device to connect if the certificate used to authenticate with AWS IoT Core is attached to the thing for which the policy is being evaluated:

```
{  
    "Version":"2012-10-17",
    "Statement":[
        {  
            "Effect":"Allow",
            "Action":["iot:Connect"],
            "Resource":[ "*" ],
            "Condition": {
                "Bool": {
                    "iot:Connection.Thing.IsAttached": ["true"]
                }
            }
        }
    ]
}
```

The following policy allows a device to publish if the certificate is attached to a thing with a particular thing type and if the thing has an attribute of `attributeName` with value `attributeValue`\. For more information about thing policy variables, see [Thing policy variables](thing-policy-variables.md)\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Publish"
      ],
      "Resource": "arn:aws:iot:us-east-1:123456789012:topic/device/stats",
      "Condition": {
        "StringEquals": {
          "iot:Connection.Thing.Attributes[attributeName]": "attributeValue",
          "iot:Connection.Thing.ThingTypeName": "Thing_Type_Name"
        },
        "Bool": {
          "iot:Connection.Thing.IsAttached": "true"
        }
      }
    }
  ]
}
```

The following policy allows a device to publish to a topic that starts with an attribute of the thing\. If the device certificate is not associated with the thing, this variable won't be resolved and will result in an access denied error\. For more information about thing policy variables, see [Thing policy variables](thing-policy-variables.md)\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Publish"
      ],
      "Resource": "arn:aws:iot:us-east-1:123456789012:topic/${iot:Connection.Thing.Attributes[attributeName]}/*"
    }
  ]
}
```