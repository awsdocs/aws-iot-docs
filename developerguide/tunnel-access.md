# Controlling Access to Tunnels<a name="tunnel-access"></a>

The Secure Tunneling service provides the following service\-specific actions, resources, and condition context keys for use in IAM permission policies\.

## Tunnel Access Prerequisites<a name="tunnel-access-prereq"></a>
+ Learn how to secure AWS resources by using [IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_controlling.html)\.
+ Learn how to create and evaluate [IAM conditions](      https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_multi-value-conditions.html)\.
+ Learn how to secure AWS resources using [resource tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html)\.

## iot:OpenTunnel<a name="open-tunnel-action"></a>

The `iot:OpenTunnel` policy action grants a principal permission to call [OpenTunnel](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-secure-tunneling_OpenTunnel.html)\. You must specify the wildcard tunnel ARN `arn:aws:iot:<aws-region>:<aws-account-id>:tunnel/*` in the `Resource` element of the IAM policy statement\. You can specify a thing ARN \(`arn:aws:iot:<aws-region>:<aws-account-id>:thing/ <thing-name>`\) in the `Resource` element of the IAM policy statement to manage `OpenTunnel` permission for specific IoT things\.

For example, the following policy statement allows you to open a tunnel to the IoT thing named `TestDevice`\.

```
{
    "Effect": "Allow",
    "Action": "iot:OpenTunnel",
    "Resource": [
        "arn:aws:iot:<aws-region>:<aws-account-id>:tunnel/*",
        "arn:aws:iot:<aws-region>:<aws-account-id>:thing/TestDevice"
    ]
}
```

The `iot:OpenTunnel` policy action supports the following condition keys:
+ `iot:ThingGroupArn`
+ `iot:TunnelDestinationService`
+ `aws:RequestTag`/*<tag\-key>*
+ `aws:TagKeys`

The following policy statement allows you to open a tunnel the thing if the thing belongs to a thing group with a name that starts with `TestGroup` and the configured destination service on the tunnel is SSH\.

```
{
    "Effect": "Allow",
    "Action": "iot:OpenTunnel",
    "Resource": [
        "arn:aws:iot:<aws-region>:<aws-account-id>:tunnel/*"
    ],
    "Condition": {
        "ForAnyValue:StringLike": {
            "iot:ThingGroupArn": [
                "arn:aws:iot:<aws-region>:<aws-account-id>:thinggroup/TestGroup*"
            ]
        },
        "ForAllValues:StringEquals": {
            "iot:TunnelDestinationService": [
                "SSH"
            ]
        }
    }
}
```

You can also use resource tags to control permission to open tunnels\. For example, the following policy statement allows a tunnel to be opened if the tag key `Owner` is present with a value of `Admin` and no other tags are specified\.

```
{
    "Effect": "Allow",
    "Action": "iot:OpenTunnel",
    "Resource": [
        "arn:aws:iot:<aws-region>:<aws-account-id>:tunnel/*"
    ],
    "Condition": {
        "StringEquals": {
            "aws:RequestTag/Owner": "Admin"
        },
        "ForAllValues:StringEquals": {
            "aws:TagKeys": "Owner"
        }
    }
}
```

## iot:DescribeTunnel<a name="describe-tunnel-action"></a>

The `iot:DescribeTunnel` policy action grants a principal permission to call [DescribeTunnel](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-secure-tunneling_DescribeTunnel.html)\. You can specify a fully qualified tunnel ARN \(for example, `arn:aws:iot:<aws-region>: <aws-account-id>:tunnel/<tunnel-id>`\) or use the wildcard tunnel ARN \(`arn:aws:iot:<aws-region>:<aws-account-id>:tunnel/*`\) in the `Resource` element of the IAM policy statement\.

The `iot:DescribeTunnel` policy action supports the following condition key:
+ `aws:ResourceTag/<tag-key>`

The following policy statement allows you to call `DescribeTunnel` if the requested tunnel is tagged with the key `Owner` with a value of `Admin`\.

```
{
    "Effect": "Allow",
    "Action": "iot:DescribeTunnel",
    "Resource": [
        "arn:aws:iot:<aws-region>:<aws-account-id>:tunnel/*"
    ],
    "Condition": {
        "StringEquals": {
            "aws:ResourceTag/Owner": "Admin"
        }
    }
}
```

## iot:ListTunnels<a name="list-tunnels-action"></a>

The `iot:ListTunnels` policy action grants a principal permission to call [ListTunnels](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-secure-tunneling_ListTunnels.html)\. You must specify the wildcard tunnel ARN \(`arn:aws:iot:<aws-region>:<aws-account-id>:tunnel/*`\) in the `Resource` element of the IAM policy statement\. To manage `ListTunnels` permission on selected IoT things, you can also specify a thing ARN \(`arn:aws:iot:<aws-region>:<aws-account-id>:thing/<thing-name>`\) in the `Resource` element of the IAM policy statement\.

The following policy statement allows you to list tunnels for the thing named `TestDevice`\.

```
{
    "Effect": "Allow",
    "Action": "iot:ListTunnels",
    "Resource": [
        "arn:aws:iot:<aws-region>:<aws-account-id>:tunnel/*",
        "arn:aws:iot:<aws-region>:<aws-account-id>:thing/TestDevice"
    ]
}
```

## iot:ListTagsForResource<a name="list-tags-for-resource-action"></a>

The `iot:ListTagsForResource` policy action grants a principal permission to call `ListTagsForResource`\. You can specify a fully qualified tunnel ARN \(`arn:aws:iot:<aws-region>: <aws-account-id>:tunnel/<tunnel-id>`\) or use the wildcard tunnel ARN \(`arn:aws:iot:<aws-region>:<aws-account-id>:tunnel/*`\) in the `Resource` element of the IAM policy statement\.

## iot:CloseTunnel<a name="close-tunnel-action"></a>

The `iot:CloseTunnel` policy action grants a principal permission to call [CloseTunnel](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-secure-tunneling_CloseTunnel.html)\. You can specify a fully qualified tunnel ARN \(`arn:aws:iot:<aws-region>: <aws-account-id>:tunnel/<tunnel-id>`\) or use the wildcard tunnel ARN \(`arn:aws:iot:<aws-region>:<aws-account-id>:tunnel/*`\) in the `Resource` element of the IAM policy statement\.

The `iot:CloseTunnel` policy action supports the following condition keys:
+ `iot:Delete`
+ `aws:ResourceTag/<tag-key>`

The following policy statement allows you to call `CloseTunnel` if the request’s `Delete` parameter is `false` and the requested tunnel is tagged with the key `Owner` with a value of `QATeam`\.

```
{
    "Effect": "Allow",
    "Action": "iot:CloseTunnel",
    "Resource": [
        "arn:aws:iot:<aws-region>:<aws-account-id>:tunnel/*"
    ],
    "Condition": {
        "Bool": {
            "iot:Delete": "false"
        },
        "StringEquals": {
            "aws:ResourceTag/Owner": "QATeam"
        }
    }
}
```

## iot:TagResource<a name="tag-resource-action"></a>

The `iot:TagResource` policy action grants a principal permission to call `TagResource`\. You can specify a fully qualified tunnel ARN \(`arn:aws:iot:<aws-region>: <aws-account-id>:tunnel/<tunnel-id>`\) or use the wildcard tunnel ARN \(`arn:aws:iot:<aws-region>:<aws-account-id>:tunnel/*`\) in the `Resource` element of the IAM policy statement\.

## iot:UntagResource<a name="untag-resource-action"></a>

The `iot:UntagResource` policy action grants a principal permission to call `UntagResource`\. You can specify a fully qualified tunnel ARN \(`arn:aws:iot:<aws-region>:<aws-account-id>:tunnel/<tunnel-id>`\) or use the wildcard tunnel ARN \(`arn:aws:iot:<aws-region>:<aws-account-id>:tunnel/*`\) in the `Resource` element of the IAM policy statement\.

For more information about AWS IoT security see [Identity and Access Management for AWS IoT](security-iam.md)\.