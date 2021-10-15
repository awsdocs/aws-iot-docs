# Revoked device certificate still active<a name="audit-chk-revoked-device-cert"></a>

A revoked device certificate is still active\.

This check appears as `REVOKED_DEVICE_CERTIFICATE_STILL_ACTIVE_CHECK` in the CLI and API\.

Severity: **Medium**

## Details<a name="audit-chk-revoked-device-cert-details"></a>

A device certificate is in its CA's [certificate revocation list](https://en.wikipedia.org/wiki/Certificate_revocation_list), but it is still active in AWS IoT\.

This check applies to device certificates that are ACTIVE or PENDING\_TRANSFER\.

The following reason codes are returned when this check finds noncompliance:
+ CERTIFICATE\_REVOKED\_BY\_ISSUER

## Why it matters<a name="audit-chk-revoked-device-cert-why-it-matters"></a>

A device certificate is usually revoked because it has been compromised\. It is possible that it has not yet been revoked in AWS IoT due to an error or oversight\.

## How to fix it<a name="audit-chk-revoked-device-cert-how-to-fix"></a>

Verify that the device certificate has not been compromised\. If it has, follow your security best practices to mitigate the situation\. You might want to:

1. Provision a new certificate for the device\.

1. Verify that the new certificate is valid and the device is able to use it to connect\.

1. Use [UpdateCertificate](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateCertificate.html) to mark the old certificate as REVOKED in AWS IoT\. You can also use mitigation actions to:
   + Apply the `UPDATE_DEVICE_CERTIFICATE` mitigation action on your audit findings to make this change\. 
   + Apply the `ADD_THINGS_TO_THING_GROUP` mitigation action to add the device to a group where you can take action on it\.
   + Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom response in response to the Amazon SNS message\. 

   For more information, see [Mitigation actions](dd-mitigation-actions.md)\. 

1. Detach the old certificate from the device\. \(See [DetachThingPrincipal](https://docs.aws.amazon.com/iot/latest/apireference/API_DetachThingPrincipal.html)\.\)