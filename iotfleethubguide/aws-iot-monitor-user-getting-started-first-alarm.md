# Create your first alarm<a name="aws-iot-monitor-user-getting-started-first-alarm"></a>

This topic walks you through the steps to create a simple Fleet Hub for AWS IoT Device Management alarm\.

## Prerequisites<a name="aws-iot-monitor-user-getting-started-first-alarm-prerequisites"></a>
+ A Fleet Hub application associated with an AWS IoT Core account that contains devices \(things\)\.
+ An account in your organization that has permissions to use the Fleet Hub application\.

## Creating your first alarm<a name="aws-iot-monitor-user-getting-started-first-alarm-steps"></a>

**Create your first Fleet Hub alarm**

1. Navigate to your Fleet Hub application\.

1. If you want to target a specific set of devices, create a query\. For instructions on how to create a simple query, see [Create your first query](aws-iot-monitor-user-getting-started-first-query.md)\. If you don't create a query, your alarm will apply to all of the devices in your fleet\.

1. On the default dashboard page, choose **Create alarm**\.

1. On the **Build aggregation metric** page, verify that your query appears under **Target query**\. In the **Configure fleet metric aggregation** section, on the **Choose field** menu, choose **Connected**\. This AWS\-managed field indicates whether a device is connected to AWS IoT Core\. The **Choose field** menu contains the AWS\-managed fields and the custom fields that your administrator has created in AWS IoT fleet indexing\.

1. For **Choose aggregation type**, choose any one of the following options\.
   + **Maximum** \-\- Configure a maximum threshold\.
   + **Count** \-\- Configure a specific count as the threshold\. 
   + **Sum** \-\- Configure a sum as the threshold\.
   + **Minimum** \-\- Configure a minimum threshold\.
   + **Average** \-\- Configure an average threshold\.

1. For **Choose period**, choose the duration of the condition specified in the preceding menus that will trigger the alarm\.

   An example setting for **Configure fleet metric aggregation** can look like the following:  
![\[Fleet Hub create first query dashboard\]](http://docs.aws.amazon.com/iot/latest/fleethubuserguide/images/iot-monitor-create-first-alarm-fleet-metric.png)

   Choose **Next**\.

1. On the **Set threshold** page, in the **Trigger the alarm whenever\.\.\.** section, choose one of any of the following options\.
   + **Greater** \-\- Alarms when the aggregation metric and type exceeds the specified value\.
   + **Greater/Equal** \-\- Alarms when the aggregation metric and type equals or exceeds the specified value\.
   + **Lower** \-\- Alarms when the aggregation metric and type falls below the specified value\.
   + **Lower/Equal** \-\- Alarms when the aggregation metric and type equals or falls below the specified value\.

1. In the **Than** text box, specify the value to use as the threshold for the alarm\.

   An example setting for **Set threshold** can look like the following:  
![\[Fleet Hub create first alarm set threshold\]](http://docs.aws.amazon.com/iot/latest/fleethubuserguide/images/iot-monitor-create-first-alarm-set-threshold.png)

   Choose **Next**\.

1. On the **Notify user** page, in the **Notify \-\- optional** section, enter a name for the email list that contains the users in your organization who receive notifications when the alarm is active\. Enter a comma\-separated list of email addresses to populate this list\.

1. In the **Alarm details** section, enter a name for your alarm, and optionally enter a description for your alarm\. Choose **Next**\.

1. On the **Review** page, verify the information that you entered on the previous pages\. Choose **Submit**\. You return to the default dashboard\.

1. On the default dashboard, the alarms widgets display information of all the alarms you created\.  
![\[Fleet Hub create first alarm set threshold\]](http://docs.aws.amazon.com/iot/latest/fleethubuserguide/images/iot-monitor-create-first-alarm-widgets.png)

   To see details of the alarms that you created, in the left navigation panel, choose **Fleet Hub alarms**\.   
![\[Fleet Hub create first alarm set threshold\]](http://docs.aws.amazon.com/iot/latest/fleethubuserguide/images/iot-monitor-create-first-alarm-fleet-hub-alarms.png)