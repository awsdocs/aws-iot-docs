# Getting started<a name="aws-iot-monitor-admin-getting-started"></a>

This topic explains how to get set up to create Fleet Hub for AWS IoT Device Management web applications and create your first web application\.

## Prerequisites<a name="aws-iot-monitor-admin-getting-started-prereqs"></a>

The following list contains the resources you need to create a Fleet Hub web application\.
+ An [AWS account](https://aws.amazon.com)\.
+ [AWS Single Sign\-On](https://aws.amazon.com/single-sign-on/) enabled in your account\. \(The AWS IoT Core console \([https://console\.aws\.amazon\.com/iot/](https://console.aws.amazon.com/iot/)\) will prompt you to activate this service, if you haven't already\.\)

## Create your first Fleet Hub application<a name="aws-iot-monitor-admin-getting-started-first-app"></a>

Follow the steps in this topic to create your first Fleet Hub web application\.

1. Navigate to the AWS IoT Core console \([https://console\.aws\.amazon\.com/iot/](https://console.aws.amazon.com/iot/)\), and in the left panel, choose **Fleet Hub**, and then **Getting started**\.

1. On the Fleet Hub getting started page, choose **Create application** at the top of the page\.

1. On the **Set up Single Sign\-On** page, if you haven't activated AWS Single Sign\-On \(AWS SSO\), follow the steps to activate it\. AWS Organizations will send you an email\. Click the link in the email to finish activating AWS SSO\.

   The page tells you if you have already activated AWS SSO\.
**Note**  
You can connect your own identity provider to AWS SSO\. For more information, see [What Is AWS Single Sign\-On?](https://docs.aws.amazon.com/singlesignon/latest/userguide/)

   Choose **Next**\.

1. On the **Index AWS IoT data** page, the **Manage indexing** button links you to the **Manage fleet indexing** page in the AWS IoT Core console where you can activate and manage the AWS IoT Core fleet indexing\. You can use fleet indexing to index, search, and aggregate your registry data, shadow data, device connectivity data \(device lifecycle events\), and device violations data\. You can also create custom fields in addition to the managed fields that AWS IoT Core fleet indexing indexes by default\.

   If you have already activated fleet indexing, this page displays your fleet indexing settings and any custom fields that you have created\.

   In addition to activating fleet indexing, you also must activate thing connectivity and thing indexing to work with Fleet Hub\.

   For more information about how to mange fleet indexing in Fleet Hub, see [Managing fleet indexing](https://docs.aws.amazon.com/iot/latest/developerguide/managing-fleet-index.html)\.

   When you're done managing and reviewing your fleet indexing settings, choose **Next**\.

1. On the **Configure application** page, in the **Application role** section, create a new service role or select an existing service role\. Your Fleet Hub web application assumes this role when it uses Fleet Hub resources\. Federated users have the same permissions as the role when they use the web application\.

   If you create a new role, the role name must begin with the following string: `AWSIotFleetHub_random_string`\.

   To see the permissions that your Fleet Hub web application needs, choose **View role details**\. This opens a window that shows you the policy document that the service applies to any new role that you create from this page\. If you select an existing role, make sure that it has the permissions that are in this policy document\.

1. On the **Configure application** page, in the **Application properties** section, enter a name for your application\. Optionally, you can also enter an application description\.

   Choose **Create application**\.

1. On the **Applications** page, select the application that you created and choose **View details**\. Review the details of the application\.

1. On the **Applications** pages, select your web application from the **Fleet Hub applications** list\. Choose **View details**\.

1. On the application details page, choose **Add user**\.

1. In the **Add Fleet Hub users** window, select the users from your organization that you want to have access to the application\. Choose **Add selected users**\.

1. On the application details page, verify that you see the users you selected in the **Fleet Hub users** list\.

**Note**  
For more information, read [Troubleshooting](aws-iot-monitor-admin-troubleshoot.md) for possible solutions to help resolve issues as an administrator of Fleet Hub\.