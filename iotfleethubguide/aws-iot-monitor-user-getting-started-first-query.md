# Create your first query<a name="aws-iot-monitor-user-getting-started-first-query"></a>


****  

|  | 
| --- |
|  The fleet indexing feature that supports indexing named shadows and AWS IoT Device Defender violations data is in preview release for AWS IoT Device Management and is subject to change\. | 

This topic walks you through the steps to create a simple Fleet Hub for AWS IoT Device Management query\. The queries are specified using search query syntax\.

## Prerequisites<a name="aws-iot-monitor-user-getting-started-first-query-prerequisites"></a>
+ A Fleet Hub application associated with an AWS IoT Core account that contains devices \(things\)\.
+ An account in your organization that has permissions to use the Fleet Hub application\.

## Create your first Fleet Hub query<a name="aws-iot-monitor-user-getting-started-first-query-steps"></a>

**Create your first Fleet Hub query**

1. Navigate to your Fleet Hub application\.

   The default dashboard view displays a list of all devices that contains the managed and custom attributes\. The attributes that contain the **attributes** prefix are custom attributes\.

1. On the menu at the top of the page, choose **Connected** from **All fields**\. Enter **false** in the text box next to the dropdown menu\.  
![\[Fleet Hub create first query dashboard\]](http://docs.aws.amazon.com/iot/latest/fleethubuserguide/images/iot-monitor-create-first-query-dashboard.png)

1. To perform the search, choose **Search**\. You see a list of all devices that aren’t connected to AWS IoT Core\.

For more information about the query syntax and example queries, see [Query syntax](https://docs.aws.amazon.com/iot/latest/developerguide/query-syntax.html), [Example thing queries](https://docs.aws.amazon.com/iot/latest/developerguide/example-queries.html), and [Example thing group queries](https://docs.aws.amazon.com/iot/latest/developerguide/example-thinggroup-queries.html)\.