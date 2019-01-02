# X\.509 Certificates<a name="x509-certs"></a>

X\.509 certificates are digital certificates that use the X\.509 public key infrastructure standard to associate a public key with an identity contained in a certificate\. X\.509 certificates are issued by a trusted entity called a certification authority \(CA\)\. The CA maintains one or more special certificates called CA certificates that it uses to issue X\.509 certificates\. Only the certification authority has access to CA certificates\.

**Note**  
We recommend that each device be given a unique certificate to enable fine\-grained management including certificate revocation\.  
Devices must support rotation and replacement of certificates in order to ensure smooth operation as certificates expire\.

AWS IoT supports the following certificate\-signing algorithms:

+ SHA256WITHRSA

+ SHA384WITHRSA

+ SHA384WITHRSA

+ SHA512WITHRSA

+ RSASSAPSS

+ DSA\_WITH\_SHA256

+ ECDSA\-WITH\-SHA256

+ ECDSA\-WITH\-SHA384

+ ECDSA\-WITH\-SHA512

Certificates provide several benefits over other identification and authentication mechanisms\. Certificates enable asymmetric keys to be used with devices\. This means you can burn private keys into secure storage on a device\. This way, sensitive cryptographic material never leaves the device\. Certificates provide stronger client authentication over other schemes, such as user name and password or bearer tokens, because the secret key never leaves the device\. 

AWS IoT authenticates certificates using the TLS protocol’s client authentication mode\. TLS is available in many programming languages and operating systems and is commonly used for encrypting data\. In TLS client authentication, AWS IoT requests a client X\.509 certificate and validates the certificate’s status and AWS account against a registry of certificates\. It then challenges the client for proof of ownership of the private key that corresponds to the public key contained in the certificate\. 

To use AWS IoT certificates, clients must support all of the following in their TLS implementation:

+ TLS 1\.2\.

+ SHA\-256 RSA certificate signature validation\.

+ One of the cipher suites from the TLS cipher suite support section\.