# Fleet indexing<a name="iot-indexing"></a>


****  

|  | 
| --- |
|  The fleet indexing feature to support indexing named shadows and AWS IoT Device Defender violations data is in preview release for AWS IoT Device Management and is subject to change\. | 

You can use fleet indexing to index, search, and aggregate your devices' data from the following sources: [AWS IoT registry](thing-registry.md), [AWS IoT Device Shadow](iot-device-shadows.md), [AWS IoT connectivity](life-cycle-events.md), and [AWS IoT Device Defender](device-defender.md) violations\. You can query a group of devices, and aggregate statistics on device records that are based on different combinations of device attributes, including state, connectivity, and device violations\. With fleet indexing, you can organize, investigate, and troubleshoot your fleet of devices\. 

Fleet indexing provides the following capabilities\.
+ **Managing index updates**

You can set up a fleet index so that it indexes updates for your thing groups, thing registries, device shadows, device connectivity, and device violations\. When you enable fleet indexing, AWS IoT creates an index for your things or thing groups\. `AWS_Things` is the index created for all of your things\. `AWS_ThingGroups` is the index that contains all of your thing groups\. After fleet indexing is active, you can run queries on your index, such as finding all devices that are handheld and have more than 70\-percent battery life\. AWS IoT keeps the index continually updated with your latest data\. For more information, see [Managing fleet indexing](managing-fleet-index.md)\.
+ **Searching across data sources** 

You can create a query string based on [a simple query language](query-syntax.md) and use it to search across the data sources you configure in the fleet indexing setting\. The query string describes the things that you want to find\. For more information about data sources that support fleet indexing, see [Managing thing indexing](managing-index.md)\.
+ **Querying for aggregate data**

You can search your devices for aggregate data and return statistics, percentile, cardinality, or a list of things with search queries pertaining to particular fields\. For more information about aggregation query, see [Querying for aggregate data](index-aggregate.md)\. 
+ **Monitoring aggregate data by using fleet metrics**

You can use fleet metrics to automatically send aggregate data to CloudWatch, analyzing trends and creating alarms to monitor the aggregate state of your fleet\. For more information about fleet metrics, see [Fleet metrics](iot-fleet-metrics.md)\. 

To use fleet indexing, you must set up your fleet indexing configuration\. To set up fleet indexing configuration, you can use the [ AWS IoT console](https://console.aws.amazon.com/iot/home), or if you prefer programmatic access, you can use the AWS SDKs, or the AWS Command Line Interface \(AWS CLI\)\.

For information about pricing this and other services, see [AWS IoT Device Management Pricing](https://aws.amazon.com/iot-device-management/pricing)\.