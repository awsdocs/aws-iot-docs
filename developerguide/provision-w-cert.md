# Provisioning Devices That Have Device Certificates<a name="provision-w-cert"></a>


****  

|  | 
| --- |
| The fleet provisioning feature is in beta and is subject to change\. We recommend that you use this feature with test devices only\. | 

AWS IoT provides three ways to provision devices when they already have a device certificate \(and associated private key\) on them:
+ Single\-thing provisioning with a provisioning template\. This is a good option if you only need to provision devices one at a time\.
+ Just\-in\-time provisioning \(JITP\) with a template that provisions a device when it first connects to AWS IoT\. This is a good option if you need to register large numbers of devices, but you don't have information about them that you can assemble into a bulk provisioning list\.
+ Bulk registration\. This option allows you to specify a list of single\-thing provisioning template values that are stored in a file in an S3 bucket\. This approach works well if you have a large number of known devices whose desired characteristics you can assemble into a list\. 