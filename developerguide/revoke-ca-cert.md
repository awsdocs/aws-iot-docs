# Revoke a client certificate<a name="revoke-ca-cert"></a>

If you detect suspicious activity on a registered client certificate, you can revoke it so that it can't be used again\.

## Revoke a client certificate \(console\)<a name="revoke-device-cert-console"></a>

**To revoke a client certificate using the AWS IoT console**

1. Sign in to the AWS Management Console and open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Secure**, choose **Certificates**\.

1. In the list of certificates, locate the certificate that you want to revoke, and open the option menu by using the ellipsis icon\.

1. In the option menu, choose **Revoke**\.

If the certificate was successfully revoked, it will show as **Revoked** in the list of certificates\.

## Revoke a client certificate \(CLI\)<a name="revoke-device-cert-cli"></a>

The AWS CLI provides the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/update-certificate.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/update-certificate.html) command to revoke a certificate\.

```
aws iot update-certificate \
    --certificate-id certificateId \
    --new-status REVOKED
```

If the command was successful, the certificate's status will be `REVOKED`\. Run [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/describe-certificate.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/describe-certificate.html) to see the certificate's status\.

```
aws iot describe-certificate \
    --certificate-id certificateId
```