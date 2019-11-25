# Diagnosing Connectivity Issues<a name="diagnosing-connectivity-issues"></a>

## Authentication<a name="troubleshooting-authentication"></a>

How do my devices authenticate AWS IoT endpoints?  
Add the AWS IoT CA certificate to your client’s trust store\. Refer to the documentation on [Server Authentication in AWS IoT Core](x509-client-certs.html#server-authentication) and then follow the links to download the appropriate CA certificate\.

How can I validate a correctly configured certificate?  
Use the OpenSSL `s_client` command to test a connection to the AWS IoT endpoint:  
` openssl s_client -connect custom_endpoint.iot.us-east-1.amazonaws.com:8443 -CAfile CA.pem -cert cert.pem -key privateKey.pem`   
For more information about using `openssl s_client`, see [OpenSSL s\_client documentation](https://www.openssl.org/docs/man1.0.2/man1/openssl-s_client.html)\.

## Authorization<a name="troubleshooting-authorization"></a>

I received a `PUBNACK` or `SUBNACK` response from the broker\. What do I do?  
Make sure that there is a policy attached to the certificate you are using to call AWS IoT\. All publish/subscribe operations are denied by default\.