# Create and Register an AWS IoT Device Certificate<a name="device-certs-create"></a>

You can use the AWS IoT console or the AWS IoT CLI to create an AWS IoT certificate\.

## To create a certificate \(console\)<a name="device-certs-create-console"></a>

****

1. Sign in to the AWS Management Console and open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Security** to expand the choices, and then choose **Certificates**\. Choose **Create**\.

1. Choose **One\-click certificate creation** \- **Create certificate**\. Alternatively, to generate a certificate with a certificate signing request \(CSR\), choose **Create with CSR**\.

1. Use the links to the public key, private key, and certificate to download each to a secure location\.

1. Choose **Activate**\.

## To create a certificate \(CLI\)<a name="device-certs-create-cli"></a>

The AWS IoT CLI provides two commands to create certificates:

+  [create\-keys\-and\-certificate](http://alpha-docs-aws.amazon.com/cli/latest/reference/iot/create-keys-and-certificate.html)

  The [CreateKeysAndCertificate](http://alpha-docs-aws.amazon.com/iot/latest/apireference/API_CreateKeysAndCertificate.html) API creates a private key, public key, and X\.509 certificate\.

+ [create\-certificate\-from\-csr](http://alpha-docs-aws.amazon.com/cli/latest/reference/iot/create-certificate-from-csr.html)

  The [CreateCertificateFromCSR](http://alpha-docs-aws.amazon.com/iot/latest/apireference/API_CreateCertificateFromCsr.html) API creates a certificate given a CSR\.