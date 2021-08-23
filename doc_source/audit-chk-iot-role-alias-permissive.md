# Role alias overly permissive<a name="audit-chk-iot-role-alias-permissive"></a>

AWS IoT role alias provides a mechanism for connected devices to authenticate to AWS IoT using X\.509 certificates and then obtain short\-lived AWS credentials from an IAM role that is associated with an AWS IoT role alias\. The permissions for these credentials must be scoped down using access policies with authentication context variables\. If your policies are not configured correctly, you could leave yourself exposed to an escalation of privilege attack\. This audit check ensures that the temporary credentials provided by AWS IoT role aliases are not overly permissive\. 

This check is triggered if one of the following conditions are found:
+ The policy provides administrative permissions to any services used in the past year by this role alias \(for example, "iot:\*", "dynamodb:\*", "iam:\*", and so on\)\.
+ The policy provides broad access to thing metadata actions, access to restricted AWS IoT actions, or broad access to AWS IoT data plane actions\.
+ The policy provides access to security auditing services such as "iam", "cloudtrail", "guardduty", "inspector", or "trustedadvisor"\.

This check appears as `IOT_ROLE_ALIAS_OVERLY_PERMISSIVE_CHECK` in the CLI and API\.

Severity: **Critical**

## Details<a name="audit-chk-iot-role-alias-permissive-details"></a>

The following reason codes are returned when this check finds a noncompliant IoT policy:
+ ALLOWS\_BROAD\_ACCESS\_TO\_USED\_SERVICES
+ ALLOWS\_ACCESS\_TO\_SECURITY\_AUDITING\_SERVICES
+ ALLOWS\_BROAD\_ACCESS\_TO\_IOT\_THING\_ADMIN\_READ\_ACTIONS
+ ALLOWS\_ACCESS\_TO\_IOT\_NON\_THING\_ADMIN\_ACTIONS
+ ALLOWS\_ACCESS\_TO\_IOT\_THING\_ADMIN\_WRITE\_ACTIONS
+ ALLOWS\_BROAD\_ACCESS\_TO\_IOT\_DATA\_PLANE\_ACTIONS

## Why it matters<a name="audit-chk-iot-role-alias-permissive-why-it-matters"></a>

By limiting permissions to those that are required for a device to perform its normal operations, you reduce the risks to your account if a device is compromised\.

## How to fix it<a name="audit-chk-iot-role-alias-permissive-how-to-fix"></a>

Follow these steps to fix any noncompliant policies attached to things, thing groups, or other entities:

1. Follow the steps in [Authorizing direct calls to AWS services using AWS IoT Core credential provider ](authorizing-direct-aws.md) to apply a more restrictive policy to your role alias\.

You can use mitigation actions to:
+ Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom action in response to the Amazon SNS message\. 

For more information, see [Mitigation actions](dd-mitigation-actions.md)\. 