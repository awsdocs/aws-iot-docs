# Create a CA verification certificate to register the CA certificate in the console<a name="create-CA-verification-cert"></a>

**Note**  
This procedure is only for use if you are registering a CA certificate from the AWS IoT console\.  
If you did not come to this procedure from the AWS IoT console, start the CA certificate registration process in the console at [Register CA certificate](https://console.aws.amazon.com/iot/home#/create/cacertificate)\. 

Make sure you have the following available on the same computer before you continue:
+ The root CA's certificate file \(referenced below as `root_CA_cert_filename.pem`\)
+ The root CA certificate's private key file \(referenced below as `root_CA_key_filename.key`\)
+ [OpenSSL v1\.1\.1i](https://www.openssl.org/) or later

**To use the command\-line interface to create a CA verification certificate to register your CA certificate in the console**

1. Replace `verification_cert_key_filename.key` with the name of the veification certificate key file you want to create\. For example, **verification\_cert\.key**, and then run this command to generate a key pair for the private key verification certificate:

   ```
   openssl genrsa -out verification_cert_key_filename.key 2048
   ```

1. Replace `verification_cert_key_filename.key` with the name of the key file you created in step 1\.

   Replace `verification_cert_csr_filename.csr` with the name of the certificate signing request \(CSR\) file that you want to create\. For example, **verification\_cert\.csr**\.

   Run this command to create the CSR file\.

   ```
   openssl req -new \
       -key verification_cert_key_filename.key \
       -out verification_cert_csr_filename.csr
   ```

   The command prompts you for additional information that's explained later\.

1. In the AWS IoT console, in the **Verification certificate** container, copy the registration code\.

1. The information that the openssl command prompts you for is shown in the following example\. Except for the `Common Name` field, you can enter your own values or leave them blank\.

   In the `Common Name` field, paste the registration code you copied in the previous step\.

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

   After you finish, the command creates the CSR file\.

1. Replace `verification_cert_csr_filename.csr` with the `verification_cert_csr_filename.csr` you used in the previous step\.

   Replace `root_CA_cert_filename.pem` with the filename of the CA certificate you want to register\.

   Replace `root_CA_key_filename.key` with the filename of the CA certificate's private key file\.

   Replace `verification_cert_filename.pem` with the filename of the verification certificate that you want to create\. For example, **verification\_cert\.pem**\.

   ```
   openssl x509 -req \
       -in verification_cert_csr_filename.csr \
       -CA root_CA_cert_filename.pem \
       -CAkey root_CA_key_filename.key \
       -CAcreateserial \
       -out verification_cert_filename.pem \
       -days 500 -sha256
   ```

1. After the openssl command completes, you should have these files ready to use for when you return to the console\.
   + Your CA certificate file \(`root_CA_cert_filename.pem` used in the previous command\)
   + The verification certificate you created in the previous step \(*verification\_cert\_filename\.pem* used in the previous command\)