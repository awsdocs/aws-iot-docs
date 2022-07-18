# Creating alarms<a name="aws-iot-monitor-user-alarms-create"></a>

This topic walks you through the steps required to create a Fleet Hub for AWS IoT Device Management alarm\. It assumes that your administrator has created an aggregation field out of a device shadow field named **shadow\.reported\.batterylevel**\. This custom field indicates the battery level of a device\. You need to ask your administrator to create searchable custom fields in the AWS IoT fleet indexing service\.

The alarm that you create sends an Amazon Simple Notification Service \(Amazon SNS\) message to a list of people in your organization whenever the average battery level of devices in your fleet falls below 50% during a period of 1 minute\.

**Create a Fleet Hub query**

1. Navigate to your Fleet Hub application\.

1. If you want to target a specific set of devices, create a query\. For instructions on how to create a simple query, see [Create queries with filters](aws-iot-monitor-user-queries-creating.md)\. If you don't create a query, your alarm applies to all of the devices in your fleet\.

1. On the default dashboard page, choose **Create alarm**\.

1. On the **Build aggregation metric** page, verify that your query appears under **Target query**\. In the **Configure fleet metric aggregation** section, for **Choose field**, choose **shadow\.reported\.batterylevel**\. This menu contains the AWS\-managed fields and the custom fields that your administrator has created in the AWS IoT fleet indexing service\.

1. For **Choose aggregation type**, choose **Average**\. This choice bases the alarm on the average battery level value in your device fleet\.

1. For **Choose period**, choose **1 minute**\. This triggers the alarm when your device fleet remains in the specified alarming state for one minute\.

   Choose **Next**\.

1. On the **Set threshold** page, in the **Trigger the alarm whenever\.\.\.** section, choose **Lower/Equal**\. This triggers the alarm when the average battery level value falls below a value that you specify\.

1. In the **Than** text box, enter 50\.

   Choose **Next**\.

1. On the **Notify user** page, in the **Notify \-\- optional** section, enter a name for the email list that contains the users in your organization who receive notifications when the alarm is active\. Enter a comma\-separated list of email addresses to populate this list\.

1. In the **Alarm details** section, enter a name for your alarm, and optionally enter a description for your alarm\. Choose **Next**\.

1. On the **Review** page, verify the information that you entered on the previous pages\. Choose **Submit**\. You return to the default dashboard\.

1. On the default dashboard, in the left navigation panel, choose **Fleet Hub alarms**\. Verify that you see the alarm that you created\.