# Create your own client certificates<a name="device-certs-your-own"></a>

AWS IoT supports client certificates signed by other root certificate authorities \(CA\)\. You can register client certificates signed by another root CA; however, if you want the device or client to register its client certificate when it first connects to AWS IoT, the root CA must be registered with AWS IoT\.

**Note**  
A CA certificate can be registered by only one account in a Region\.

For more information about using X\.509 certificates to support more than a few devices, see [Device provisioning](iot-provision.md) to review the different certificate management and provisioning options that AWS IoT supports\.

**Topics**
+ [Manage your CA certificates](manage-your-CA-certs.md)
+ [Create a client certificate using your CA certificate](create-device-cert.md)