# CA\_CERT\_APPROACHING\_EXPIRATION\_CHECK<a name="audit-chk-ca-cert-approaching-expiration"></a>

A CA certificate is expiring within 30 days or has expired\.

Severity: **Medium**

## Details<a name="audit-chk-ca-cert-approaching-expiration-details"></a>

This check applies to CA certificates that are ACTIVE or PENDING\_TRANSFER\.

The following reason codes are returned when this check finds a noncompliant CA certificate:
+ CERTIFICATE\_APPROACHING\_EXPIRATION
+ CERTIFICATE\_PAST\_EXPIRATION

## Why it matters<a name="audit-chk-ca-cert-approaching-expiration-why-it-matters"></a>

An expired CA certificate should not be used to sign new device certificates\.

## How to fix it<a name="audit-chk-ca-cert-approaching-expiration-how-to-fix"></a>

Consult your security best practices for how to proceed\. You might want to:

1. Register a new CA certificate with AWS IoT\.

1. Verify that you are able to sign device certificates using the new CA certificate\.

1. Use [UpdateCACertificate](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateCACertificate.html) to mark the old CA certificate as INACTIVE in AWS IoT\. You can also use mitigation actions to do the following:
   + Apply the `UPDATE_CA_CERTIFICATE` mitigation action on your audit findings to make this change\. 
   + Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action if you want to implement a custom response in response to the Amazon SNS message\. 

   For more information, see [Mitigation Actions](device-defender-mitigation-actions.md)\. 