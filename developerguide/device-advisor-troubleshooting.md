# AWS IoT Device Advisor troubleshooting guide<a name="device-advisor-troubleshooting"></a>

**Help us improve this topic**  
 [Let us know what would help make it better](https://docs.aws.amazon.com/forms/aws-doc-feedback?hidden_service_name=IoT%20Docs&topic_url=http://docs.aws.amazon.com/en_us/iot/latest/developerguide/device-advisor-troubleshooting.html) General

Q: Can I run multiple test suites in parallel?  
A: Yes\. Device Advisor now supports running multiple test suites on different devices using a Device\-level endpoint\. If you use the Account\-level endpoint, you can run one suite at a time because one Device Advisor endpoint is available per account\. For more information see [ Configure your device](https://docs.aws.amazon.com/iot/latest/developerguide/device-advisor-setting-up.html#da-configure-device)\.

Q: I saw from my device that the TLS connection was denied by Device Advisor\. Is this expected?  
A: Yes\. Device Advisor denies the TLS connection before and after each test run\. We recommend that users implement a device retry mechanism to have a fully automated testing experience with Device Advisor\. If you execute a test suite with more than one test case, for example TLS connect, MQTT connect, and MQTT publish, then we recommend that you have a mechanism built for your device\. The mechanism can try to connect to our test endpoint every 5 seconds for a minute to two\. In this way you can run multiple test cases in sequence in an automated manner\.

Q: Can I get a history of Device Advisor API calls made on my account for security analysis and operational troubleshooting purposes?   
A: Yes\. To receive a history of Device Advisor API calls made on your account, you simply turn on CloudTrail in the AWS IoT Management Console and filter the event source to be `iotdeviceadvisor.amazonaws.com`\.

Q: How do I view Device Advisor logs in CloudWatch?  
A: Logs generated during a test suite run are uploaded to CloudWatch if you add the required policy \(for example, **CloudWatchFullAccess**\) to your service role \(see [Setting up](device-advisor-setting-up.md)\)\. If there is at least one test case in the test suite, a log group "aws/iot/deviceadvisor/$testSuiteId" is created with two log streams\. One stream is the "$testRunId" and includes logs of actions taken before and after executing the test cases in your test suite, such as setup and cleanup steps\. The other log stream is "$suiteRunId\_$testRunId," which is specific to a test suite run\. Events sent from devices and AWS IoT Core will be logged to this log stream\.

Q: What is the purpose of the device permission role?  
A: Device Advisor stands between your test device and AWS IoT Core to simulate test scenarios\. It accepts connections and messages from your test devices and forwards them to AWS IoT Core by assuming your device permission role and initiating a connection on your behalf\. It’s important to make sure the device role permissions are the same as those on the certificate you use for running tests\. AWS IoT certificate policies are not enforced when Device Advisor initiates a connection to AWS IoT Core on your behalf by using the device permission role\. However, the permissions from the device permission role you set are enforced\.

Q: In what Regions is Device Advisor supported?  
A: Device Advisor is supported in us\-east\-1, us\-west\-2, ap\-northeast\-1, and eu\-west\-1 Regions\.

Q: Why do I see inconsistent results?  
A: One of the primary causes of inconsistent results is setting a test's `EXECUTION_TIMEOUT` to a value that is too low\. For more information about recommended and default `EXECUTION_TIMEOUT` values, see [Device Advisor test cases](https://docs.aws.amazon.com/iot/latest/developerguide/device-advisor-tests.htm)\.

Q: What MQTT protocol does Device Advisor support?  
A: Device Advisor supports MQTT with X509 client certificates\.

Q: What if my test case failed with an execution timed out message even though I tried to connect my device to the test endpoint?  
A: Validate all the steps under [ Create an IAM role to be used as your device role](https://docs.aws.amazon.com/iot/latest/developerguide/device-advisor-setting-up.html#da-iam-role)\. If the test still fails, it could be that the device is not sending the correct Server Name Indication \(SNI\) extension, which is required for Device Advisor to work\. The correct SNI value is the endpoint address returned when following the [ Configure your device section](https://docs.aws.amazon.com/iot/latest/developerguide/device-advisor-setting-up.html#da-configure-device)\. AWS IoT also requires devices to send the Server Name Indication \(SNI\) extension to the Transport Layer Security \(TLS\) protocol\. For more information, see [Transport security in AWS IoT](https://docs.aws.amazon.com/iot/latest/developerguide/transport-security.html)\.