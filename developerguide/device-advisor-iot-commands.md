# Device Advisor commands<a name="device-advisor-iot-commands"></a>


|  | 
| --- |
| Device Advisor is in preview and is subject to change\. | 

**Topics**
+ [CreateSuiteDefinition](#api-iot-CreateSuiteDefinition)
+ [DeleteSuiteDefinition](#api-iot-DeleteSuiteDefinition)
+ [GetSuiteDefinition](#api-iot-GetSuiteDefinition)
+ [GetSuiteRun](#api-iot-GetSuiteRun)
+ [GetSuiteRunReport](#api-iot-GetSuiteRunReport)
+ [ListSuiteDefinitions](#api-iot-ListSuiteDefinitions)
+ [ListSuiteRuns](#api-iot-ListSuiteRuns)
+ [ListTagsForResource](#api-iot-ListTagsForResource)
+ [ListTestCases](#api-iot-ListTestCases)
+ [StartSuiteRun](#api-iot-StartSuiteRun)
+ [TagResource](#api-iot-TagResource)
+ [UntagResource](#api-iot-UntagResource)
+ [UpdateSuiteDefinition](#api-iot-UpdateSuiteDefinition)

## CreateSuiteDefinition<a name="api-iot-CreateSuiteDefinition"></a>

Creates a Device Advisor test suite\.

 **Synopsis**

```
aws iotdeviceadvisor create-suite-definition \
    [--suite-definition-configuration <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "suiteDefinitionConfiguration": {
    "suiteDefinitionName": "string",
    "devices": [
      {
        "thingArn": "string",
        "certificateArn": "string"
      }
    ],
    "intendedForQualification": "boolean",
    "rootGroup": "string",
    "devicePermissionRoleArn": "string"
  }
}
```


**`cli-input-json` fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `suiteDefinitionConfiguration`  |  Configuration values that apply to all test groups and cases in the test suite\. SuiteDefinitionConfiguration  | 
|   `suiteDefinitionName`  |  The name of the test suite\. string  length\- max:256 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  | 
|   `devices`  |  The devices on which the test suite may be run\. You must set up each device to use the Device Advisor test endpoint for your account\. list  member: DeviceUnderTest  | 
|   `thingArn`  |  The ARN of the AWS IoT thing \(device\) on which the test is run\. string  length\- max:2048 min:20  pattern: arn:\*  | 
|   `certificateArn`  |  The ARN of the certificate associated with the AWS IoT thing \(device\) on which the test is run\. string  length\- max:2048 min:20  pattern: arn:\*  | 
|   `intendedForQualification`  |  True if the test suite is run to qualify the device for the AWS Device Qualification Program\. boolean  | 
|   `rootGroup`  |  The top\-most group of test groups and cases in the test suite\. string  length\- max:2048 min:1  pattern: \\S\+  | 
|   `devicePermissionRoleArn`  |  The ARN of the role that grants permission to Device Advisor to access the devices on which the test suite is run\. string  length\- max:2048 min:20  pattern: arn:\*  | 

Output

```
{
  "suiteDefinitionId": "string",
  "suiteDefinitionArn": "string",
  "suiteDefinitionName": "string",
  "createdAt": "timestamp"
}
```


**CLI output fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `suiteDefinitionId`  |  The ID of the test suite that was created\. string  length\- max:36 min:36  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 
|   `suiteDefinitionArn`  |  The ARN of the test suite that was created\. string  length\- max:2048 min:20  pattern: arn:\*  | 
|   `suiteDefinitionName`  |  The name you gave the test suite\. string  length\- max:256 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  | 
|   `createdAt`  |  The time the test suite was created\. timestamp  | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`InternalFailureException`  
An internal failure occurred\.

## DeleteSuiteDefinition<a name="api-iot-DeleteSuiteDefinition"></a>

Deletes a Device Advisor test suite, including all versions and all resources associated with test suite runs\. CloudWatch logs are not deleted\.

 **Synopsis**

```
aws iotdeviceadvisor  delete-suite-definition \
    --suite-definition-id <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "suiteDefinitionId": "string"
}
```


**`cli-input-json` fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `suiteDefinitionId`  |  The ID of the test suite\. string  length\- max:36 min:36  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 

Output

None

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`InternalFailureException`  
An internal failure occurred\.

## GetSuiteDefinition<a name="api-iot-GetSuiteDefinition"></a>

Gets information about a Device Advisor test suite\.

 **Synopsis**

```
aws iotdeviceadvisor  get-suite-definition \
    --suite-definition-id <value> \
    [--suite-definition-version <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "suiteDefinitionId": "string",
  "suiteDefinitionVersion": "string"
}
```


**`cli-input-json` fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `suiteDefinitionId`  |  The ID of the test suite\. string  length\- max:36 min:36  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 
|   `suiteDefinitionVersion`  |  The version of the test suite\. string  length\- max:255 min:2  pattern: v\[0\-9\]\+  | 

Output

```
{
  "suiteDefinitionId": "string",
  "suiteDefinitionVersion": "string",
  "suiteDefinitionConfiguration": {
    "suiteDefinitionName": "string",
    "devices": [
      {
        "thingArn": "string",
        "certificateArn": "string"
      }
    ],
    "intendedForQualification": "boolean",
    "rootGroup": "string",
    "devicePermissionRoleArn": "string"
  },
  "createdAt": "timestamp",
  "lastModifiedAt": "timestamp"
}
```


**CLI output fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `suiteDefinitionId`  |  The ID of the test suite\. string  length\- max:36 min:36  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 
|   `suiteDefinitionVersion`  |  The version of the test suite\. string  length\- max:255 min:2  pattern: v\[0\-9\]\+  | 
|   `suiteDefinitionConfiguration`  |  Configuration values for all test groups and test cases in the test suite\. SuiteDefinitionConfiguration  | 
|   `suiteDefinitionName`  |  The name of the test suite\. string  length\- max:256 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  | 
|   `devices`  |  The devices on which the test suite may be run\. You must set up each device to use the Device Advisor test endpoint for your account\. list  member: DeviceUnderTest  | 
|   `thingArn`  |  The ARN of the AWS IoT thing \(device\) on which the test is run\. string  length\- max:2048 min:20  pattern: arn:\*  | 
|   `certificateArn`  |  The ARN of the certificate associated with the AWS IoT thing \(device\) on which the test is run\. string  length\- max:2048 min:20  pattern: arn:\*  | 
|   `intendedForQualification`  |  True if the test suite is run to qualify the device for the AWS Device Qualification Program\. boolean  | 
|   `rootGroup`  |  The top\-most group of test groups and cases in the test suite\. string  length\- max:2048 min:1  pattern: \\S\+  | 
|   `devicePermissionRoleArn`  |  The ARN of the role that grants permission to Device Advisor to access the devices on which the test suite is run\. string  length\- max:2048 min:20  pattern: arn:\*  | 
|   `createdAt`  |  The time the test suite was created\. timestamp  | 
|   `lastModifiedAt`  |  The time the test suite was last modified\. timestamp  | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`InternalFailureException`  
An internal failure occurred\.

`ResourceNotFoundException`  
A resource could not be found\.

## GetSuiteRun<a name="api-iot-GetSuiteRun"></a>

Gets information about a Device Advisor test suite run\.

 **Synopsis**

```
aws iotdeviceadvisor  get-suite-run \
    --suite-definition-id <value> \
    --suite-run-id <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "suiteDefinitionId": "string",
  "suiteRunId": "string"
}
```


**`cli-input-json` fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `suiteDefinitionId`  |  The ID of the test suite\. string  length\- max:36 min:36  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 
|   `suiteRunId`  |  The ID of the test suite run\. string  length\- max:36 min:36  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 

Output

```
{
  "suiteDefinitionId": "string",
  "suiteDefinitionVersion": "string",
  "suiteRunId": "string",
  "suiteRunConfiguration": {
    "primaryDevice": {
      "thingArn": "string",
      "certificateArn": "string"
    },
    "secondaryDevice": {
      "thingArn": "string",
      "certificateArn": "string"
    },
    "selectedTestList": [
      "string"
    ]
  },
  "testCaseRuns": [
    {
      "testCaseDefinitionId": "string",
      "testCaseDefinitionName": "string",
      "status": "string",
      "startTime": "timestamp",
      "endTime": "timestamp",
      "logUrl": "string",
      "warnings": "string",
      "failure": "string"
    }
  ],
  "startTime": "timestamp",
  "endTime": "timestamp",
  "status": "string",
  "errorReason": "string",
}
```


**CLI output fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `suiteDefinitionId`  |  The ID of the test suite\. string  length\- max:36 min:36  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 
|   `suiteDefinitionVersion`  |  The version of the test suite\. string  length\- max:255 min:2  pattern: v\[0\-9\]\+  | 
|   `suiteRunId`  |  The ID of the test suite run\. string  length\- max:36 min:36  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 
|   `suiteRunConfiguration`  |  The configuration values applied to all test groups and test cases in this test suite during this run\. SuiteRunConfiguration  | 
|   `primaryDevice`  |  The device on which the tests in the test suite are run\. At this time, a Device Advisor test suite targets primarily one device, which is specified by this parameter\. Some tests do compare the behavior of two devices, so if you include such a test, you must also specify a `secondaryDevice`\. DeviceUnderTest  | 
|   `thingArn`  |  The ARN of the AWS IoT thing \(device\) on which the test is run\. string  length\- max:2048 min:20  pattern: arn:\*  | 
|   `certificateArn`  |  The ARN of the certificate associated with the AWS IoT thing \(device\) on which the test is run\. string  length\- max:2048 min:20  pattern: arn:\*  | 
|   `secondaryDevice`  |  If a test compares the behavior of two devices \(for example, "Device re\-connect with Jitter \(multiple device with deterministic jitter\)"\), this parameter specifies the second device used for the test\. DeviceUnderTest  | 
|   `thingArn`  |  The ARN of the AWS IoT thing \(device\) on which the test is run\. string  length\- max:2048 min:20  pattern: arn:\*  | 
|   `certificateArn`  |  The ARN of the certificate associated with the AWS IoT thing \(device\) on which the test is run\. string  length\- max:2048 min:20  pattern: arn:\*  | 
|   `selectedTestList`  |  \[optional\] The ID of the specific test group or test case that will be run\. list  member: UUID  | 
|   `testCaseRuns`  |  The results of each test case run\. list  member: TestCaseRun  | 
|   `testCaseDefinitionId`  |  The ID of the test case\. string  length\- max:36 min:36  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 
|   `testCaseDefinitionName`  |  The name of the test case\. string  | 
|   `status`  |  The status of the test case run\. string  | 
|   `startTime`  |  The time the test case run started\. timestamp  | 
|   `endTime`  |  The time the test case run ended\. timestamp  | 
|   `logUrl`  |  The URL of the CloudWatch logs that contain the test case run results\. string  | 
|   `warnings`  |  Any warnings generated by the test case run\. string  | 
|   `failure`  |  Any failures generated by the test case run\. string  | 
|   `startTime`  |  The time this test suite run started\. timestamp  | 
|   `endTime`  |  The time this test suite run ended\. timestamp  | 
|   `status`  |  The status of this test suite run\. string  | 
|   `errorReason`  |  The reason an error \(if any\) occurred during this test suite run\. string  | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`InternalFailureException`  
An internal failure occurred\.

`ResourceNotFoundException`  
A resource could not be found\.

## GetSuiteRunReport<a name="api-iot-GetSuiteRunReport"></a>

Gets suite run report\.

 **Synopsis**

```
aws iotdeviceadvisor  get-suite-run-report \
    --suite-definition-id <value> \
    --suite-run-id <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "suiteDefinitionId": "string",
  "suiteRunId": "string"
}
```


**`cli-input-json` fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `suiteDefinitionId`  |  The ID of the test suite\. string  length\- max:36 min:36  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 
|   `suiteRunId`  |  The ID of the test suite run\. string  length\- max:36 min:36  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 

Output

```
{
    "qualificationReportDownloadUrl": "string"
}
```


**CLI output fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `qualificationReportDownloadUrl`  |  Gets the download URL of the qualification report\. string  length\- max:36 min:36  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 

 **Errors**

`ValidationException`  
Invalid request exception\.

`InternalServerException`  
An internal failure occurred\.

`ResourceNotFoundException`  
A resource could not be found\.

## ListSuiteDefinitions<a name="api-iot-ListSuiteDefinitions"></a>

Lists the Device Advisor test suites you have created\.

 **Synopsis**

```
aws iotdeviceadvisor  list-suite-definitions \
    [--max-results <value>] \
    [--next-token <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "maxResults": "integer",
  "nextToken": "string"
}
```


**`cli-input-json` fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `maxResults`  |  The maximum number of results to return at one time\. integer  range\- max:50 min:1  | 
|   `nextToken`  |  The token for the next set of results\. string  length\- max:2000  pattern: \\S\+  | 

Output

```
{
  "suiteDefinitionInformationList": [
    {
      "suiteDefinitionId": "string",
      "suiteDefinitionName": "string",
      "defaultDevicesUnderTest": [
        {
          "thingArn": "string",
          "certificateArn": "string"
        }
      ],
      "intendedForQualification": "boolean",
      "createdAt": "timestamp"
    }
  ],
  "nextToken": "string"
}
```


**CLI output fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `suiteDefinitionInformationList`  |  Information about the test suites\. list  member: SuiteDefinitionInformation  | 
|   `suiteDefinitionId`  |  The ID of the test suite\. string  length\- max:36 min:36  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 
|   `suiteDefinitionName`  |  The name of the test suite\. string  length\- max:256 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  | 
|   `defaultDevicesUnderTest`  |  The devices on which the test suite is run\. list  member: DeviceUnderTest  | 
|   `thingArn`  |  The ARN of the AWS IoT thing \(device\) on which the test is run\. string  length\- max:2048 min:20  pattern: arn:\*  | 
|   `certificateArn`  |  The ARN of the certificate associated with the AWS IoT thing \(device\) on which the test is run\. string  length\- max:2048 min:20  pattern: arn:\*  | 
|   `intendedForQualification`  |  True if the test suite is intended to qualify the devices tested for the AWS Device Partner Program\. boolean  | 
|   `createdAt`  |  The time the test suite was created\. timestamp  | 
|   `nextToken`  |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\. string  length\- max:2000  pattern: \\S\+  | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`InternalFailureException`  
An internal failure occurred\.

## ListSuiteRuns<a name="api-iot-ListSuiteRuns"></a>

Lists the runs of the specified Device Advisor test suite\.

You can list all runs of the test suite, or the runs of a specific test suite, or all runs of a specific test suite with a specific version\.

 **Synopsis**

```
aws iotdeviceadvisor  list-suite-runs \
    [--suite-definition-id <value>] \
    [--suite-definition-version <value>] \
    [--max-results <value>] \
    [--next-token <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "suiteDefinitionId": "string",
  "suiteDefinitionVersion": "string",
  "maxResults": "integer",
  "nextToken": "string"
}
```


**`cli-input-json` fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `suiteDefinitionId`  |  The ID of the test suite for which runs will be listed\. string  length\- max:36 min:36  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 
|   `suiteDefinitionVersion`  |  The version of the test suite for which runs will be listed\. string  length\- max:255 min:2  pattern: v\[0\-9\]\+  | 
|   `maxResults`  |  The maximum number of results to return at one time\. integer  range\- max:50 min:1  | 
|   `nextToken`  |  The token for the next set of results\. string  length\- max:2000  pattern: \\S\+  | 

Output

```
{
  "suiteRunsList": [
    {
      "suiteDefinitionId": "string",
      "suiteDefinitionVersion": "string",
      "suiteDefinitionName": "string",
      "suiteRunId": "string",
      "createdAt": "timestamp",
      "startedAt": "timestamp",
      "endAt": "timestamp",
      "status": "string",
      "passed": "integer",
      "failed": "integer"
    }
  ],
  "nextToken": "string"
}
```


**CLI output fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `suiteRunsList`  |  Information about the test suite runs\. list  member: SuiteRunInformation  | 
|   `suiteDefinitionId`  |  The test suite ID\. string  length\- max:36 min:36  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 
|   `suiteDefinitionVersion`  |  The test suite version\. string  length\- max:255 min:2  pattern: v\[0\-9\]\+  | 
|   `suiteDefinitionName`  |  The name you gave the test suite\. string  length\- max:256 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  | 
|   `suiteRunId`  |  The ID of the test suite run\. string  length\- max:36 min:36  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 
|   `createdAt`  |  The time the test suite run was created\. timestamp  | 
|   `startedAt`  |  The time the test suite run started\. timestamp  | 
|   `endAt`  |  The time the test suite run ended\. timestamp  | 
|   `status`  |  The status of the test suite run\. string  | 
|   `passed`  |  The number of test cases that passed\. integer  range\- max:500 min:0  | 
|   `failed`  |  The number of test cases that failed\. integer  range\- max:500 min:0  | 
|   `nextToken`  |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\. string  length\- max:2000  pattern: \\S\+  | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`InternalFailureException`  
An internal failure occurred\.

## ListTagsForResource<a name="api-iot-ListTagsForResource"></a>

Lists all tags for a given Device Advisor resource\.

 **Synopsis**

```
aws iotdeviceadvisor  list-tags-for-resource \
    [--resource-arn <value>] \
```

 `cli-input-json` format

```
{
  "resourceArn": "string",
}
```


**`cli-input-json` fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `resourceArn`  |  The ARN of the IoT Device Advisor resource\. string  length\- max:2048 min:20  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 

Output

```
{
    "tags": "string"
}
```


**CLI output fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `tags`  |  The tags for a given resource\. string  | 

 **Errors**

`ValidationException`  
Invalid request exception\.

`InternalServerException`  
An internal failure occurred\.

`ResourceNotFoundException`  
A resource could not be found\.

## ListTestCases<a name="api-iot-ListTestCases"></a>

This API is deprecated\. For more information on available test cases, see [Device Advisor test cases](device-advisor-tests.md)\.

 **Synopsis**

```
aws iotdeviceadvisor  list-test-cases \
    [--intended-for-qualification <value>] \    
    [--max-results <value>] \
    [--next-token <value>] \
```

 `cli-input-json` format

```
{
  "intendedForQualification": "string",
  "maxResults": integer,
  "nextToken": "string"
}
```


**`cli-input-json` fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `intendedForQualification`  |  Lists all the qualification test cases in the test suite\. string  length\- max:2048 min:20  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 
|   `maxResults`  |  The maximum number of results to return at one time\. integer  range\- max:50 min:1  | 
|   `nextToken`  |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\. string  length\- max:2000  pattern: \\S\+  | 

Output

```
{
   "categories": [ 
      { 
         "name": "string",
         "tests": [ 
            { 
               "configuration": { 
                  "string" : "string" 
               },
               "name": "string",
               "test": { 
                  "id": "string",
                  "testCaseVersion": "string"
               }
            }
         ]
      }
   ],
   "groupConfiguration": { 
      "string" : "string" 
   },
   "nextToken": "string",
   "rootGroupConfiguration": { 
      "string" : "string" 
   }
}
```


**CLI output fields**  

|  Name  |  Description  | 
| --- | --- | 
| categories |  This group of test cases belong to this category\. array  | 
| groupConfiguration |  Available configurations for test groups and cases in the test suite\.  | 
|   `nextToken`  |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\. string  length\- max:2000  pattern: \\S\+  | 
| rootGroupConfiguration |  Available configurations for the top\-most group of test groups and cases in the test suite\.  | 

 **Errors**

`InternalServerException`  
An internal failure occurred\.

## StartSuiteRun<a name="api-iot-StartSuiteRun"></a>

Starts a Device Advisor test suite run\.

 **Synopsis**

```
aws iotdeviceadvisor  start-suite-run \
    --suite-definition-id <value> \
    [--suite-definition-version <value>] \
    [--suite-run-configuration <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "suiteDefinitionId": "string",
  "suiteDefinitionVersion": "string",
  "suiteRunConfiguration": {
    "primaryDevice": {
      "thingArn": "string",
      "certificateArn": "string"
    },
    "secondaryDevice": {
      "thingArn": "string",
      "certificateArn": "string"
    },
    "selectedTestList": [
      "string"
    ]
  }
}
```


**`cli-input-json` fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `suiteDefinitionId`  |  The ID of the test suite\. string  length\- max:36 min:36  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 
|   `suiteDefinitionVersion`  |  The version of the test suite\. \(Versions are incremented whenever a test suite is updated\.\) string  length\- max:255 min:2  pattern: v\[0\-9\]\+  | 
|   `suiteRunConfiguration`  |  Configuration values for all test groups and test cases in the test suite\. SuiteRunConfiguration  | 
|   `primaryDevice`  |  The device on which the tests in the test suite are run\. \[At this time, a Device Advisor test suite targets primarily one device, which is specified by this parameter\. Some tests do compare the behavior of two devices, so if you include such a test, you must also specify a `secondaryDevice`\.\] DeviceUnderTest  | 
|   `thingArn`  |  The ARN of the AWS IoT thing \(device\) on which the test is run\. string  length\- max:2048 min:20  pattern: arn:\*  | 
|   `certificateArn`  |  The ARN of the certificate associated with the AWS IoT thing \(device\) on which the test is run\. string  length\- max:2048 min:20  pattern: arn:\*  | 
|   `secondaryDevice`  |  If a test compares the behavior of two devices \(for example, "Device re\-connect with Jitter \(multiple device w/ deterministic jitter\)"\), this parameter specifies the second device used for the test\. DeviceUnderTest  | 
|   `thingArn`  |  The ARN of the AWS IoT thing \(device\) on which the test is run\. string  length\- max:2048 min:20  pattern: arn:\*  | 
|   `certificateArn`  |  The ARN of the certificate associated with the AWS IoT thing \(device\) on which the test is run\. string  length\- max:2048 min:20  pattern: arn:\*  | 
|   `selectedTestList`  |  \[optional\] The ID of the specific test group or test case that will be run\. list  member: UUID  | 

Output

```
{
  "suiteRunId": "string",
  "suiteRunArn": "string",
  "createdAt": "timestamp"
}
```


**CLI output fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `suiteRunId`  |  The ID of the test suite run\. string  length\- max:36 min:36  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 
|   `suiteRunArn`  |  The ARN of the test suite run\. string  length\- max:2048 min:20  pattern: arn:\*  | 
|   `createdAt`  |  The time and date when the test suite was created\. timestamp  | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`InternalFailureException`  
An internal failure occurred\.

`ActiveRunningException`  
There is an active suite running\.

## TagResource<a name="api-iot-TagResource"></a>

Adds to and modifies existing tags of an IoT Device Advisor resource\.

 **Synopsis**

```
aws iotdeviceadvisor  tag-resource \
    --resource-arn <value> \
```

 `cli-input-json` format

```
{
  "resourceArn": "string",
}
```


**`cli-input-json` fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `resourceArn`  |  The ARN of the IoT Device Advisor resource\. string  length\- max:2048 min:20  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 

Output

None\.


**CLI output fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `tags`  |  The tags for a given resource\. string  | 

 **Errors**

`ValidationException`  
Invalid request exception\.

`InternalServerException`  
An internal failure occurred\.

`ResourceNotFoundException`  
A resource could not be found\.

## UntagResource<a name="api-iot-UntagResource"></a>

Removes tags from an IoT Device Advisor resource\.

 **Synopsis**

```
aws iotdeviceadvisor  untag-resource \
    --resource-arn <value> \
    --tag-keys <value> \
```

 `cli-input-json` format

```
{
  "resourceArn": "string",
  "tagKeys": integer,
}
```


**`cli-input-json` fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `resourceArn`  |  The ARN of the IoT Device Advisor resource\. string  length\- max:2048 min:20  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 
| tagKeys |  List of tag keys to remove from the IoT Device Advisor resource\. array  | 

Output

None\.

 **Errors**

`ValidationException`  
Invalid request exception\.

`InternalServerException`  
An internal failure occurred\.

`ResourceNotFoundException`  
A resource could not be found\.

## UpdateSuiteDefinition<a name="api-iot-UpdateSuiteDefinition"></a>

Updates a Device Advisor test suite\.

**Synopsis**

```
aws iotdeviceadvisor update-suite-definition \
    [--suite-definition-configuration <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
    "suiteDefinitionConfiguration": {
        "suiteDefinitionName": "string",
        "devices": [
            {
                "thingArn": "string",
                "certificateArn": "string"
            }
        ],
        "rootGroup": "string",
        "devicePermissionRoleArn": "string"
    }
}
```


**`cli-input-json` fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `suiteDefinitionConfiguration`  |  Configuration values that apply to all test groups and cases in the test suite\. SuiteDefinitionConfiguration  | 
|   `suiteDefinitionName`  |  The name of the test suite\. string  length\- max:256 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  | 
|   `devices`  |  The devices on which the test suite can be run\. You must set up each device to use the Device Advisor test endpoint for your account\. list  member: DeviceUnderTest  | 
|   `thingArn`  |  The ARN of the AWS IoT thing \(device\) on which the test is run\. string  length\- max:2048 min:20  pattern: arn:\*  | 
|   `certificateArn`  |  The ARN of the certificate associated with the AWS IoT thing \(device\) on which the test is run\. string  length\- max:2048 min:20  pattern: arn:\*  | 
|   `rootGroup`  |  The top\-most group of test groups and cases in the test suite\. string  length\- max:2048 min:1  pattern: \\S\+  | 
|   `devicePermissionRoleArn`  |  The ARN of the role that grants permission to Device Advisor to access the devices on which the test suite is run\. string  length\- max:2048 min:20  pattern: arn:\*  | 

Output

```
{
   "suiteDefinitionId": "string",
   "suiteDefinitionArn": "string",
   "suiteDefinitionName": "string",
   "suiteDefinitionVersion": "string",
   "createdAt": "string", 
   "lastUpdatedAt": "string"
}
```


**CLI output fields**  

|  Name  |  Description  | 
| --- | --- | 
|   `suiteDefinitionId`  |  The ID of the test suite that was created\. string  length\- max:36 min:36  pattern: \[a\-f0\-9\]\{8\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{4\}\-\[a\-f0\-9\]\{12\}  | 
|   `suiteDefinitionArn`  |  The ARN of the test suite that was created\. string  length\- max:2048 min:20  pattern: arn:\*  | 
|   `suiteDefinitionName`  |  The name you gave the test suite\. string  length\- max:256 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  | 
|   `createdAt`  |  The time the test suite was created\. timestamp  | 
|   `lastUpdatedAt`  |  The time the test suite was last updated\. timestamp  | 

 **Errors**

`InvalidRequestException`  
The request was invalid\.

`InternalFailureException`  
An internal failure occurred\.