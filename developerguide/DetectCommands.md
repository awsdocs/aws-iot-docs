# Detect Commands<a name="DetectCommands"></a>

## AttachSecurityProfile<a name="dd-api-iot-AttachSecurityProfile-doc"></a>

Associates an security profile with a thing group or with this account\. Each thing group or account can have up to five security profiles associated with it\.

### https<a name="dd-api-iot-AttachSecurityProfile-https"></a>

 **Request syntax:**

```
PUT /security-profiles/securityProfileName/targets?securityProfileTargetArn=securityProfileTargetArn 
```


**URI Request Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  securityProfileName |  SecurityProfileName |  yes |  The security profile that is attached\. | 
|  securityProfileTargetArn |  SecurityProfileTargetArn |  yes |  The ARN of the target \(thing group\) to which the security profile is attached\. | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.  
HTTP response code: 400

`ResourceNotFoundException`  
The specified resource does not exist\.  
HTTP response code: 404

`LimitExceededException`  
A limit has been exceeded\.  
HTTP response code: 410

`VersionConflictException`  
An exception thrown when the version of a thing passed to a command is different than the version specified with the \-\-version parameter\.  
HTTP response code: 409

`ThrottlingException`  
The rate exceeds the limit\.  
HTTP response code: 429

`InternalFailureException`  
An unexpected error has occurred\.  
HTTP response code: 500

### cli<a name="dd-api-iot-AttachSecurityProfile-cli"></a>

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

Output:

None

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ResourceNotFoundException`  
The specified resource does not exist\.

`LimitExceededException`  
A limit has been exceeded\.

`VersionConflictException`  
An exception thrown when the version of a thing passed to a command is different than the version specified with the \-\-version parameter\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## CreateSecurityProfile<a name="dd-api-iot-CreateSecurityProfile-doc"></a>

Creates an security profile\.

### https<a name="dd-api-iot-CreateSecurityProfile-https"></a>

 **Request syntax:**

```
POST /security-profiles/securityProfileName 
Content-type: application/json

{
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
        "durationSeconds": "integer"
      }
    }
  ],
  "alertTargets": {
    "string": {
      "alertTargetArn": "string",
      "roleArn": "string"
    }
  }
}
```


**URI Request Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  securityProfileName |  SecurityProfileName |  yes |  The name you are giving to the security profile\. | 


**Request Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  securityProfileDescription |  SecurityProfileDescription |  no |  A description of the security profile\. | 
|  behaviors |  Behaviors |  yes |  Specifies the behaviors that, when violated by a device \(thing\), cause an alert\. | 
|  alertTargets |  AlertTargets |  no |  Specifies the destinations to which alerts are sent\. \(Alerts are always sent to the console\.\) Alerts are generated when a device \(thing\) violates a behavior\.  | 

 **Response syntax:**

```
Content-type: application/json

{
  "securityProfileName": "string",
  "securityProfileArn": "string"
}
```


**Response Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  securityProfileName |   SecurityProfileName  |  no |  The name you gave to the security profile\. | 
|  securityProfileArn |   SecurityProfileArn  |  no |  The ARN of the security profile\. | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.  
HTTP response code: 400

`ResourceAlreadyExistsException`  
The resource already exists\.  
HTTP response code: 409

`ThrottlingException`  
The rate exceeds the limit\.  
HTTP response code: 429

`InternalFailureException`  
An unexpected error has occurred\.  
HTTP response code: 500

### cli<a name="dd-api-iot-CreateSecurityProfile-cli"></a>

 **Synopsis:**

```
aws iot  create-security-profile \
    --security-profile-name <value> \
    [--security-profile-description <value>] \
    --behaviors <value> \
    [--alert-targets <value>]  \
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
        "durationSeconds": "integer"
      }
    }
  ],
  "alertTargets": {
    "string": {
      "alertTargetArn": "string",
      "roleArn": "string"
    }
  }
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
|  comparisonOperator |  string |  The operator that relates the thing measured \(`metric`\) to the criteria \(`value`\)\.  enum: less\-than | less\-than\-equals | greater\-than | greater\-than\-equals | in\-cidr\-set | not\-in\-cidr\-set | in\-port\-set | not\-in\-port\-set  | 
|  value |  MetricValue |  The value to be compared with the `metric`\. | 
|  count |  long  range\- min:0  |  If the `comparisonOperator` calls for a numeric value, use this to specify that numeric value to be compared with the `metric`\.  | 
|  cidrs |  list  member: Cidr  |  If the `comparisonOperator` calls for a set of CIDRs, use this to specify that set to be compared with the `metric`\.  | 
|  ports |  list  member: Port  |  If the `comparisonOperator` calls for a set of ports, use this to specify that set to be compared with the `metric`\.  | 
|  durationSeconds |  integer |  Use this to specify the period of time over which the behavior is evaluated, for those criteria which have a time dimension \(for example, `NUM_MESSAGES_SENT`\)\.  | 
|  alertTargets |  map |  Specifies the destinations to which alerts are sent\. \(Alerts are always sent to the console\.\) Alerts are generated when a device \(thing\) violates a behavior\.  | 
|  alertTargetArn |  string |  The ARN of the notification target to which alerts are sent\. | 
|  roleArn |  string  length\- max:2048 min:20  |  The ARN of the role that grants permission to send alerts to the notification target\.  | 

Output:

```
{
  "securityProfileName": "string",
  "securityProfileArn": "string"
}
```


**cli output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  securityProfileName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name you gave to the security profile\. | 
|  securityProfileArn |  string |  The ARN of the security profile\. | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ResourceAlreadyExistsException`  
The resource already exists\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## DeleteSecurityProfile<a name="dd-api-iot-DeleteSecurityProfile-doc"></a>

Deletes an security profile\.

### https<a name="dd-api-iot-DeleteSecurityProfile-https"></a>

 **Request syntax:**

```
DELETE /security-profiles/securityProfileName?expectedVersion=expectedVersion 
```


**URI Request Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  securityProfileName |  SecurityProfileName |  yes |  The name of the security profile to be deleted\. | 
|  expectedVersion |  OptionalVersion |  no |  The expected version of the security profile\. A new version is generated whenever the security profile is updated\. If you specify a value that is different than the actual version, a `VersionConflictException` is thrown\.  | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.  
HTTP response code: 400

`ThrottlingException`  
The rate exceeds the limit\.  
HTTP response code: 429

`InternalFailureException`  
An unexpected error has occurred\.  
HTTP response code: 500

`VersionConflictException`  
An exception thrown when the version of a thing passed to a command is different than the version specified with the \-\-version parameter\.  
HTTP response code: 409

### cli<a name="dd-api-iot-DeleteSecurityProfile-cli"></a>

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
|  expectedVersion |  long |  The expected version of the security profile\. A new version is generated whenever the security profile is updated\. If you specify a value that is different than the actual version, a `VersionConflictException` is thrown\.  | 

Output:

None

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

`VersionConflictException`  
An exception thrown when the version of a thing passed to a command is different than the version specified with the \-\-version parameter\.

## DescribeSecurityProfile<a name="dd-api-iot-DescribeSecurityProfile-doc"></a>

Gets information about an security profile\.

### https<a name="dd-api-iot-DescribeSecurityProfile-https"></a>

 **Request syntax:**

```
GET /security-profiles/securityProfileName 
```


**URI Request Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  securityProfileName |  SecurityProfileName |  yes |  The name of the security profile whose information you want to get\. | 

 **Response syntax:**

```
Content-type: application/json

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
        "durationSeconds": "integer"
      }
    }
  ],
  "alertTargets": {
    "string": {
      "alertTargetArn": "string",
      "roleArn": "string"
    }
  },
  "version": "long",
  "creationDate": "timestamp",
  "lastModifiedDate": "timestamp"
}
```


**Response Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  securityProfileName |   SecurityProfileName  |  no |  The name of the security profile\. | 
|  securityProfileArn |   SecurityProfileArn  |  no |  The ARN of the security profile\. | 
|  securityProfileDescription |   SecurityProfileDescription  |  no |  A description of the security profile \(associated with the security profile when it was created or updated\)\.  | 
|  behaviors |   Behaviors  |  no |  Specifies the behaviors that, when violated by a device \(thing\), cause an alert\. | 
|  alertTargets |   AlertTargets  |  no |  Where the alerts are sent\. \(Alerts are always sent to the console\.\) | 
|  version |   Version  |  no |  The version of the security profile\. A new version is generated whenever the security profile is updated\.  | 
|  creationDate |   Timestamp  |  no |  The time the security profile was created\. | 
|  lastModifiedDate |   Timestamp  |  no |  The time the security profile was last modified\. | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.  
HTTP response code: 400

`ResourceNotFoundException`  
The specified resource does not exist\.  
HTTP response code: 404

`ThrottlingException`  
The rate exceeds the limit\.  
HTTP response code: 429

`InternalFailureException`  
An unexpected error has occurred\.  
HTTP response code: 500

### cli<a name="dd-api-iot-DescribeSecurityProfile-cli"></a>

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
        "durationSeconds": "integer"
      }
    }
  ],
  "alertTargets": {
    "string": {
      "alertTargetArn": "string",
      "roleArn": "string"
    }
  },
  "version": "long",
  "creationDate": "timestamp",
  "lastModifiedDate": "timestamp"
}
```


**cli output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  securityProfileName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the security profile\. | 
|  securityProfileArn |  string |  The ARN of the security profile\. | 
|  securityProfileDescription |  string  length\- max:1000  pattern: \[\\\\p\{Graph\} \]\*  |  A description of the security profile \(associated with the security profile when it was created or updated\)\.  | 
|  behaviors |  list  member: Behavior  |  Specifies the behaviors that, when violated by a device \(thing\), cause an alert\. | 
|  name |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name you have given to the behavior\. | 
|  metric |  string |  What is measured by the behavior\. | 
|  criteria |  BehaviorCriteria |  The criteria that determine if a device is behaving normally in regard to the `metric`\.  | 
|  comparisonOperator |  string |  The operator that relates the thing measured \(`metric`\) to the criteria \(`value`\)\.  enum: less\-than | less\-than\-equals | greater\-than | greater\-than\-equals | in\-cidr\-set | not\-in\-cidr\-set | in\-port\-set | not\-in\-port\-set  | 
|  value |  MetricValue |  The value to be compared with the `metric`\. | 
|  count |  long  range\- min:0  |  If the `comparisonOperator` calls for a numeric value, use this to specify that numeric value to be compared with the `metric`\.  | 
|  cidrs |  list  member: Cidr  |  If the `comparisonOperator` calls for a set of CIDRs, use this to specify that set to be compared with the `metric`\.  | 
|  ports |  list  member: Port  |  If the `comparisonOperator` calls for a set of ports, use this to specify that set to be compared with the `metric`\.  | 
|  durationSeconds |  integer |  Use this to specify the period of time over which the behavior is evaluated, for those criteria which have a time dimension \(for example, `NUM_MESSAGES_SENT`\)\.  | 
|  alertTargets |  map |  Where the alerts are sent\. \(Alerts are always sent to the console\.\) | 
|  alertTargetArn |  string |  The ARN of the notification target to which alerts are sent\. | 
|  roleArn |  string  length\- max:2048 min:20  |  The ARN of the role that grants permission to send alerts to the notification target\.  | 
|  version |  long |  The version of the security profile\. A new version is generated whenever the security profile is updated\.  | 
|  creationDate |  timestamp |  The time the security profile was created\. | 
|  lastModifiedDate |  timestamp |  The time the security profile was last modified\. | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ResourceNotFoundException`  
The specified resource does not exist\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## DetachSecurityProfile<a name="dd-api-iot-DetachSecurityProfile-doc"></a>

Disassociates an security profile from a thing group or from this account\.

### https<a name="dd-api-iot-DetachSecurityProfile-https"></a>

 **Request syntax:**

```
DELETE /security-profiles/securityProfileName/targets?securityProfileTargetArn=securityProfileTargetArn 
```


**URI Request Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  securityProfileName |  SecurityProfileName |  yes |  The security profile that is detached\. | 
|  securityProfileTargetArn |  SecurityProfileTargetArn |  yes |  The ARN of the thing group from which the security profile is detached\. | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.  
HTTP response code: 400

`ResourceNotFoundException`  
The specified resource does not exist\.  
HTTP response code: 404

`ThrottlingException`  
The rate exceeds the limit\.  
HTTP response code: 429

`InternalFailureException`  
An unexpected error has occurred\.  
HTTP response code: 500

### cli<a name="dd-api-iot-DetachSecurityProfile-cli"></a>

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
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ResourceNotFoundException`  
The specified resource does not exist\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## ListActiveViolations<a name="dd-api-iot-ListActiveViolations-doc"></a>

Lists the active violations for a given security profile\.

### https<a name="dd-api-iot-ListActiveViolations-https"></a>

 **Request syntax:**

```
GET /active-violations?maxResults=maxResults&nextToken=nextToken&thingName=thingName&securityProfileName=securityProfileName 
```


**URI Request Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  thingName |  ThingName |  no |  The name of the thing whose active violations are listed\. | 
|  securityProfileName |  SecurityProfileName |  no |  The name of the security profile for which violations are listed\. | 
|  nextToken |  NextToken |  no |  The token for the next set of results\. | 
|  maxResults |  MaxResults |  no |  The maximum number of results to return at one time\. | 

 **Response syntax:**

```
Content-type: application/json

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
          "durationSeconds": "integer"
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


**Response Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  activeViolations |   ActiveViolations  |  no |  The list of active violations\. | 
|  nextToken |   NextToken  |  no |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.  
HTTP response code: 400

`ResourceNotFoundException`  
The specified resource does not exist\.  
HTTP response code: 404

`ThrottlingException`  
The rate exceeds the limit\.  
HTTP response code: 429

`InternalFailureException`  
An unexpected error has occurred\.  
HTTP response code: 500

### cli<a name="dd-api-iot-ListActiveViolations-cli"></a>

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
|  securityProfileName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the security profile for which violations are listed\. | 
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
          "durationSeconds": "integer"
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


**cli output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  activeViolations |  list  member: ActiveViolation  |  The list of active violations\. | 
|  violationId |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9\-\]\+  |  The ID of the active violation\. | 
|  thingName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the thing responsible for the active violation\. | 
|  securityProfileName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The security profile whose behavior is in violation\. | 
|  behavior |  Behavior |  The behavior which is being violated\. | 
|  name |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name you have given to the behavior\. | 
|  metric |  string |  What is measured by the behavior\. | 
|  criteria |  BehaviorCriteria |  The criteria that determine if a device is behaving normally in regard to the `metric`\.  | 
|  comparisonOperator |  string |  The operator that relates the thing measured \(`metric`\) to the criteria \(`value`\)\.  enum: less\-than | less\-than\-equals | greater\-than | greater\-than\-equals | in\-cidr\-set | not\-in\-cidr\-set | in\-port\-set | not\-in\-port\-set  | 
|  value |  MetricValue |  The value to be compared with the `metric`\. | 
|  count |  long  range\- min:0  |  If the `comparisonOperator` calls for a numeric value, use this to specify that numeric value to be compared with the `metric`\.  | 
|  cidrs |  list  member: Cidr  |  If the `comparisonOperator` calls for a set of CIDRs, use this to specify that set to be compared with the `metric`\.  | 
|  ports |  list  member: Port  |  If the `comparisonOperator` calls for a set of ports, use this to specify that set to be compared with the `metric`\.  | 
|  durationSeconds |  integer |  Use this to specify the period of time over which the behavior is evaluated, for those criteria which have a time dimension \(for example, `NUM_MESSAGES_SENT`\)\.  | 
|  lastViolationValue |  MetricValue |  The value of the metric \(the measurement\) which caused the most recent violation\. | 
|  count |  long  range\- min:0  |  If the `comparisonOperator` calls for a numeric value, use this to specify that numeric value to be compared with the `metric`\.  | 
|  cidrs |  list  member: Cidr  |  If the `comparisonOperator` calls for a set of CIDRs, use this to specify that set to be compared with the `metric`\.  | 
|  ports |  list  member: Port  |  If the `comparisonOperator` calls for a set of ports, use this to specify that set to be compared with the `metric`\.  | 
|  lastViolationTime |  timestamp |  The time the most recent violation occurred\. | 
|  violationStartTime |  timestamp |  The time the violation started\. | 
|  nextToken |  string |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ResourceNotFoundException`  
The specified resource does not exist\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## ListSecurityProfiles<a name="dd-api-iot-ListSecurityProfiles-doc"></a>

Lists the security profiles you have created\. You can use filters to list only those security profiles associated with a thing group or only those associated with your account\.

### https<a name="dd-api-iot-ListSecurityProfiles-https"></a>

 **Request syntax:**

```
GET /security-profiles?maxResults=maxResults&nextToken=nextToken 
```


**URI Request Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  nextToken |  NextToken |  no |  The token for the next set of results\. | 
|  maxResults |  MaxResults |  no |  The maximum number of results to return at one time\. | 

 **Response syntax:**

```
Content-type: application/json

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


**Response Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  securityProfileIdentifiers |   SecurityProfileIdentifiers  |  no |  A list of security profile identifiers \(names and ARNs\)\. | 
|  nextToken |   NextToken  |  no |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.  
HTTP response code: 400

`ThrottlingException`  
The rate exceeds the limit\.  
HTTP response code: 429

`InternalFailureException`  
An unexpected error has occurred\.  
HTTP response code: 500

### cli<a name="dd-api-iot-ListSecurityProfiles-cli"></a>

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


**cli output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  securityProfileIdentifiers |  list  member: SecurityProfileIdentifier  java class: java\.util\.List  |  A list of security profile identifiers \(names and ARNs\)\. | 
|  name |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name you have given to the security profile\. | 
|  arn |  string |  The ARN of the security profile\. | 
|  nextToken |  string |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## ListSecurityProfilesForTarget<a name="dd-api-iot-ListSecurityProfilesForTarget-doc"></a>

Lists the security profiles attached to a target \(thing group\)\.

### https<a name="dd-api-iot-ListSecurityProfilesForTarget-https"></a>

 **Request syntax:**

```
GET /security-profiles-for-target?maxResults=maxResults&nextToken=nextToken&recursive=recursive&securityProfileTargetArn=securityProfileTargetArn 
```


**URI Request Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  nextToken |  NextToken |  no |  The token for the next set of results\. | 
|  maxResults |  MaxResults |  no |  The maximum number of results to return at one time\. | 
|  recursive |  Recursive |  no |  If true, return child groups as well\. | 
|  securityProfileTargetArn |  SecurityProfileTargetArn |  yes |  The ARN of the target \(thing group\) whose attached security profiles you want to get\. | 

 **Response syntax:**

```
Content-type: application/json

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


**Response Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  securityProfileTargetMappings |   SecurityProfileTargetMappings  |  no |  A list of security profiles and their associated targets\. | 
|  nextToken |   NextToken  |  no |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.  
HTTP response code: 400

`ThrottlingException`  
The rate exceeds the limit\.  
HTTP response code: 429

`InternalFailureException`  
An unexpected error has occurred\.  
HTTP response code: 500

`ResourceNotFoundException`  
The specified resource does not exist\.  
HTTP response code: 404

### cli<a name="dd-api-iot-ListSecurityProfilesForTarget-cli"></a>

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


**cli output fields:**  

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
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

`ResourceNotFoundException`  
The specified resource does not exist\.

## ListTargetsForSecurityProfile<a name="dd-api-iot-ListTargetsForSecurityProfile-doc"></a>

Lists the targets \(thing groups\) associated with a given security profile\.

### https<a name="dd-api-iot-ListTargetsForSecurityProfile-https"></a>

 **Request syntax:**

```
GET /security-profiles/securityProfileName/targets?maxResults=maxResults&nextToken=nextToken 
```


**URI Request Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  securityProfileName |  SecurityProfileName |  yes |  The security profile\. | 
|  nextToken |  NextToken |  no |  The token for the next set of results\. | 
|  maxResults |  MaxResults |  no |  The maximum number of results to return at one time\. | 

 **Response syntax:**

```
Content-type: application/json

{
  "securityProfileTargets": [
    {
      "arn": "string"
    }
  ],
  "nextToken": "string"
}
```


**Response Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  securityProfileTargets |   SecurityProfileTargets  |  no |  The thing groups to which the security profile is attached\. | 
|  nextToken |   NextToken  |  no |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.  
HTTP response code: 400

`ResourceNotFoundException`  
The specified resource does not exist\.  
HTTP response code: 404

`ThrottlingException`  
The rate exceeds the limit\.  
HTTP response code: 429

`InternalFailureException`  
An unexpected error has occurred\.  
HTTP response code: 500

### cli<a name="dd-api-iot-ListTargetsForSecurityProfile-cli"></a>

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


**cli output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  securityProfileTargets |  list  member: SecurityProfileTarget  java class: java\.util\.List  |  The thing groups to which the security profile is attached\. | 
|  arn |  string |  The ARN of the security profile\. | 
|  nextToken |  string |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ResourceNotFoundException`  
The specified resource does not exist\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## ListViolationEvents<a name="dd-api-iot-ListViolationEvents-doc"></a>

Lists the security profile violations discovered during the given time period\. You can use filters to limit the results to those alerts issued for a particular security profile, behavior or thing \(device\)\.

### https<a name="dd-api-iot-ListViolationEvents-https"></a>

 **Request syntax:**

```
GET /violation-events?maxResults=maxResults&nextToken=nextToken&startTime=startTime&endTime=endTime&thingName=thingName&securityProfileName=securityProfileName 
```


**URI Request Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  startTime |  Timestamp |  yes |  The start time for the alerts to be listed\. | 
|  endTime |  Timestamp |  yes |  The end time for the alerts to be listed\. | 
|  thingName |  ThingName |  no |  A filter to limit results to those alerts caused by the specified thing\. | 
|  securityProfileName |  SecurityProfileName |  no |  A filter to limit results to those alerts generated by the specified security profile\. | 
|  nextToken |  NextToken |  no |  The token for the next set of results\. | 
|  maxResults |  MaxResults |  no |  The maximum number of results to return at one time\. | 

 **Response syntax:**

```
Content-type: application/json

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
          "durationSeconds": "integer"
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


**Response Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  violationEvents |   ViolationEvents  |  no |  The security profile violation alerts issued for this account during the given time frame, potentially filtered by security profile, behavior violated, or thing \(device\) violating\.  | 
|  nextToken |   NextToken  |  no |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.  
HTTP response code: 400

`ThrottlingException`  
The rate exceeds the limit\.  
HTTP response code: 429

`InternalFailureException`  
An unexpected error has occurred\.  
HTTP response code: 500

### cli<a name="dd-api-iot-ListViolationEvents-cli"></a>

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
          "durationSeconds": "integer"
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


**cli output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  violationEvents |  list  member: ViolationEvent  |  The security profile violation alerts issued for this account during the given time frame, potentially filtered by security profile, behavior violated, or thing \(device\) violating\.  | 
|  violationId |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9\-\]\+  |  The ID of the violation event\. | 
|  thingName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the thing responsible for the violation event\. | 
|  securityProfileName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the security profile whose behavior was violated\. | 
|  behavior |  Behavior |  The behavior which was violated\. | 
|  name |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name you have given to the behavior\. | 
|  metric |  string |  What is measured by the behavior\. | 
|  criteria |  BehaviorCriteria |  The criteria that determine if a device is behaving normally in regard to the `metric`\.  | 
|  comparisonOperator |  string |  The operator that relates the thing measured \(`metric`\) to the criteria \(`value`\)\.  enum: less\-than | less\-than\-equals | greater\-than | greater\-than\-equals | in\-cidr\-set | not\-in\-cidr\-set | in\-port\-set | not\-in\-port\-set  | 
|  value |  MetricValue |  The value to be compared with the `metric`\. | 
|  count |  long  range\- min:0  |  If the `comparisonOperator` calls for a numeric value, use this to specify that numeric value to be compared with the `metric`\.  | 
|  cidrs |  list  member: Cidr  |  If the `comparisonOperator` calls for a set of CIDRs, use this to specify that set to be compared with the `metric`\.  | 
|  ports |  list  member: Port  |  If the `comparisonOperator` calls for a set of ports, use this to specify that set to be compared with the `metric`\.  | 
|  durationSeconds |  integer |  Use this to specify the period of time over which the behavior is evaluated, for those criteria which have a time dimension \(for example, `NUM_MESSAGES_SENT`\)\.  | 
|  metricValue |  MetricValue |  The value of the metric \(the measurement\)\. | 
|  count |  long  range\- min:0  |  If the `comparisonOperator` calls for a numeric value, use this to specify that numeric value to be compared with the `metric`\.  | 
|  cidrs |  list  member: Cidr  |  If the `comparisonOperator` calls for a set of CIDRs, use this to specify that set to be compared with the `metric`\.  | 
|  ports |  list  member: Port  |  If the `comparisonOperator` calls for a set of ports, use this to specify that set to be compared with the `metric`\.  | 
|  violationEventType |  string |  The type of violation event\.  enum: in\-alarm | alarm\-cleared | alarm\-invalidated  | 
|  violationEventTime |  timestamp |  The time the violation event occurred\. | 
|  nextToken |  string |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## UpdateSecurityProfile<a name="dd-api-iot-UpdateSecurityProfile-doc"></a>

Updates an security profile\.

### https<a name="dd-api-iot-UpdateSecurityProfile-https"></a>

 **Request syntax:**

```
PATCH /security-profiles/securityProfileName?expectedVersion=expectedVersion 
Content-type: application/json

{
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
        "durationSeconds": "integer"
      }
    }
  ],
  "alertTargets": {
    "string": {
      "alertTargetArn": "string",
      "roleArn": "string"
    }
  }
}
```


**URI Request Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  securityProfileName |  SecurityProfileName |  yes |  The name of the security profile you want to update\. | 
|  expectedVersion |  OptionalVersion |  no |  The expected version of the security profile\. A new version is generated whenever the security profile is updated\. If you specify a value that is different than the actual version, a `VersionConflictException` is thrown\.  | 


**Request Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  securityProfileDescription |  SecurityProfileDescription |  no |  A description of the security profile\. | 
|  behaviors |  Behaviors |  no |  Specifies the behaviors that, when violated by a device \(thing\), cause an alert\. | 
|  alertTargets |  AlertTargets |  no |  Where the alerts are sent\. \(Alerts are always sent to the console\.\) | 

 **Response syntax:**

```
Content-type: application/json

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
        "durationSeconds": "integer"
      }
    }
  ],
  "alertTargets": {
    "string": {
      "alertTargetArn": "string",
      "roleArn": "string"
    }
  },
  "version": "long",
  "creationDate": "timestamp",
  "lastModifiedDate": "timestamp"
}
```


**Response Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  securityProfileName |   SecurityProfileName  |  no |  The name of the security profile that was updated\. | 
|  securityProfileArn |   SecurityProfileArn  |  no |  The ARN of the security profile that was updated\. | 
|  securityProfileDescription |   SecurityProfileDescription  |  no |  The description of the security profile\. | 
|  behaviors |   Behaviors  |  no |  Specifies the behaviors that, when violated by a device \(thing\), cause an alert\. | 
|  alertTargets |   AlertTargets  |  no |  Where the alerts are sent\. \(Alerts are always sent to the console\.\) | 
|  version |   Version  |  no |  The updated version of the security profile\. | 
|  creationDate |   Timestamp  |  no |  The time the security profile was created\. | 
|  lastModifiedDate |   Timestamp  |  no |  The time the security profile was last modified\. | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.  
HTTP response code: 400

`ResourceNotFoundException`  
The specified resource does not exist\.  
HTTP response code: 404

`VersionConflictException`  
An exception thrown when the version of a thing passed to a command is different than the version specified with the \-\-version parameter\.  
HTTP response code: 409

`ThrottlingException`  
The rate exceeds the limit\.  
HTTP response code: 429

`InternalFailureException`  
An unexpected error has occurred\.  
HTTP response code: 500

### cli<a name="dd-api-iot-UpdateSecurityProfile-cli"></a>

 **Synopsis:**

```
aws iot  update-security-profile \
    --security-profile-name <value> \
    [--security-profile-description <value>] \
    [--behaviors <value>] \
    [--alert-targets <value>] \
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
        "durationSeconds": "integer"
      }
    }
  ],
  "alertTargets": {
    "string": {
      "alertTargetArn": "string",
      "roleArn": "string"
    }
  },
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
|  comparisonOperator |  string |  The operator that relates the thing measured \(`metric`\) to the criteria \(`value`\)\.  enum: less\-than | less\-than\-equals | greater\-than | greater\-than\-equals | in\-cidr\-set | not\-in\-cidr\-set | in\-port\-set | not\-in\-port\-set  | 
|  value |  MetricValue |  The value to be compared with the `metric`\. | 
|  count |  long  range\- min:0  |  If the `comparisonOperator` calls for a numeric value, use this to specify that numeric value to be compared with the `metric`\.  | 
|  cidrs |  list  member: Cidr  |  If the `comparisonOperator` calls for a set of CIDRs, use this to specify that set to be compared with the `metric`\.  | 
|  ports |  list  member: Port  |  If the `comparisonOperator` calls for a set of ports, use this to specify that set to be compared with the `metric`\.  | 
|  durationSeconds |  integer |  Use this to specify the period of time over which the behavior is evaluated, for those criteria which have a time dimension \(for example, `NUM_MESSAGES_SENT`\)\.  | 
|  alertTargets |  map |  Where the alerts are sent\. \(Alerts are always sent to the console\.\) | 
|  alertTargetArn |  string |  The ARN of the notification target to which alerts are sent\. | 
|  roleArn |  string  length\- max:2048 min:20  |  The ARN of the role that grants permission to send alerts to the notification target\.  | 
|  expectedVersion |  long |  The expected version of the security profile\. A new version is generated whenever the security profile is updated\. If you specify a value that is different than the actual version, a `VersionConflictException` is thrown\.  | 

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
        "durationSeconds": "integer"
      }
    }
  ],
  "alertTargets": {
    "string": {
      "alertTargetArn": "string",
      "roleArn": "string"
    }
  },
  "version": "long",
  "creationDate": "timestamp",
  "lastModifiedDate": "timestamp"
}
```


**cli output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  securityProfileName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name of the security profile that was updated\. | 
|  securityProfileArn |  string |  The ARN of the security profile that was updated\. | 
|  securityProfileDescription |  string  length\- max:1000  pattern: \[\\\\p\{Graph\} \]\*  |  The description of the security profile\. | 
|  behaviors |  list  member: Behavior  |  Specifies the behaviors that, when violated by a device \(thing\), cause an alert\. | 
|  name |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9:\_\-\]\+  |  The name you have given to the behavior\. | 
|  metric |  string |  What is measured by the behavior\. | 
|  criteria |  BehaviorCriteria |  The criteria that determine if a device is behaving normally in regard to the `metric`\.  | 
|  comparisonOperator |  string |  The operator that relates the thing measured \(`metric`\) to the criteria \(`value`\)\.  enum: less\-than | less\-than\-equals | greater\-than | greater\-than\-equals | in\-cidr\-set | not\-in\-cidr\-set | in\-port\-set | not\-in\-port\-set  | 
|  value |  MetricValue |  The value to be compared with the `metric`\. | 
|  count |  long  range\- min:0  |  If the `comparisonOperator` calls for a numeric value, use this to specify that numeric value to be compared with the `metric`\.  | 
|  cidrs |  list  member: Cidr  |  If the `comparisonOperator` calls for a set of CIDRs, use this to specify that set to be compared with the `metric`\.  | 
|  ports |  list  member: Port  |  If the `comparisonOperator` calls for a set of ports, use this to specify that set to be compared with the `metric`\.  | 
|  durationSeconds |  integer |  Use this to specify the period of time over which the behavior is evaluated, for those criteria which have a time dimension \(for example, `NUM_MESSAGES_SENT`\)\.  | 
|  alertTargets |  map |  Where the alerts are sent\. \(Alerts are always sent to the console\.\) | 
|  alertTargetArn |  string |  The ARN of the notification target to which alerts are sent\. | 
|  roleArn |  string  length\- max:2048 min:20  |  The ARN of the role that grants permission to send alerts to the notification target\.  | 
|  version |  long |  The updated version of the security profile\. | 
|  creationDate |  timestamp |  The time the security profile was created\. | 
|  lastModifiedDate |  timestamp |  The time the security profile was last modified\. | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ResourceNotFoundException`  
The specified resource does not exist\.

`VersionConflictException`  
An exception thrown when the version of a thing passed to a command is different than the version specified with the \-\-version parameter\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## ValidateSecurityProfileBehaviors<a name="dd-api-iot-ValidateSecurityProfileBehaviors-doc"></a>

Validates an security profile behaviors specification\.

### https<a name="dd-api-iot-ValidateSecurityProfileBehaviors-https"></a>

 **Request syntax:**

```
POST /security-profile-behaviors/validate 
Content-type: application/json

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
        "durationSeconds": "integer"
      }
    }
  ]
}
```


**Request Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  behaviors |  Behaviors |  yes |  Specifies the behaviors that, when violated by a device \(thing\), cause an alert\. | 

 **Response syntax:**

```
Content-type: application/json

{
  "valid": "boolean",
  "validationErrors": [
    {
      "errorMessage": "string"
    }
  ]
}
```


**Response Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  valid |   Valid  |  no |  True if the behaviors were valid\. | 
|  validationErrors |   ValidationErrors  |  no |  The list of any errors found in the behaviors\. | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.  
HTTP response code: 400

`ThrottlingException`  
The rate exceeds the limit\.  
HTTP response code: 429

`InternalFailureException`  
An unexpected error has occurred\.  
HTTP response code: 500

### cli<a name="dd-api-iot-ValidateSecurityProfileBehaviors-cli"></a>

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
        "durationSeconds": "integer"
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
|  comparisonOperator |  string |  The operator that relates the thing measured \(`metric`\) to the criteria \(`value`\)\.  enum: less\-than | less\-than\-equals | greater\-than | greater\-than\-equals | in\-cidr\-set | not\-in\-cidr\-set | in\-port\-set | not\-in\-port\-set  | 
|  value |  MetricValue |  The value to be compared with the `metric`\. | 
|  count |  long  range\- min:0  |  If the `comparisonOperator` calls for a numeric value, use this to specify that numeric value to be compared with the `metric`\.  | 
|  cidrs |  list  member: Cidr  |  If the `comparisonOperator` calls for a set of CIDRs, use this to specify that set to be compared with the `metric`\.  | 
|  ports |  list  member: Port  |  If the `comparisonOperator` calls for a set of ports, use this to specify that set to be compared with the `metric`\.  | 
|  durationSeconds |  integer |  Use this to specify the period of time over which the behavior is evaluated, for those criteria which have a time dimension \(for example, `NUM_MESSAGES_SENT`\)\.  | 

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


**cli output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  valid |  boolean |  True if the behaviors were valid\. | 
|  validationErrors |  list  member: ValidationError  |  The list of any errors found in the behaviors\. | 
|  errorMessage |  string  length\- max:2048  |  The description of an error found in the behaviors\. | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.