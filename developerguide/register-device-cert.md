# Register a client certificate<a name="register-device-cert"></a>

Client certificates must be registered with AWS IoT to enable communications between the client and AWS IoT\. You can register each client certificate manually, or you can configure the client certificates to register automatically when the client connects to AWS IoT for the first time\.

 If you want your clients and devices to register their client certificates when they first connect, you must [Register your CA certificate](register-CA-cert.md) used to sign the client certificate with AWS IoT in the Regions in which you want to use it\. The Amazon Root CA is automatically registered with AWS IoT\. 

Client certificates can be shared by AWS accounts and Regions\. The procedures in these topics must be performed in each account and Region in which you want to use the client certificate\. The registration of a client certificate in one account or Region is not automatically recognized by another\.

**Note**  
Clients that use the Transport Layer Security \(TLS\) protocol to connect to AWS IoT must support the [Server Name Indication \(SNI\) extension](https://tools.ietf.org/html/rfc3546#section-3.1) to TLS\. For more information, see [Transport security in AWS IoT](transport-security.md)\.

**Topics**
+ [Register a client certificate manually](manual-cert-registration.md)
+ [Register a client certificate when the client connects to AWS IoT just\-in\-time registration \(JITR\)](auto-register-device-cert.md)