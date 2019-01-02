# Cross Account Access<a name="cross-account-access"></a>

AWS IoT allows you to enable a principal to publish or subscribe to a topic that is defined in an AWS account not owned by the principal\. You configure cross account access by creating an IAM policy and IAM role and then attaching the policy to the role\.

First, create an IAM policy just like you would for other users and certificates in your AWS account\. For example, the following policy grants permissions to connect and publish to the `/foo/bar` topic\.

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
             "arn:aws:iot:us-east-1:123456789012:topic/foo/bar"
         ]
     }]
}
```

Next, follow the steps in [Creating a Role for an IAM User](http://alpha-docs-aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html)\. Enter the AWS account ID of the AWS account with which you want to share access\. Then, in the final step, attach the policy you just created to the role\. If, at a later time, you need to modify the AWS account ID to which you are granting access, you can use the following trust policy format to do so\.

```
{
    "Version":"2012-10-17",
    "Statement":[{
        "Effect": "Allow",
        "Principal": {
             "AWS": "arn:aws:iam:us-east-1:111111111111:user/MyUser"
        },
        "Action": "sts:AssumeRole"
    }]
}
```