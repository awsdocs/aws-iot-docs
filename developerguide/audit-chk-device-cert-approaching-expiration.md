# Device certificate expiring<a name="audit-chk-device-cert-approaching-expiration"></a>

A device certificate is expiring within 30 days or has expired\.

This check appears as `DEVICE_CERTIFICATE_EXPIRING_CHECK` in the CLI and API\.

Severity: **Medium**

## Details<a name="audit-chk-device-cert-approaching-expiration-details"></a>

This check applies to device certificates that are ACTIVE or PENDING\_TRANSFER\.

The following reason codes are returned when this check finds a noncompliant device certificate:
+ CERTIFICATE\_APPROACHING\_EXPIRATION
+ CERTIFICATE\_PAST\_EXPIRATION

## Why it matters<a name="audit-chk-device-cert-approaching-expiration-why-it-matters"></a>

A device certificate should not be used after it expires\.

## How to fix it<a name="audit-chk-device-cert-approaching-expiration-how-to-fix"></a>

Consult your security best practices for how to proceed\. You might want to:

1. Provision a new certificate and attach it to the device\. 

1. Verify that the new certificate is valid and the device is able to use it to connect\.

1. Use [UpdateCertificate](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateCertificate.html) to mark the old certificate as INACTIVE in AWS IoT\. You can also use mitigation actions to:
   + Apply the `UPDATE_DEVICE_CERTIFICATE` mitigation action on your audit findings to make this change\. 
   + Apply the `ADD_THINGS_TO_THING_GROUP` mitigation action to add the device to a group where you can take action on it\.
   + Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom response in response to the Amazon SNS message\. 

   For more information, see [Mitigation actions](dd-mitigation-actions.md)\. 

1. Detach the old certificate from the device\. \(See [DetachThingPrincipal](https://docs.aws.amazon.com/iot/latest/apireference/API_DetachThingPrincipal.html)\.\)