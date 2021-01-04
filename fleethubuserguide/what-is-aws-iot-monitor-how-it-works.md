--------

 The Fleet Hub for AWS IoT Device Management service is currently in public preview\. This service is subject to change\.

--------

# How Fleet Hub for AWS IoT Device Management works<a name="what-is-aws-iot-monitor-how-it-works"></a>

Administrators can use Fleet Hub for AWS IoT Device Management to create secure web applications in a few minutes without provisioning any resources or writing any code\. Web applications that you create by using Fleet Hub integrate with your existing identity systems, such as Active Directory\. This allows your administrators to apply their own authentication and authorization models\.

Fleet Hub web applications integrate with AWS IoT Core fleet indexing and device monitoring\. These integrations provide the ability to monitor device health data and create alarms when devices in your fleet reach a specified state\.

**Example use cases:**
+ Visualize device connectivity issues \- You can see the number of disconnected devices in your fleet, the last connection status for a device, and the reason or reasons why devices disconnected\.
+ Set alarms \- You can set thresholds that trigger alarms when a particular number of devices disconnect\. Alarms can also notify you when a device or devices disconnect for a particular reason\. You can then look at detailed device data to investigate and troubleshoot\.

## How Fleet Hub data indexing works<a name="what-is-aws-iot-monitor-how-it-works-indexing"></a>

You can use the Fleet Hub console to activate fleet indexing for your device fleet\. When you activate fleet indexing in Fleet Hub, you activate it for the entire fleet and make it available to all Fleet Hub applications\.

When it's enabled, fleet indexing indexes all AWS IoT Core\-managed fields automatically\. You can also use fleet indexing to add custom data that you can use to query and aggregate data in Fleet Hub applications\.

## How Fleet Hub alarms work<a name="what-is-aws-iot-monitor-how-it-works-indexing"></a>

Fleet Hub web applications provide an interface that allows your users to create alarms\. The following steps show how users create alarms in Fleet Hub\.

1. Create a query to aggregate data \- Specify a query that aggregates the devices your users want to target by using searchable fields\.

1. Configure threshold \- Set a threshold that triggers the alarms when a condition in the indexed data \(such as connectivity status over a specified interval\) is reached\.

1. Configure notification \- Specify a group of recipients whom Fleet Hub notifies when the specified devices are in alarm\.