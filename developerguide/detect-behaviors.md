# Behaviors<a name="detect-behaviors"></a>

A security profile contains a set of behaviors\. Each behavior contains a metric that specifies the normal behavior for a group of devices or for all devices in your account\. See [Metrics](detect-metrics.md) and [CreateSecurityProfile](dd-api-iot-CreateSecurityProfile.md)\. 

The following describes some of the fields that are used in the definition of a `behavior`:

**`name`**  
The name for the behavior\.

**`metric`**  
The name of the metric used \(that is, what is measured by the behavior\)\.

**`dimension`**  
You can define a dimension to adjust the scope of a behavior\. For example, you can define a topic filter dimension that applies a behavior to MQTT topics that match a pattern\. To define a dimension for use in a security profile, see [CreateDimension](dd-api-iot-CreateDimension.md)\.

**`criteria`**  
The criteria that determine if a device is behaving normally in regard to the `metric`\.    
`comparisonOperator`  
The operator that relates the thing measured \(`metric`\) to the criteria \(`value` or `statisticalThreshold`\)\.  
Possible values are: "less\-than", "less\-than\-equals", "greater\-than", "greater\-than\-equals", "in\-cidr\-set", "not\-in\-cidr\-set", "in\-port\-set", and "not\-in\-port\-set"\. Not all operators are valid for every metric\. Operators for CIDR sets and ports are only for use with metrics involving such entities\.  
`value`  
The value to be compared with the `metric`\. Depending on the type of metric, this should contain a `count` \(a value\), `cidrs` \(a list of CIDRs\), or `ports` \(a list of ports\)\.  
`statisticalThreshold`  
The statistical threshold by which a behavior violation is determined\. This field contains a `statistic` field that has the following possible values: "p0", "p0\.1", "p0\.01", "p1", "p10", "p50", "p90", "p99", "p99\.9", "p99\.99", or "p100"\.  
This `statistic` indicates a percentile\. It resolves to a value by which compliance with the behavior is determined\. Metrics are collected one or more times over the specified duration \(`durationSeconds`\) from all reporting devices associated with this security profile, and percentiles are calculated based on that data\. After that, measurements are collected for a device and accumulated over the same duration\. If the resulting value for the device falls above or below \(`comparisonOperator`\) the value associated with the percentile specified, then the device is considered to be in compliance with the behavior\. Otherwise, the device is in violation of the behavior\.  
A [percentile](https://en.wikipedia.org/wiki/Percentile) indicates the percentage of all the measurements considered that fall below the associated value\. For example, if the value associated with "p90" \(the 90th percentile\) is 123, then 90% of all measurements were below 123\.  
`durationSeconds`  
Use this to specify the period of time over which the behavior is evaluated, for those criteria that have a time dimension \(for example, `NUM_MESSAGES_SENT`\)\. For a `statisticalThreshhold` metric comparison, this is the time period during which measurements are collected for all devices to determine the `statisticalThreshold` values, and then for each device to determine how its behavior ranks in comparison\.  
`consecutiveDatapointsToAlarm`  
If a device is in violation of the behavior for the specified number of consecutive data points, an alarm occurs\. If not specified, the default is 1\. \(This differs from the AWS IoT console where a value of 3 is presented by default, but can be overridden\.\)  
`consecutiveDatapointsToClear`  
If an alert has occurred and the offending device is no longer in violation of the behavior for the specified number of consecutive data points, the alarm is cleared\. If not specified, the default is 1\. \(This differs from the AWS IoT console where a value of 3 is presented by default, but can be overridden\.\)