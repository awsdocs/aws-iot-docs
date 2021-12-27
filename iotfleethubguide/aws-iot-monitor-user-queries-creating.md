# Creating queries with filters<a name="aws-iot-monitor-user-queries-creating"></a>

This topic explains how Fleet Hub for AWS IoT Device Management queries work and walks you through the steps required to create a query with filters\.

You can control the number and types of devices that display on your dashboard summary and list views by using queries\. You filter queries by using AWS\-managed and custom fields from AWS IoT fleet indexing\. If you want a field, including a device shadow field, to appear in your dashboard, your administrator must create it as an aggregation field in the fleet indexing service\. For more information about the fleet indexing, see [Fleet indexing](https://docs.aws.amazon.com/iot/latest/developerguide/iot-indexing.html)\.

You can also add keywords to your queries\. Keywords apply across all searchable fields\. They also count against the limit of three filters that you can apply in a single query\.

The following section describes the steps required to create a typical query\.

## Creating queries<a name="aws-iot-monitor-user-queries-create"></a>

The following steps describe how to create a typical query\.

**Prerequisites**
+ A Fleet Hub application tied to an AWS IoT Core account that contains several devices \(things\)
+ An account that has permissions to use the Fleet Hub application

**Create your first Fleet Hub query with a filter in the console**

1. Navigate to your Fleet Hub application\.

1. On the default dashboard, verify that you can see the **Device list** tab and the total number of devices \(things\) in the associate AWS IoT Core account\.  


1. On the default dashboard, choose the **Device list** tab\. Verify that you see a list of all devices that contain the managed and custom attributes\. The custom attributes contain the **attributes** prefix\.  


1. At the top of the page, enter any keyword you want to include in your query\. Keyword queries apply to all fields\.

1. At the top of the page, choose **Filter**\.

1. In the **Filter** modal, under **Field**, choose the field that you want to use as a filter\. Under **Operator**, choose an option\. Finally, for **Value**, choose the field value to use in your filter\.

   You can add up to three filters\. A keyword query counts against this number\.

1. To perform your query, choose **Apply filters**\. The results show all the devices that match your query\.