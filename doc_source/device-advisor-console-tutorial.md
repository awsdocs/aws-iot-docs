# Device Advisor detailed console workflow<a name="device-advisor-console-tutorial"></a>

In this tutorial, you'll create a custom test suite and run tests against the device you want to test in the console\. After the tests are complete, you can view the test results and detailed logs\.

**Topics**
+ [Prerequisites](#da-detailed-prereqs)
+ [Create a test suite definition](#device-advisor-console-create-suite)
+ [Start a test suite run](#device-advisor-console-run-test-suite)
+ [View test suite run details and logs](#device-advisor-console-view-logs)
+ [Download an AWS IoT qualification report](#device-advisor-console-qualification-report)

## Prerequisites<a name="da-detailed-prereqs"></a>

To complete this tutorial, you need to complete the steps outlined in [Setting up](device-advisor-setting-up.md)\.

## Create a test suite definition<a name="device-advisor-console-create-suite"></a>

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Test**, **Device Advisor** and then choose **Test suites**\.
**Note**  
Only the us\-east\-1 Region is supported for preview\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-testsuite.png)

1. Select **Create Test Suite**\. Choose between Use the AWS Qualification test suite and Create a new test suite\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-create-test-suite.png)

   Choose Use the AWS Qualification test suite if you'd like to qualify and list your device to the AWS Partner Device Catalog\. By choosing this option, test cases required for qualification of your device to the AWS IoT Core qualification program are pre\-selected\. Test groups and test cases can't be added or removed\. However, you'll still need to configure the test suite properties\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-core-test-suite.png)

   Choose Create a new test suite to create and configure a custom test suite\. We recommend starting with this option for initial testing and troubleshooting\. A custom test suite must have at least one test group, and each test group must have at least one test case\. For the purpose of this tutorial, we'll select this option and choose **Next**\.

1. Choose **Test suite properties**\. You must add the test suite properties when you create your test suite\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-test-suite-properties.png)

   Under **Test suite properties**, fill out the following\.
   + **Test suite name**: You can create the suite with a custom name\.
   + **Device role ARN**: Provide the device role ARN that was created as part of the [prerequisites](device-advisor-setting-up.md)\.
   + **Timeout** \(optional\): The timeout in milliseconds for each test case in the current test suite\. If you don't specify a timeout value, the default value is used\.
   + **Tags** \(optional\): Add tags to the test suite that you're going to create\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-test-suite-properties-panel.png)

   When you've finished, choose **Update properties**\.

1. To modify the group level configuration, under `Test group 1`, choose **Edit**\. Then, enter a **Name** to give the group a custom name\. Optionally, you can also enter a **Timeout** value in milliseconds under the selected test group\. If you don't specify a timeout value, the default value is used\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-edit-test-group.png)

   Choose **Done**\.

1. Drag one of the available test cases from **Test cases** into the test group\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-drag-test-cases.png)

1. To modify the test case level configuration on the test case under your test group, choose **Edit**\. Then, enter a **Name** to give the group a custom name\. Optionally, you can also enter a **Timeout** value in milliseconds under the selected test group\. If you don't specify a timeout value, the default value is used\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-edit-test-case.png)

   Choose **Done**\.
**Note**  
To add more test groups to the test suite, choose **Add test group**\. Follow the preceding steps to create and configure more test groups or to add more test cases to one or more test groups\. Test groups and test cases can be reordered by dragging\. Device Advisor runs tests in the order in which you define the test groups and test cases\.

1. Choose **Create test suite**\.

   The test suite should be created successfully and you'll be redirected to the **Test suites** page where you can view all the test suite that have been created\.

   If the test suite creation failed, make sure the test suite, test groups, and test cases have been configured according to the instructions\.

## Start a test suite run<a name="device-advisor-console-run-test-suite"></a>

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Test**, **Device Advisor**, and then choose **Test suites**\.

1. Choose the test suite for which you'd like to view the test suite details\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-test-suites.png)

   The test suite detail page displays all the information related to the test suite\. The **Device Advisor Endpoint** displayed on this page can be used to configure the firmware/software on the device that you'll use for testing to connect to the Device Advisor test endpoint for your account\.

1. Choose **Actions**, then **Run test suite**\.

1. Under **Run configuration**, you'll need to select an AWS IoT thing or certificate to test using Device Advisor\. If you don't have any existing things or certificates, first [create AWS IoT Core resources](device-advisor-setting-up.md)\. After you select a thing or certificate, choose **Run test**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-choose-thing-certificate.png)

1. Choose **Go to results** on the top banner for viewing the test run details\.

## View test suite run details and logs<a name="device-advisor-console-view-logs"></a>

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Test**, **Device Advisor** and then choose **Test runs and results**\.

   This page displays:
   + Number of IoT things
   + Number of IoT certificates
   + Number of test suites currently running
   + All the test suite runs that have been created

1. Choose the test suite for which you'd like to view the run details and logs\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-test-suite-run.png)

   The run summary page displays the status of the current test suite run\. This page auto refreshes every 10 seconds\. We recommend that you have a mechanism built for your device to try connecting to our test endpoint every five seconds for one to two minutes\. Then you can run multiple test cases in sequence in an automated manner\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-run-summary.png)

1. To access the CloudWatch logs for the test suite run, choose **Test suite log**\.

   To access CloudWatch logs for any test case, choose **Test case log**\.

1. Based on your test results, [troubleshoot](https://docs.aws.amazon.com/iot/latest/developerguide/iot_troubleshooting.html#device-advisor-troubleshooting) your device until all tests pass\.

## Download an AWS IoT qualification report<a name="device-advisor-console-qualification-report"></a>

If you chose the **Use the AWS IoT Qualification test suite** option while creating a test suite and were able to successfully run a qualification test suite, you can download a qualification report by choosing **Download qualification report** in the test run summary page\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/da-qualification-report.png)