# View device details<a name="aws-iot-monitor-user-getting-started-view-device-details"></a>

This topic walks you through the steps to view details about your device groups and your devices\.

## Prerequisites<a name="aws-iot-monitor-user-getting-started-view-devices-prerequisites"></a>
+ A Fleet Hub application associated with an AWS IoT Core account that contains devices \(things\)\.
+ An account in your organization that has permissions to use the Fleet Hub application\.

## Device groups<a name="aws-iot-monitor-user-getting-started-device-groups"></a>

When you log in to your Fleet Hub web application, you see **Device groups** on the left navigation panel\. The **Device groups** page lists all the device groups in your Fleet Hub web application\. To view the details of a device group, choose a specific device group from the **Group name** column\. 

![\[Fleet Hub device groups\]](http://docs.aws.amazon.com/iot/latest/fleethubuserguide/images/iot-monitor-device-groups.png)

## Device group details<a name="aws-iot-monitor-user-getting-started-device-group-details"></a>

The **Device group details** page contains information about your selected device group\. To view the details of a device, choose a specific device from the **Device name** column of the **Devices in *XXX*** section\. 

![\[Fleet Hub queries dashboard device list\]](http://docs.aws.amazon.com/iot/latest/fleethubuserguide/images/iot-monitor-device-group.png)

## Device details<a name="aws-iot-monitor-user-getting-started-device-details"></a>

The **Device details** page contains information about your selected device\.

### Details<a name="aws-iot-monitor-user-getting-started-device-details-top"></a>

The **Details** section contains the following information about your device:
+ **Device name** – The name of the thing resource that represents your device\. For more information, see [How to manage things with the registry](https://docs.aws.amazon.com/iot/latest/developerguide/thing-registry.html)\.
+ **Thing type** – The thing type that's associated with your device\. You can use the thing type to store information that's common to all things with the same thing type\. For more information, see [Thing types](https://docs.aws.amazon.com/iot/latest/developerguide/thing-types.html)\.
+ **Last connection timestamp** – The timestamp for when your device last connected to AWS IoT\.
+ **Shareable device link** – A shareable link that points to the **Device details** page of the selected device\.
+ **Last connection status** – The connection status of your device to AWS IoT\. If your device is connected, the value is `true`\. If it's not connected, the value is `false`\.
+ **Disconnect reason** – The reason why your device is disconnected\.

### Reported data<a name="aws-iot-monitor-user-getting-started-device-details-reported-data"></a>

The **Reported data** section contains information about your device's registry data, device shadows data, and thing groups\. 
+ **Device fields** – The indexed fields of your device in AWS IoT fleet indexing\. For more information, see [Managing fleet indexing](https://docs.aws.amazon.com/iot/latest/developerguide/managing-fleet-index.html)\.
+ **Device shadows** – The shadows that are associated with your device\. The device shadows can include both classic unnamed shadows and named shadows\. For more information, see [AWS IoT device shadow](https://docs.aws.amazon.com/iot/latest/developerguide/iot-device-shadows.html)\.
+ **Device groups** – The device groups that are associated with your device\. The device groups can include both static thing groups and dynamic thing groups\. For more information, see [Static thing groups](https://docs.aws.amazon.com/iot/latest/developerguide/thing-groups.html) and [Dynamic thing groups](https://docs.aws.amazon.com/iot/latest/developerguide/dynamic-thing-groups.html)\.

### Jobs<a name="aws-iot-monitor-user-getting-started-device-details-jobs"></a>

The **Jobs** section displays all of the jobs running on the device\. Each job has a details page that displays summary information about the job, including target and runtime information\. For more information, see [ Working with jobs and job templates in Fleet Hub for AWS IoT Device Management](https://docs.aws.amazon.com/iot/latest/fleethubuserguide/aws-iot-monitor-technician-job-templates.html), and [Jobs](https://docs.aws.amazon.com/iot/latest/developerguide/iot-jobs.html)\.

### Defender metrics<a name="aws-iot-monitor-user-getting-started-device-details-metrics"></a>

The **Defender metrics** section displays AWS IoT Device Defender metrics that are associated with your currently selected device\. You can use the displayed metrics data to visualize your device operation across a time frame you choose\. To view the defender metrics data from your Fleet Hub application, your Fleet Hub administrator must first set up AWS IoT Device Defender metrics that are associated with the selected device\. For more information about how to create and set up AWS IoT Device Defender metrics for your devices, see [Custom metrics](https://docs.aws.amazon.com/iot/latest/developerguide/dd-detect-custom-metrics.html), [Device\-side metrics](https://docs.aws.amazon.com/iot/latest/developerguide/detect-device-side-metrics.html), and [Cloud\-side metrics](https://docs.aws.amazon.com/iot/latest/developerguide/detect-cloud-side-metrics.html)\. 

![\[Fleet Hub queries dashboard device list\]](http://docs.aws.amazon.com/iot/latest/fleethubuserguide/images/iot-monitor-defender-metrics.png)

### Behavior violations<a name="aws-iot-monitor-user-getting-started-device-details-behavior-violations"></a>

The **Behavior violations** section displays the indexed AWS IoT Device Defender detect violations data that are associated with your currently selected device\. The behavior violations data can include violation count, last violation time, and last violation metric value\. To view the behavior violations data from your Fleet Hub application, your Fleet Hub administrator should set up AWS IoT Device Defender behavior violations in a security profile and configure AWS IoT Device Defender violations in [fleet indexing](https://docs.aws.amazon.com/iot/latest/fleethubuserguide/aws-iot-monitor-admin-fleet-indexing.html)\. For more information about how to set up behavior violations in an AWS IoT Device Defender security profile, see [AWS IoT Device Defender Detect](https://docs.aws.amazon.com/iot/latest/developerguide/device-defender-detect.html)\. For more information about how to configure AWS IoT Device Defender violations, see [Manage fleet indexing for Fleet Hub applications](https://docs.aws.amazon.com/iot/latest/fleethubuserguide/aws-iot-monitor-admin-fleet-indexing.html) and [Managing thing indexing](https://docs.aws.amazon.com/iot/latest/developerguide/managing-index.html)\.