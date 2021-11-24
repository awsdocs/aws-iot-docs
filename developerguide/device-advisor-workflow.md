# Device Advisor workflow<a name="device-advisor-workflow"></a>

This tutorial provides instructions on how to create a custom test suite and run tests against the device you want to test in the console\. After the tests are complete, you can view the test results and detailed logs\.

**Topics**
+ [Prerequisites](#device-advisor-workflow-prereqs)
+ [Create a test suite definition](#device-advisor-workflow-create-suite-definition)
+ [Get a test suite definition](#device-advisor-workflow-describe-suite-run)
+ [Get a test endpoint](#device-advisor-workflow-get-test-endpoint)
+ [Start a test suite run](#device-advisor-workflow-start-suite-run)
+ [Get a test suite run](#device-advisor-workflow-describe-suite)
+ [Stop a test suite run](#device-advisor-workflow-stop-suite-run)
+ [Get a qualification report for a successful qualification test suite run](#device-advisor-workflow-qualification-report)

## Prerequisites<a name="device-advisor-workflow-prereqs"></a>

To complete this tutorial, complete the steps outlined in [Setting up](device-advisor-setting-up.md)\.

## Create a test suite definition<a name="device-advisor-workflow-create-suite-definition"></a>

First, [install an AWS SDK](https://docs.aws.amazon.com/iot/latest/developerguide/iot-connect-service.html#iot-service-sdks)\.

### `rootGroup` syntax<a name="rootGroup"></a>

A root group is a JSON string that specifies which test cases are included in your test suite as well as any necessary configurations for those test cases\. Use the root group to structure and order your test suite in any way you like\. The hierarchy of a test suite is: 

```
test suite → test group(s) → test case(s)
```

A test suite must have at least one test group, and each test group must have at least one test case\. Device Advisor runs tests in the order in which you define the test groups and test cases\.

Each root group follows this basic structure:

```
{
    "configuration": {  // for all tests in the test suite
        "": ""
    }
    "tests": [{
        "name": ""
        "configuration": {  // for all sub-groups in this test group 
            "": ""
        },
        "tests": [{
            "name": ""
            "configuration": {  // for all test cases in this test group 
                "": ""
            },
            "test": {
                "id": ""  
                "version": ""
            }
        }]
    }]
}
```

A block that contains a `"name"`, `"configuration"`, and `"tests"` is referred to as a "group definition"\. A block that contains a `"name"`, `"configuration"`, and `"test"` is referred to as a "test case definition"\. Each `"test"` block that contains an `"id"` and `"version"` is referred to as a "test case"\.

For information on how to fill in the `"id"` and `"version"` fields for each test case \(`"test"` block\), see [Device Advisor test cases](device-advisor-tests.md)\. That section also contains information on the available `"configuration"` settings\.

The following block is an example of a root group configuration that specifies the "MQTT Connect Happy Case" and "MQTT Connect Exponential Backoff Retries" test cases, along with descriptions of the configuration fields\.

```
{
    "configuration": {},  // Suite-level configuration
    "tests": [            // Group definitions should be provided here
      {
        "name": "My_MQTT_Connect_Group",  // Group definition name
        "configuration": {}               // Group definition-level configuration,
        "tests": [                        // Test case definitions should be provided here
        {
            "name": "My_MQTT_Connect_Happy_Case",  // Test case definition name
            "configuration": {
                "EXECUTION_TIMEOUT": 300        // Test case definition-level configuration, in seconds
            }, 
            "test": {
                "id": "MQTT_Connect",              // test case id
                "version": "0.0.0"                 // test case version
            }
        },
        {
            "name": "My_MQTT_Connect_Jitter_Backoff_Retries",  // Test case definition name
            "configuration": {
                "EXECUTION_TIMEOUT": 600                 // Test case definition-level configuration,  in seconds
            },
            "test": {
                "id": "MQTT_Connect_Jitter_Backoff_Retries",  // test case id
                "version": "0.0.0"                                 // test case version
            }
        }]
    }]
}
```

You must supply the root group configuration when you create your test suite definition\. Save the `suiteDefinitionId` that is returned in the response object\. This ID is used to retrieve your test suite definition information and to run your test suite\.

Here is a Java SDK example:

```
response = iotDeviceAdvisorClient.createSuiteDefinition(
        CreateSuiteDefinitionRequest.builder()
            .suiteDefinitionConfiguration(SuiteDefinitionConfiguration.builder()
                .suiteDefinitionName("your-suite-definition-name")
                .devices(
                    DeviceUnderTest.builder()
                        .thingArn("your-test-device-thing-arn")
                        .certificateArn("your-test-device-certificate-arn")
                        .build()
                )
                .rootGroup("your-root-group-configuration")
                .devicePermissionRoleArn("your-device-permission-role-arn")
                .build()
            )
            .build()
)
```

## Get a test suite definition<a name="device-advisor-workflow-describe-suite-run"></a>

After you start a test suite run, you can check its progress and its results with the `GetSuiteRun` API\.

SDK example:

```
// Using the SDK, call the GetSuiteRun API.

response = iotDeviceAdvisorClient.GetSuiteRun(
GetSuiteRunRequest.builder()
    .suiteDefinitionId("your-suite-definition-id")
    .suiteRunId("your-suite-run-id")
.build())
```

## Get a test endpoint<a name="device-advisor-workflow-get-test-endpoint"></a>

You can use `GetTestEndpoint` API to get the test endpoint used by your device\. While choosing the endpoint, select the endpoint that best fits the situation\. To simultaneously run multiple test suites, use Device\-level endpoint by providing a `thing ARN` or a `certificate ARN`\. To run a single test suite, choose the Account\-level endpoint by providing no arguments\. 

SDK example:

```
response = iotDeviceAdvisorClient.getEndpoint(GetEndpointRequest.builder()
.certificateArn("your-test-device-certificate-arn")
.thingArn("your-test-device-thing-arn")
.build())
```

## Start a test suite run<a name="device-advisor-workflow-start-suite-run"></a>

After you've successfully created a test suite definition and configured your test device to connect to your Device Advisor test endpoint, run your test suite with the `StartSuiteRun` API\. Use either `certificateArn` or `thingArn` to run the test suite\. If both are configured, the certificate will be used if it belongs to the thing\.

For `.parallelRun()`, use `true` if you use Device\-level endpoint to run multiple test suites in parallel using one AWS account\.

SDK example:

```
response = iotDeviceAdvisorClient.startSuiteRun(StartSuiteRunRequest.builder()
.suiteDefinitionId("your-suite-definition-id")
.suiteRunConfiguration(SuiteRunConfiguration.builder()
    .primaryDevice(DeviceUnderTest.builder()
        .certificateArn("your-test-device-certificate-arn")
        .thingArn("your-test-device-thing-arn")
        .build())
    .parallelRun(true | false)    
    .build())
.build())
```

Save the `suiteRunId` that is returned in the response\. You will use this to retrieve the results of this test suite run\.

## Get a test suite run<a name="device-advisor-workflow-describe-suite"></a>

After you create your test suite definition, you receive the `suiteDefinitionId` in the response object of the `CreateSuiteDefinition` API\.

You may see that there are new `id` fields within each of the group and test case definitions in the root group that is returned\. This is expected; you can use these IDs to run a subset of your test suite definition\.

Java SDK example: 

```
response = iotDeviceAdvisorClient.GetSuiteDefinition(
    GetSuiteDefinitionRequest.builder()
        .suiteDefinitionId("your-suite-definition-id")
        .build()
)
```

## Stop a test suite run<a name="device-advisor-workflow-stop-suite-run"></a>

To stop a test suite run that is still in progress, you can call the `StopSuiteRun` API\. After you call the `StopSuiteRun` API, the service will start the cleanup process\. While the service is running the cleanup process, the test suite run status is updated to `Stopping`\. The cleanup process will take several minutes, and once the process is complete, the test suite run status is updated to `Stopped`\. After a test run has completely stopped you will be able to start another test suite run\. You can periodically check the suite run status using the `GetSuiteRun` API as shown in the previous section\. 

SDK example:

```
// Using the SDK, call the StopSuiteRun API.

response = iotDeviceAdvisorClient.StopSuiteRun(
StopSuiteRun.builder()
    .suiteDefinitionId("your-suite-definition-id")
    .suiteRunId("your-suite-run-id")
.build())
```

## Get a qualification report for a successful qualification test suite run<a name="device-advisor-workflow-qualification-report"></a>

If you run a qualification test suite that completes, you can retrieve a qualification report by using the `GetSuiteRunReport` API\. You can use this qualification report to qualify your device with the AWS IoT Core qualification program\. To determine whether your test suite is a qualification test suite, check whether the `intendedForQualification` parameter is set to `true`\. After you call the `GetSuiteRunReport` API, the download URL returned is available for you to download for 90 seconds\. If more than 90 seconds elapse from the previous time you called the `GetSuiteRunReport` API, call the API again to retrieve a valid URL\. 

SDK example:

```
// Using the SDK, call the getSuiteRunReport API. 

response = iotDeviceAdvisorClient.getSuiteRunReport( 
    GetSuiteRunReportRequest.builder() 
        .suiteDefinitionId("your-suite-definition-id")
        .suiteRunId("your-suite-run-id")
        .build()
)
```