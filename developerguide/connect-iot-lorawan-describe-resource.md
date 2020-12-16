# Describe your AWS IoT Core for LoRaWAN resources<a name="connect-iot-lorawan-describe-resource"></a>

 AWS IoT Core for LoRaWAN provides several options to identify the resources you create\. While AWS IoT Core for LoRaWAN resources are given a unique ID when they're created, this ID is not descriptive nor can it be changed after the resource is created\. You can also assign a name, add a description, and attach tags and tag values to most AWS IoT Core for LoRaWAN resources to make it more convenient to select, identify, and manage your AWS IoT Core for LoRaWAN resources\. 
+ 

**[Resource names](#connect-iot-lorawan-describe-resource-names)**  
For gateways, devices, and profiles, the resource name is an optional field that you can change after the resource is created\. The name appears in the lists displayed on the resource hub pages\. 

  For destinations, you provide a name that is unique in your AWS account and AWS Region\. You can't change the destination name after you create the destination resource\.

  While a name can have up to 256 characters, the display space in the resource hub is limited\. Make sure that the distinguishing part of the name appears in the first 20 to 30 characters, if possible\.
+ 

**[Resource tags](#connect-iot-lorawan-describe-resource-tags)**  
Tags are key\-value pairs of metadata that can be attached to AWS resources\. You choose both tag keys and their corresponding values\.

   Gateways, destinations, and profiles can have up to 50 tags attached to them\. Devices don't support tags\. 

## Resource names<a name="connect-iot-lorawan-describe-resource-names"></a>


**AWS IoT Core for LoRaWAN resource support for name**  

|  Resource  |  Name field support  | 
| --- | --- | 
|  Destination  |  Name is unique ID of resource and can't be changed\.  | 
|  Device  |  Name is optional descriptor of resource and can be changed\.  | 
|  Gateway  |  Name is optional descriptor of resource and can be changed\.  | 
|  Profile  |  Name is optional descriptor of resource and can be changed\.  | 

The name field appears in resource hub lists of resources; however, the space is limited and so only the first 15\-30 characters of the name might be visible\.

When selecting names for your resources, consider how you want them to identify the resources and how they'll be displayed in the console\.

**Description**  
Destination, device, and gateway resources also support a description field, which can accept up to 2,048 characters\. The description field appears only in the individual resource's detail page\. While the description field can hold a lot of information, because it only appears in the resource's detail page, it isn't convenient for scanning in the context of multiple resources\.

## Resource tags<a name="connect-iot-lorawan-describe-resource-tags"></a>


**AWS IoT Core for LoRaWAN resource support for AWS tags**  

|  Resource  |  AWS tag support  | 
| --- | --- | 
|  Destination  |  Up to 50 AWS tags can be added to the resource\.  | 
|  Device  |  This resource doesn't support AWS tags\.  | 
|  Gateway  |  Up to 50 AWS tags can be added to the resource\.  | 
|  Profile  |  Up to 50 AWS tags can be added to the resource\.  | 

Tags are words or phrases that act as metadata that you can use to identify and organize your AWS resources\. You can think of the tag key as a category of information and the tag value as a specific value in that category\. 

For example, you might have a tag value of *color* and then give some resources a value of *blue* for that tag and others a value of *red*\. With that, you could use the [Tag editor](https://docs.aws.amazon.com/ARG/latest/userguide/tag-editor.html) in the AWS console to find the resources with a *color* tag value of *blue*\.

For more information about tagging and tagging strategies, see [Tag editor](https://docs.aws.amazon.com/ARG/latest/userguide/tag-editor.html)\.