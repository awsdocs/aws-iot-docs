# Getting started with Device Advisor in the console<a name="da-console-guide"></a>

This tutorial helps you quickly get started with Device Advisor in the console\. Device Advisor offers features such as required tests and signed qualification reports to qualify and list devices in the [AWS Partner Device Catalog](https://devices.amazonaws.com/) as per the [AWS IoT Core qualification program](https://aws.amazon.com/partners/dqp/)\.

For more information about using Device Advisor, see [Device Advisor workflow](device-advisor-workflow.md) and [Device Advisor detailed console workflow](device-advisor-console-tutorial.md)\.

To complete this tutorial, follow the steps outlined in [Setting up](device-advisor-setting-up.md)\.

**Note**  
Device Advisor is supported in us\-east\-1, us\-west\-2, ap\-northeast\-1, and eu\-west\-1 regions\.

**Getting started**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Test**, **Device Advisor**, and then choose **Start walkthrough**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-console-gs.png)

1. The **Getting started with Device Advisor** gives an overview of the steps required to create a test suite and run tests against your device\. You can also find the Device Advisor test endpoint for your account\. You must configure the firmware or software on the device that you'll use for testing to connect to this test endpoint\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-console-gs1.png)

   After you've reviewed the informaton, choose **Next**\.

1. In **Step 1**, you selected an AWS IoT thing or certificate to test using Device Advisor\. If you don't have any existing things or certificates, see [Setting up](device-advisor-setting-up.md)\.

   If you see the AWS IoT thing or certificate that you've configured with your Device Advisor test endpoint, choose that device from the list, and then choose **Next**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-console-thing-certificate.png)

1. In **Step 2**, you can create and configure a custom test suite\. A custom test suite must have at least one test group, and each test group must have at least one test case\. We've added the **MQTT Connect** test case for you to get started\.

1. Choose **Test suite properties**\. You must supply the test suite properties when you create your test suite\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-console-step1.png)

   You can configure the suite\-level properties here\.
   + **Test suite name**: You can create the suite with a custom name\.
   + **Device role ARN**: Provide the device role ARN that was created as part of [Setting up](device-advisor-setting-up.md)\.
   + **Timeout** \(optional\): The timeout in seconds for each test case in the current test suite\. If you don't specify a timeout value, the default value is used\.
   + **Tags** \(optional\): Add tags to the test suite that you're going to create\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-test-suite-properties-1.png)

   When you've finished, choose **Update properties**\.

1. \(Optional\) You can update the test suite group configuration by choosing **Edit** next to the test group name\.
   + **Name**: You can update the group with a custom name\.
   + **Timeout** \(optional\): The timeout in seconds for each test case in the current test suite\. If you don't specify a timeout value, the default value is used\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-console-test-suite-config.png)

   Choose **Done**\.

1. \(Optional\) You can update the test case configuration by choosing **Edit** next to the test case name\.
   + **Name**: You can update the test case with a custom name\.
   + **Timeout** \(optional\): The timeout in seconds for the selected test case\. If you don't specify a timeout value, the default value is used\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-console-test-case-config.png)

   Choose **Done**\.

1. \(Optional\) To add more test groups to the test suite, choose **Add test group** and configure accordingly\.

1. \(Optional\) To add more test cases, drag the test cases under **Test cases** into any of the test groups you have\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-console-drag.png)

1. Test groups and test cases can be reordered by dragging\. Device Advisor runs tests in the order they're defined\.

   After you've configured your test suite, choose **Next**\.

1. **Step 3** provides you with an overview of the selected test device and test suite that you've configured\. If you want to make changes, choose **Edit**\.
**Note**  
For a smoother experience, you can connect your selected test device to the Device Advisor test endpoint before starting the suite run\. We recommend that you have a mechanism built for your device to try connecting to our test endpoint every 5 seconds for one to two minutes\.

   To create the test suite and run the tests against your device, choose **Run**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-run-tests.png)

1. In the navigation pane, expand **Test**, **Device Advisor**, and then choose **Test runs and results** to view the run details and logs\.

   Select the test suite run that has been started to view the run details and logs\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-console-runs-results.png)

1. To access the CloudWatch logs for the suite run:
   + Choose **Test suite log** to view the CloudWatch logs for the test suite run\.
   + Choose **Test case log** for any test case to view test case\-specific CloudWatch logs\.

1. Based on your test results, [troubleshoot](https://docs.aws.amazon.com/iot/latest/developerguide/iot_troubleshooting.html#device-advisor-troubleshooting) your device until all tests pass\.