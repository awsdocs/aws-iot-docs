# Role alias allows access to unused services<a name="audit-chk-role-alias-unused-svcs"></a>

AWS IoT role alias provides a mechanism for connected devices to authenticate to AWS IoT using X\.509 certificates and then obtain short\-lived AWS credentials from an IAM role that is associated with an AWS IoT role alias\. The permissions for these credentials must be scoped down using access policies with authentication context variables\. If your policies are not configured correctly, you could leave yourself exposed to an escalation of privilege attack\. This audit check ensures that the temporary credentials provided by AWS IoT role aliases are not overly permissive\. 

This check is triggered if the role alias has access to services that haven't been used for the AWS IoT device in the last year\. For example, the audit reports if you have an IAM role linked to the role alias that has only used AWS IoT in the past year but the policy attached to the role also grants permission to `"iam:getRole"` and `"dynamodb:PutItem"`\.

This check appears as `IOT_ROLE_ALIAS_ALLOWS_ACCESS_TO_UNUSED_SERVICES_CHECK` in the CLI and API\.

Severity: **Medium**

## Details<a name="audit-chk-role-alias-unused-svcs-details"></a>

The following reason codes are returned when this check finds a noncompliant AWS IoT policy:
+ ALLOWS\_ACCESS\_TO\_UNUSED\_SERVICES

## Why it matters<a name="audit-chk-role-alias-unused-svcs-why-it-matters"></a>

By limiting permissions to those services that are required for a device to perform its normal operations, you reduce the risks to your account if a device is compromised\.

## How to fix it<a name="audit-chk-role-alias-unused-svcs-how-to-fix"></a>

Follow these steps to fix any noncompliant policies attached to things, thing groups, or other entities:

1. Follow the steps in [Authorizing direct calls to AWS services using AWS IoT Core credential provider ](authorizing-direct-aws.md) to apply a more restrictive policy to your role alias\.

You can use mitigation actions to:
+ Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom action in response to the Amazon SNS message\. 

For more information, see [Mitigation actions](dd-mitigation-actions.md)\. 