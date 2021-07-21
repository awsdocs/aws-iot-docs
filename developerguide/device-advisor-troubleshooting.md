# Device Advisor troubleshooting guide<a name="device-advisor-troubleshooting"></a>General

Q: Can I run multiple test suites in parallel?  
A: No\. Currently Device Advisor does not support running multiple test suites since only one Device Advisor endpoint is available per account\. However, you can test multiple device types by sequencing the test suites one after the other\.

Q: I saw from my device that the TLS connection got denied by Device Advisor\. Is this expected?  
A: Yes\. Device Advisor denies the TLS connection before and after each test run\. We recommend users to implement device retry mechanism in order to have a full\-automated testing experience with Device Advisor\. If you execute a test suite with more than one test case say \- TLS connect, MQTT connect and MQTT publish then we recommend that you have a mechanism built for your device to try connecting to our test end point every 5 seconds for a minute to two\. This will enable you to run multiple test cases in sequence in automated manner\.

Q: Can I get a history of Device Advisor API calls made on my account for security analysis and operational troubleshooting purposes?   
A: Yes\. To receive a history of Device Advisor API calls made on your account, you simply turn on CloudTrail in the AWS IoT Management Console and filter the event source to be `iotdeviceadvisor.amazonaws.com`\.

Q: How do I view Device Advisor logs in CloudWatch?  
Logs generated during a test suite run are uploaded to CloudWatch if you add the required policy \(for example, **CloudWatchFullAccess**\) to your service role \(see [Setting up](device-advisor-setting-up.md)\. A log group "aws/iot/deviceadvisor/$testSuiteId" will be created\. In this log group, two log streams will be created if there is at least  one test case in your test suite\. One is named "$testRunId" and includes logs of actions taken before and after executing the test cases in your test suite, such as setup and cleanup steps\. Another is "$suiteRunId\_$testRunId" which is specific to a test suite run\. Events sent from devices and AWS IoT Core will be logged to this log stream\.

Q: What is the purpose of the device permission role?  
A: Device Advisor stands between your test device and AWS IoT Core to simulate test scenarios\. It accepts connections and messages from your test devices and forwards them to AWS IoT Core by assuming your device permission role and initiating a connection on your behalf\. It’s important to make sure the device role permissions are the same as those on the certificate you use for running tests\. AWS IoT certificate policies are not enforced when Device Advisor initiates a connection to AWS IoT Core on your behalf by using the device permission role\. However, the permissions from the device permission role you set are enforced\.

Q: What Regions is Device Advisor supported in?  
A: Device Advisor is supported in us\-east\-1, us\-west\-2, ap\-northeast\-1, and eu\-west\-1 Regions\.

Q: What if I see inconsistent results?  
A: One of the primary causes of inconsistent results is setting a test's `EXECUTION_TIMEOUT` to a value that is too low\. For more information about recommended and default `EXECUTION_TIMEOUT` values, see [Device Advisor test cases](https://docs.aws.amazon.com/iot/latest/developerguide/device-advisor-tests.htm)\.

Q: What MQTT protocol does Device Advisor support?  
A: Device Advisor supports MQTT with X509 client certificates\.