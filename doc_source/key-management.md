# Key management in AWS IoT<a name="key-management"></a>

All connections to AWS IoT are done using TLS, so no client\-side encryption keys are necessary for the initial TLS connection\.

Devices must authenticate using an X\.509 certificate or an Amazon Cognito Identity\. You can have AWS IoT generate a certificate for you, in which case it will generate a public/private key pair\. If you are using the AWS IoT console you will be prompted to download the certificate and keys\. If you are using the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/create-keys-and-certificate.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/create-keys-and-certificate.html) CLI command, the certificate and keys are returned by the CLI command\. You are responsible for copying the certificate and private key onto your device and keeping it safe\.

AWS IoT does not currently support customer\-managed customer master keys \(CMKs\) from AWS Key Management Service \(AWS KMS\); however, Device Advisor and AWS IoT Wireless uses only a AWS KMS Owned Customer Master Key \(AOCMK\) to encrypt customer data\.

## Device Advisor<a name="device-advisor-key-management"></a>

All data sent to Device Advisor when using the AWS APIs is encrypted at rest\. Device Advisor encrypts all of your data at rest using AWS KMS customer master keys \(CMKs\) stored and managed in [AWS Key Management Service](https://aws.amazon.com/kms/)\. Device Advisor encrypts your data using AWS\-owned customer master keys\. For more information about AWS owned customer master keys, see [AWS\-owned CMKs](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#aws-owned-cmk)\.