# Certificate Policy Examples<a name="certificate-policy-examples"></a>

The following policy allows a device to publish on a topic whose name is equal to the `certificateId` of the certificate with which the device authenticated itself:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action":["iot:Publish"],
        "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/${iot:CertificateId}"]
    },
    {
        "Effect": "Allow",
        "Action": ["iot:Connect"],
        "Resource": ["*"]
    }]
}
```

The following policy allows a device to publish on a topic whose name is equal to the subject's common name field of the certificate with which the device authenticated itself:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action":["iot:Publish"],
        "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/${iot:Certificate.Issuer.CommonName}"]
    },
    {
        "Effect": "Allow",
        "Action": ["iot:Connect"],
        "Resource": ["*"]
    }]
}
```

The following policy allows a device to publish on a topic that is prefixed with "admin/" when the certificate used to authenticate the device has its `Subject.CommonName.2` field set to `"Administrator"`:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Connect"],
        "Resource": ["*"]
    },
    {
        "Effect": "Allow",
        "Action": ["iot:Publish"],
        "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/admin/*"],
        "Condition": {
            "StringEquals": {
                "iot:Certificate.Subject.CommonName.2": "Administrator"
            }
        }
    }]
}
```

The following policy allows a device to publish on a topic that is prefixed with "admin/" when the certificate used to authenticate the device has any one of its `Subject.Common` fields set to `"Administrator"`:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["iot:Connect"],
        "Resource": ["*"]
    },
    {
        "Effect": "Allow",
        "Action": ["iot:Publish"],
        "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/admin/*"],
        "Condition": {
            "ForAnyValue:StringEquals": {
                "iot:Certificate.Subject.CommonName.List": "Administrator"
            }
        }
    }]
}
```