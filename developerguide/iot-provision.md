# Device provisioning<a name="iot-provision"></a>

AWS provides several different ways to provision a device and install unique client certificates on it\. This section describes each way and how to select the best one for your IoT solution\. These options are described in detail in the whitepaper titled [Device Manufacturing and Provisioning with X\.509 Certificates in AWS IoT Core](https://d1.awsstatic.com/whitepapers/device-manufacturing-provisioning.pdf)\. 

**Select the option that fits your situation best**
+ 

**You can install certificates on IoT devices before they are delivered**  
If you can securely install unique client certificates on your IoT devices before they are delivered for use by the end user, you want to use [*just\-in\-time* provisioning \(JITP\)](jit-provisioning.md) or [*just\-in\-time* registration \(JITR\)](auto-register-device-cert.md)\.

  Using JITP and JITR, the certificate authority \(CA\) used to sign the device certificate is registered with AWS IoT and is recognized by AWS IoT when the device first connects\. The device is provisioned in AWS IoT on its first connection using the details of its provisioning template\.

  For more information on single thing, JITP, JITR, and bulk provisioning of devices that have unique certificates, see [Provisioning devices that have device certificates](provision-w-cert.md)\.
+ 

**End users or installers can use an app to install certificates on their IoT devices**  
If you cannot securely install unique client certificates on your IoT device before they are delivered to the end user, but the end user or an installer can use an app to register the devices and install the unique device certificates, you want to use the [provisioning by trusted user](provision-wo-cert.md#trusted-user) process\.

  Using a trusted user, such as an end user or an installer with a known account, can simplify the device manufacturing process\. Instead of a unique client certificate, devices have a temporary certificate that enables the device to connect to AWS IoT for only 5 minutes\. During that 5\-minute window, the trusted user obtains a unique client certificate with a longer life and installs it on the device\. The limited life of the claim certificate minimizes the risk of a compromised certificate\.

  For more information, see [Provisioning by trusted user](provision-wo-cert.md#trusted-user)\.
+ 

**End users CANNOT use an app to install certificates on their IoT devices**  
If neither of the previous options will work in your IoT solution, the [provisioning by claim](provision-wo-cert.md#claim-based) process is an option\. With this process, your IoT devices have a claim certificate that is shared by other devices in the fleet\. The first time a device connects with a claim certificate, AWS IoT registers the device using its provisioning template and issues the device its unique client certificate for subsequent access to AWS IoT\.

   This option enables automatic provisioning of a device when it connects to AWS IoT, but could present a larger risk in the event of a compromised claim certificate\. If a claim certificate becomes compromised, you can deactivate the certificate\. Deactivating the claim certificate prevents all devices with that claim certificate from being registered in the future\. However; deactivating the claim certificate does not block devices that have already been provisioned\.

  For more information, see [Provisioning by claim](provision-wo-cert.md#claim-based)\.

## Provisioning devices in AWS IoT<a name="provisioning-in-iot"></a>

When you provision a device with AWS IoT, you must create resources so your devices and AWS IoT can communicate securely\. Other resources can be created to help you manage your device fleet\. The following resources can be created during the provisioning process: 
+ An IoT thing\.

  IoT things are entries in the AWS IoT device registry\. Each thing has a unique name and set of attributes, and is associated with a physical device\. Things can be defined using a thing type or grouped into thing groups\. For more information, see [Managing devices with AWS IoT](iot-thing-management.md)\.

   Although not required, creating a thing makes it possible to manage your device fleet more effectively by searching for devices by thing type, thing group, and thing attributes\. For more information, see [Fleet indexing](iot-indexing.md)\.
**Note**  
For Fleet Hub to index your Thing's connectivity status data, provision your Thing and configure it so the Thing name matches the client ID used on the Connect request\.
+ An X\.509 certificate\.

  Devices use X\.509 certificates to perform mutual authentication with AWS IoT\. You can register an existing certificate or have AWS IoT generate and register a new certificate for you\. You associate a certificate with a device by attaching it to the thing that represents the device\. You must also copy the certificate and associated private key onto the device\. Devices present the certificate when connecting to AWS IoT\. For more information, see [Authentication](authentication.md)\.
+ An IoT policy\.

  IoT policies define the operations that a device can perform in AWS IoT\. IoT policies are attached to device certificates\. When a device presents the certificate to AWS IoT, it is granted the permissions specified in the policy\. For more information, see [Authorization](iot-authorization.md)\. Each device needs a certificate to communicate with AWS IoT\.

AWS IoT supports automated fleet provisioning using provisioning templates\. Provisioning templates describe the resources AWS IoT requires to provision your device\. Templates contain variables that enable you to use one template to provision multiple devices\. When you provision a device, you specify values for the variables specific to the device using a dictionary or *map*\. To provision another device, specify new values in the dictionary\.

You can use automated provisioning whether or not your devices have unique certificates \(and their associated private key\)\.

## Fleet provisioning APIs<a name="provisioning-apis"></a>

There are several categories of APIs used in fleet provisioning:
+ These control plane functions create and manage the fleet provisioning templates and configure trusted user policies\.
  + [CreateProvisioningTemplate](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateProvisioningTemplate.html)
  + [ CreateProvisioningTemplateVersion](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateProvisioningTemplateVersion.html)
  + [DeleteProvisioningTemplate](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteProvisioningTemplate.html)
  + [DeleteProvisioningTemplateVersion](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteProvisioningTemplateVersion.html)
  + [DescribeProvisioningTemplate](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeProvisioningTemplate.html)
  + [DescribeProvisioningTemplateVersion](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeProvisioningTemplateVersion.html)
  + [ListProvisioningTemplates](https://docs.aws.amazon.com/iot/latest/apireference/API_ListProvisioningTemplates.html)
  + [ListProvisioningTemplateVersions](https://docs.aws.amazon.com/iot/latest/apireference/API_ListProvisioningTemplateVersions.html)
  + [UpdateProvisioningTemplate](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateProvisioningTemplate.html)
+ Trusted users can use this control plane function to generate a temporary onboarding claim\. This temporary claim is passed to the device during Wi\-Fi configuration or a similar method\.
  + [CreateProvisioningClaim](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateProvisioningClaim.html)
+ The MQTT API used during the provisioning process by devices with a provisioning claim certificate embedded in a device, or passed to it by a trusted user\.
  + [CreateCertificateFromCsr](fleet-provision-api.md#create-cert-csr)
  + [CreateKeysAndCertificate](fleet-provision-api.md#create-keys-cert)
  + [RegisterThing](fleet-provision-api.md#register-thing)