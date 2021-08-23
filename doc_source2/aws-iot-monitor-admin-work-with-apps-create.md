# How to create Fleet Hub for AWS IoT Device Management applications<a name="aws-iot-monitor-admin-work-with-apps-create"></a>

The following steps describe how to create Fleet Hub for AWS IoT Device Management web applications\.

1. Navigate to the AWS IoT Core console \([https://console\.aws\.amazon\.com/iot/](https://console.aws.amazon.com/iot/)\), and in the left panel, choose **Fleet Hub**, and then **Applications**\.

1. On the applications page, choose **Create application**\.

1. On the **Set up Single Sign\-On** page, if you haven't activated AWS Single Sign\-On \(AWS SSO\), follow the steps to activate it\. AWS Organizations will send you an email\. Click the link in the email to finish activating AWS SSO\.
**Note**  
You can connect your own identity provider to AWS SSO\. For more information, see [What Is AWS Single Sign\-On?](https://docs.aws.amazon.com/singlesignon/latest/userguide/)

   The page tells you if you have already activated AWS SSO\.

   Choose **Next**\.

1. On the **Index AWS IoT data** page, review the information in the **How the data flow works from AWS IoT to Fleet Hub** section\. This page links you to the pages in the AWS IoT Core console where you can activate and manage the AWS IoT Core fleet indexing service\. You can use this service to index, search, and aggregate your registry data, shadow data, and device connectivity data \(device lifecycle events\)\. You can also create custom fields in addition to the managed fields that AWS IoT Core fleet indexing indexes by default\.

   If you have already activated fleet indexing, this page displays your fleet indexing settings and any custom fields that you have created\. You must enable thing indexing and thing connectivity to use Fleet Hub\.

   In addition to activating fleet indexing, you also must activate thing connectivity and thing indexing to work with Fleet Hub\.

   For more information about fleet indexing, see [Fleet indexing service](https://docs.aws.amazon.com/iot/latest/developerguide/iot-indexing.html)\.

   When you're done managing and reviewing your fleet indexing settings, choose **Next**\.

1. On the **Configure application** page, in the **Application role** section, create a new service role or select an existing service role\. Your Fleet Hub web application assumes this role when it uses Fleet Hub resources\. Federated users have the same permissions as the role when they use the web application\.

   If you create a new role, the role name must begin with the following string: `AWSIotFleetHub_random_string`\.

   To see the permissions that your Fleet Hub web application needs, choose **View role details**\. This opens a window that shows you the policy document that the service applies to any new role that you create from this page\. If you select an existing role, make sure that it has the permissions that are in this policy document\.

1. On the **Configure application** page, in the **Application properties** section, enter a name for your application\. Optionally, you can also enter an application description\.

   Choose **Create application**\.

1. On the **Applications** page, select the application that you created and choose **View details**\. Review the details of the application\.