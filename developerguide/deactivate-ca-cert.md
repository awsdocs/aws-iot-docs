# Deactivate a CA certificate<a name="deactivate-ca-cert"></a>

When a certificate authority \(CA\) certificate is enabled for automatic client certificate registration, AWS IoT checks the CA certificate used to sign the client certificate to make sure the CA is `ACTIVE`\. If the CA certificate is `INACTIVE`, AWS IoT doesn't allow the client certificate to be registered\.

By setting the CA certificate as `INACTIVE`, you prevent any new client certificates issued by the CA from being registered automatically\.

**Note**  
Any registered client certificates that were signed by the compromised CA certificate continue to work until you explicitly revoke each one of them\.

## Deactivate a CA certificate \(console\)<a name="deactivate-ca-cert-console"></a>

**To deactivate a CA certificate using the AWS IoT console**

1. Sign in to the AWS Management Console and open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Secure**, choose **CAs**\.

1. In the list of certificate authorities, find the one that you want to deactivate, and open the option menu by using the ellipsis icon\.

1. On the option menu, choose **Deactivate**\.

The certificate authority should show as **Inactive** in the list\.

**Note**  
The AWS IoT console does not provide a way to list the certificates that were signed by the CA you deactivated\. For an AWS CLI option to list those certificates, see [Deactivate a CA certificate \(CLI\)](#deactivate-ca-cert-cli)\.

## Deactivate a CA certificate \(CLI\)<a name="deactivate-ca-cert-cli"></a>

The AWS CLI provides the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/update-ca-certificate.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/update-ca-certificate.html) command to deactivate a CA certificate\.

```
aws iot update-ca-certificate \
    --certificate-id certificateId \
    --new-status INACTIVE
```

Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/list-certificates-by-ca.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/list-certificates-by-ca.html) command to get a list of all registered client certificates that were signed by the specified CA\. For each client certificate signed by the specified CA certificate, use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/update-certificate.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/update-certificate.html) command to revoke the client certificate to prevent it from being used\.

Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/describe-ca-certificate.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/describe-ca-certificate.html) command to see the status of the CA certificate\.