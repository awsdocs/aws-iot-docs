# Create your own client certificates<a name="device-certs-your-own"></a>

AWS IoT supports client certificates signed by any root or intermediate certificate authorities \(CA\)\. AWS IoT uses CA certificates to verify the ownership of certificates\. To use device certificates signed by a CA that’s not Amazon’s CA, the CA’s certificate must be registered with AWS IoT so that we can verify the device certificate’s ownership\.

AWS IoT supports multiple ways for bringing your own certificates \(BYOC\): 
+ First, register the CA that’s used for signing the client certificates and then register individual client certificates\. If you want to register the device or client to its client certificate when it first connects to AWS IoT \(also known as [Just\-in\-Time Provisioning](https://docs.aws.amazon.com/iot/latest/developerguide/jit-provisioning.html)\), you must register the signing CA with AWS IoT and activate auto\-registration\.
+ If you can’t register the signing CA, you can choose to register client certificates without CA\. For devices registered without CA, you’ll need to present [Server Name Indication \(SNI\)](https://www.rfc-editor.org/rfc/rfc3546#section-3.1) when you connect them to AWS IoT\.

**Note**  
To register client certificates using CA, you must register the signing CA with AWS IoT, not any other CAs in the hierarchy\.

**Note**  
A CA certificate can be registered in `DEFAULT` mode by only one account in a Region\. A CA certificate can be registered in `SNI_ONLY` mode by multiple accounts in a Region\. 

For more information about using X\.509 certificates to support more than a few devices, see [Device provisioning](iot-provision.md) to review the different certificate management and provisioning options that AWS IoT supports\.

**Topics**
+ [Manage your CA certificates](manage-your-CA-certs.md)
+ [Create a client certificate using your CA certificate](create-device-cert.md)