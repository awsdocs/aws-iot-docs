# Pass Role Permissions<a name="pass-role"></a>

Part of a rule definition is an IAM role that grants permission to access resources specified in the rule's action\. The rules engine assumes that role when the rule's action is triggered\. The role must be defined in the same AWS account as the rule\.

When creating or replacing a rule you are, in effect, passing a role to the rules engine\. The user performing this operation requires the `iam:PassRole` permission\. To ensure you have this permission, create a policy that grants the `iam:PassRole` permission and attach it to your IAM user\. The following policy shows how to allow `iam:PassRole` permission for a role\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1",
      "Effect": "Allow",
      "Action": [
        "iam:PassRole"
      ],
      "Resource": [
        "arn:aws:iam::123456789012:role/myRole"
      ]
    }
  ]
}
```

In this policy example, the `iam:PassRole` permission is granted for the role `myRole`\. The role is specified using the role's ARN\. You must attach this policy to your IAM user or role to which your user belongs\. For more information, see [Working with Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html)\.

**Note**  
Lambda functions use resource\-based policy, where the policy is attached directly to the Lambda function itself\. When creating a rule that invokes a Lambda function, you do not pass a role, so the user creating the rule does not need the `iam:PassRole` permission\. For more information about Lambda function authorization, see [Granting Permissions Using a Resource Policy](https://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html#intro-permission-model-access-policy)\. 