# Permissions<a name="device-defender-detect-permissions"></a>

This section contains information about how to set up the IAM roles and policies required to manage AWS IoT Device Defender Detect\. For more information, see the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)\.

## Give AWS IoT Device Defender detect permission to publish alerts to an SNS topic<a name="device-defender-detect-permissions-publish"></a>

If you use the `alertTargets` parameter in [CreateSecurityProfile](dd-api-iot-CreateSecurityProfile.md), you must specify an IAM role with two policies: a permissions policy and a trust policy\. The permissions policy grants permission to AWS IoT Device Defender to publish notifications to your SNS topic\. The trust policy grants AWS IoT Device Defender permission to assume the required role\.

### Permission policy<a name="detect-account-sns-permissions-policy"></a>

```
{
  "Version":"2012-10-17",
  "Statement":[
      {
        "Effect":"Allow",
        "Action":[
          "sns:Publish"
        ],
        "Resource":[
          "arn:aws:sns:region:account-id:your-topic-name"
        ]
    }
  ]
}
```

### Trust policy<a name="detect-account-sns-trust-policy"></a>

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "iot.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

### Pass role policy<a name="detect-account-passrole-policy"></a>

You also need an IAM permissions policy attached to the IAM user that allows the user to pass roles\. See [Granting a User Permissions to Pass a Role to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html)\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Action": [
          "iam:GetRole",
          "iam:PassRole"
      ],
      "Resource": "arn:aws:iam::account-id:role/Role_To_Pass"
    }
  ]
}
```