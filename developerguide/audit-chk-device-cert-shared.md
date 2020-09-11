# Device certificate shared<a name="audit-chk-device-cert-shared"></a>

Multiple, concurrent connections use the same X\.509 certificate to authenticate with AWS IoT\.

This check appears as `DEVICE_CERTIFICATE_SHARED_CHECK` in the CLI and API\.

Severity: **Critical**

## Details<a name="audit-chk-device-cert-shared-details"></a>

When performed as part of an on\-demand audit, this check looks at the certificates and client IDs that were used by devices to connect during the 31 days before the start of the audit up to 2 hours before the check is run\. For scheduled audits, this check looks at data from 2 hours before the last time the audit was run to 2 hours before the time this instance of the audit started\. If you have taken steps to mitigate this condition during the time checked, note when the concurrent connections were made to determine if the problem persists\.

The following reason codes are returned when this check finds a noncompliant certificate:
+ CERTIFICATE\_SHARED\_BY\_MULTIPLE\_DEVICES

In addition, the findings returned by this check include the ID of the shared certificate, the IDs of the clients using the certificate to connect, and the connect/disconnect times\. Most recent results are listed first\.

## Why it matters<a name="audit-chk-device-cert-shared-why-it-matters"></a>

Each device should have a unique certificate to authenticate with AWS IoT\. When multiple devices use the same certificate, this might indicate that a device has been compromised\. Its identity might have been cloned to further compromise the system\. 

## How to fix it<a name="audit-chk-device-cert-shared-how-to-fix"></a>

Verify that the device certificate has not been compromised\. If it has, follow your security best practices to mitigate the situation\. 

If you use the same certificate on multiple devices, you might want to:

1. Provision new, unique certificates and attach them to each device\. 

1. Verify that the new certificates are valid and the devices can use them to connect\.

1. Use [UpdateCertificate](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateCertificate.html) to mark the old certificate as REVOKED in AWS IoT\. You can also use mitigation actions to do the following:
   + Apply the `UPDATE_DEVICE_CERTIFICATE` mitigation action on your audit findings to make this change\. 
   + Apply the `ADD_THINGS_TO_THING_GROUP` mitigation action to add the device to a group where you can take action on it\.
   + Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom response in response to the Amazon SNS message\. 

   For more information, see [Mitigation actions](device-defender-mitigation-actions.md)\. 

1. Detach the old certificate from each of the devices\.