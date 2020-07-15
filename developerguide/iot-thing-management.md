# Managing devices with AWS IoT<a name="iot-thing-management"></a>

AWS IoT provides a registry that helps you manage *things*\. A thing is a representation of a specific device or logical entity\. It can be a physical device or sensor \(for example, a light bulb or a switch on a wall\)\. It can also be a logical entity like an instance of an application or physical entity that does not connect to AWS IoT but is related to other devices that do \(for example, a car that has engine sensors or a control panel\)\.

 Information about a thing is stored in the registry as JSON data\. Here is an example thing:

```
{
     "version": 3,
    "thingName": "MyLightBulb",
    "defaultClientId": "MyLightBulb",
    "thingTypeName": "LightBulb",
    "attributes": {
        "model": "123",
        "wattage": "75"
    }
}
```

Things are identified by a name\. Things can also have attributes, which are name\-value pairs you can use to store information about the thing, such as its serial number or manufacturer\. 

A typical device use case involves the use of the thing name as the default MQTT client ID\. Although we do not enforce a mapping between a thing's registry name and its use of MQTT client IDs, certificates, or shadow state, we recommend you choose a thing name and use it as the MQTT client ID for both the registry and the Device Shadow service\. This provides organization and convenience to your IoT fleet without removing the flexibility of the underlying device certificate model or shadows\.

You do not need to create a thing in the registry to connect a device to AWS IoT\. Adding things to the registry allows you to manage and search for devices more easily\.