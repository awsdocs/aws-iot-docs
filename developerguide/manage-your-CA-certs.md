# Manage your CA certificates<a name="manage-your-CA-certs"></a>

This section describes common tasks for managing your own certificate authority \(CA\) certificates\.

You might need to register your certificate authority \(CA\) with AWS IoT if you are using client certificates signed by a CA that AWS IoT doesn't recognize\.

If you want clients to automatically register their client certificates with AWS IoT when they first connect, the CA that signed the client certificates must be registered with AWS IoT\. Otherwise, you don't need to register the CA certificate that signed the client certificates\.

**Note**  
A CA certificate can be registered by only one account in a Region\.
+ 

**[Create a CA certificate](create-your-CA-cert.md), if you need one\.**  
Create the certificate and key files that you need for the next step\.
+ 

**[Register your CA certificate](register-CA-cert.md)**  
Register your CA certificate with AWS IoT
+ **[Deactivate a CA certificate](deactivate-ca-cert.md)**