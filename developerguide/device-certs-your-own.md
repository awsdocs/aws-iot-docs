# Create Your Own Client Certificates<a name="device-certs-your-own"></a>

AWS IoT supports client certificates signed by other root certificate authorities \(CA\)\. You can register client certificates signed by another root CA; however, if you want the device or client to register its client certificate when it first connects to AWS IoT, the root CA must be registered with AWS IoT\.

**Note**  
A CA certificate can be registered by only one account in a Region\.

For more information about using X\.509 certificates to support more than a few devices, see [Device Provisioning](iot-provision.md) to review the different certificate management and provisioning options that AWS IoT supports\.

**Topics**
+ [Manage Your CA certificates](manage-your-CA-certs.md)
+ [Create a Client Certificate Using Your CA Certificate](create-device-cert.md)