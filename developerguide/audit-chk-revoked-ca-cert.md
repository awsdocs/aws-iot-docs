# CA certificate revoked but device certificates still active<a name="audit-chk-revoked-ca-cert"></a>

A CA certificate was revoked, but is still active in AWS IoT\.

This check appears as `REVOKED_CA_CERT_CHECK` in the CLI and API\.

Severity: **Critical**

## Details<a name="audit-chk-revoked-ca-cert-details"></a>

A CA certificate is marked as revoked in the certificate revocation list maintained by the issuing authority, but is still marked as ACTIVE or PENDING\_TRANSFER in AWS IoT\.

The following reason codes are returned when this check finds a noncompliant CA certificate:
+ CERTIFICATE\_REVOKED\_BY\_ISSUER

## Why it matters<a name="audit-chk-revoked-ca-cert-why-it-matters"></a>

A revoked CA certificate should no longer be used to sign device certificates\. It might have been revoked because it was compromised\. Newly added devices with certificates signed using this CA certificate might pose a security threat\. 

## How to fix it<a name="audit-chk-revoked-ca-cert-how-to-fix"></a>

1. Use [UpdateCACertificate](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateCACertificate.html) to mark the CA certificate as INACTIVE in AWS IoT\. You can also use mitigation actions to:
   + Apply the `UPDATE_CA_CERTIFICATE` mitigation action on your audit findings to make this change\. 
   + Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action to implement a custom response in response to the Amazon SNS message\. 

   For more information, see [Mitigation actions](dd-mitigation-actions.md)\.

1. Review the device certificate registration activity for the time after the CA certificate was revoked and consider revoking any device certificates that might have been issued with it during this time\. You can use [ListCertificatesByCA](https://docs.aws.amazon.com/iot/latest/apireference/API_ListCertificatesByCA.html) to list the device certificates signed by the CA certificate and [UpdateCertificate](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateCertificate.html) to revoke a device certificate\.