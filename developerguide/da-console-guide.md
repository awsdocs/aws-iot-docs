# Getting started with Device Advisor in the console<a name="da-console-guide"></a>

This tutorial helps you quickly get started with Device Advisor on the console\. Device Advisor offers features such as required tests and signed qualification reports to qualify and list devices in the [AWS Partner Device Catalog](https://devices.amazonaws.com/) as detailed in the [AWS IoT Core qualification program](https://aws.amazon.com/partners/dqp/)\.

For more information about using Device Advisor, see [Device Advisor workflow](device-advisor-workflow.md) and [Device Advisor detailed console workflow](device-advisor-console-tutorial.md)\.

To complete this tutorial, follow the steps outlined in [Setting up](device-advisor-setting-up.md)\.

**Note**  
Device Advisor is supported in us\-east\-1, us\-west\-2, ap\-northeast\-1, and eu\-west\-1 regions\.

**Getting started**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Test**, and then **Device Advisor**, and then choose **Start walkthrough**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-console-gs.png)

1. The **Getting started with Device Advisor** page gives an overview of the steps required to create a test suite and run tests against your device\. You can also find the Device Advisor test endpoint for your account\. You must configure the firmware or software on the device that you'll use for testing to connect to this test endpoint\.

   To complete this tutorial, you need to [ create a thing and certificate](https://docs.aws.amazon.com/iot/latest/developerguide/device-advisor-setting-up.html#da-create-thing-certificate)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-console-gs1.png)

   After you've reviewed the information, choose **Next**\.

1.  In **Step 1**, select an AWS IoT thing or certificate to test using Device Advisor\. If you don't have any existing things or certificates, see [Setting up](https://docs.aws.amazon.com/iot/latest/developerguide/device-advisor-setting-up.html)\. 

    In the **Test endpoint** section, select the endpoint that best fits our use case\. If you plan to run multiple test suites simultaneously using the same AWS account, select **Device\-level endpoint**\. Otherwise, to run one test suite at a time, select **Account\-level endpoint**\. 

   Configure your test device with the selected Device Advisor's test endpoint\.

   Choose **Next**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-console-endpoint.png)

1. In **Step 2**, you create and configure a custom test suite\. A custom test suite must have at least one test group, and each test group must have at least one test case\. We've added the **MQTT Connect** test case for you to get started\.

   Choose **Test suite properties**\. You must supply the test suite properties when you create your test suite\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-console-test-suite-create.png)

   You can configure the following suite\-level properties:
   + **Test suite name**: Enter a name for your test suite\.
   + **Timeout** \(optional\): The timeout in seconds for each test case in the current test suite\. If you don't specify a timeout value, the default value is used\.
   + **Tags** \(optional\): Add tags to the test suite that you're going to create\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-console-test-suite-properties.png)

   When you’ve finished, hoose **Update properties**\.

1. \(Optional\) You can update the test suite group configuration by choosing **Edit** next to the test group name\.
   + **Name**: Enter a custom name for the test suite group\.
   + **Timeout** \(optional\): The timeout in seconds for each test case in the current test suite\. If you don't specify a timeout value, the default value is used\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-console-test-suite-config.png)

   Choose **Done**\.

1. \(Optional\) You can update the test case configuration by choosing **Edit** next to the test case name\.
   + **Name**: Enter a custom name for the test suite group\.
   + **Timeout** \(optional\): The timeout in seconds for the selected test case\. If you don't specify a timeout value, the default value is used\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-console-test-case-config.png)

   Choose **Done**\.

1. \(Optional\) To add more test groups to the test suite, choose **Add test group** and ollow the instructions in Step 5\.\.

1. \(Optional\) To add more test cases, drag the test cases displayed under **Test cases** into any of your test groups\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-console-drag.png)

1. Test groups and test cases can be reordered by selecting and dragging the listed test cases\. Device Advisor runs tests in the order in which your test cases are listed\.

   After you've configured your test suite, choose **Next**\.

1. In **Step 3** you can configure a device role which Device Advisor will use to perform AWS IoT MQTT actions on behalf of your test device\. If you selected the **MQTT Connect** test case only, the **Connect** action will be selected automatically since that permission is required on device role to run this test suite\. If you selected other test cases, the corresponding actions will be selected\. 

   Provide the resource values for each of the selected actions\. For example, for the **Connect** action, provide the client id with which your device will connect to the Device Advisor endpoint\. You can provide multiple values by using commas to separate the values, and you can provide prefix values using a wildcard \(\*\) character\. For example, to provide permission to publish on any topic beginning with `MyTopic`, you can provide “`MyTopic*`” as the resource value\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-console-device-role.png)

   If you have already created a device role previously from [Setting up](https://docs.aws.amazon.com/iot/latest/developerguide/device-advisor-setting-up.html), and you would like to use that role, choose **Select an existing role** and choose your device role under **Select role**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-console-select-device-role.png)

   Configure your device role using one of the two provided options and choose **Next**\.

1. **Step 4** shows an overview of the selected test device, test endpoint, test suite, and test device role that you've configured\. If you want to make changes to a section, choose the **Edit** button above the section you want to edit\.
**Note**  
For best results, you can connect your selected test device to the Device Advisor test endpoint before starting the test suite run\. We recommend that you have a mechanism built for your device to try connecting to our test endpoint every five seconds for one to two minutes\.

   To create the test suite and run the selected tests against your device, choose **Run**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-console-device-review.png)

1. In the navigation pane, expand **Test**, **Device Advisor**, and then choose **Test runs and results** to view the run details and logs\. Select the test suite run that you started to view the run details and logs\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-console-runs-results.png)

1. To access the CloudWatch logs for the suite run:
   + Choose **Test suite log** to view the CloudWatch logs for the test suite run\.
   + Choose **Test case log** for any test case to view test case\-specific CloudWatch logs\.

1. Based on your test results, [troubleshoot](https://docs.aws.amazon.com/iot/latest/developerguide/iot_troubleshooting.html#device-advisor-troubleshooting) your device until all tests pass\.