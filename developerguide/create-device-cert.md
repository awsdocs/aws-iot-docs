# Create a client certificate using your CA certificate<a name="create-device-cert"></a>

You can use your own certificate authority \(CA\) to create client certificates\. The client certificate must be registered with AWS IoT before use\. For information about the registration options for your client certificates, see [Register a client certificate](register-device-cert.md)\.

## Create a client certificate \(CLI\)<a name="create-device-cert-cli"></a>

**Note**  
You can't perform this procedure in the AWS IoT console\.

**To create a client certificate using the AWS CLI**

1. Generate a key pair\.

   ```
   openssl genrsa -out device_cert_key_filename 2048
   ```

1. Create a CSR for the client certificate\.

   ```
   openssl req -new \
       -key device_cert_key_filename \
       -out device_cert_csr_filename
   ```

   You are prompted for some information, as shown here:

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
       Common Name (e.g. server FQDN or YOUR name) []:
       Email Address []:
   
       Please enter the following 'extra' attributes
       to be sent with your certificate request
       A challenge password []:
       An optional company name []:
   ```

1. Create a client certificate from the CSR\.

   ```
   openssl x509 -req \
       -in device_cert_csr_filename \
       -CA root_CA_pem_filename \
       -CAkey root_CA_key_filename \
       -CAcreateserial \
       -out device_cert_pem_filename \
       -days 500 -sha256
   ```

 At this point, the client certificate has been created, but it has not yet been registered with AWS IoT\. For information about how and when to register the client certificate, see [Register a client certificate](register-device-cert.md)\. 
