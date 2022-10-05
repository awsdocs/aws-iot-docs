# Create your first Fleet Hub application<a name="aws-iot-monitor-admin-getting-started-first-app"></a>

## Prerequisites<a name="aws-iot-monitor-admin-getting-started-prereqs"></a>

The following list contains the resources you need to create a Fleet Hub web application\.
+ An [AWS account](https://aws.amazon.com)\.
+ [AWS IAM Identity Center \(successor to AWS Single Sign\-On\)](https://aws.amazon.com/single-sign-on/) turned on for your account\. \(If you haven't already activated this service, the AWS IoT Core console \([https://console\.aws\.amazon\.com/iot/](https://console.aws.amazon.com/iot/)\) prompts you to do so\.\)

## Create your first Fleet Hub web application<a name="aws-iot-monitor-admin-getting-started-steps"></a>



The following steps describe how to create Fleet Hub for AWS IoT Device Management web applications\.

1. Navigate to the AWS IoT Core console \([https://console\.aws\.amazon\.com/iot/](https://console.aws.amazon.com/iot/)\), and in the left panel, choose **Fleet Hub**, and then **Applications**\.

1. On the applications page, choose **Create application**\.

1. On the **Set up IAM Identity Center** page, if you haven't activated AWS IAM Identity Center \(successor to AWS Single Sign\-On\) \(IAM Identity Center\), follow the steps to activate it\. AWS Organizations sends you an email\. Choose the link in the email to finish activating IAM Identity Center\.
**Note**  
You can connect your own identity provider to IAM Identity Center\. For more information, see [What Is AWS IAM Identity Center \(successor to AWS Single Sign\-On\)?](https://docs.aws.amazon.com/singlesignon/latest/userguide/) and [ Connect to your external identity provider](https://docs.aws.amazon.com/singlesignon/latest/userguide/manage-your-identity-source-idp.html)\.

   The page tells you if you have already activated IAM Identity Center\.

   Choose **Next**\.

1. On the **Index AWS IoT data** page, review the information in the **How the data flow works from AWS IoT to Fleet Hub** section\. This page links you to the pages in the AWS IoT Core console where you can activate and manage AWS IoT Core fleet indexing\. You can use this service to index, search, and aggregate your registry data, shadow data, device connectivity data \(device lifecycle events\), and device violations data\. You can also create custom fields in addition to the managed fields that AWS IoT Core fleet indexing indexes by default\.
   + If you've activated fleet indexing, this pages displays your fleet indexing settings and custom fields\.
   + If you haven't enabled thing indexing and thing connectivity, you must do so to use Fleet Hub\.

   When you're done managing and reviewing your fleet indexing settings, choose **Next**\.

   For more information about how to activate fleet indexing for Fleet Hub applications, see [Managing fleet indexing for Fleet Hub applications](aws-iot-monitor-admin-fleet-indexing.md)\.

1. On the **Configure application** page, in the **Application role** section, create a new service role or select an existing service role\. Your Fleet Hub web application assumes this role when it uses Fleet Hub resources\. Federated users have the same permissions as the role when they use the web application\.
   + If you create a new role, the role name must begin with the following string: `AWSIotFleetHub_random_string`\.
   + If you select an existing role, make sure that it has the permissions that are in the policy document\. To see the permissions that your Fleet Hub web application needs, choose **View role details**\. A window opens that shows you the policy document that the service applies to any new role that you create from this page\.

1. On the **Configure application** page, in the **Application properties** section, enter a name for your application\. Optionally, you can also enter an application description\.

   Choose **Create application**\.

1. On the **Applications** page, select the application that you created and choose **View details**\. Review the details of the application\.

**Note**  
For more information about possible solutions for resolving issues as an administrator of Fleet Hub, see [Troubleshooting](aws-iot-monitor-admin-troubleshoot.md)\.