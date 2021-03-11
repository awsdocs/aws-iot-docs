--------

 The Fleet Hub service is currently in public preview\. This service is subject to change\.

--------

# Create your first query<a name="aws-iot-monitor-user-getting-started-first-query"></a>

This topic walks you through the steps to create a simple Fleet Hub for AWS IoT Device Management query\.

**Prerequisites**
+ A Fleet Hub application associated with an AWS IoT Core account that contains devices \(things\)\.
+ An account in your organization that has permissions to use the Fleet Hub application\.

**Create your first Fleet Hub query**

1. Navigate to your Fleet Hub application\.

   The default dashboard view displays a list of all devices that contains the managed and custom attributes\. The attributes that contain the **attributes** prefix are custom attributes\.

1. On the menu at the top of the page, choose the **Connectivity** filter\. Enter **false** in the text box next to the dropdown menu\.

1. To perform the search, choose **Search**\. You see a list of all devices that aren’t connected to AWS IoT Core\.

1. To download the contents of the query as a CSV file, choose **Export**\. You can open this file in your preferred spreadsheet application to view the contents of the list\.