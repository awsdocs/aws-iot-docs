# Cross Account Access<a name="cross-account-access"></a>

AWS IoT allows you to enable a principal to publish or subscribe to a topic that is defined in an AWS account not owned by the principal\. You configure cross account access by creating an IAM policy and IAM role and then attaching the policy to the role\.

First, create a customer managed IAM policy as described in [Creating IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html), just like you would for other users and certificates in your AWS account\. 

------
#### [ registered devices \(16\) ]

For devices registered in AWS IoT registry, the following policy grants permission to devices connect to AWS IoT using a client ID that matches the device's thing name and to publish to a the `my/topic/<thing-name> ` where *<thing\-name>* is the device's thing name:

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
                "iot:Publish"
            ],
            "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/my/topic/${iot:Connection.Thing.ThingName}"],
        }
    ]
}
```

------
#### [ unregistered devices \(16\) ]

For devices not registered in AWS IoT registry, the following policy grants permission to a device to use the thing name `client1` registered in your account's \(123456789012\) AWS IoT registry to connect to AWS IoT and to publish to a client id specific topic whose name is prefixed with `my/topic/`:

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
            "Resource": ["arn:aws:iot:us-east-1:123456789012:topic/my/topic/${iot:ClientId}"]
        }
    ]
}
```

------

Next, follow the steps in [Creating a Role to Delegate Permissions to an IAM User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html)\. Enter the account id of the AWS account with which you want to share access\. Then, in the final step, attach the policy you just created to the role\. If, at a later time, you need to modify the AWS account ID to which you are granting access, you can use the following trust policy format to do so: 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": { "AWS": "arn:aws:iam:us-east-1:567890123456:user:MyUser" },
            "Action": "sts:AssumeRole",
        }
    ]
}
```