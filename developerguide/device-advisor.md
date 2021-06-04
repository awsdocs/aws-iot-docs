# Device Advisor<a name="device-advisor"></a>

[Device Advisor](https://aws.amazon.com/iot-core/features/) is a cloud\-based, fully managed test capability for validating IoT devices during device software development\. Device Advisor provides pre\-built tests that you can use to validate IoT devices for reliable and secure connectivity with AWS IoT Core, before deploying devices to production\. Device Advisor’s pre\-built tests help you validate your device software against best practices for usage of [TLS](https://docs.aws.amazon.com/iot/latest/developerguide/protocols.html), [MQTT](https://docs.aws.amazon.com/iot/latest/developerguide/protocols.html), [Device Shadow](https://docs.aws.amazon.com/iot/latest/developerguide/iot-device-shadows.html), and [IoT Jobs](https://docs.aws.amazon.com/iot/latest/developerguide/iot-jobs.html)\. You can also download signed qualification reports to submit to the AWS Partner Network to get your device qualified for the [AWS Partner Device Catalog](https://devices.amazonaws.com/) without the need to send your device in and wait for it to be tested\.

**Note**  
Device Advisor is supported in us\-east\-1, us\-west\-2, ap\-northeast\-1, and eu\-west\-1 regions\. Device Advisor supports MQTT with X509 client certificates\.

**Topics**
+ [Setting up](device-advisor-setting-up.md)
+ [Getting started with Device Advisor in the console](da-console-guide.md)
+ [Device Advisor workflow](device-advisor-workflow.md)
+ [Device Advisor detailed console workflow](device-advisor-console-tutorial.md)
+ [Device Advisor test cases](device-advisor-tests.md)

Any device that has been built to connect to AWS IoT Core can take advantage of Device Advisor\. You can access Device Advisor from the [AWS IoT console](https://us-east-1.console.aws.amazon.com/iot/home?region=us-east-1#/deviceadvisor/intro), or by using the AWS CLI or SDK\. When you're ready to test your device, register it with AWS IoT Core and configure the device software with the Device Advisor endpoint\. Then choose the prebuilt tests, configure them, run the tests on your device, and get the test results along with detailed logs or a qualification report\.

Device Advisor is a test endpoint in the AWS cloud\. You can test your devices by configuring them to connect to the test endpoint provided by the Device Advisor\. After a device is configured to connect to the test endpoint, you can visit the Device Advisor’s console or use the AWS SDK to choose the tests you want to run on your devices\. Device Advisor then manages the full lifecycle of a test, including the provisioning of resources, scheduling of the test process, managing the state machine, recording the device behavior, logging the results, and providing the final results in form of a test report\.