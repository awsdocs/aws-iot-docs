# Detect Commands<a name="DetectCommands"></a>

## AttachSecurityProfile<a name="dd-api-iot-AttachSecurityProfile"></a>

Associates an AWS IoT Device Defender security profile with one of the following target types: 
+ All devices
+ All registered devices \(things in the AWS IoT registry\)
+ All unregistered devices
+ Devices in a thing group

Each target type can have up to five security profiles associated with it\.

 **Synopsis:**

```
aws iot  attach-security-profile \
    --security-profile-name <value> \
    --security-profile-target-arn <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "securityProfileName": "string",
  "securityProfileTargetArn": "string"
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  securityProfileName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The security profile that is attached\. | 
|  securityProfileTargetArn |  string |  The ARN of the target \(thing group\) to which the security profile is attached\. | 

To attach a security profile to a group of devices, you must specify the ARN of the thing group that contains them\. A thing group ARN has the following format: 

```
arn:aws:iot:<region>:<accountid>:thinggroup/<thing-group-name>
```

To attach a security profile to all of the registered things in an account \(ignoring unregistered things\), you must specify an ARN with the following format:

```
arn:aws:iot:<region>:<accountid>:all/registered-things
```

To attach a security profile to all unregistered things, you must specify an ARN with the following format:

```
arn:aws:iot:<region>:<accountid>:all/unregistered-things
```

To attach a security profile to all devices, you must specify an ARN with the following format: 

```
arn:aws:iot:<region>:<accountid>:all/things
```

Output:

None

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\.

`ResourceNotFoundException`  
The specified resource does not exist\.

`LimitExceededException`  
A limit has been exceeded\.

`VersionConflictException`  
An exception thrown when the version of a thing passed to a command is different from the version specified with the `--version` parameter\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## CreateSecurityProfile<a name="dd-api-iot-CreateSecurityProfile"></a>

Creates a Device Defender security profile\.

 **Synopsis:**

```
aws iot  create-security-profile \
    --security-profile-name <value> \
    [--security-profile-description <value>] \
    [--behaviors <value>] \
    [--alert-targets <value>] \
    [--additional-metrics-to-retain <value>] \
    [--tags <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "securityProfileName": "string",
  "securityProfileDescription": "string",
  "behaviors": [
    {
      "name": "string",
      "metric": "string",
      "criteria": {
        "comparisonOperator": "string",
        "value": {
          "count": "long",
          "cidrs": [
            "string"
          ],
          "ports": [
            "integer"
          ]
        },
        "durationSeconds": "integer",
        "consecutiveDatapointsToAlarm": "integer",
        "consecutiveDatapointsToClear": "integer",
        "statisticalThreshold": {
          "statistic": "string"
        }
      }
    }
  ],
  "alertTargets": {
    "string": {
      "alertTargetArn": "string",
      "roleArn": "string"
    }
  },
  "additionalMetricsToRetain": [
    "string"
  ],
  "tags": [
    {
      "Key": "string",
      "Value": "string"
    }
  ]
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  securityProfileName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name you are giving to the security profile\. | 
|  securityProfileDescription |  string  length\- max:1000  pattern: \[\\\\p\{Graph\} \]\*  |  A description of the security profile\. | 
|  behaviors |  list  member: Behavior  |  Specifies the behaviors that, when violated by a device \(thing\), cause an alert\. | 
|  name |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name you have given to the behavior\. | 
|  metric |  string |  What is measured by the behavior\. | 
|  criteria |  BehaviorCriteria |  The criteria that determine if a device is behaving normally in regard to the `metric`\.  | 
|  comparisonOperator |  string |  The operator that relates the thing measured \(`metric`\) to the criteria \(containing a `value` or `statisticalThreshold`\)\.  enum: less\-than \| less\-than\-equals \| greater\-than \| greater\-than\-equals \| in\-cidr\-set \| not\-in\-cidr\-set \| in\-port\-set \| not\-in\-port\-set  | 
|  value |  MetricValue |  The value to be compared with the `metric`\. | 
|  count |  long  range\- min:0  |  If the `comparisonOperator` calls for a numeric value, use this to specify that numeric value to be compared with the `metric`\.  | 
|  cidrs |  list  member: Cidr  |  If the `comparisonOperator` calls for a set of CIDRs, use this to specify that set to be compared with the `metric`\.  | 
|  ports |  list  member: Port  |  If the `comparisonOperator` calls for a set of ports, use this to specify that set to be compared with the `metric`\.  | 
|  durationSeconds |  integer |  Use this to specify the time duration over which the behavior is evaluated, for those criteria that have a time dimension \(for example, `NUM_MESSAGES_SENT`\)\. For a `statisticalThreshhold` metric comparison, measurements from all devices are accumulated over this time duration before being used to calculate percentiles, and later, measurements from an individual device are also accumulated over this time duration before being given a percentile rank\.  | 
|  consecutiveDatapointsToAlarm |  integer  range\- max:10 min:1  |  If a device is in violation of the behavior for the specified number of consecutive data points, an alarm occurs\. If not specified, the default is 1\.  | 
|  consecutiveDatapointsToClear |  integer  range\- max:10 min:1  |  If an alarm has occurred and the offending device is no longer in violation of the behavior for the specified number of consecutive data points, the alarm is cleared\. If not specified, the default is 1\.  | 
|  statisticalThreshold |  StatisticalThreshold |  A statistical ranking \(percentile\) that indicates a threshold value by which a behavior is determined to be in compliance or in violation of the behavior\.  | 
|  statistic |  string  pattern: \(p0\|p0\.1\|p0\.01\|p1\|p10\|p50\|p90\|p99\|p99\.9\|p99\.99\|p100\)  |  The percentile that resolves to a threshold value by which compliance with a behavior is determined\. Metrics are collected over the specified period \(`durationSeconds`\) from all reporting devices in your account and statistical ranks are calculated\. Then, the measurements from a device are collected over the same period\. If the accumulated measurements from the device fall above or below \(`comparisonOperator`\) the value associated with the percentile specified, then the device is considered to be in compliance with the behavior, otherwise a violation occurs\.  | 
|  alertTargets |  map |  Specifies the destinations to which alerts are sent\. \(Alerts are always sent to the console\.\) Alerts are generated when a device \(thing\) violates a behavior\.  | 
|  alertTargetArn |  string |  The ARN of the notification target to which alerts are sent\. | 
|  roleArn |  string  length\- max:2048 min:20  |  The ARN of the role that grants permission to send alerts to the notification target\.  | 
|  additionalMetricsToRetain |  list  member: BehaviorMetric  |  A list of metrics whose data is retained \(stored\)\. By default, data is retained for any metric used in the profile's `behaviors` but it is also retained for any metric specified here\.  | 
|  tags |  list  member: Tag  java class: java\.util\.List  |  Metadata that can be used to manage the security profile\. | 
|  Key |  string |  The tag's key\. | 
|  Value |  string |  The tag's value\. | 

Output:

```
{
  "securityProfileName": "string",
  "securityProfileArn": "string"
}
```


**CLI output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  securityProfileName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name you gave to the security profile\. | 
|  securityProfileArn |  string |  The ARN of the security profile\. | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\.

`ResourceAlreadyExistsException`  
The resource already exists\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## DeleteSecurityProfile<a name="dd-api-iot-DeleteSecurityProfile"></a>

Deletes a Device Defender security profile\.

 **Synopsis:**

```
aws iot  delete-security-profile \
    --security-profile-name <value> \
    [--expected-version <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "securityProfileName": "string",
  "expectedVersion": "long"
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  securityProfileName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the security profile to be deleted\. | 
|  expectedVersion |  long |  The expected version of the security profile\. A new version is generated whenever the security profile is updated\. If you specify a value that is different from the actual version, a `VersionConflictException` is thrown\.  | 

Output:

None

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

`VersionConflictException`  
An exception thrown when the version of a thing passed to a command is different from the version specified with the `--version` parameter\.

## DescribeSecurityProfile<a name="dd-api-iot-DescribeSecurityProfile"></a>

Gets information about a Device Defender security profile\.

 **Synopsis:**

```
aws iot  describe-security-profile \
    --security-profile-name <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "securityProfileName": "string"
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  securityProfileName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the security profile whose information you want to get\. | 

Output:

```
{
  "securityProfileName": "string",
  "securityProfileArn": "string",
  "securityProfileDescription": "string",
  "behaviors": [
    {
      "name": "string",
      "metric": "string",
      "criteria": {
        "comparisonOperator": "string",
        "value": {
          "count": "long",
          "cidrs": [
            "string"
          ],
          "ports": [
            "integer"
          ]
        },
        "durationSeconds": "integer",
        "consecutiveDatapointsToAlarm": "integer",
        "consecutiveDatapointsToClear": "integer",
        "statisticalThreshold": {
          "statistic": "string"
        }
      }
    }
  ],
  "alertTargets": {
    "string": {
      "alertTargetArn": "string",
      "roleArn": "string"
    }
  },
  "additionalMetricsToRetain": [
    "string"
  ],
  "version": "long",
  "creationDate": "timestamp",
  "lastModifiedDate": "timestamp"
}
```


**CLI output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  securityProfileName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the security profile\. | 
|  securityProfileArn |  string |  The ARN of the security profile\. | 
|  securityProfileDescription |  string  length\- max:1000  pattern: \[\\\\p\{Graph\} \]\*  |  A description of the security profile \(associated with the security profile when it was created or updated\)\.  | 
|  behaviors |  list  member: Behavior  |  Specifies the behaviors that, when violated by a device \(thing\), cause an alert\. | 
|  name |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name you have given to the behavior\. | 
|  metric |  string |  What is measured by the behavior\. | 
|  criteria |  BehaviorCriteria |  The criteria that determine if a device is behaving normally in regard to the `metric`\.  | 
|  comparisonOperator |  string |  The operator that relates the thing measured \(`metric`\) to the criteria \(containing a `value` or `statisticalThreshold`\)\.  enum: less\-than \| less\-than\-equals \| greater\-than \| greater\-than\-equals \| in\-cidr\-set \| not\-in\-cidr\-set \| in\-port\-set \| not\-in\-port\-set  | 
|  value |  MetricValue |  The value to be compared with the `metric`\. | 
|  count |  long  range\- min:0  |  If the `comparisonOperator` calls for a numeric value, use this to specify that numeric value to be compared with the `metric`\.  | 
|  cidrs |  list  member: Cidr  |  If the `comparisonOperator` calls for a set of CIDRs, use this to specify that set to be compared with the `metric`\.  | 
|  ports |  list  member: Port  |  If the `comparisonOperator` calls for a set of ports, use this to specify that set to be compared with the `metric`\.  | 
|  durationSeconds |  integer |  Use this to specify the time duration over which the behavior is evaluated, for those criteria that have a time dimension \(for example, `NUM_MESSAGES_SENT`\)\. For a `statisticalThreshhold` metric comparison, measurements from all devices are accumulated over this time duration before being used to calculate percentiles, and later, measurements from an individual device are also accumulated over this time duration before being given a percentile rank\.  | 
|  consecutiveDatapointsToAlarm |  integer  range\- max:10 min:1  |  If a device is in violation of the behavior for the specified number of consecutive data points, an alarm occurs\. If not specified, the default is 1\.  | 
|  consecutiveDatapointsToClear |  integer  range\- max:10 min:1  |  If an alarm has occurred and the offending device is no longer in violation of the behavior for the specified number of consecutive data points, the alarm is cleared\. If not specified, the default is 1\.  | 
|  statisticalThreshold |  StatisticalThreshold |  A statistical ranking \(percentile\) that indicates a threshold value by which a behavior is determined to be in compliance or in violation of the behavior\.  | 
|  statistic |  string  pattern: \(p0\|p0\.1\|p0\.01\|p1\|p10\|p50\|p90\|p99\|p99\.9\|p99\.99\|p100\)  |  The percentile that resolves to a threshold value by which compliance with a behavior is determined\. Metrics are collected over the specified period \(`durationSeconds`\) from all reporting devices in your account and statistical ranks are calculated\. Then, the measurements from a device are collected over the same period\. If the accumulated measurements from the device fall above or below \(`comparisonOperator`\) the value associated with the percentile specified, then the device is considered to be in compliance with the behavior\. Otherwise, a violation occurs\.  | 
|  alertTargets |  map |  Where the alerts are sent\. \(Alerts are always sent to the console\.\) | 
|  alertTargetArn |  string |  The ARN of the notification target to which alerts are sent\. | 
|  roleArn |  string  length\- max:2048 min:20  |  The ARN of the role that grants permission to send alerts to the notification target\.  | 
|  additionalMetricsToRetain |  list  member: BehaviorMetric  |  A list of metrics whose data is retained \(stored\)\. By default, data is retained for any metric used in the profile's `behaviors` but it is also retained for any metric specified here\.  | 
|  version |  long |  The version of the security profile\. A new version is generated whenever the security profile is updated\.  | 
|  creationDate |  timestamp |  The time the security profile was created\. | 
|  lastModifiedDate |  timestamp |  The time the security profile was last modified\. | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\.

`ResourceNotFoundException`  
The specified resource does not exist\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## DetachSecurityProfile<a name="dd-api-iot-DetachSecurityProfile"></a>

Disassociates a Device Defender security profile from a thing group or from this account\.

 **Synopsis:**

```
aws iot  detach-security-profile \
    --security-profile-name <value> \
    --security-profile-target-arn <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "securityProfileName": "string",
  "securityProfileTargetArn": "string"
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  securityProfileName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The security profile that is detached\. | 
|  securityProfileTargetArn |  string |  The ARN of the thing group from which the security profile is detached\. | 

Output:

None

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\.

`ResourceNotFoundException`  
The specified resource does not exist\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## ListActiveViolations<a name="dd-api-iot-ListActiveViolations"></a>

Lists the active violations for a given Device Defender security profile\.

 **Synopsis:**

```
aws iot  list-active-violations \
    [--thing-name <value>] \
    [--security-profile-name <value>] \
    [--next-token <value>] \
    [--max-results <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "thingName": "string",
  "securityProfileName": "string",
  "nextToken": "string",
  "maxResults": "integer"
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  thingName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the thing whose active violations are listed\. | 
|  securityProfileName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the Device Defender security profile for which violations are listed\. | 
|  nextToken |  string |  The token for the next set of results\. | 
|  maxResults |  integer  range\- max:250 min:1  |  The maximum number of results to return at one time\. | 

Output:

```
{
  "activeViolations": [
    {
      "violationId": "string",
      "thingName": "string",
      "securityProfileName": "string",
      "behavior": {
        "name": "string",
        "metric": "string",
        "criteria": {
          "comparisonOperator": "string",
          "value": {
            "count": "long",
            "cidrs": [
              "string"
            ],
            "ports": [
              "integer"
            ]
          },
          "durationSeconds": "integer",
          "consecutiveDatapointsToAlarm": "integer",
          "consecutiveDatapointsToClear": "integer",
          "statisticalThreshold": {
            "statistic": "string"
          }
        }
      },
      "lastViolationValue": {
        "count": "long",
        "cidrs": [
          "string"
        ],
        "ports": [
          "integer"
        ]
      },
      "lastViolationTime": "timestamp",
      "violationStartTime": "timestamp"
    }
  ],
  "nextToken": "string"
}
```


**CLI output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  activeViolations |  list  member: ActiveViolation  |  The list of active violations\. | 
|  violationId |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9\-\]\+  |  The ID of the active violation\. | 
|  thingName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the thing responsible for the active violation\. | 
|  securityProfileName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The security profile whose behavior is in violation\. | 
|  behavior |  Behavior |  The behavior that is being violated\. | 
|  name |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name you have given to the behavior\. | 
|  metric |  string |  What is measured by the behavior\. | 
|  criteria |  BehaviorCriteria |  The criteria that determine if a device is behaving normally in regard to the `metric`\.  | 
|  comparisonOperator |  string |  The operator that relates the thing measured \(`metric`\) to the criteria \(containing a `value` or `statisticalThreshold`\)\.  enum: less\-than \| less\-than\-equals \| greater\-than \| greater\-than\-equals \| in\-cidr\-set \| not\-in\-cidr\-set \| in\-port\-set \| not\-in\-port\-set  | 
|  value |  MetricValue |  The value to be compared with the `metric`\. | 
|  count |  long  range\- min:0  |  If the `comparisonOperator` calls for a numeric value, use this to specify that numeric value to be compared with the `metric`\.  | 
|  cidrs |  list  member: Cidr  |  If the `comparisonOperator` calls for a set of CIDRs, use this to specify that set to be compared with the `metric`\.  | 
|  ports |  list  member: Port  |  If the `comparisonOperator` calls for a set of ports, use this to specify that set to be compared with the `metric`\.  | 
|  durationSeconds |  integer |  Use this to specify the time duration over which the behavior is evaluated, for those criteria that have a time dimension \(for example, `NUM_MESSAGES_SENT`\)\. For a `statisticalThreshhold` metric comparison, measurements from all devices are accumulated over this time duration before being used to calculate percentiles, and later, measurements from an individual device are also accumulated over this time duration before being given a percentile rank\.  | 
|  consecutiveDatapointsToAlarm |  integer  range\- max:10 min:1  |  If a device is in violation of the behavior for the specified number of consecutive data points, an alarm occurs\. If not specified, the default is 1\.  | 
|  consecutiveDatapointsToClear |  integer  range\- max:10 min:1  |  If an alarm has occurred and the offending device is no longer in violation of the behavior for the specified number of consecutive data points, the alarm is cleared\. If not specified, the default is 1\.  | 
|  statisticalThreshold |  StatisticalThreshold |  A statistical ranking \(percentile\) which indicates a threshold value by which a behavior is determined to be in compliance or in violation of the behavior\.  | 
|  statistic |  string  pattern: \(p0\|p0\.1\|p0\.01\|p1\|p10\|p50\|p90\|p99\|p99\.9\|p99\.99\|p100\)  |  The percentile which resolves to a threshold value by which compliance with a behavior is determined\. Metrics are collected over the specified period \(`durationSeconds`\) from all reporting devices in your account and statistical ranks are calculated\. Then, the measurements from a device are collected over the same period\. If the accumulated measurements from the device fall above or below \(`comparisonOperator`\) the value associated with the percentile specified, then the device is considered to be in compliance with the behavior, otherwise a violation occurs\.  | 
|  lastViolationValue |  MetricValue |  The value of the metric \(the measurement\) which caused the most recent violation\. | 
|  count |  long  range\- min:0  |  If the `comparisonOperator` calls for a numeric value, use this to specify that numeric value to be compared with the `metric`\.  | 
|  cidrs |  list  member: Cidr  |  If the `comparisonOperator` calls for a set of CIDRs, use this to specify that set to be compared with the `metric`\.  | 
|  ports |  list  member: Port  |  If the `comparisonOperator` calls for a set of ports, use this to specify that set to be compared with the `metric`\.  | 
|  lastViolationTime |  timestamp |  The time the most recent violation occurred\. | 
|  violationStartTime |  timestamp |  The time the violation started\. | 
|  nextToken |  string |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\.

`ResourceNotFoundException`  
The specified resource does not exist\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## ListSecurityProfiles<a name="dd-api-iot-ListSecurityProfiles"></a>

Lists the Device Defender security profiles you have created\. You can use filters to list only those security profiles associated with a thing group or only those associated with your account\.

 **Synopsis:**

```
aws iot  list-security-profiles \
    [--next-token <value>] \
    [--max-results <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "nextToken": "string",
  "maxResults": "integer"
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  nextToken |  string |  The token for the next set of results\. | 
|  maxResults |  integer  range\- max:250 min:1  |  The maximum number of results to return at one time\. | 

Output:

```
{
  "securityProfileIdentifiers": [
    {
      "name": "string",
      "arn": "string"
    }
  ],
  "nextToken": "string"
}
```


**CLI output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  securityProfileIdentifiers |  list  member: SecurityProfileIdentifier  java class: java\.util\.List  |  A list of security profile identifiers \(names and ARNs\)\. | 
|  name |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name you have given to the security profile\. | 
|  arn |  string |  The ARN of the security profile\. | 
|  nextToken |  string |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## ListSecurityProfilesForTarget<a name="dd-api-iot-ListSecurityProfilesForTarget"></a>

Lists the Device Defender security profiles attached to a target \(thing group\)\.

 **Synopsis:**

```
aws iot  list-security-profiles-for-target \
    [--next-token <value>] \
    [--max-results <value>] \
    [--recursive | --no-recursive] \
    --security-profile-target-arn <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "nextToken": "string",
  "maxResults": "integer",
  "recursive": "boolean",
  "securityProfileTargetArn": "string"
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  nextToken |  string |  The token for the next set of results\. | 
|  maxResults |  integer  range\- max:250 min:1  |  The maximum number of results to return at one time\. | 
|  recursive |  boolean |  If true, return child groups as well\. | 
|  securityProfileTargetArn |  string |  The ARN of the target \(thing group\) whose attached security profiles you want to get\. | 

Output:

```
{
  "securityProfileTargetMappings": [
    {
      "securityProfileIdentifier": {
        "name": "string",
        "arn": "string"
      },
      "target": {
        "arn": "string"
      }
    }
  ],
  "nextToken": "string"
}
```


**CLI output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  securityProfileTargetMappings |  list  member: SecurityProfileTargetMapping  java class: java\.util\.List  |  A list of security profiles and their associated targets\. | 
|  securityProfileIdentifier |  SecurityProfileIdentifier |  Information that identifies the security profile\. | 
|  name |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name you have given to the security profile\. | 
|  arn |  string |  The ARN of the security profile\. | 
|  target |  SecurityProfileTarget |  Information about the target \(thing group\) associated with the security profile\. | 
|  arn |  string |  The ARN of the security profile\. | 
|  nextToken |  string |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

`ResourceNotFoundException`  
The specified resource does not exist\.

## ListTargetsForSecurityProfile<a name="dd-api-iot-ListTargetsForSecurityProfile"></a>

Lists the targets \(thing groups\) associated with a given Device Defender security profile\.

 **Synopsis:**

```
aws iot  list-targets-for-security-profile \
    --security-profile-name <value> \
    [--next-token <value>] \
    [--max-results <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "securityProfileName": "string",
  "nextToken": "string",
  "maxResults": "integer"
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  securityProfileName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The security profile\. | 
|  nextToken |  string |  The token for the next set of results\. | 
|  maxResults |  integer  range\- max:250 min:1  |  The maximum number of results to return at one time\. | 

Output:

```
{
  "securityProfileTargets": [
    {
      "arn": "string"
    }
  ],
  "nextToken": "string"
}
```


**CLI output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  `securityProfileTargets` |  list  member: SecurityProfileTarget  java class: java\.util\.List  |  The thing groups to which the security profile is attached\. | 
|  `arn` |  string |  The ARN of the security profile\. | 
|  `nextToken` |  string |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\.

`ResourceNotFoundException`  
The specified resource does not exist\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## ListViolationEvents<a name="dd-api-iot-ListViolationEvents"></a>

Lists the Device Defender security profile violations discovered during the given time period\. You can use filters to limit the results to those alerts issued for a particular security profile, behavior or thing \(device\)\.

 **Synopsis:**

```
aws iot  list-violation-events \
    --start-time <value> \
    --end-time <value> \
    [--thing-name <value>] \
    [--security-profile-name <value>] \
    [--next-token <value>] \
    [--max-results <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "startTime": "timestamp",
  "endTime": "timestamp",
  "thingName": "string",
  "securityProfileName": "string",
  "nextToken": "string",
  "maxResults": "integer"
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  startTime |  timestamp |  The start time for the alerts to be listed\. | 
|  endTime |  timestamp |  The end time for the alerts to be listed\. | 
|  thingName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  A filter to limit results to those alerts caused by the specified thing\. | 
|  securityProfileName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  A filter to limit results to those alerts generated by the specified security profile\. | 
|  nextToken |  string |  The token for the next set of results\. | 
|  maxResults |  integer  range\- max:250 min:1  |  The maximum number of results to return at one time\. | 

Output:

```
{
  "violationEvents": [
    {
      "violationId": "string",
      "thingName": "string",
      "securityProfileName": "string",
      "behavior": {
        "name": "string",
        "metric": "string",
        "criteria": {
          "comparisonOperator": "string",
          "value": {
            "count": "long",
            "cidrs": [
              "string"
            ],
            "ports": [
              "integer"
            ]
          },
          "durationSeconds": "integer",
          "consecutiveDatapointsToAlarm": "integer",
          "consecutiveDatapointsToClear": "integer",
          "statisticalThreshold": {
            "statistic": "string"
          }
        }
      },
      "metricValue": {
        "count": "long",
        "cidrs": [
          "string"
        ],
        "ports": [
          "integer"
        ]
      },
      "violationEventType": "string",
      "violationEventTime": "timestamp"
    }
  ],
  "nextToken": "string"
}
```


**CLI output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  violationEvents |  list  member: ViolationEvent  |  The security profile violation alerts issued for this account during the given time frame, potentially filtered by security profile, behavior violated, or thing \(device\) violating\.  | 
|  violationId |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9\-\]\+  |  The ID of the violation event\. | 
|  thingName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the thing responsible for the violation event\. | 
|  securityProfileName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the security profile whose behavior was violated\. | 
|  behavior |  Behavior |  The behavior that was violated\. | 
|  name |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name you have given to the behavior\. | 
|  metric |  string |  What is measured by the behavior\. | 
|  criteria |  BehaviorCriteria |  The criteria that determine if a device is behaving normally in regard to the `metric`\.  | 
|  comparisonOperator |  string |  The operator that relates the thing measured \(`metric`\) to the criteria \(containing a `value` or `statisticalThreshold`\)\.  enum: less\-than \| less\-than\-equals \| greater\-than \| greater\-than\-equals \| in\-cidr\-set \| not\-in\-cidr\-set \| in\-port\-set \| not\-in\-port\-set  | 
|  value |  MetricValue |  The value to be compared with the `metric`\. | 
|  count |  long  range\- min:0  |  If the `comparisonOperator` calls for a numeric value, use this to specify that numeric value to be compared with the `metric`\.  | 
|  cidrs |  list  member: Cidr  |  If the `comparisonOperator` calls for a set of CIDRs, use this to specify that set to be compared with the `metric`\.  | 
|  ports |  list  member: Port  |  If the `comparisonOperator` calls for a set of ports, use this to specify that set to be compared with the `metric`\.  | 
|  durationSeconds |  integer |  Use this to specify the time duration over which the behavior is evaluated, for those criteria that have a time dimension \(for example, `NUM_MESSAGES_SENT`\)\. For a `statisticalThreshhold` metric comparison, measurements from all devices are accumulated over this time duration before being used to calculate percentiles, and later, measurements from an individual device are also accumulated over this time duration before being given a percentile rank\.  | 
|  consecutiveDatapointsToAlarm |  integer  range\- max:10 min:1  |  If a device is in violation of the behavior for the specified number of consecutive data points, an alarm occurs\. If not specified, the default is 1\.  | 
|  consecutiveDatapointsToClear |  integer  range\- max:10 min:1  |  If an alarm has occurred and the offending device is no longer in violation of the behavior for the specified number of consecutive data points, the alarm is cleared\. If not specified, the default is 1\.  | 
|  statisticalThreshold |  StatisticalThreshold |  A statistical ranking \(percentile\) that indicates a threshold value by which a behavior is determined to be in compliance or in violation of the behavior\.  | 
|  statistic |  string  pattern: \(p0\|p0\.1\|p0\.01\|p1\|p10\|p50\|p90\|p99\|p99\.9\|p99\.99\|p100\)  |  The percentile that resolves to a threshold value by which compliance with a behavior is determined\. Metrics are collected over the specified period \(`durationSeconds`\) from all reporting devices in your account and statistical ranks are calculated\. Then, the measurements from a device are collected over the same period\. If the accumulated measurements from the device fall above or below \(`comparisonOperator`\) the value associated with the percentile specified, then the device is considered to be in compliance with the behavior, otherwise a violation occurs\.  | 
|  metricValue |  MetricValue |  The value of the metric \(the measurement\)\. | 
|  count |  long  range\- min:0  |  If the `comparisonOperator` calls for a numeric value, use this to specify that numeric value to be compared with the `metric`\.  | 
|  cidrs |  list  member: Cidr  |  If the `comparisonOperator` calls for a set of CIDRs, use this to specify that set to be compared with the `metric`\.  | 
|  ports |  list  member: Port  |  If the `comparisonOperator` calls for a set of ports, use this to specify that set to be compared with the `metric`\.  | 
|  violationEventType |  string |  The type of violation event\.  enum: in\-alarm \| alarm\-cleared \| alarm\-invalidated  | 
|  violationEventTime |  timestamp |  The time the violation event occurred\. | 
|  nextToken |  string |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## UpdateSecurityProfile<a name="dd-api-iot-UpdateSecurityProfile"></a>

Updates a Device Defender security profile\.

 **Synopsis:**

```
aws iot  update-security-profile \
    --security-profile-name <value> \
    [--security-profile-description <value>] \
    [--behaviors <value>] \
    [--alert-targets <value>] \
    [--additional-metrics-to-retain <value>] \
    [--delete-behaviors | --no-delete-behaviors] \
    [--delete-alert-targets | --no-delete-alert-targets] \
    [--delete-additional-metrics-to-retain | --no-delete-additional-metrics-to-retain] \
    [--expected-version <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "securityProfileName": "string",
  "securityProfileDescription": "string",
  "behaviors": [
    {
      "name": "string",
      "metric": "string",
      "criteria": {
        "comparisonOperator": "string",
        "value": {
          "count": "long",
          "cidrs": [
            "string"
          ],
          "ports": [
            "integer"
          ]
        },
        "durationSeconds": "integer",
        "consecutiveDatapointsToAlarm": "integer",
        "consecutiveDatapointsToClear": "integer",
        "statisticalThreshold": {
          "statistic": "string"
        }
      }
    }
  ],
  "alertTargets": {
    "string": {
      "alertTargetArn": "string",
      "roleArn": "string"
    }
  },
  "additionalMetricsToRetain": [
    "string"
  ],
  "deleteBehaviors": "boolean",
  "deleteAlertTargets": "boolean",
  "deleteAdditionalMetricsToRetain": "boolean",
  "expectedVersion": "long"
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  securityProfileName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the security profile you want to update\. | 
|  securityProfileDescription |  string  length\- max:1000  pattern: \[\\\\p\{Graph\} \]\*  |  A description of the security profile\. | 
|  behaviors |  list  member: Behavior  |  Specifies the behaviors that, when violated by a device \(thing\), cause an alert\. | 
|  name |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name you have given to the behavior\. | 
|  metric |  string |  What is measured by the behavior\. | 
|  criteria |  BehaviorCriteria |  The criteria that determine if a device is behaving normally in regard to the `metric`\.  | 
|  comparisonOperator |  string |  The operator that relates the thing measured \(`metric`\) to the criteria \(containing a `value` or `statisticalThreshold`\)\.  enum: less\-than \| less\-than\-equals \| greater\-than \| greater\-than\-equals \| in\-cidr\-set \| not\-in\-cidr\-set \| in\-port\-set \| not\-in\-port\-set  | 
|  value |  MetricValue |  The value to be compared with the `metric`\. | 
|  count |  long  range\- min:0  |  If the `comparisonOperator` calls for a numeric value, use this to specify that numeric value to be compared with the `metric`\.  | 
|  cidrs |  list  member: Cidr  |  If the `comparisonOperator` calls for a set of CIDRs, use this to specify that set to be compared with the `metric`\.  | 
|  ports |  list  member: Port  |  If the `comparisonOperator` calls for a set of ports, use this to specify that set to be compared with the `metric`\.  | 
|  durationSeconds |  integer |  Use this to specify the time duration over which the behavior is evaluated, for those criteria that have a time dimension \(for example, `NUM_MESSAGES_SENT`\)\. For a `statisticalThreshhold` metric comparison, measurements from all devices are accumulated over this time duration before being used to calculate percentiles, and later, measurements from an individual device are also accumulated over this time duration before being given a percentile rank\.  | 
|  consecutiveDatapointsToAlarm |  integer  range\- max:10 min:1  |  If a device is in violation of the behavior for the specified number of consecutive data points, an alarm occurs\. If not specified, the default is 1\.  | 
|  consecutiveDatapointsToClear |  integer  range\- max:10 min:1  |  If an alarm has occurred and the offending device is no longer in violation of the behavior for the specified number of consecutive data points, the alarm is cleared\. If not specified, the default is 1\.  | 
|  statisticalThreshold |  StatisticalThreshold |  A statistical ranking \(percentile\) that indicates a threshold value by which a behavior is determined to be in compliance or in violation of the behavior\.  | 
|  statistic |  string  pattern: \(p0\|p0\.1\|p0\.01\|p1\|p10\|p50\|p90\|p99\|p99\.9\|p99\.99\|p100\)  |  The percentile that resolves to a threshold value by which compliance with a behavior is determined\. Metrics are collected over the specified period \(`durationSeconds`\) from all reporting devices in your account and statistical ranks are calculated\. Then, the measurements from a device are collected over the same period\. If the accumulated measurements from the device fall above or below \(`comparisonOperator`\) the value associated with the percentile specified, then the device is considered to be in compliance with the behavior, otherwise a violation occurs\.  | 
|  alertTargets |  map |  Where the alerts are sent\. \(Alerts are always sent to the console\.\) | 
|  alertTargetArn |  string |  The ARN of the notification target to which alerts are sent\. | 
|  roleArn |  string  length\- max:2048 min:20  |  The ARN of the role that grants permission to send alerts to the notification target\.  | 
|  additionalMetricsToRetain |  list  member: BehaviorMetric  |  A list of metrics whose data is retained \(stored\)\. By default, data is retained for any metric used in the profile's `behaviors` but it is also retained for any metric specified here\.  | 
|  deleteBehaviors |  boolean |  If true, delete all `behaviors` defined for this security profile\. If any `behaviors` are defined in the current invocation an exception occurs\.  | 
|  deleteAlertTargets |  boolean |  If true, delete all `alertTargets` defined for this security profile\. If any `alertTargets` are defined in the current invocation an exception occurs\.  | 
|  deleteAdditionalMetricsToRetain |  boolean |  If true, delete all `additionalMetricsToRetain` defined for this security profile\. If any `additionalMetricsToRetain` are defined in the current invocation an exception occurs\.  | 
|  expectedVersion |  long |  The expected version of the security profile\. A new version is generated whenever the security profile is updated\. If you specify a value that is different from the actual version, a `VersionConflictException` is thrown\.  | 

Output:

```
{
  "securityProfileName": "string",
  "securityProfileArn": "string",
  "securityProfileDescription": "string",
  "behaviors": [
    {
      "name": "string",
      "metric": "string",
      "criteria": {
        "comparisonOperator": "string",
        "value": {
          "count": "long",
          "cidrs": [
            "string"
          ],
          "ports": [
            "integer"
          ]
        },
        "durationSeconds": "integer",
        "consecutiveDatapointsToAlarm": "integer",
        "consecutiveDatapointsToClear": "integer",
        "statisticalThreshold": {
          "statistic": "string"
        }
      }
    }
  ],
  "alertTargets": {
    "string": {
      "alertTargetArn": "string",
      "roleArn": "string"
    }
  },
  "additionalMetricsToRetain": [
    "string"
  ],
  "version": "long",
  "creationDate": "timestamp",
  "lastModifiedDate": "timestamp"
}
```


**CLI output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  securityProfileName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the security profile that was updated\. | 
|  securityProfileArn |  string |  The ARN of the security profile that was updated\. | 
|  securityProfileDescription |  string  length\- max:1000  pattern: \[\\\\p\{Graph\} \]\*  |  The description of the security profile\. | 
|  behaviors |  list  member: Behavior  |  Specifies the behaviors that, when violated by a device \(thing\), cause an alert\. | 
|  name |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name you have given to the behavior\. | 
|  metric |  string |  What is measured by the behavior\. | 
|  criteria |  BehaviorCriteria |  The criteria that determine if a device is behaving normally in regard to the `metric`\.  | 
|  comparisonOperator |  string |  The operator that relates the thing measured \(`metric`\) to the criteria \(containing a `value` or `statisticalThreshold`\)\.  enum: less\-than \| less\-than\-equals \| greater\-than \| greater\-than\-equals \| in\-cidr\-set \| not\-in\-cidr\-set \| in\-port\-set \| not\-in\-port\-set  | 
|  value |  MetricValue |  The value to be compared with the `metric`\. | 
|  count |  long  range\- min:0  |  If the `comparisonOperator` calls for a numeric value, use this to specify that numeric value to be compared with the `metric`\.  | 
|  cidrs |  list  member: Cidr  |  If the `comparisonOperator` calls for a set of CIDRs, use this to specify that set to be compared with the `metric`\.  | 
|  ports |  list  member: Port  |  If the `comparisonOperator` calls for a set of ports, use this to specify that set to be compared with the `metric`\.  | 
|  durationSeconds |  integer |  Use this to specify the time duration over which the behavior is evaluated, for those criteria that have a time dimension \(for example, `NUM_MESSAGES_SENT`\)\. For a `statisticalThreshhold` metric comparison, measurements from all devices are accumulated over this time duration before being used to calculate percentiles, and later, measurements from an individual device are also accumulated over this time duration before being given a percentile rank\.  | 
|  consecutiveDatapointsToAlarm |  integer  range\- max:10 min:1  |  If a device is in violation of the behavior for the specified number of consecutive data points, an alarm occurs\. If not specified, the default is 1\.  | 
|  consecutiveDatapointsToClear |  integer  range\- max:10 min:1  |  If an alarm has occurred and the offending device is no longer in violation of the behavior for the specified number of consecutive data points, the alarm is cleared\. If not specified, the default is 1\.  | 
|  statisticalThreshold |  StatisticalThreshold |  A statistical ranking \(percentile\) that indicates a threshold value by which a behavior is determined to be in compliance or in violation of the behavior\.  | 
|  statistic |  string  pattern: \(p0\|p0\.1\|p0\.01\|p1\|p10\|p50\|p90\|p99\|p99\.9\|p99\.99\|p100\)  |  The percentile that resolves to a threshold value by which compliance with a behavior is determined\. Metrics are collected over the specified period \(`durationSeconds`\) from all reporting devices in your account and statistical ranks are calculated\. Then, the measurements from a device are collected over the same period\. If the accumulated measurements from the device fall above or below \(`comparisonOperator`\) the value associated with the percentile specified, then the device is considered to be in compliance with the behavior\. Otherwise, a violation occurs\.  | 
|  alertTargets |  map |  Where the alerts are sent\. \(Alerts are always sent to the console\.\) | 
|  alertTargetArn |  string |  The ARN of the notification target to which alerts are sent\. | 
|  roleArn |  string  length\- max:2048 min:20  |  The ARN of the role that grants permission to send alerts to the notification target\.  | 
|  additionalMetricsToRetain |  list  member: BehaviorMetric  |  A list of metrics whose data is retained \(stored\)\. By default, data is retained for any metric used in the security profile's `behaviors`, but it is also retained for any metric specified here\.  | 
|  version |  long |  The updated version of the security profile\. | 
|  creationDate |  timestamp |  The time the security profile was created\. | 
|  lastModifiedDate |  timestamp |  The time the security profile was last modified\. | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\.

`ResourceNotFoundException`  
The specified resource does not exist\.

`VersionConflictException`  
An exception thrown when the version of a thing passed to a command is different from the version specified with the `--version` parameter\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## ValidateSecurityProfileBehaviors<a name="dd-api-iot-ValidateSecurityProfileBehaviors"></a>

Validates a Device Defender security profile behaviors specification\.

 **Synopsis:**

```
aws iot  validate-security-profile-behaviors \
    --behaviors <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "behaviors": [
    {
      "name": "string",
      "metric": "string",
      "criteria": {
        "comparisonOperator": "string",
        "value": {
          "count": "long",
          "cidrs": [
            "string"
          ],
          "ports": [
            "integer"
          ]
        },
        "durationSeconds": "integer",
        "consecutiveDatapointsToAlarm": "integer",
        "consecutiveDatapointsToClear": "integer",
        "statisticalThreshold": {
          "statistic": "string"
        }
      }
    }
  ]
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  behaviors |  list  member: Behavior  |  Specifies the behaviors that, when violated by a device \(thing\), cause an alert\. | 
|  name |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name you have given to the behavior\. | 
|  metric |  string |  What is measured by the behavior\. | 
|  criteria |  BehaviorCriteria |  The criteria that determine if a device is behaving normally in regard to the `metric`\.  | 
|  comparisonOperator |  string |  The operator that relates the thing measured \(`metric`\) to the criteria \(containing a `value` or `statisticalThreshold`\)\.  enum: less\-than \| less\-than\-equals \| greater\-than \| greater\-than\-equals \| in\-cidr\-set \| not\-in\-cidr\-set \| in\-port\-set \| not\-in\-port\-set  | 
|  value |  MetricValue |  The value to be compared with the `metric`\. | 
|  count |  long  range\- min:0  |  If the `comparisonOperator` calls for a numeric value, use this to specify that numeric value to be compared with the `metric`\.  | 
|  cidrs |  list  member: Cidr  |  If the `comparisonOperator` calls for a set of CIDRs, use this to specify that set to be compared with the `metric`\.  | 
|  ports |  list  member: Port  |  If the `comparisonOperator` calls for a set of ports, use this to specify that set to be compared with the `metric`\.  | 
|  durationSeconds |  integer |  Use this to specify the time duration over which the behavior is evaluated, for those criteria that have a time dimension \(for example, `NUM_MESSAGES_SENT`\)\. For a `statisticalThreshhold` metric comparison, measurements from all devices are accumulated over this time duration before being used to calculate percentiles, and later, measurements from an individual device are also accumulated over this time duration before being given a percentile rank\.  | 
|  consecutiveDatapointsToAlarm |  integer  range\- max:10 min:1  |  If a device is in violation of the behavior for the specified number of consecutive data points, an alarm occurs\. If not specified, the default is 1\.  | 
|  consecutiveDatapointsToClear |  integer  range\- max:10 min:1  |  If an alarm has occurred and the offending device is no longer in violation of the behavior for the specified number of consecutive data points, the alarm is cleared\. If not specified, the default is 1\.  | 
|  statisticalThreshold |  StatisticalThreshold |  A statistical ranking \(percentile\) that indicates a threshold value by which a behavior is determined to be in compliance or in violation of the behavior\.  | 
|  statistic |  string  pattern: \(p0\|p0\.1\|p0\.01\|p1\|p10\|p50\|p90\|p99\|p99\.9\|p99\.99\|p100\)  |  The percentile that resolves to a threshold value by which compliance with a behavior is determined\. Metrics are collected over the specified period \(`durationSeconds`\) from all reporting devices in your account and statistical ranks are calculated\. Then, the measurements from a device are collected over the same period\. If the accumulated measurements from the device fall above or below \(`comparisonOperator`\) the value associated with the percentile specified, then the device is considered to be in compliance with the behavior\. Otherwise, a violation occurs\.  | 

Output:

```
{
  "valid": "boolean",
  "validationErrors": [
    {
      "errorMessage": "string"
    }
  ]
}
```


**CLI output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  `valid` |  boolean |  True if the behaviors were valid\. | 
|  `validationErrors` |  list  member: ValidationError  |  The list of any errors found in the behaviors\. | 
|  `errorMessage` |  string  length\- max:2048  |  The description of an error found in the behaviors\. | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.