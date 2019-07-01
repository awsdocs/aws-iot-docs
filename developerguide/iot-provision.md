# Device Provisioning<a name="iot-provision"></a>

AWS IoT device provisioning involves the creation and registration of the following entities:
+ A certificate\. You can provision a device with an existing certificate or have AWS IoT create and register one for you\.
+ A policy attached to the certificate\.
+ A unique identifier for the thing \(device\)\.
+ A set of attributes for the thing, including existing thing types and groups\.

To provision a device, create a template that describes the resources required for your device\. Devices require a thing, a certificate, and one or more policies\. A *thing* is an entry in the registry that contains attributes that describe the device\. Devices use certificates to authenticate with AWS IoT\. Policies determine which operations a device can perform in AWS IoT\.

Templates contain variables that are replaced when the template is used to provision a device\. A dictionary \(map\) is used to provide values for the variables used in a template\. You can use the same template to provision multiple devices\. You simply pass in different values for the template variables in the dictionary\.

AWS IoT provides three ways to provision devices:
+ Single\-thing provisioning with a provisioning template\.

  This is a good option if you only need to provision devices one at a time\.
+ Just\-in\-time provisioning \(JITP\) with a template that registers and provisions a device when it first connects to AWS IoT\.

  This is a good option if you need to register large numbers of devices, but you don't have information about them that you can assemble into a bulk provisioning list\.
+ Bulk provisioning\.

  This option allows you to specify a list of single\-thing provisioning template values that are stored in a file in an S3 bucket\. This approach works well if you have a large number of known devices whose desired characteristics you can assemble into a list\.

Just\-in\-time provisioning and bulk provisioning are better options if you need to provision large numbers of devices\. AWS IoT also provides a [RegisterThing](https://docs.aws.amazon.com/iot/latest/apireference/API_RegisterThing.html) API that you can use to provision single devices programmatically\.