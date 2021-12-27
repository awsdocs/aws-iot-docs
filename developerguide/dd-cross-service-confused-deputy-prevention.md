# Cross\-service confused deputy prevention<a name="dd-cross-service-confused-deputy-prevention"></a>

The confused deputy problem is a security issue where an entity that doesn't have permission to perform an action can coerce a more\-privileged entity to perform the action\. In AWS, cross\-service impersonation can result in the confused deputy problem\. Cross\-service impersonation can occur when one service \(the *calling service*\) calls another service \(the *called service*\)\. The calling service can be manipulated to use its permissions to act on another customer's resources through the called service in a way it should not otherwise have permission to access\. To prevent this, AWS provides tools that help you protect your data for all services with service principals that have been given access to resources in your account\. 

There are three resources AWS IoT Device Defender acesses from you that can be effected by the confused deptuy security issue, running audits, sending SNS notifications for security profile violations and running mitigation actions\. For each of these actions, the values for `aws:SourceArn` must be as follows:
+ For resources passed in [UpdateAccountAuditConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateAccountAuditConfiguration.html) API \(RoleArn and notificationTarget RoleArn attributes\) you should scope down the resource policy by using `aws:SourceArn` as `arn:arnPartition:iot:region:accountId:`\.
+ For resources passed in [CreateMitigationAction](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateMitigationAction.html) API \(The RoleArn attribute\) you should scope down the resource policy by using `aws:SourceArn` as `arn:arnPartition:iot:region:accountId:mitigationaction/mitigationActionName`\.
+  For resources passed in [CreateSecurityProfile](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateSecurityProfile.html) API \(the alertTargets attribute\) you should scope down the resource policy by using `aws:SourceArn` as `arn:arnPartition:iot:region:accountId:securityprofile/securityprofileName`\.

The most effective way to protect against the confused deputy problem is to use the `aws:SourceArn` global condition context key with the full ARN of the resource\. If you don't know the full ARN of the resource or if you are specifying multiple resources, use the `aws:SourceArn` global context condition key with wildcards \(`*`\) for the unknown portions of the ARN\. For example, `arn:aws:servicename:*:123456789012:*`\.

The following example shows how you can use the `aws:SourceArn` and `aws:SourceAccount` global condition context keys in AWS IoT Device Defender to prevent the confused deputy problem\.

```
 {
"Version": "2012-10-17",
"Statement": {
  "Sid": "ConfusedDeputyPreventionExamplePolicy",
  "Effect": "Allow",
  "Principal": {
    "Service": "iot.amazonaws.com"
  },
  "Action": "sts:AssumeRole",
  "Condition": {
    "ArnLike": {
      "aws:SourceArn": "arn:aws:iot:*:123456789012::*"
    },
    "StringEquals": {
      "aws:SourceAccount": "123456789012:"
    }
  }
}
}
```