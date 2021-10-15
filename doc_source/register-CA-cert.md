# Register your CA certificate<a name="register-CA-cert"></a>

You might need to register your certificate authority \(CA\) with AWS IoT if you are using client certificates signed by a CA that AWS IoT doesn't recognize\.

If you want clients to automatically register their client certificates with AWS IoT when they first connect, the CA that signed the client certificates must be registered with AWS IoT\. Otherwise, you don't need to register the CA certificate that signed the client certificates\.

**Note**  
A CA certificate can be registered by only one account in a Region\.

## Register a CA certificate \(console\)<a name="register-CA-cert-console"></a>

**Note**  
Make sure you have the root CA's certificate file and private key file before you begin\.  
This procedure also requires using the command line interface to run OpenSSL v1\.1\.1i commands\.

**To register a CA certificate using the AWS IoT console**

1. Sign in to the AWS Management Console and open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Secure**, choose **CAs**, and then **Register**\.

1. In **Select a CA**, choose **Register CA**\.

1. In **Register a CA certificate**, follow the steps displayed\.

   Steps 1 \- 4 are performed in the command line interface\.

   Steps 5 and 6 require the files created in Steps 3 and 4\.

1. If you want to activate this certificate when you register it, check **Activate CA certificate**\.

   The CA certificate must be active before you can register any client certificates that are signed by it\.

1. If you want to enable this certificate to automatically register client certificates signed by this certificate, select **Enable auto\-registration of device certificates**\.

1. Choose **Register CA certificate** to complete the registration\.

The CA certificate appears in the list of certificate authorities with its current status\.

## Register a CA certificate \(CLI\)<a name="register-CA-cert-cli"></a>

**Note**  
Make sure you have the root CA's certificate file and private key file before you begin\.

**To register a CA certificate using the AWS CLI**

1. Use [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/get-registration-code.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/get-registration-code.html) to get a registration code from AWS IoT\. Save the `registrationCode` returned to use as the `Common Name` of the private key verification certificate\.

   ```
   aws iot get-registration-code
   ```

1. Generate a key pair for the private key verification certificate:

   ```
   openssl genrsa -out verification_cert_key_filename 2048
   ```

1. Create a certificate signing request \(CSR\) for the private key verification certificate\. Set the `Common Name` field of the certificate to the `registrationCode` returned by get\-registration\-code\.

   ```
   openssl req -new \
       -key verification_cert_key_filename \
       -out verification_cert_csr_filename
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
       -in verification_cert_csr_filename \
       -CA root_CA_pem_filename \
       -CAkey root_CA_key_filename \
       -CAcreateserial \
       -out verification_cert_pem_filename \
       -days 500 -sha256
   ```

1. Register the CA certificate with AWS IoT\. Pass in the CA certificate filename and the private key verification certificate filename to the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/register-ca-certificate.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/register-ca-certificate.html) command:

   ```
   aws iot register-ca-certificate \
       --ca-certificate file://root_CA_pem_filename \
       --verification-cert file://verification_cert_pem_filename
   ```

   This command returns the *certificateId*, if successful\.

1. At this point, the CA certificate has been registered with AWS IoT, but is not active\. The CA certificate must be active before you can register any client certificates that are signed by it\.

   This step activates the CA certificate\.

   Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/update-certificate.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/update-certificate.html) CLI command to activate the CA certificate:

   ```
   aws iot update-ca-certificate \
       --certificate-id certificateId \
       --new-status ACTIVE
   ```

Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/describe-ca-certificate.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/describe-ca-certificate.html) command to see the status of the CA certificate\.