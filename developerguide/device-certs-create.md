# Create and Register an AWS IoT Device Certificate<a name="device-certs-create"></a>

You can use the AWS IoT console or the AWS CLI to create an AWS IoT certificate\.

## To create a certificate \(console\)<a name="device-certs-create-console"></a>

****

1. Sign in to the AWS Management Console and open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Security**, choose **Certificates**, and then choose **Create**\.

1. Choose **One\-click certificate creation** \- **Create certificate**\. Or to generate a certificate with a certificate signing request \(CSR\), choose **Create with CSR**\.

1. Use the links to the public key, private key, and certificate to download each to a secure location\.

1. Choose **Activate**\.

## To create a certificate \(CLI\)<a name="device-certs-create-cli"></a>

The AWS CLI provides two commands to create certificates:
+  [create\-keys\-and\-certificate](https://docs.aws.amazon.com/cli/latest/reference/iot/create-keys-and-certificate.html)

  The [CreateKeysAndCertificate](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateKeysAndCertificate.html) API creates a private key, public key, and X\.509 certificate\.
+ [create\-certificate\-from\-csr](https://docs.aws.amazon.com/cli/latest/reference/iot/create-certificate-from-csr.html)

  The [CreateCertificateFromCSR](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateCertificateFromCsr.html) API creates a certificate given a CSR\.