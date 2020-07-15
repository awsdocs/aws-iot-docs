# Register a client certificate when the client connects to AWS IoT \(just\-in\-time registration\)<a name="auto-register-device-cert"></a>

You can configure a CA certificate to enable client certificates it has signed to register with AWS IoT automatically the first time the client connects to AWS IoT\.

To register client certificates when a client connects to AWS IoT for the first time, you must enable the CA certificate for automatic registration and configure the first connection by the client to provide the required certificates\.

## Configure a CA certificate to support automatic registration \(console\)<a name="enable-auto-registration-console"></a>

**To configure a CA certificate to support automatic client certificate registration using the AWS IoT console**

1. Sign in to the AWS Management Console and open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Secure**, choose **CAs**\.

1. In the list of certificate authorities, find the one for which you want to enable automatic registration, and open the option menu by using the ellipsis icon\.

1. On the option menu, choose **Enable auto\-registration**\.

**Note**  
The auto\-registration status is not shown in the list of certificate authorities\. To see the auto\-registration status of a certificate authority, you must open the **Details** page of the certificate authority\.

## Configure a CA certificate to support automatic registration \(CLI\)<a name="enable-auto-registration-cli"></a>

If you have already registered your CA certificate with AWS IoT, use the [https://docs.aws.amazon.com/cli/latest/reference/iot/update-ca-certificate.html](https://docs.aws.amazon.com/cli/latest/reference/iot/update-ca-certificate.html) command to set `autoRegistrationStatus` of the CA certificate to `ENABLE`\.

```
aws iot update-ca-certificate \
--certificate-idcaCertificateId \
--new-auto-registration-status ENABLE
```

If you want to enable `autoRegistrationStatus` when you register the CA certificate, use the [https://docs.aws.amazon.com/cli/latest/reference/iot/register-ca-certificate.html](https://docs.aws.amazon.com/cli/latest/reference/iot/register-ca-certificate.html) command\.

```
aws iot register-ca-certificate \
--allow-auto-registration  \
--ca-certificate file://root_CA_pem_filename \
--verification-cert file://verification_cert_pem_filename
```

Use the [https://docs.aws.amazon.com/cli/latest/reference/iot/describe-ca-certificate.html](https://docs.aws.amazon.com/cli/latest/reference/iot/describe-ca-certificate.html) command to see the status of the CA certificate\.

## Configure the first connection by a client for automatic registration<a name="configure-auto-reg-first-connect"></a>

When a client attempts to connect to AWS IoT for the first time, it must present a file that contains both your registered CA certificate and the client certificate signed by your CA certificate as part of the TLS handshake\. You can combine the two files using a command, such as the following:

```
cat device_cert_filename ca_certificate_pem_filename  combined_filename
```

When the client connects to AWS IoT, use the *combined\_filename* file as the certificate file\. AWS IoT recognizes the CA certificate as a registered CA certificate, registers the client certificate, and sets its status to `PENDING_ACTIVATION`\. This means that the client certificate was automatically registered and is awaiting activation\. The client certificate's state must be `ACTIVE` before it can be used to connect to AWS IoT\.

When AWS IoT automatically registers a certificate or when a client presents a certificate in the `PENDING_ACTIVATION` status, AWS IoT publishes a message to the following MQTT topic:

`$aws/events/certificates/registered/caCertificateId`

Where `caCertificateId` is the ID of the CA certificate that issued the client certificate\.

The message published to this topic has the following structure:

```
{
        "certificateId": "certificateId",
        "caCertificateId": "caCertificateId",
        "timestamp": timestamp,
        "certificateStatus": "PENDING_ACTIVATION",
        "awsAccountId": "awsAccountId",
        "certificateRegistrationTimestamp": "certificateRegistrationTimestamp"
}
```

You can create a rule that listens on this topic and performs some actions\. We recommend that you create a Lambda rule that verifies the client certificate is not on a certificate revocation list \(CRL\), activates the certificate, and creates and attaches a policy to the certificate\. The policy determines which resources the client can access\. For more information about how to create a Lambda rule that listens on the `$aws/events/certificates/registered/caCertificateID` topic and performs these actions, see [Just\-in\-Time Registration of Client Certificates on AWS IoT](https://aws.amazon.com/blogs/iot/just-in-time-registration-of-device-certificates-on-aws-iot/)\.

If any error or exception occurs during the auto\-registration of the client certificates, AWS IoT sends events or messages to your logs in CloudWatch Logs\. For more information about setting up the logs for your account, see the [Amazon CloudWatch documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/)\. 