# Register your CA certificate<a name="register-CA-cert"></a>

These procedures describe how to register a CA certificate with AWS IoT\. 

## Register a CA certificate \(console\)<a name="register-CA-cert-console"></a>

**Note**  
To register a CA certificate in the console, start in the console at [Register CA certificate](https://console.aws.amazon.com/iot/home#/create/cacertificate)\. 

## Register a CA certificate \(CLI\)<a name="register-CA-cert-cli"></a>

 You can register a CA certificate in `DEFAULT` mode or `SNI_ONLY` mode\. For more information about CA certificate mode, see [certificateMode](https://docs.aws.amazon.com/iot/latest/apireference/API_CACertificateDescription.html#iot-Type-CACertificateDescription-certificateMode)\. 

### Register a CA certificate in `DEFAULT` mode \(CLI\)<a name="register-CA-cert-default-cli"></a>

Make sure you have the following available on your computer before you continue:
+ The root CA's certificate file \(referenced below as `root_CA_cert_filename.pem`\)
+ The root CA certificate's private key file \(referenced below as `root_CA_key_filename.key`\)
+ [OpenSSL v1\.1\.1i](https://www.openssl.org/) or later

**To register a CA certificate in `DEFAULT` mode using the AWS CLI**

1. To get a registration code from AWS IoT use get\-registration\-code\. Save the returned `registrationCode` to use as the `Common Name` of the private key verification certificate\. For more information, see [get\-registration\-code](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/get-registration-code.html) in the *AWS CLI Command Reference*\.

   ```
   aws iot get-registration-code
   ```

1. Generate a key pair for the private key verification certificate:

   ```
   openssl genrsa -out verification_cert_key_filename.key 2048
   ```

1. Create a certificate signing request \(CSR\) for the private key verification certificate\. Set the `Common Name` field of the certificate to the `registrationCode` returned by get\-registration\-code\.

   ```
   openssl req -new \
       -key verification_cert_key_filename.key \
       -out verification_cert_csr_filename.csr
   ```

   You are prompted for some information, including the `Common Name` for the certificate\.

   ```
   You are about to be asked to enter information that will be incorporated
   into your certificate request.
   What you are about to enter is what is called a Distinguished Name or a DN.
   There are quite a few fields but you can leave some blank
   For some fields there will be a default value,
   If you enter '.', the field will be left blank.
   -----
   Country Name (2 letter code) [AU]:
       State or Province Name (full name) []:
       Locality Name (for example, city) []:
       Organization Name (for example, company) []:
       Organizational Unit Name (for example, section) []:
       Common Name (e.g. server FQDN or YOUR name) []:your_registration_code
       Email Address []:
   
       Please enter the following 'extra' attributes
       to be sent with your certificate request
       A challenge password []:
       An optional company name []:
   ```

1. Use the CSR to create a private key verification certificate:

   ```
   openssl x509 -req \
       -in verification_cert_csr_filename.csr \
       -CA root_CA_cert_filename.pem \
       -CAkey root_CA_key_filename.key \
       -CAcreateserial \
       -out verification_cert_filename.pem \
       -days 500 -sha256
   ```

1. Register the CA certificate with AWS IoT\. Pass in the CA certificate file name and the private key verification certificate file name to the register\-ca\-certificate command, as follows\. For more information, see [register\-ca\-certificate](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/register-ca-certificate.html) in the *AWS CLI Command Reference*\.

   ```
   aws iot register-ca-certificate \
       --ca-certificate file://root_CA_cert_filename.pem \
       --verification-cert file://verification_cert_filename.pem
   ```

   This command returns the *certificateId*, if successful\.

1. At this point, the CA certificate has been registered with AWS IoT but is not active\. The CA certificate must be active before you can register any client certificates it has signed\.

   This step activates the CA certificate\.

   To activate the CA certificate, use the update\-certificate command as follows\. For more information, see [update\-certificate](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/update-certificate.html) in the *AWS CLI Command Reference*\.

   ```
   aws iot update-ca-certificate \
       --certificate-id certificateId \
       --new-status ACTIVE
   ```

To see the status of the CA certificate, use the describe\-ca\-certificate command\. For more information, see [describe\-ca\-certificate](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/describe-ca-certificate.html) in the *AWS CLI Command Reference*\.

### Register a CA certificate in SNI\_ONLY mode \(CLI\)<a name="register-CA-cert-SNI-cli"></a>

Make sure you have the following available on your computer before you continue:
+ The root CA's certificate file \(referenced below as `root_CA_cert_filename.pem`\)
+ [OpenSSL v1\.1\.1i](https://www.openssl.org/) or later

**To register a CA certificate in `SNI_ONLY` mode using the AWS CLI**

1. Register the CA certificate with AWS IoT\. Pass in the CA certificate file name to the register\-ca\-certificate command\. For more information, see [register\-ca\-certificate](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/register-ca-certificate.html) in the *AWS CLI Command Reference*\.

   ```
   aws iot register-ca-certificate \
       --ca-certificate file://root_CA_cert_filename.pem \
       --certificate-mode SNI_ONLY
   ```

   This command returns the *certificateId*, if successful\.

1. At this point, the CA certificate has been registered with AWS IoT but is not active\. The CA certificate must be active before you can register any client certificates that it has signed\.

   This step activates the CA certificate\.

   To activate the CA certificate, use the update\-certificate command as follows\. For more information, see [update\-certificate](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/update-certificate.html) in the *AWS CLI Command Reference*\.

   ```
   aws iot update-ca-certificate \
       --certificate-id certificateId \
       --new-status ACTIVE
   ```

To see the status of the CA certificate, use the describe\-ca\-certificate command\. For more information, see [describe\-ca\-certificate](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/describe-ca-certificate.html) in the *AWS CLI Command Reference*\.