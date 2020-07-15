# Activate or deactivate a client certificate<a name="activate-or-deactivate-device-cert"></a>

AWS IoT verifies that a client certificate is active when it authenticates a connection\.

You can create and register client certificates without activating them so they can't be used until you want to use them\. You can also deactivate active client certificates to disable them temporarily\. Finally, you can revoke client certificates to prevent them from any future use\. 

## Activate a client certificate \(console\)<a name="activate-device-cert-console"></a>

**To activate a client certificate using the AWS IoT console**

1. Sign in to the AWS Management Console and open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Secure**, choose **Certificates**\.

1. In the list of certificates, locate the certificate that you want to activate, and open the option menu by using the ellipsis icon\.

1. In the option menu, choose **Activate**\.

The certificate should show as **Active** in the list of certificates\.

## Deactivate a client certificate \(console\)<a name="deactivate-device-cert-console"></a>

**To deactivate a client certificate using the AWS IoT console**

1. Sign in to the AWS Management Console and open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Secure**, choose **Certificates**\.

1. In the list of certificates, locate the certificate that you want to deactivate, and open the option menu by using the ellipsis icon\.

1. In the option menu, choose **Deactivate**\.

The certificate should show as **Inactive** in the list of certificates\.

## Activate a client certificate \(CLI\)<a name="activate-device-cert-cli"></a>

The AWS CLI provides the [https://docs.aws.amazon.com/cli/latest/reference/iot/update-certificate.html](https://docs.aws.amazon.com/cli/latest/reference/iot/update-certificate.html) command to activate a certificate\.

```
aws iot update-certificate \
    --certificate-id certificateId \
    --new-status ACTIVE
```

If the command was successful, the certificate's status will be `ACTIVE`\. Run [https://docs.aws.amazon.com/cli/latest/reference/iot/describe-certificate.html](https://docs.aws.amazon.com/cli/latest/reference/iot/describe-certificate.html) to see the certificate's status\.

```
aws iot describe-certificate \
    --certificate-id certificateId
```

## Deactivate a client certificate \(CLI\)<a name="deactivate-device-cert-cli"></a>

The AWS CLI provides the [https://docs.aws.amazon.com/cli/latest/reference/iot/update-certificate.html](https://docs.aws.amazon.com/cli/latest/reference/iot/update-certificate.html) command to deactivate a certificate\.

```
aws iot update-certificate \
    --certificate-id certificateId \
    --new-status INACTIVE
```

If the command was successful, the certificate's status will be `INACTIVE`\. Run [https://docs.aws.amazon.com/cli/latest/reference/iot/describe-certificate.html](https://docs.aws.amazon.com/cli/latest/reference/iot/describe-certificate.html) to see the certificate's status\.

```
aws iot describe-certificate \
    --certificate-id certificateId
```