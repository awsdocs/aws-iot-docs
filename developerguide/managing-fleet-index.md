# Managing fleet indexing<a name="managing-fleet-index"></a>


****  

|  | 
| --- |
|  The fleet indexing feature to support indexing named shadows and AWS IoT Device Defender violations data is in preview release for AWS IoT Device Management and is subject to change\. | 

Fleet indexing manages two types of indexes for you, thing indexing and thing group indexing\. 

## Thing indexing<a name="thing-index"></a>

The index created for all of your things is `AWS_Things`\. Thing indexing supports the following data sources: [AWS IoT registry](thing-registry.md) data, [AWS IoT Device Shadow ](iot-device-shadows.md)data, [AWS IoT connectivity](life-cycle-events.md) data, and [AWS IoT Device Defender](device-defender.md) violations data\. 

**Registry**\-AWS IoT provides a registry that helps you manage things\. With fleet indexing, you can add things to the registry and search for devices\. For more information about the registry, see [How to manage things with the registry](thing-registry.md)\.

**Shadow**\-The [AWS IoT Device Shadow service](iot-device-shadows.md) provides shadows that help you store your devices' state data\. Thing indexing supports both classic unnamed shadows and named shadows\. For more information about shadows, see [AWS IoT Device Shadow service](iot-device-shadows.md)\.

**Connectivity**\-Device connectivity data helps you identify your devices' connection status\. This connectivity data is driven by [lifecycle events](life-cycle-events.md)\. When a client connects or disconnects, AWS IoT publishes lifecycle events with messages to MQTT topics\. A connect or disconnect message can be a list of JSON elements that provide details of the connection status\. For more information about device connectivity, see [Lifecycle events](life-cycle-events.md)\.

**Device Defender violations**\-AWS IoT Device Defender violations data helps identify your devices' anomalous behaviors against the normal behaviors you define in a Security Profile\. A Security Profile contains a set of expected device behaviors\. Each behavior uses a metric that specifies the normal behavior of your devices\. For more information about Device Defender violations, see [AWS IoT Device Defender Detect](device-defender-detect.md)\.

For more information, see [Managing thing indexing](managing-index.md)\.

## Thing group indexing<a name="thing-group-index"></a>

`AWS_ThingGroups` is the index that contains all of your thing groups\. You can use this index to search for groups based on group name, description, attributes, and all parent group names\.

For more information, see [Managing thing group indexing](thinggroup-index.md)\.

## Managed fields<a name="managed-field"></a>

Managed fields contain data associated with things, thing groups, device shadows, device connectivity, and Device Defender violations\. AWS IoT defines the data type in managed fields\. You specify the values of each managed field when you create an IoT thing\. For example, thing names, thing groups, and thing descriptions are all managed fields\. Fleet indexing indexes managed fields based on the indexing mode you specify\. Managed fields can't be changed or appear in `customFields`\. For more information, see [Custom fields](#custom-field)\.

The following lists managed fields for thing indexing: 
+ Managed fields for the registry

  ```
  "managedFields" : [
    {name:thingId, type:String},
    {name:thingName, type:String},
    {name:registry.version, type:Number},
    {name:registry.thingTypeName, type:String},
    {name:registry.thingGroupNames, type:String},
  ]
  ```
+ Managed fields for classic unnamed shadows

  ```
  "managedFields" : [
    {name:shadow.version, type:Number},
    {name:shadow.hasDelta, type:Boolean}
  ]
  ```
+ Managed fields for named shadows

  ```
  "managedFields" : [
    {name:shadow.name.shadowName.version, type:Number},
    {name:shadow.name.shadowName.hasDelta, type:Boolean}
  ]
  ```
+ Managed fields for thing connectivity

  ```
  "managedFields" : [
    {name:connectivity.timestamp, type:Number},
    {name:connectivity.version, type:Number},
    {name:connectivity.connected, type:Boolean},
    {name:connectivity.disconnectReason, type:String}
  ]
  ```
+ Managed fields for Device Defender

  ```
  "managedFields" : [
    {name:deviceDefender.violationCount, type:Number},
    {name:deviceDefender.securityprofile.behaviorname.metricName, type:String},
    {name:deviceDefender.securityprofile.behaviorname.lastViolationTime, type:Number},
    {name:deviceDefender.securityprofile.behaviorname.lastViolationValue, type:String},
    {name:deviceDefender.securityprofile.behaviorname.inViolation, type:Boolean}
  ]
  ```
+ Managed fields for thing groups

  ```
  "managedFields" : [
    {name:description, type:String},
    {name:parentGroupNames, type:String},
    {name:thingGroupId, type:String},
    {name:thingGroupName, type:String},
    {name:version, type:Number},
  ]
  ```

The following table lists managed fields that are not searchable\. 


| Data source | Managed field that is unsearchable | 
| --- | --- | 
| Registry | registry\.version | 
| Unnamed shadows | shadow\.version | 
| Named shadows | shadow\.name\.\*\.version | 
| Device Defender | deviceDefender\.version | 
| Thing groups | version | 

## Custom fields<a name="custom-field"></a>

You can aggregate thing attributes, Device Shadow data, and Device Defender violations data by creating custom fields to index them\. The `customFields` attribute is a list of field name and data type pairs\. You can perform aggregation queries based on data type\. The indexing mode you choose affects what fields can be specified in `customFields`\. For example, if you specify the `REGISTRY` indexing mode, you can't specify a custom field from a thing shadow\. You can use the [update\-indexing\-configuration](https://docs.aws.amazon.com/cli/latest/reference/iot/update-indexing-configuration.html) CLI command to create or update the custom fields \(see an example command in [Updating indexing configuration examples](managing-index.md#update-index-examples)\)\. 
+ **Custom field names**

Custom field names for thing and thing group attributes begin with `attributes.`, followed by the attribute name\. If unnamed shadow indexing is on, things can have custom field names that begin with `shadow.desired` or `shadow.reported`, followed by the unnamed shadow data value name\. If named shadow indexing is on, things can have custom field names that begin with `shadow.name.*.desired.` or `shadow.name.*.reported.`, followed by the named shadow data value\. If Device Defender violations indexing is on, things can have custom field names that begin with `deviceDefender.`, followed by the Device Defender violations data value\.

The attribute or data value name that follows the prefix can have only alphanumeric, \- \(hyphen\) and \_ \(underscore\) characters\. It can't have any spaces\.

If there is a type inconsistency between a custom field in your configuration and the value being indexed, fleet indexing ignores the inconsistent value for aggregation queries\. CloudWatch logs are helpful when troubleshooting aggregation query problems\. For more information, see [Troubleshooting aggregation queries for the fleet indexing service](fleet-indexing-troubleshooting.md#aggregation-troubleshooting)\. 
+ **Custom field types**

Custom field types have the following supported values: `Number`, `String`, and `Boolean`\.