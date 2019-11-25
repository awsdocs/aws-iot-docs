# Device Provisioning<a name="iot-provision"></a>

When you provision a device with AWS IoT there are a set of resources that need to be created so your devices and AWS IoT can communicate securely\. Other resources can be created to help you manage your device fleet\. The following resources can be created during the provisioning process: 
+ An IoT thing
+ An X\.509 certificate
+ An IoT policy

**Note**  
Creating an IoT thing is not required, but it enables you to manage your device fleet more effectively by searching for devices by thing type, thing group, and thing attributes\. For more information, see [Fleet Indexing Service](iot-indexing.md)\.

IoT things are entries in the AWS IoT device registry, each thing contains a unique name, a set of attributes and is associated with a physical device\. Things can be defined using a thing type or grouped into thing groups\. For more information see, [Managing Devices with AWS IoT ](iot-thing-management.md)\.

Devices use X\.509 certificates to perform mutual authentication with AWS IoT\. You can register an existing certificate or have AWS IoT generate and register a new certificate for you\. You associate a certificate with a device by attaching it to the thing that represents the device\. You must also copy the certificate and associated private key onto the device\. Devices present the certificate when connecting to AWS IoT\. For more information, see [Authentication](authentication.md)\.

IoT policies define what operations a device can perform in AWS IoT\. IoT policies are attached to device certificates\. When a device presents the certificate to AWS IoT it is granted the permissions specified in the policy\. For more information, see [](iot-authorization.md)\. Each device needs a certificate to communicate with AWS IoT\.

AWS IoT supports automated fleet provisioning using provisioning templates\. Provisioning templates describe the resources AWS IoT requires to provision your device as described previously\. Templates contain variables that enable you to use one template to provision multiple devices\. When you provision a device you specify values for the variables specific to the device using a dictionary \(map\)\. To provision another device, specify new values in the dictionary\.

You can use automated provisioning whether your devices have unique certificates \(and their associated private key\) or not\.