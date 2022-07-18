# View the dashboard<a name="aws-iot-monitor-user-queries-dashboard"></a>

When you first log in to your Fleet Hub for AWS IoT Device Management web application, you see a dashboard that presents two views of data about the devices in your fleet\.

## Summary<a name="aws-iot-monitor-user-queries-dashboard-summary"></a>

The **summary** view displays a rolled\-up view of data about all of the devices in your fleet\. It provides the following information\.
+ Total number of devices
+ Number of connected devices
+ A list of reasons why devices have disconnected
+ The thing types that you have created for your fleet and the number of devices for each type
+ The thing groups that you have created for your fleet and the number of devices in each group

![\[Fleet Hub queries dashboard summary\]](http://docs.aws.amazon.com/iot/latest/fleethubuserguide/images/iot-monitor-queries-dashboard-summary.png)

## Device list<a name="aws-iot-monitor-user-queries-dashboard-devicelist"></a>

The **Device list** view displays a table that lists the devices in your fleet\. The table provides the following information for each device in the list\.
+ The device name
+ The device's connection status
+ The timestamp for the device's last connection
+ For a device that isn't connected, the reason why it disconnected
+ The device's thing type
+ The device's thing group
+ The custom fields that you've created in the fleet indexing service

![\[Fleet Hub queries dashboard device list\]](http://docs.aws.amazon.com/iot/latest/fleethubuserguide/images/iot-monitor-queries-dashboard-device-list.png)

On the device list, you can choose **Export current page** to download a CSV file that contains the devices displayed on the page \(but not on subsequent pages, if the list is paginated\)\.

You can use queries and filters to narrow the number of devices that generate the summary data in the first view and that appear in the device list\. For more information about using queries and filters to get more specific information about devices in your fleet, see [Creating queries](aws-iot-monitor-user-queries-creating.md#aws-iot-monitor-user-queries-create)\.