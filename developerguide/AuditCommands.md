# Audit Commands<a name="AuditCommands"></a>

## Manage Audit Settings<a name="AuditCommandsManageSettings"></a>

Configure audit settings for your account using `UpdateAccountAuditConfiguration`\. This command allows you to enable those checks you want to be available for audits, set up optional notifications, and configure permissions\. 

Check these settings with `DescribeAccountAuditConfiguration`\.

Use `DeleteAccountAuditConfiguration` to delete your audit settings\. This restores all default values, and effectively disables audits since all checks are disabled by default\.

### UpdateAccountAuditConfiguration<a name="dd-api-iot-UpdateAccountAuditConfiguration-doc"></a>

Configures or reconfigures the audit settings for this account\. Settings include how audit notifications are sent and which audit checks are enabled or disabled\.

#### https<a name="dd-api-iot-UpdateAccountAuditConfiguration-https"></a>

 **Request syntax:**

```
PATCH /audit/configuration 
Content-type: application/json

{
  "roleArn": "string",
  "auditNotificationTargetConfigurations": {
    "string": {
      "targetArn": "string",
      "roleArn": "string",
      "enabled": "boolean"
    }
  },
  "auditCheckConfigurations": {
    "string": {
      "enabled": "boolean"
    }
  }
}
```


**Request Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  roleArn |  RoleArn |  no |  The ARN of the role that grants permission to AWS IoT to access information about your devices, policies, certificates and other items as necessary when performing an audit\.  | 
|  auditNotificationTargetConfigurations |  AuditNotificationTargetConfigurations |  no |  Information about the targets to which audit notifications are sent\. | 
|  auditCheckConfigurations |  AuditCheckConfigurations |  no |  Specifies which audit checks are enabled and disabled for this account\. Use `DescribeAccountAuditConfiguration` to see the list of all checks including those that are currently enabled\. Note that some data collection may begin immediately when certain checks are enabled\. When a check is disabled, any data collected so far in relation to the check is deleted\. You cannot disable a check if it is used by any scheduled audit\. You must first delete the check from the scheduled audit or delete the scheduled audit itself\. On the first call to `UpdateAccountAuditConfiguration` this parameter is required and must specify at least one enabled check\.  | 

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

#### cli<a name="dd-api-iot-UpdateAccountAuditConfiguration-cli"></a>

 **Synopsis:**

```
aws iot  update-account-audit-configuration \
    [--role-arn <value>] \
    [--audit-notification-target-configurations <value>] \
    [--audit-check-configurations <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "roleArn": "string",
  "auditNotificationTargetConfigurations": {
    "string": {
      "targetArn": "string",
      "roleArn": "string",
      "enabled": "boolean"
    }
  },
  "auditCheckConfigurations": {
    "string": {
      "enabled": "boolean"
    }
  }
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  roleArn |  string  length\- max:2048 min:20  |  The ARN of the role that grants permission to AWS IoT to access information about your devices, policies, certificates and other items as necessary when performing an audit\.  | 
|  auditNotificationTargetConfigurations |  map |  Information about the targets to which audit notifications are sent\. | 
|  targetArn |  string |  The ARN of the target \(SNS topic\) to which audit notifications are sent\. | 
|  roleArn |  string  length\- max:2048 min:20  |  The ARN of the role that grants permission to send notifications to the target\. | 
|  enabled |  boolean |  True if notifications to the target are enabled\. | 
|  auditCheckConfigurations |  map |  Specifies which audit checks are enabled and disabled for this account\. Use `DescribeAccountAuditConfiguration` to see the list of all checks including those that are currently enabled\. Note that some data collection may begin immediately when certain checks are enabled\. When a check is disabled, any data collected so far in relation to the check is deleted\. You cannot disable a check if it is used by any scheduled audit\. You must first delete the check from the scheduled audit or delete the scheduled audit itself\. On the first call to `UpdateAccountAuditConfiguration` this parameter is required and must specify at least one enabled check\.  | 
|  enabled |  boolean |  True if this audit check is enabled for this account\. | 

Output:

None

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

### DescribeAccountAuditConfiguration<a name="dd-api-iot-DescribeAccountAuditConfiguration-doc"></a>

#### https<a name="dd-api-iot-DescribeAccountAuditConfiguration-https"></a>

 **Request syntax:**

```
GET /audit/configuration 
```

 **Response syntax:**

```
Content-type: application/json

{
  "roleArn": "string",
  "auditNotificationTargetConfigurations": {
    "string": {
      "targetArn": "string",
      "roleArn": "string",
      "enabled": "boolean"
    }
  },
  "auditCheckConfigurations": {
    "string": {
      "enabled": "boolean"
    }
  }
}
```


**Response Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  roleArn |   RoleArn  |  no |  The ARN of the role that grants permission to AWS IoT to access information about your devices, policies, certificates and other items as necessary when performing an audit\. On the first call to `UpdateAccountAuditConfiguration` this parameter is required\.  | 
|  auditNotificationTargetConfigurations |   AuditNotificationTargetConfigurations  |  no |  Information about the targets to which audit notifications are sent for this account\.  | 
|  auditCheckConfigurations |   AuditCheckConfigurations  |  no |  Which audit checks are enabled and disabled for this account\. | 

 **Errors:**

`ThrottlingException`  
The rate exceeds the limit\.  
HTTP response code: 429

`InternalFailureException`  
An unexpected error has occurred\.  
HTTP response code: 500

#### cli<a name="dd-api-iot-DescribeAccountAuditConfiguration-cli"></a>

 **Synopsis:**

```
aws iot  describe-account-audit-configuration  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
}
```

Output:

```
{
  "roleArn": "string",
  "auditNotificationTargetConfigurations": {
    "string": {
      "targetArn": "string",
      "roleArn": "string",
      "enabled": "boolean"
    }
  },
  "auditCheckConfigurations": {
    "string": {
      "enabled": "boolean"
    }
  }
}
```


**cli output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  roleArn |  string  length\- max:2048 min:20  |  The ARN of the role that grants permission to AWS IoT to access information about your devices, policies, certificates and other items as necessary when performing an audit\. On the first call to `UpdateAccountAuditConfiguration` this parameter is required\.  | 
|  auditNotificationTargetConfigurations |  map |  Information about the targets to which audit notifications are sent for this account\.  | 
|  targetArn |  string |  The ARN of the target \(SNS topic\) to which audit notifications are sent\. | 
|  roleArn |  string  length\- max:2048 min:20  |  The ARN of the role that grants permission to send notifications to the target\. | 
|  enabled |  boolean |  True if notifications to the target are enabled\. | 
|  auditCheckConfigurations |  map |  Which audit checks are enabled and disabled for this account\. | 
|  enabled |  boolean |  True if this audit check is enabled for this account\. | 

 **Errors:**

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

### DeleteAccountAuditConfiguration<a name="dd-api-iot-DeleteAccountAuditConfiguration-doc"></a>

Restores the default settings for audits for this account\. Any configuration data you entered is deleted and all audit checks are reset to disabled\.  

#### https<a name="dd-api-iot-DeleteAccountAuditConfiguration-https"></a>

 **Request syntax:**

```
DELETE /audit/configuration?deleteScheduledAudits=deleteScheduledAudits 
```


**URI Request Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  deleteScheduledAudits |  DeleteScheduledAudits |  no |  If true, all scheduled audits are deleted\. | 

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

#### cli<a name="dd-api-iot-DeleteAccountAuditConfiguration-cli"></a>

 **Synopsis:**

```
aws iot  delete-account-audit-configuration \
    [--delete-scheduled-audits | --no-delete-scheduled-audits]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "deleteScheduledAudits": "boolean"
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  deleteScheduledAudits |  boolean |  If true, all scheduled audits are deleted\. | 

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

## Schedule Audits<a name="device-defender-AuditCommandsManageSchedules"></a>

Create one or more scheduled audits using `CreateScheduledAudit`\. This command allows you to specify the checks you want to perform during an audit and how often the audit should be run\. 

Keep track of your scheduled audits with `ListScheduledAudits` and `DescribeScheduledAudit`\. 

Change an existing scheduled audit with `UpdateScheduledAudit` or delete it with `DeleteScheduledAudit`\. 

### CreateScheduledAudit<a name="dd-api-iot-CreateScheduledAudit-doc"></a>

Creates a scheduled audit that is run at a specified time interval\.

#### https<a name="dd-api-iot-CreateScheduledAudit-https"></a>

 **Request syntax:**

```
POST /audit/scheduledaudits/scheduledAuditName 
Content-type: application/json

{
  "frequency": "string",
  "dayOfMonth": "string",
  "dayOfWeek": "string",
  "targetCheckNames": [
    "string"
  ]
}
```


**URI Request Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  scheduledAuditName |  ScheduledAuditName |  yes |  The name you want to give to the scheduled audit\. \(Max\. 128 chars\) | 


**Request Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  frequency |  AuditFrequency |  yes |  How often the scheduled audit takes place\. Can be one of "DAILY", "WEEKLY", "BIWEEKLY" or "MONTHLY"\. The actual start time of each audit is determined by the system\.  | 
|  dayOfMonth |  DayOfMonth |  no |  The day of the month on which the scheduled audit takes place\. Can be "1" through "31" or "LAST"\. This field is required if the "frequency" parameter is set to "MONTHLY"\. If days 29\-31 are specified, and the month does not have that many days, the audit takes place on the "LAST" day of the month\.  | 
|  dayOfWeek |  DayOfWeek |  no |  The day of the week on which the scheduled audit takes place\. Can be one of "SUN", "MON", "TUE", "WED", "THU", "FRI" or "SAT"\. This field is required if the "frequency" parameter is set to "WEEKLY" or "BIWEEKLY"\.  | 
|  targetCheckNames |  TargetAuditCheckNames |  yes |  Which checks are performed during the scheduled audit\. Checks must be enabled for your account\. \(Use `DescribeAccountAuditConfiguration` to see the list of all checks including those that are enabled or `UpdateAccountAuditConfiguration` to select which checks are enabled\.\)  | 

 **Response syntax:**

```
Content-type: application/json

{
  "scheduledAuditArn": "string"
}
```


**Response Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  scheduledAuditArn |   ScheduledAuditArn  |  no |  The ARN of the scheduled audit\. | 

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

`LimitExceededException`  
A limit has been exceeded\.  
HTTP response code: 410

#### cli<a name="dd-api-iot-CreateScheduledAudit-cli"></a>

 **Synopsis:**

```
aws iot  create-scheduled-audit \
    --frequency <value> \
    [--day-of-month <value>] \
    [--day-of-week <value>] \
    --target-check-names <value> \
    --scheduled-audit-name <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "frequency": "string",
  "dayOfMonth": "string",
  "dayOfWeek": "string",
  "targetCheckNames": [
    "string"
  ],
  "scheduledAuditName": "string"
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  frequency |  string |  How often the scheduled audit takes place\. Can be one of "DAILY", "WEEKLY", "BIWEEKLY" or "MONTHLY"\. The actual start time of each audit is determined by the system\.  enum: DAILY | WEEKLY | BIWEEKLY | MONTHLY  | 
|  dayOfMonth |  string  pattern: ^\(\[1\-9\]|\[12\]\[0\-9\]|3\[01\]\)$|^LAST$  |  The day of the month on which the scheduled audit takes place\. Can be "1" through "31" or "LAST"\. This field is required if the "frequency" parameter is set to "MONTHLY"\. If days 29\-31 are specified, and the month does not have that many days, the audit takes place on the "LAST" day of the month\.  | 
|  dayOfWeek |  string |  The day of the week on which the scheduled audit takes place\. Can be one of "SUN", "MON", "TUE", "WED", "THU", "FRI" or "SAT"\. This field is required if the "frequency" parameter is set to "WEEKLY" or "BIWEEKLY"\.  enum: SUN | MON | TUE | WED | THU | FRI | SAT  | 
|  targetCheckNames |  list  member: AuditCheckName  |  Which checks are performed during the scheduled audit\. Checks must be enabled for your account\. \(Use `DescribeAccountAuditConfiguration` to see the list of all checks including those that are enabled or `UpdateAccountAuditConfiguration` to select which checks are enabled\.\)  | 
|  scheduledAuditName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The name you want to give to the scheduled audit\. \(Max\. 128 chars\) | 

Output:

```
{
  "scheduledAuditArn": "string"
}
```


**cli output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  scheduledAuditArn |  string |  The ARN of the scheduled audit\. | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

`LimitExceededException`  
A limit has been exceeded\.

### ListScheduledAudits<a name="dd-api-iot-ListScheduledAudits-doc"></a>

Lists all of your scheduled audits\.

#### https<a name="dd-api-iot-ListScheduledAudits-https"></a>

 **Request syntax:**

```
GET /audit/scheduledaudits?maxResults=maxResults&nextToken=nextToken 
```


**URI Request Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  nextToken |  NextToken |  no |  The token for the next set of results\. | 
|  maxResults |  MaxResults |  no |  The maximum number of results to return at one time\. The default is 25\. | 

 **Response syntax:**

```
Content-type: application/json

{
  "scheduledAudits": [
    {
      "scheduledAuditName": "string",
      "scheduledAuditArn": "string",
      "frequency": "string",
      "dayOfMonth": "string",
      "dayOfWeek": "string"
    }
  ],
  "nextToken": "string"
}
```


**Response Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  scheduledAudits |   ScheduledAuditMetadataList  |  no |  The list of scheduled audits\. | 
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

#### cli<a name="dd-api-iot-ListScheduledAudits-cli"></a>

 **Synopsis:**

```
aws iot  list-scheduled-audits \
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
|  maxResults |  integer  range\- max:250 min:1  |  The maximum number of results to return at one time\. The default is 25\. | 

Output:

```
{
  "scheduledAudits": [
    {
      "scheduledAuditName": "string",
      "scheduledAuditArn": "string",
      "frequency": "string",
      "dayOfMonth": "string",
      "dayOfWeek": "string"
    }
  ],
  "nextToken": "string"
}
```


**cli output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  scheduledAudits |  list  member: ScheduledAuditMetadata  java class: java\.util\.List  |  The list of scheduled audits\. | 
|  scheduledAuditName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The name of the scheduled audit\. | 
|  scheduledAuditArn |  string |  The ARN of the scheduled audit\. | 
|  frequency |  string |  How often the scheduled audit takes place\.  enum: DAILY | WEEKLY | BIWEEKLY | MONTHLY  | 
|  dayOfMonth |  string  pattern: ^\(\[1\-9\]|\[12\]\[0\-9\]|3\[01\]\)$|^LAST$  |  The day of the month on which the scheduled audit is run \(if the `frequency` is "MONTHLY"\)\. If days 29\-31 are specified, and the month does not have that many days, the audit takes place on the "LAST" day of the month\.  | 
|  dayOfWeek |  string |  The day of the week on which the scheduled audit is run \(if the `frequency` is "WEEKLY" or "BIWEEKLY"\)\.  enum: SUN | MON | TUE | WED | THU | FRI | SAT  | 
|  nextToken |  string |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

### DescribeScheduledAudit<a name="dd-api-iot-DescribeScheduledAudit-doc"></a>

Gets information about a scheduled audit\.

#### https<a name="dd-api-iot-DescribeScheduledAudit-https"></a>

 **Request syntax:**

```
GET /audit/scheduledaudits/scheduledAuditName 
```


**URI Request Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  scheduledAuditName |  ScheduledAuditName |  yes |  The name of the scheduled audit whose information you want to get\. | 

 **Response syntax:**

```
Content-type: application/json

{
  "frequency": "string",
  "dayOfMonth": "string",
  "dayOfWeek": "string",
  "targetCheckNames": [
    "string"
  ],
  "scheduledAuditName": "string",
  "scheduledAuditArn": "string"
}
```


**Response Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  frequency |   AuditFrequency  |  no |  How often the scheduled audit takes place\. One of "DAILY", "WEEKLY", "BIWEEKLY" or "MONTHLY"\. The actual start time of each audit is determined by the system\.  | 
|  dayOfMonth |   DayOfMonth  |  no |  The day of the month on which the scheduled audit takes place\. Will be "1" through "31" or "LAST"\. If days 29\-31 are specified, and the month does not have that many days, the audit takes place on the "LAST" day of the month\.  | 
|  dayOfWeek |   DayOfWeek  |  no |  The day of the week on which the scheduled audit takes place\. One of "SUN", "MON", "TUE", "WED", "THU", "FRI" or "SAT"\.  | 
|  targetCheckNames |   TargetAuditCheckNames  |  no |  Which checks are performed during the scheduled audit\. \(Note that checks must be enabled for your account\. \(Use `DescribeAccountAuditConfiguration` to see the list of all checks including those that are enabled or `UpdateAccountAuditConfiguration` to select which checks are enabled\.\)  | 
|  scheduledAuditName |   ScheduledAuditName  |  no |  The name of the scheduled audit\. | 
|  scheduledAuditArn |   ScheduledAuditArn  |  no |  The ARN of the scheduled audit\. | 

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

#### cli<a name="dd-api-iot-DescribeScheduledAudit-cli"></a>

 **Synopsis:**

```
aws iot  describe-scheduled-audit \
    --scheduled-audit-name <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "scheduledAuditName": "string"
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  scheduledAuditName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The name of the scheduled audit whose information you want to get\. | 

Output:

```
{
  "frequency": "string",
  "dayOfMonth": "string",
  "dayOfWeek": "string",
  "targetCheckNames": [
    "string"
  ],
  "scheduledAuditName": "string",
  "scheduledAuditArn": "string"
}
```


**cli output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  frequency |  string |  How often the scheduled audit takes place\. One of "DAILY", "WEEKLY", "BIWEEKLY" or "MONTHLY"\. The actual start time of each audit is determined by the system\.  enum: DAILY | WEEKLY | BIWEEKLY | MONTHLY  | 
|  dayOfMonth |  string  pattern: ^\(\[1\-9\]|\[12\]\[0\-9\]|3\[01\]\)$|^LAST$  |  The day of the month on which the scheduled audit takes place\. Will be "1" through "31" or "LAST"\. If days 29\-31 are specified, and the month does not have that many days, the audit takes place on the "LAST" day of the month\.  | 
|  dayOfWeek |  string |  The day of the week on which the scheduled audit takes place\. One of "SUN", "MON", "TUE", "WED", "THU", "FRI" or "SAT"\.  enum: SUN | MON | TUE | WED | THU | FRI | SAT  | 
|  targetCheckNames |  list  member: AuditCheckName  |  Which checks are performed during the scheduled audit\. \(Note that checks must be enabled for your account\. \(Use `DescribeAccountAuditConfiguration` to see the list of all checks including those that are enabled or `UpdateAccountAuditConfiguration` to select which checks are enabled\.\)  | 
|  scheduledAuditName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The name of the scheduled audit\. | 
|  scheduledAuditArn |  string |  The ARN of the scheduled audit\. | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ResourceNotFoundException`  
The specified resource does not exist\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

### UpdateScheduledAudit<a name="dd-api-iot-UpdateScheduledAudit-doc"></a>

Updates a scheduled audit, including what checks are performed and how often the audit takes place\.

#### https<a name="dd-api-iot-UpdateScheduledAudit-https"></a>

 **Request syntax:**

```
PATCH /audit/scheduledaudits/scheduledAuditName 
Content-type: application/json

{
  "frequency": "string",
  "dayOfMonth": "string",
  "dayOfWeek": "string",
  "targetCheckNames": [
    "string"
  ]
}
```


**URI Request Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  scheduledAuditName |  ScheduledAuditName |  yes |  The name of the scheduled audit\. \(Max\. 128 chars\) | 


**Request Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  frequency |  AuditFrequency |  no |  How often the scheduled audit takes place\. Can be one of "DAILY", "WEEKLY", "BIWEEKLY" or "MONTHLY"\. The actual start time of each audit is determined by the system\.  | 
|  dayOfMonth |  DayOfMonth |  no |  The day of the month on which the scheduled audit takes place\. Can be "1" through "31" or "LAST"\. This field is required if the "frequency" parameter is set to "MONTHLY"\. If days 29\-31 are specified, and the month does not have that many days, the audit takes place on the "LAST" day of the month\.  | 
|  dayOfWeek |  DayOfWeek |  no |  The day of the week on which the scheduled audit takes place\. Can be one of "SUN", "MON", "TUE", "WED", "THU", "FRI" or "SAT"\. This field is required if the "frequency" parameter is set to "WEEKLY" or "BIWEEKLY"\.  | 
|  targetCheckNames |  TargetAuditCheckNames |  no |  Which checks are performed during the scheduled audit\. Checks must be enabled for your account\. \(Use `DescribeAccountAuditConfiguration` to see the list of all checks including those that are enabled or `UpdateAccountAuditConfiguration` to select which checks are enabled\.\)  | 

 **Response syntax:**

```
Content-type: application/json

{
  "scheduledAuditArn": "string"
}
```


**Response Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  scheduledAuditArn |   ScheduledAuditArn  |  no |  The ARN of the scheduled audit\. | 

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

#### cli<a name="dd-api-iot-UpdateScheduledAudit-cli"></a>

 **Synopsis:**

```
aws iot  update-scheduled-audit \
    [--frequency <value>] \
    [--day-of-month <value>] \
    [--day-of-week <value>] \
    [--target-check-names <value>] \
    --scheduled-audit-name <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "frequency": "string",
  "dayOfMonth": "string",
  "dayOfWeek": "string",
  "targetCheckNames": [
    "string"
  ],
  "scheduledAuditName": "string"
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  frequency |  string |  How often the scheduled audit takes place\. Can be one of "DAILY", "WEEKLY", "BIWEEKLY" or "MONTHLY"\. The actual start time of each audit is determined by the system\.  enum: DAILY | WEEKLY | BIWEEKLY | MONTHLY  | 
|  dayOfMonth |  string  pattern: ^\(\[1\-9\]|\[12\]\[0\-9\]|3\[01\]\)$|^LAST$  |  The day of the month on which the scheduled audit takes place\. Can be "1" through "31" or "LAST"\. This field is required if the "frequency" parameter is set to "MONTHLY"\. If days 29\-31 are specified, and the month does not have that many days, the audit takes place on the "LAST" day of the month\.  | 
|  dayOfWeek |  string |  The day of the week on which the scheduled audit takes place\. Can be one of "SUN", "MON", "TUE", "WED", "THU", "FRI" or "SAT"\. This field is required if the "frequency" parameter is set to "WEEKLY" or "BIWEEKLY"\.  enum: SUN | MON | TUE | WED | THU | FRI | SAT  | 
|  targetCheckNames |  list  member: AuditCheckName  |  Which checks are performed during the scheduled audit\. Checks must be enabled for your account\. \(Use `DescribeAccountAuditConfiguration` to see the list of all checks including those that are enabled or `UpdateAccountAuditConfiguration` to select which checks are enabled\.\)  | 
|  scheduledAuditName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The name of the scheduled audit\. \(Max\. 128 chars\) | 

Output:

```
{
  "scheduledAuditArn": "string"
}
```


**cli output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  scheduledAuditArn |  string |  The ARN of the scheduled audit\. | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ResourceNotFoundException`  
The specified resource does not exist\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

### DeleteScheduledAudit<a name="dd-api-iot-DeleteScheduledAudit-doc"></a>

Deletes a scheduled audit\.

#### https<a name="dd-api-iot-DeleteScheduledAudit-https"></a>

 **Request syntax:**

```
DELETE /audit/scheduledaudits/scheduledAuditName 
```


**URI Request Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  scheduledAuditName |  ScheduledAuditName |  yes |  The name of the scheduled audit you want to delete\. | 

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

#### cli<a name="dd-api-iot-DeleteScheduledAudit-cli"></a>

 **Synopsis:**

```
aws iot  delete-scheduled-audit \
    --scheduled-audit-name <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "scheduledAuditName": "string"
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  scheduledAuditName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The name of the scheduled audit you want to delete\. | 

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

## Run An On\-Demand Audit<a name="device-defender-AuditCommandsOnDemand"></a>

Use `StartOnDemandAuditTask` to specify the checks you want to perform and start an audit running right away\.

### StartOnDemandAuditTask<a name="dd-api-iot-StartOnDemandAuditTask-doc"></a>

Starts an on\-demand audit\.

#### https<a name="dd-api-iot-StartOnDemandAuditTask-https"></a>

 **Request syntax:**

```
POST /audit/tasks 
Content-type: application/json

{
  "targetCheckNames": [
    "string"
  ]
}
```


**Request Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  targetCheckNames |  TargetAuditCheckNames |  yes |  Which checks are performed during the audit\. The checks you specify must be enabled for your account or an exception occurs\. Use `DescribeAccountAuditConfiguration` to see the list of all checks including those that are enabled or `UpdateAccountAuditConfiguration` to select which checks are enabled\.  | 

 **Response syntax:**

```
Content-type: application/json

{
  "taskId": "string"
}
```


**Response Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  taskId |   AuditTaskId  |  no |  The ID of the on\-demand audit you started\. | 

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

`LimitExceededException`  
A limit has been exceeded\.  
HTTP response code: 410

#### cli<a name="dd-api-iot-StartOnDemandAuditTask-cli"></a>

 **Synopsis:**

```
aws iot  start-on-demand-audit-task \
    --target-check-names <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "targetCheckNames": [
    "string"
  ]
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  targetCheckNames |  list  member: AuditCheckName  |  Which checks are performed during the audit\. The checks you specify must be enabled for your account or an exception occurs\. Use `DescribeAccountAuditConfiguration` to see the list of all checks including those that are enabled or `UpdateAccountAuditConfiguration` to select which checks are enabled\.  | 

Output:

```
{
  "taskId": "string"
}
```


**cli output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  taskId |  string  length\- max:40 min:1  pattern: \[a\-zA\-Z0\-9\-\]\+  |  The ID of the on\-demand audit you started\. | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

`LimitExceededException`  
A limit has been exceeded\.

## Manage Audit Instances<a name="device-defender-AuditCommandsManageInstances"></a>

Use `DescribeAuditTask` to get information about a specific audit instance\. If it has already run, the results include which checks failed and which passed, those that the system was unable to complete, and if the audit is still in progress, those it is still working on\.

Use `ListAuditTasks` to find the audits that were run during a specific time interval\.

Use `CancelAuditTask` to halt an audit in progress\.

### DescribeAuditTask<a name="dd-api-iot-DescribeAuditTask-doc"></a>

Gets information about an audit\.

#### https<a name="dd-api-iot-DescribeAuditTask-https"></a>

 **Request syntax:**

```
GET /audit/tasks/taskId 
```


**URI Request Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  taskId |  AuditTaskId |  yes |  The ID of the audit whose information you want to get\. | 

 **Response syntax:**

```
Content-type: application/json

{
  "taskStatus": "string",
  "taskType": "string",
  "taskStartTime": "timestamp",
  "taskStatistics": {
    "totalChecks": "integer",
    "inProgressChecks": "integer",
    "waitingForDataCollectionChecks": "integer",
    "compliantChecks": "integer",
    "nonCompliantChecks": "integer",
    "failedChecks": "integer",
    "canceledChecks": "integer"
  },
  "scheduledAuditName": "string",
  "auditDetails": {
    "string": {
      "checkRunStatus": "string",
      "checkCompliant": "boolean",
      "totalResourcesCount": "long",
      "nonCompliantResourcesCount": "long",
      "errorCode": "string",
      "message": "string"
    }
  }
}
```


**Response Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  taskStatus |   AuditTaskStatus  |  no |  The status of the audit: one of "IN\_PROGRESS", "COMPLETED", "FAILED", or "CANCELED"\.  | 
|  taskType |   AuditTaskType  |  no |  The type of audit: "ON\_DEMAND\_AUDIT\_TASK" or "SCHEDULED\_AUDIT\_TASK"\. | 
|  taskStartTime |   Timestamp  |  no |  The time the audit started\. | 
|  taskStatistics |   TaskStatistics  |  no |  Statistical information about the audit\. | 
|  scheduledAuditName |   ScheduledAuditName  |  no |  The name of the scheduled audit \(only if the audit was a scheduled audit\)\. | 
|  auditDetails |   AuditDetails  |  no |  Detailed information about each check performed during this audit\. | 

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

#### cli<a name="dd-api-iot-DescribeAuditTask-cli"></a>

 **Synopsis:**

```
aws iot  describe-audit-task \
    --task-id <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "taskId": "string"
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  taskId |  string  length\- max:40 min:1  pattern: \[a\-zA\-Z0\-9\-\]\+  |  The ID of the audit whose information you want to get\. | 

Output:

```
{
  "taskStatus": "string",
  "taskType": "string",
  "taskStartTime": "timestamp",
  "taskStatistics": {
    "totalChecks": "integer",
    "inProgressChecks": "integer",
    "waitingForDataCollectionChecks": "integer",
    "compliantChecks": "integer",
    "nonCompliantChecks": "integer",
    "failedChecks": "integer",
    "canceledChecks": "integer"
  },
  "scheduledAuditName": "string",
  "auditDetails": {
    "string": {
      "checkRunStatus": "string",
      "checkCompliant": "boolean",
      "totalResourcesCount": "long",
      "nonCompliantResourcesCount": "long",
      "errorCode": "string",
      "message": "string"
    }
  }
}
```


**cli output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  taskStatus |  string |  The status of the audit: one of "IN\_PROGRESS", "COMPLETED", "FAILED", or "CANCELED"\.  enum: IN\_PROGRESS | COMPLETED | FAILED | CANCELED  | 
|  taskType |  string |  The type of audit: "ON\_DEMAND\_AUDIT\_TASK" or "SCHEDULED\_AUDIT\_TASK"\.  enum: ON\_DEMAND\_AUDIT\_TASK | SCHEDULED\_AUDIT\_TASK  | 
|  taskStartTime |  timestamp |  The time the audit started\. | 
|  taskStatistics |  TaskStatistics |  Statistical information about the audit\. | 
|  totalChecks |  integer |  The number of checks in this audit\. | 
|  inProgressChecks |  integer |  The number of checks in progress\. | 
|  waitingForDataCollectionChecks |  integer |  The number of checks waiting for data collection\. | 
|  compliantChecks |  integer |  The number of checks that found compliant resources\. | 
|  nonCompliantChecks |  integer |  The number of checks that found non\-compliant resources\. | 
|  failedChecks |  integer |  The number of checks  | 
|  canceledChecks |  integer |  The number of checks that did not run because the audit was canceled\. | 
|  scheduledAuditName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The name of the scheduled audit \(only if the audit was a scheduled audit\)\. | 
|  auditDetails |  map |  Detailed information about each check performed during this audit\. | 
|  checkRunStatus |  string |  The completion status of this check, one of "IN\_PROGRESS", "WAITING\_FOR\_DATA\_COLLECTION", "CANCELED", "COMPLETED\_COMPLIANT", "COMPLETED\_NON\_COMPLIANT", or "FAILED"\.  enum: IN\_PROGRESS | WAITING\_FOR\_DATA\_COLLECTION | CANCELED | COMPLETED\_COMPLIANT | COMPLETED\_NON\_COMPLIANT | FAILED  | 
|  checkCompliant |  boolean |  True if the check completed and found all resources compliant\. | 
|  totalResourcesCount |  long |  The number of resources on which the check was performed\. | 
|  nonCompliantResourcesCount |  long |  The number of resources that the check found non\-compliant\. | 
|  errorCode |  string |  The code of any error encountered when performing this check during this audit\. One of "INSUFFICIENT\_PERMISSIONS", or "AUDIT\_CHECK\_DISABLED"\.  | 
|  message |  string  length\- max:2048  |  The message associated with any error encountered when performing this check during this audit\. | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ResourceNotFoundException`  
The specified resource does not exist\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

### ListAuditTasks<a name="dd-api-iot-ListAuditTasks-doc"></a>

Lists the audits that have been performed during a given time period\.

#### https<a name="dd-api-iot-ListAuditTasks-https"></a>

 **Request syntax:**

```
GET /audit/tasks?maxResults=maxResults&nextToken=nextToken&taskStatus=taskStatus&taskType=taskType&startTime=startTime&endTime=endTime 
```


**URI Request Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  startTime |  Timestamp |  yes |  The beginning of the time period\. Note that audit information is retained for a limited time \(180 days\)\. Requesting a start time prior to what is retained results in an "InvalidRequestException"\.  | 
|  endTime |  Timestamp |  yes |  The end of the time period\. | 
|  taskType |  AuditTaskType |  no |  A filter to limit the output to the specified type of audit: can be one of "ON\_DEMAND\_AUDIT\_TASK" or "SCHEDULED\_\_AUDIT\_TASK"\.  | 
|  taskStatus |  AuditTaskStatus |  no |  A filter to limit the output to audits with the specified completion status: can be one of "IN\_PROGRESS", "COMPLETED", "FAILED" or "CANCELED"\.  | 
|  nextToken |  NextToken |  no |  The token for the next set of results\. | 
|  maxResults |  MaxResults |  no |  The maximum number of results to return at one time\. The default is 25\. | 

 **Response syntax:**

```
Content-type: application/json

{
  "tasks": [
    {
      "taskId": "string",
      "taskStatus": "string",
      "taskType": "string"
    }
  ],
  "nextToken": "string"
}
```


**Response Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  tasks |   AuditTaskMetadataList  |  no |  The audits that were performed during the specified time period\. | 
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

#### cli<a name="dd-api-iot-ListAuditTasks-cli"></a>

 **Synopsis:**

```
aws iot  list-audit-tasks \
    --start-time <value> \
    --end-time <value> \
    [--task-type <value>] \
    [--task-status <value>] \
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
  "taskType": "string",
  "taskStatus": "string",
  "nextToken": "string",
  "maxResults": "integer"
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  startTime |  timestamp |  The beginning of the time period\. Note that audit information is retained for a limited time \(180 days\)\. Requesting a start time prior to what is retained results in an "InvalidRequestException"\.  | 
|  endTime |  timestamp |  The end of the time period\. | 
|  taskType |  string |  A filter to limit the output to the specified type of audit: can be one of "ON\_DEMAND\_AUDIT\_TASK" or "SCHEDULED\_\_AUDIT\_TASK"\.  enum: ON\_DEMAND\_AUDIT\_TASK | SCHEDULED\_AUDIT\_TASK  | 
|  taskStatus |  string |  A filter to limit the output to audits with the specified completion status: can be one of "IN\_PROGRESS", "COMPLETED", "FAILED" or "CANCELED"\.  enum: IN\_PROGRESS | COMPLETED | FAILED | CANCELED  | 
|  nextToken |  string |  The token for the next set of results\. | 
|  maxResults |  integer  range\- max:250 min:1  |  The maximum number of results to return at one time\. The default is 25\. | 

Output:

```
{
  "tasks": [
    {
      "taskId": "string",
      "taskStatus": "string",
      "taskType": "string"
    }
  ],
  "nextToken": "string"
}
```


**cli output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  tasks |  list  member: AuditTaskMetadata  java class: java\.util\.List  |  The audits that were performed during the specified time period\. | 
|  taskId |  string  length\- max:40 min:1  pattern: \[a\-zA\-Z0\-9\-\]\+  |  The ID of this audit\. | 
|  taskStatus |  string |  The status of this audit: one of "IN\_PROGRESS", "COMPLETED", "FAILED" or "CANCELED"\.  enum: IN\_PROGRESS | COMPLETED | FAILED | CANCELED  | 
|  taskType |  string |  The type of this audit: one of "ON\_DEMAND\_AUDIT\_TASK" or "SCHEDULED\_AUDIT\_TASK"\.  enum: ON\_DEMAND\_AUDIT\_TASK | SCHEDULED\_AUDIT\_TASK  | 
|  nextToken |  string |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

### CancelAuditTask<a name="dd-api-iot-CancelAuditTask-doc"></a>

Cancels an audit that is in progress\. The audit can be either scheduled or on\-demand\. If the audit is not in progress, an "InvalidRequestException" occurs\.

#### https<a name="dd-api-iot-CancelAuditTask-https"></a>

 **Request syntax:**

```
PUT /audit/tasks/taskId/cancel 
```


**URI Request Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  taskId |  AuditTaskId |  yes |  The ID of the audit you want to cancel\. You can only cancel an audit that is "IN\_PROGRESS"\.  | 

 **Errors:**

`ResourceNotFoundException`  
The specified resource does not exist\.  
HTTP response code: 404

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.  
HTTP response code: 400

`ThrottlingException`  
The rate exceeds the limit\.  
HTTP response code: 429

`InternalFailureException`  
An unexpected error has occurred\.  
HTTP response code: 500

#### cli<a name="dd-api-iot-CancelAuditTask-cli"></a>

 **Synopsis:**

```
aws iot  cancel-audit-task \
    --task-id <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "taskId": "string"
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  taskId |  string  length\- max:40 min:1  pattern: \[a\-zA\-Z0\-9\-\]\+  |  The ID of the audit you want to cancel\. You can only cancel an audit that is "IN\_PROGRESS"\.  | 

Output:

None

 **Errors:**

`ResourceNotFoundException`  
The specified resource does not exist\.

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## Check Audit Results<a name="device-defender-AuditCommandsFindings"></a>

Use `ListAuditFindings` to see the results of an audit\. You can filter the results by the type of check, a specific resource, or the time of the audit\. Armed with this information, you can take the appropriate steps to mitigate any problems that were found\.

### ListAuditFindings<a name="dd-api-iot-ListAuditFindings-doc"></a>

Lists the findings \(results\) of an audit or of the audits performed during a specified time period\. \(Findings are retained for 180 days\.\)

#### https<a name="dd-api-iot-ListAuditFindings-https"></a>

 **Request syntax:**

```
POST /audit/findings 
Content-type: application/json

{
  "taskId": "string",
  "checkName": "string",
  "resourceIdentifier": {
    "deviceCertificateId": "string",
    "caCertificateId": "string",
    "cognitoIdentityPoolId": "string",
    "clientId": "string",
    "policyVersionIdentifier": {
      "policyName": "string",
      "policyVersionId": "string"
    },
    "account": "string"
  },
  "maxResults": "integer",
  "nextToken": "string",
  "startTime": "timestamp",
  "endTime": "timestamp"
}
```


**Request Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  taskId |  AuditTaskId |  no |  A filter to limit results to the audit with the specified ID\. You must specify either the taskId or the startTime and endTime, but not both\.  | 
|  checkName |  AuditCheckName |  no |  A filter to limit results to the findings for the specified audit check\. | 
|  resourceIdentifier |  ResourceIdentifier |  no |  Information identifying the non\-compliant resource\. | 
|  maxResults |  MaxResults |  no |  The maximum number of results to return at one time\. The default is 25\. | 
|  nextToken |  NextToken |  no |  The token for the next set of results\. | 
|  startTime |  Timestamp |  no |  A filter to limit results to those found after the specified time\. You must specify either the startTime and endTime or the taskId, but not both\.  | 
|  endTime |  Timestamp |  no |  A filter to limit results to those found before the specified time\. You must specify either the startTime and endTime or the taskId, but not both\.  | 

 **Response syntax:**

```
Content-type: application/json

{
  "findings": [
    {
      "taskId": "string",
      "checkName": "string",
      "taskStartTime": "timestamp",
      "findingTime": "timestamp",
      "severity": "string",
      "nonCompliantResource": {
        "resourceType": "string",
        "resourceIdentifier": {
          "deviceCertificateId": "string",
          "caCertificateId": "string",
          "cognitoIdentityPoolId": "string",
          "clientId": "string",
          "policyVersionIdentifier": {
            "policyName": "string",
            "policyVersionId": "string"
          },
          "account": "string"
        },
        "additionalInfo": {
          "string": "string"
        }
      },
      "relatedResources": [
        {
          "resourceType": "string",
          "resourceIdentifier": {
            "deviceCertificateId": "string",
            "caCertificateId": "string",
            "cognitoIdentityPoolId": "string",
            "clientId": "string",
            "policyVersionIdentifier": {
              "policyName": "string",
              "policyVersionId": "string"
            },
            "account": "string"
          },
          "additionalInfo": {
            "string": "string"
          }
        }
      ],
      "reasonForNonCompliance": "string",
      "reasonForNonComplianceCode": "string"
    }
  ],
  "nextToken": "string"
}
```


**Response Body Parameters:**  

|  Name |  Type |  Req? |  Description | 
| --- | --- | --- | --- | 
|  findings |   AuditFindings  |  no |  The findings \(results\) of the audit\. | 
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

#### cli<a name="dd-api-iot-ListAuditFindings-cli"></a>

 **Synopsis:**

```
aws iot  list-audit-findings \
    [--task-id <value>] \
    [--check-name <value>] \
    [--resource-identifier <value>] \
    [--max-results <value>] \
    [--next-token <value>] \
    [--start-time <value>] \
    [--end-time <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format:

```
{
  "taskId": "string",
  "checkName": "string",
  "resourceIdentifier": {
    "deviceCertificateId": "string",
    "caCertificateId": "string",
    "cognitoIdentityPoolId": "string",
    "clientId": "string",
    "policyVersionIdentifier": {
      "policyName": "string",
      "policyVersionId": "string"
    },
    "account": "string"
  },
  "maxResults": "integer",
  "nextToken": "string",
  "startTime": "timestamp",
  "endTime": "timestamp"
}
```


**`cli-input-json` fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  taskId |  string  length\- max:40 min:1  pattern: \[a\-zA\-Z0\-9\-\]\+  |  A filter to limit results to the audit with the specified ID\. You must specify either the taskId or the startTime and endTime, but not both\.  | 
|  checkName |  string |  A filter to limit results to the findings for the specified audit check\. | 
|  resourceIdentifier |  ResourceIdentifier |  Information identifying the non\-compliant resource\. | 
|  deviceCertificateId |  string  length\- max:64 min:64  pattern: \(0x\)?\[a\-fA\-F0\-9\]\+  |  The ID of the certificate attached to the resource\. | 
|  caCertificateId |  string  length\- max:64 min:64  pattern: \(0x\)?\[a\-fA\-F0\-9\]\+  |  The ID of the CA certificate used to authorize the certificate\. | 
|  cognitoIdentityPoolId |  string |  The ID of the Cognito Identity Pool\. | 
|  clientId |  string |  The client ID\. | 
|  policyVersionIdentifier |  PolicyVersionIdentifier |  The version of the policy associated with the resource\. | 
|  policyName |  string  length\- max:128 min:1  pattern: \[w\+=,\.@\-\]\+  |  The name of the policy\. | 
|  policyVersionId |  string  pattern: \[0\-9\]\+  |  The ID of the version of the policy associated with the resource\. | 
|  account |  string  length\- max:12 min:12  pattern: \[0\-9\]\+  |  The account with which the resource is associated\. | 
|  maxResults |  integer  range\- max:250 min:1  |  The maximum number of results to return at one time\. The default is 25\. | 
|  nextToken |  string |  The token for the next set of results\. | 
|  startTime |  timestamp |  A filter to limit results to those found after the specified time\. You must specify either the startTime and endTime or the taskId, but not both\.  | 
|  endTime |  timestamp |  A filter to limit results to those found before the specified time\. You must specify either the startTime and endTime or the taskId, but not both\.  | 

Output:

```
{
  "findings": [
    {
      "taskId": "string",
      "checkName": "string",
      "taskStartTime": "timestamp",
      "findingTime": "timestamp",
      "severity": "string",
      "nonCompliantResource": {
        "resourceType": "string",
        "resourceIdentifier": {
          "deviceCertificateId": "string",
          "caCertificateId": "string",
          "cognitoIdentityPoolId": "string",
          "clientId": "string",
          "policyVersionIdentifier": {
            "policyName": "string",
            "policyVersionId": "string"
          },
          "account": "string"
        },
        "additionalInfo": {
          "string": "string"
        }
      },
      "relatedResources": [
        {
          "resourceType": "string",
          "resourceIdentifier": {
            "deviceCertificateId": "string",
            "caCertificateId": "string",
            "cognitoIdentityPoolId": "string",
            "clientId": "string",
            "policyVersionIdentifier": {
              "policyName": "string",
              "policyVersionId": "string"
            },
            "account": "string"
          },
          "additionalInfo": {
            "string": "string"
          }
        }
      ],
      "reasonForNonCompliance": "string",
      "reasonForNonComplianceCode": "string"
    }
  ],
  "nextToken": "string"
}
```


**cli output fields:**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  findings |  list  member: AuditFinding  |  The findings \(results\) of the audit\. | 
|  taskId |  string  length\- max:40 min:1  pattern: \[a\-zA\-Z0\-9\-\]\+  |  The ID of the audit that generated this result \(finding\) | 
|  checkName |  string |  The audit check that generated this result\. | 
|  taskStartTime |  timestamp |  The time the audit started\. | 
|  findingTime |  timestamp |  The time the result \(finding\) was discovered\. | 
|  severity |  string |  The severity of the result \(finding\)\.  enum: CRITICAL | HIGH | MEDIUM | LOW  | 
|  nonCompliantResource |  NonCompliantResource |  The resource that was found to be non\-compliant with the audit check\.  | 
|  resourceType |  string |  The type of the non\-compliant resource\.  enum: DEVICE\_CERTIFICATE | CA\_CERTIFICATE | IOT\_POLICY | COGNITO\_IDENTITY\_POOL | CLIENT\_ID | ACCOUNT\_SETTINGS  | 
|  resourceIdentifier |  ResourceIdentifier |  Information identifying the non\-compliant resource\. | 
|  deviceCertificateId |  string  length\- max:64 min:64  pattern: \(0x\)?\[a\-fA\-F0\-9\]\+  |  The ID of the certificate attached to the resource\. | 
|  caCertificateId |  string  length\- max:64 min:64  pattern: \(0x\)?\[a\-fA\-F0\-9\]\+  |  The ID of the CA certificate used to authorize the certificate\. | 
|  cognitoIdentityPoolId |  string |  The ID of the Cognito Identity Pool\. | 
|  clientId |  string |  The client ID\. | 
|  policyVersionIdentifier |  PolicyVersionIdentifier |  The version of the policy associated with the resource\. | 
|  policyName |  string  length\- max:128 min:1  pattern: \[w\+=,\.@\-\]\+  |  The name of the policy\. | 
|  policyVersionId |  string  pattern: \[0\-9\]\+  |  The ID of the version of the policy associated with the resource\. | 
|  account |  string  length\- max:12 min:12  pattern: \[0\-9\]\+  |  The account with which the resource is associated\. | 
|  additionalInfo |  map |  Additional information about the non\-compliant resource\. | 
|  relatedResources |  list  member: RelatedResource  |  The list of related resources\. | 
|  resourceType |  string |  The type of resource\.  enum: DEVICE\_CERTIFICATE | CA\_CERTIFICATE | IOT\_POLICY | COGNITO\_IDENTITY\_POOL | CLIENT\_ID | ACCOUNT\_SETTINGS  | 
|  resourceIdentifier |  ResourceIdentifier |  Information identifying the resource\. | 
|  deviceCertificateId |  string  length\- max:64 min:64  pattern: \(0x\)?\[a\-fA\-F0\-9\]\+  |  The ID of the certificate attached to the resource\. | 
|  caCertificateId |  string  length\- max:64 min:64  pattern: \(0x\)?\[a\-fA\-F0\-9\]\+  |  The ID of the CA certificate used to authorize the certificate\. | 
|  cognitoIdentityPoolId |  string |  The ID of the Cognito Identity Pool\. | 
|  clientId |  string |  The client ID\. | 
|  policyVersionIdentifier |  PolicyVersionIdentifier |  The version of the policy associated with the resource\. | 
|  policyName |  string  length\- max:128 min:1  pattern: \[w\+=,\.@\-\]\+  |  The name of the policy\. | 
|  policyVersionId |  string  pattern: \[0\-9\]\+  |  The ID of the version of the policy associated with the resource\. | 
|  account |  string  length\- max:12 min:12  pattern: \[0\-9\]\+  |  The account with which the resource is associated\. | 
|  additionalInfo |  map |  Additional information about the resource\. | 
|  reasonForNonCompliance |  string |  The reason the resource was non\-compliant\. | 
|  reasonForNonComplianceCode |  string |  A code which indicates the reason that the resource was non\-compliant\. | 
|  nextToken |  string |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors:**

`InvalidRequestException`  
The contents of the request were invalid\. For example, this code is returned when an UpdateJobExecution request contains invalid status details\. The message contains details about the error\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.