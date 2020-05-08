# Create a CA certificate<a name="create-your-CA-cert"></a>

If you do not have a CA certificate, you can use [OpenSSL](https://www.openssl.org/) tools to create one\.

**Note**  
You can't perform this procedure in the AWS IoT console\.

**To create a CA certificate using [OpenSSL](https://www.openssl.org/) tools**

1. Generate a key pair\.

   ```
   openssl genrsa -out <root_CA_key_filename> 2048
   ```

1. Use the private key from the key pair to generate a CA certificate\.

   ```
   openssl req -x509 -new -nodes \
       -key <root_CA_key_filename> \
       -sha256 -days 1024 \
       -out <root_CA_pem_filename>
   ```