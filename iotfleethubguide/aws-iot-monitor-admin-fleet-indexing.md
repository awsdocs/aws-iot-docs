# Managing fleet indexing for Fleet Hub applications<a name="aws-iot-monitor-admin-fleet-indexing"></a>


****  

|  | 
| --- |
|  The fleet indexing feature to support indexing named shadows and AWS IoT Device Defender violations data is in preview release for AWS IoT Device Management and is subject to change\. | 

You can use the following data sources to enable fleet indexing and configure what to index: [AWS IoT registry](https://docs.aws.amazon.com/iot/latest/developerguide/thing-registry.html) data, AWS IoT [Device Shadow](https://docs.aws.amazon.com/iot/latest/developerguide/iot-device-shadows.html) data, [AWS IoT connectivity](https://docs.aws.amazon.com/iot/latest/developerguide/life-cycle-events.html) data, and [AWS IoT Device Defender violations](https://docs.aws.amazon.com/iot/latest/developerguide/device-defender-detect.html) data\. The following steps describe how to enable fleet indexing for Fleet Hub for AWS IoT Device Management applications\. 

1. Navigate to the AWS IoT Core console \([https://console\.aws\.amazon\.com/iot/](https://console.aws.amazon.com/iot/)\), and in the left panel, choose **Settings**\.

1. On the **Settings** page, scroll to the **Fleet indexing** section, then choose **Manage indexing**\.

1. On the **Manage fleet indexing** page, and in the **Configuration** section, choose **Thing indexing** and the data sources you want AWS IoT to index\. You must activate thing indexing and thing connectivity to use Fleet Hub\.

1. \(Optional\) On the **Manage fleet indexing** page, and in the **Custom search fields\-optional** section, create custom fields in addition to the managed fields that fleet indexing indexes by default\. 

   When you're done managing and reviewing your fleet indexing settings, choose **Next**\.

   It can take a moment for fleet indexing to update the settings\. For more information about how to manage fleet indexing, see [Managing fleet indexing](https://docs.aws.amazon.com/iot/latest/developerguide/managing-fleet-index.html)\.