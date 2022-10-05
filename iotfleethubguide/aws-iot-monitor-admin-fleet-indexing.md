# Manage fleet indexing for Fleet Hub applications<a name="aws-iot-monitor-admin-fleet-indexing"></a>

You can use the AWS IoT Core console or the AWS CLI to activate fleet indexing and configure the following data sources to index: [AWS IoT registry](https://docs.aws.amazon.com/iot/latest/developerguide/thing-registry.html) data, AWS IoT [Device Shadow](https://docs.aws.amazon.com/iot/latest/developerguide/iot-device-shadows.html) data, [AWS IoT connectivity](https://docs.aws.amazon.com/iot/latest/developerguide/life-cycle-events.html) data, and [AWS IoT Device Defender violations](https://docs.aws.amazon.com/iot/latest/developerguide/device-defender-detect.html) data\. The following steps describe how to activate fleet indexing for Fleet Hub for AWS IoT Device Management applications in AWS IoT Core console\. To view the steps using AWS CLI, see [Managing thing indexing](https://docs.aws.amazon.com/iot/latest/developerguide/managing-index.html)\. 

**Important**  
July 20th, 2022 is the General Availability release of AWS IoT Device Management fleet indexing's integration with AWS IoT Core named shadows and AWS IoT Device Defender detect violations\. With this GA release, you can index specific named shadows by specifying shadow names\. If you added your named shadows for indexing during this feature's public preview period from November 30, 2021 to July 19, 2022, we encourage you to reconfigure your fleet indexing settings and choose specific shadow names to reduce indexing cost and optimize performance\. For more information about how to reconfigure your fleet indexing settings, see [Managing fleet indexing](https://docs.aws.amazon.com/iot/latest/developerguide/managing-fleet-index.html)\.

1. Navigate to the AWS IoT Core console \([https://console\.aws\.amazon\.com/iot/](https://console.aws.amazon.com/iot/)\), and in the left panel, choose **Settings**\.

1. On the **Settings** page, navigate to the **Fleet indexing** section, then choose **Manage indexing**\.

1. On the **Manage fleet indexing** page, in the **Configuration** section, choose **Thing indexing** and the data sources that you want AWS IoT to index\. You must activate thing indexing and thing connectivity to use Fleet Hub\.

1. \(Optional\) On the **Manage fleet indexing** page, in the **Custom fields for aggregation\-optional** section, create custom fields in addition to the managed fields that fleet indexing indexes by default\. 

   When you're done managing and reviewing your fleet indexing settings, choose **Next**\.

   It can take a moment for fleet indexing to update the settings\. For more information about how to manage fleet indexing, see [Managing fleet indexing](https://docs.aws.amazon.com/iot/latest/developerguide/managing-fleet-index.html)\.