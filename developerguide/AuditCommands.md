# Audit Commands<a name="AuditCommands"></a>

## Manage Audit Settings<a name="AuditCommandsManageSettings"></a>

Use `UpdateAccountAuditConfiguration` to configure audit settings for your account\. This command allows you to enable those checks you want to be available for audits, set up optional notifications, and configure permissions\. 

Check these settings with `DescribeAccountAuditConfiguration`\.

Use `DeleteAccountAuditConfiguration` to delete your audit settings\. This restores all default values, and effectively disables audits because all checks are disabled by default\.

### UpdateAccountAuditConfiguration<a name="dd-api-iot-UpdateAccountAuditConfiguration"></a>

Configures or reconfigures the Device Defender audit settings for this account\. Settings include how audit notifications are sent and which audit checks are enabled or disabled\.

 **Synopsis**

```
aws iot  update-account-audit-configuration \
    [--role-arn <value>] \
    [--audit-notification-target-configurations <value>] \
    [--audit-check-configurations <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

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


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  roleArn |  string  length\- max:2048 min:20  |  The ARN of the role that grants permission to AWS IoT to access information about your devices, policies, certificates, and other items when performing an audit\.  | 
|  auditNotificationTargetConfigurations |  map |  Information about the targets to which audit notifications are sent\. | 
|  targetArn |  string |  The ARN of the target \(SNS topic\) to which audit notifications are sent\. | 
|  roleArn |  string  length\- max:2048 min:20  |  The ARN of the role that grants permission to send notifications to the target\. | 
|  enabled |  boolean |  True if notifications to the target are enabled\. | 
|  auditCheckConfigurations |  map |  Specifies which audit checks are enabled and disabled for this account\. Use `DescribeAccountAuditConfiguration` to see the list of all checks, including those that are currently enabled\. Some data collection might start immediately when certain checks are enabled\. When a check is disabled, any data collected so far in relation to the check is deleted\. You cannot disable a check if it is used by any scheduled audit\. You must first delete the check from the scheduled audit or delete the scheduled audit itself\. On the first call to `UpdateAccountAuditConfiguration`, this parameter is required and must specify at least one enabled check\.  | 
|  enabled |  boolean |  True if this audit check is enabled for this account\. | 

Output

None

 **Errors**

`InvalidRequestException`  
The contents of the request were invalid\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

### DescribeAccountAuditConfiguration<a name="dd-api-iot-DescribeAccountAuditConfiguration"></a>

Gets information about the Device Defender audit settings for this account\. Settings include how audit notifications are sent and which audit checks are enabled or disabled\.

 **Synopsis**

```
aws iot  describe-account-audit-configuration  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
}
```

Output

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


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  roleArn |  string  length\- max:2048 min:20  |  The ARN of the role that grants permission to AWS IoT to access information about your devices, policies, certificates, and other items when performing an audit\. On the first call to `UpdateAccountAuditConfiguration`, this parameter is required\.  | 
|  auditNotificationTargetConfigurations |  map |  Information about the targets to which audit notifications are sent for this account\.  | 
|  targetArn |  string |  The ARN of the target \(SNS topic\) to which audit notifications are sent\. | 
|  roleArn |  string  length\- max:2048 min:20  |  The ARN of the role that grants permission to send notifications to the target\. | 
|  enabled |  boolean |  True if notifications to the target are enabled\. | 
|  auditCheckConfigurations |  map |  Which audit checks are enabled and disabled for this account\. | 
|  enabled |  boolean |  True if this audit check is enabled for this account\. | 

 **Errors**

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

### DeleteAccountAuditConfiguration<a name="dd-api-iot-DeleteAccountAuditConfiguration"></a>

Restores the default settings for Device Defender audits for this account\. Any configuration data you entered is deleted and all audit checks are reset to disabled\.  

 **Synopsis**

```
aws iot  delete-account-audit-configuration \
    [--delete-scheduled-audits | --no-delete-scheduled-audits]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "deleteScheduledAudits": "boolean"
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  deleteScheduledAudits |  boolean |  If true, all scheduled audits are deleted\. | 

Output

None

 **Errors**

`InvalidRequestException`  
The contents of the request were invalid\.

`ResourceNotFoundException`  
The specified resource does not exist\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## Schedule Audits<a name="device-defender-AuditCommandsManageSchedules"></a>

Use `CreateScheduledAudit` to create one or more scheduled audits\. This command allows you to specify the checks you want to perform during an audit and how often the audit should be run\. 

Keep track of your scheduled audits with `ListScheduledAudits` and `DescribeScheduledAudit`\. 

Change an existing scheduled audit with `UpdateScheduledAudit` or delete it with `DeleteScheduledAudit`\. 

### CreateScheduledAudit<a name="dd-api-iot-CreateScheduledAudit"></a>

Creates a scheduled audit that is run at a specified time interval\.

 **Synopsis**

```
aws iot  create-scheduled-audit \
    --frequency <value> \
    [--day-of-month <value>] \
    [--day-of-week <value>] \
    --target-check-names <value> \
    [--tags <value>] \
    --scheduled-audit-name <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "frequency": "string",
  "dayOfMonth": "string",
  "dayOfWeek": "string",
  "targetCheckNames": [
    "string"
  ],
  "tags": [
    {
      "Key": "string",
      "Value": "string"
    }
  ],
  "scheduledAuditName": "string"
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  frequency |  string |  How often the scheduled audit takes place\. Can be one of DAILY, WEEKLY, BIWEEKLY, or MONTHLY\. The actual start time of each audit is determined by the system\.  enum: DAILY \| WEEKLY \| BIWEEKLY \| MONTHLY  | 
|  dayOfMonth |  string  pattern: ^\(\[1\-9\]\|\[12\]\[0\-9\]\|3\[01\]\)$\|^LAST$  |  The day of the month on which the scheduled audit takes place\. Can be 1 through 31 or LAST\. This field is required if the `frequency` parameter is set to MONTHLY\. If days 29\-31 are specified, and the month does not have that many days, the audit takes place on the LAST day of the month\.  | 
|  dayOfWeek |  string |  The day of the week on which the scheduled audit takes place\. Can be one of SUN, MON, TUE,WED, THU, FRI, or SAT\. This field is required if the `frequency` parameter is set to WEEKLY or BIWEEKLY\.  enum: SUN \| MON \| TUE \| WED \| THU \| FRI \| SAT  | 
|  targetCheckNames |  list  member: AuditCheckName  |  Which checks are performed during the scheduled audit\. Checks must be enabled for your account\. \(Use `DescribeAccountAuditConfiguration` to see the list of all checks, including those that are enabled or `UpdateAccountAuditConfiguration` to select which checks are enabled\.\)  | 
|  tags |  list  member: Tag  java class: java\.util\.List  |  Metadata that can be used to manage the scheduled audit\. | 
|  Key |  string |  The tag's key\. | 
|  Value |  string |  The tag's value\. | 
|  scheduledAuditName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The name you want to give to the scheduled audit\. \(Maximum of 128 characters\) | 

Output

```
{
  "scheduledAuditArn": "string"
}
```


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  scheduledAuditArn |  string |  The ARN of the scheduled audit\. | 

 **Errors**

`InvalidRequestException`  
The contents of the request were invalid\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

`LimitExceededException`  
A limit has been exceeded\.

### ListScheduledAudits<a name="dd-api-iot-ListScheduledAudits"></a>

Lists all of your scheduled audits\.

 **Synopsis**

```
aws iot  list-scheduled-audits \
    [--next-token <value>] \
    [--max-results <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "nextToken": "string",
  "maxResults": "integer"
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  nextToken |  string |  The token for the next set of results\. | 
|  maxResults |  integer  range\- max:250 min:1  |  The maximum number of results to return at one time\. The default is 25\. | 

Output

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


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  scheduledAudits |  list  member: ScheduledAuditMetadata  java class: java\.util\.List  |  The list of scheduled audits\. | 
|  scheduledAuditName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The name of the scheduled audit\. | 
|  scheduledAuditArn |  string |  The ARN of the scheduled audit\. | 
|  frequency |  string |  How often the scheduled audit takes place\.  enum: DAILY \| WEEKLY \| BIWEEKLY \| MONTHLY  | 
|  dayOfMonth |  string  pattern: ^\(\[1\-9\]\|\[12\]\[0\-9\]\|3\[01\]\)$\|^LAST$  |  The day of the month on which the scheduled audit is run \(if the `frequency` is MONTHLY\)\. If days 29\-31 are specified, and the month does not have that many days, the audit takes place on the LAST day of the month\.  | 
|  dayOfWeek |  string |  The day of the week on which the scheduled audit is run \(if the `frequency` is WEEKLY or BIWEEKLY\)\.  enum: SUN \| MON \| TUE \| WED \| THU \| FRI \| SAT  | 
|  nextToken |  string |  A token that can be used to retrieve the next set of results, or `null` if there are no more results\.  | 

 **Errors**

`InvalidRequestException`  
The contents of the request were invalid\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

### DescribeScheduledAudit<a name="dd-api-iot-DescribeScheduledAudit"></a>

Gets information about a scheduled audit\.

 **Synopsis**

```
aws iot  describe-scheduled-audit \
    --scheduled-audit-name <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "scheduledAuditName": "string"
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  scheduledAuditName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The name of the scheduled audit whose information you want to get\. | 

Output

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


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  frequency |  string |  How often the scheduled audit takes place\. One of DAILY, WEEKLY, BIWEEKLY, or MONTHLY\. The actual start time of each audit is determined by the system\.  enum: DAILY \| WEEKLY \| BIWEEKLY \| MONTHLY  | 
|  dayOfMonth |  string  pattern: ^\(\[1\-9\]\|\[12\]\[0\-9\]\|3\[01\]\)$\|^LAST$  |  The day of the month on which the scheduled audit takes place\. Can be 1 through 31 or LAST\. If days 29\-31 are specified, and the month does not have that many days, the audit takes place on the LAST day of the month\.  | 
|  dayOfWeek |  string |  The day of the week on which the scheduled audit takes place\. One of SUN, MON, TUE, WED, THU, FRI, or SAT\.  enum: SUN \| MON \| TUE \| WED \| THU \| FRI \| SAT  | 
|  targetCheckNames |  list  member: AuditCheckName  |  Which checks are performed during the scheduled audit\. Checks must be enabled for your account\. \(Use `DescribeAccountAuditConfiguration` to see the list of all checks, including those that are enabled or use `UpdateAccountAuditConfiguration` to select which checks are enabled\.\)  | 
|  scheduledAuditName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The name of the scheduled audit\. | 
|  scheduledAuditArn |  string |  The ARN of the scheduled audit\. | 

 **Errors**

`InvalidRequestException`  
The contents of the request were invalid\.

`ResourceNotFoundException`  
The specified resource does not exist\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

### UpdateScheduledAudit<a name="dd-api-iot-UpdateScheduledAudit"></a>

Updates a scheduled audit, including which checks are performed and how often the audit takes place\.

 **Synopsis**

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

 `cli-input-json` format

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


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  frequency |  string |  How often the scheduled audit takes place\. Can be one of DAILY, WEEKLY, BIWEEKLY, or MONTHLY\. The actual start time of each audit is determined by the system\.  enum: DAILY \| WEEKLY \| BIWEEKLY \| MONTHLY  | 
|  dayOfMonth |  string  pattern: ^\(\[1\-9\]\|\[12\]\[0\-9\]\|3\[01\]\)$\|^LAST$  |  The day of the month on which the scheduled audit takes place\. Can be 1 through 31 or LAST\. This field is required if the `frequency` parameter is set to MONTHLY\. If days 29\-31 are specified, and the month does not have that many days, the audit takes place on the LAST day of the month\.  | 
|  dayOfWeek |  string |  The day of the week on which the scheduled audit takes place\. Can be one of SUN, MON, TUE, WED, THU, FRI, or SAT\. This field is required if the `frequency` parameter is set to WEEKLY or BIWEEKLY\.  enum: SUN \| MON \| TUE \| WED \| THU \| FRI \| SAT  | 
|  targetCheckNames |  list  member: AuditCheckName  |  Which checks are performed during the scheduled audit\. Checks must be enabled for your account\. \(Use `DescribeAccountAuditConfiguration` to see the list of all checks, including those that are enabled or use `UpdateAccountAuditConfiguration` to select which checks are enabled\.\)  | 
|  scheduledAuditName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The name of the scheduled audit\. \(Maximum of 128 characters\) | 

Output

```
{
  "scheduledAuditArn": "string"
}
```


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  scheduledAuditArn |  string |  The ARN of the scheduled audit\. | 

 **Errors**

`InvalidRequestException`  
The contents of the request were invalid\.

`ResourceNotFoundException`  
The specified resource does not exist\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

### DeleteScheduledAudit<a name="dd-api-iot-DeleteScheduledAudit"></a>

Deletes a scheduled audit\.

 **Synopsis**

```
aws iot  delete-scheduled-audit \
    --scheduled-audit-name <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "scheduledAuditName": "string"
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  scheduledAuditName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The name of the scheduled audit you want to delete\. | 

Output

None

 **Errors**

`InvalidRequestException`  
The contents of the request were invalid\.

`ResourceNotFoundException`  
The specified resource does not exist\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## Run an On\-Demand Audit<a name="device-defender-AuditCommandsOnDemand"></a>

Use `StartOnDemandAuditTask` to specify the checks you want to perform and start an audit running right away\.

### StartOnDemandAuditTask<a name="dd-api-iot-StartOnDemandAuditTask"></a>

Starts an on\-demand Device Defender audit\.

 **Synopsis**

```
aws iot  start-on-demand-audit-task \
    --target-check-names <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "targetCheckNames": [
    "string"
  ]
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  targetCheckNames |  list  member: AuditCheckName  |  Which checks are performed during the audit\. The checks you specify must be enabled for your account or an exception occurs\. Use `DescribeAccountAuditConfiguration` to see the list of all checks, including those that are enabled or use `UpdateAccountAuditConfiguration` to select which checks are enabled\.  | 

Output

```
{
  "taskId": "string"
}
```


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  taskId |  string  length\- max:40 min:1  pattern: \[a\-zA\-Z0\-9\-\]\+  |  The ID of the on\-demand audit you started\. | 

 **Errors**

`InvalidRequestException`  
The contents of the request were invalid\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

`LimitExceededException`  
A limit has been exceeded\.

## Manage Audit Instances<a name="device-defender-AuditCommandsManageInstances"></a>

Use `DescribeAuditTask` to get information about a specific audit instance\. If it has already run, the results include which checks failed and which passed, those that the system was unable to complete, and if the audit is still in progress, those it is still working on\.

Use `ListAuditTasks` to find the audits that were run during a specified time interval\.

Use `CancelAuditTask` to halt an audit in progress\.

### DescribeAuditTask<a name="dd-api-iot-DescribeAuditTask"></a>

Gets information about a Device Defender audit\.

 **Synopsis**

```
aws iot  describe-audit-task \
    --task-id <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "taskId": "string"
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  taskId |  string  length\- max:40 min:1  pattern: \[a\-zA\-Z0\-9\-\]\+  |  The ID of the audit whose information you want to get\. | 

Output

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


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  taskStatus |  string |  The status of the audit: one of IN\_PROGRESS, COMPLETED, FAILED, or CANCELED\.  enum: IN\_PROGRESS \| COMPLETED \| FAILED \| CANCELED  | 
|  taskType |  string |  The type of audit: ON\_DEMAND\_AUDIT\_TASK or SCHEDULED\_AUDIT\_TASK\.  enum: ON\_DEMAND\_AUDIT\_TASK \| SCHEDULED\_AUDIT\_TASK  | 
|  taskStartTime |  timestamp |  The time the audit started\. | 
|  taskStatistics |  TaskStatistics |  Statistical information about the audit\. | 
|  totalChecks |  integer |  The number of checks in this audit\. | 
|  inProgressChecks |  integer |  The number of checks in progress\. | 
|  waitingForDataCollectionChecks |  integer |  The number of checks waiting for data collection\. | 
|  compliantChecks |  integer |  The number of checks that found compliant resources\. | 
|  nonCompliantChecks |  integer |  The number of checks that found noncompliant resources\. | 
|  failedChecks |  integer |  The number of checks\.  | 
|  canceledChecks |  integer |  The number of checks that did not run because the audit was canceled\. | 
|  scheduledAuditName |  string  length\- max:128 min:1  pattern: \[a\-zA\-Z0\-9\_\-\]\+  |  The name of the scheduled audit \(only if the audit was a scheduled audit\)\. | 
|  auditDetails |  map |  Detailed information about each check performed during this audit\. | 
|  checkRunStatus |  string |  The completion status of this check, one of IN\_PROGRESS, WAITING\_FOR\_DATA\_COLLECTION, CANCELED, COMPLETED\_COMPLIANT, COMPLETED\_NON\_COMPLIANT, or FAILED\.  enum: IN\_PROGRESS \| WAITING\_FOR\_DATA\_COLLECTION \| CANCELED \| COMPLETED\_COMPLIANT \| COMPLETED\_NON\_COMPLIANT \| FAILED  | 
|  checkCompliant |  boolean |  True if the check completed and found all resources compliant\. | 
|  totalResourcesCount |  long |  The number of resources on which the check was performed\. | 
|  nonCompliantResourcesCount |  long |  The number of resources that the check found noncompliant\. | 
|  errorCode |  string |  The code of any error encountered when performing this check during this audit\. One of INSUFFICIENT\_PERMISSIONS or AUDIT\_CHECK\_DISABLED\.  | 
|  message |  string  length\- max:2048  |  The message associated with any error encountered when performing this check during this audit\. | 

 **Errors**

`InvalidRequestException`  
The contents of the request were invalid\.

`ResourceNotFoundException`  
The specified resource does not exist\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

### ListAuditTasks<a name="dd-api-iot-ListAuditTasks"></a>

Lists the Device Defender audits that have been performed during a given time period\.

 **Synopsis**

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

 `cli-input-json` format

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


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  startTime |  timestamp |  The beginning of the time period\. Audit information is retained for a limited time \(180 days\)\. Requesting a start time prior to what is retained results in an `InvalidRequestException`\.  | 
|  endTime |  timestamp |  The end of the time period\. | 
|  taskType |  string |  A filter to limit the output to the specified type of audit: can be one of ON\_DEMAND\_AUDIT\_TASK or SCHEDULED\_\_AUDIT\_TASK\.  enum: ON\_DEMAND\_AUDIT\_TASK \| SCHEDULED\_AUDIT\_TASK  | 
|  taskStatus |  string |  A filter to limit the output to audits with the specified completion status: can be one of IN\_PROGRESS, COMPLETED, FAILED, or CANCELED\.  enum: IN\_PROGRESS \| COMPLETED \| FAILED \| CANCELED  | 
|  nextToken |  string |  The token for the next set of results\. | 
|  maxResults |  integer  range\- max:250 min:1  |  The maximum number of results to return at one time\. The default is 25\. | 

Output

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


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  tasks |  list  member: AuditTaskMetadata  java class: java\.util\.List  |  The audits that were performed during the specified time period\. | 
|  taskId |  string  length\- max:40 min:1  pattern: \[a\-zA\-Z0\-9\-\]\+  |  The ID of this audit\. | 
|  taskStatus |  string |  The status of this audit: one of IN\_PROGRESS, COMPLETED, FAILED, or CANCELED\.  enum: IN\_PROGRESS \| COMPLETED \| FAILED \| CANCELED  | 
|  taskType |  string |  The type of this audit: one of ON\_DEMAND\_AUDIT\_TASK or SCHEDULED\_AUDIT\_TASK\.  enum: ON\_DEMAND\_AUDIT\_TASK \| SCHEDULED\_AUDIT\_TASK  | 
|  nextToken |  string |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors**

`InvalidRequestException`  
The contents of the request were invalid\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

### CancelAuditTask<a name="dd-api-iot-CancelAuditTask"></a>

Cancels an audit that is in progress\. The audit can be either scheduled or on\-demand\. If the audit is not in progress, an `InvalidRequestException` occurs\.

 **Synopsis**

```
aws iot  cancel-audit-task \
    --task-id <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
  "taskId": "string"
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  taskId |  string  length\- max:40 min:1  pattern: \[a\-zA\-Z0\-9\-\]\+  |  The ID of the audit you want to cancel\. You can only cancel an audit that is IN\_PROGRESS\.  | 

Output

None

 **Errors**

`ResourceNotFoundException`  
The specified resource does not exist\.

`InvalidRequestException`  
The contents of the request were invalid\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## Check Audit Results<a name="device-defender-AuditCommandsFindings"></a>

Use `ListAuditFindings` to see the results of an audit\. You can filter the results by the type of check, a specific resource, or the time of the audit\. You can use this information to mitigate any problems that were found\.

You can define mitigation actions and apply them to the findings from your audit\. For more information, see [Mitigation Actions](device-defender-mitigation-actions.md)\.

### ListAuditFindings<a name="dd-api-iot-ListAuditFindings"></a>

Lists the findings \(results\) of a Device Defender audit or of the audits performed during a specified time period\. \(Findings are retained for 180 days\.\)

 **Synopsis**

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

 `cli-input-json` format

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
   
    "roleAliasArn": "string",
    "account": "string"
  },
  "maxResults": "integer",
  "nextToken": "string",
  "startTime": "timestamp",
  "endTime": "timestamp"
}
```


**`cli-input-json` fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  taskId  |  string  length\- max:40 min:1  pattern: \[a\-zA\-Z0\-9\-\]\+  |  A filter to limit results to the audit with the specified ID\. You must specify either the taskId or the startTime and endTime, but not both\.  | 
|  checkName  |  string  |  A filter to limit results to the findings for the specified audit check\.  | 
|  resourceIdentifier  |  ResourceIdentifier  |  Information that identifies the noncompliant resource\.  | 
|  deviceCertificateId  |  string  length\- max:64 min:64  pattern: \(0x\)?\[a\-fA\-F0\-9\]\+  |  The ID of the certificate attached to the resource\.  | 
|  caCertificateId  |  string  length\- max:64 min:64  pattern: \(0x\)?\[a\-fA\-F0\-9\]\+  |  The ID of the CA certificate used to authorize the certificate\.  | 
|  cognitoIdentityPoolId  |  string  |  The ID of the Amazon Cognito identity pool\.  | 
|  clientId  |  string  |  The client ID\.  | 
|  policyVersionIdentifier  |  PolicyVersionIdentifier  |  The version of the policy associated with the resource\.  | 
|  policyName  |  string  length\- max:128 min:1  pattern: \[w\+=,\.@\-\]\+  |  The name of the policy\.  | 
|  policyVersionId  |  string  pattern: \[0\-9\]\+  |  The ID of the version of the policy associated with the resource\.  | 
|  roleAliasArn  |  string  |  The ARN of the role alias that has overly permissive actions\.  length\- max:2048 min:1  | 
|  account  |  string  length\- max:12 min:12  pattern: \[0\-9\]\+  |  The account with which the resource is associated\.  | 
|  maxResults  |  integer  range\- max:250 min:1  |  The maximum number of results to return at one time\. The default is 25\.  | 
|  nextToken  |  string  |  The token for the next set of results\.  | 
|  startTime  |  timestamp  |  A filter to limit results to those found after the specified time\. You must specify either the startTime and endTime or the taskId, but not both\.  | 
|  endTime  |  timestamp  |  A filter to limit results to those found before the specified time\. You must specify either the startTime and endTime or the taskId, but not both\.  | 

Output

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
    
            "iamRoleArn": "string",
    
            "policyVersionIdentifier": {
              "policyName": "string",
              "policyVersionId": "string"
            },
            "account": "string"
          },
    
          "roleAliasArn": "string",
    
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


**CLI output fields**  

|  Name |  Type |  Description | 
| --- | --- | --- | 
|  findings  |  list  member: AuditFinding  |  The findings \(results\) of the audit\.  | 
|  taskId  |  string  length\- max:40 min:1  pattern: \[a\-zA\-Z0\-9\-\]\+  |  The ID of the audit that generated this result \(finding\)\.  | 
|  checkName  |  string  |  The audit check that generated this result\.  | 
|  taskStartTime  |  timestamp  |  The time the audit started\.  | 
|  findingTime  |  timestamp  |  The time the result \(finding\) was discovered\.  | 
|  severity  |  string  |  The severity of the result \(finding\)\.  enum: CRITICAL \| HIGH \| MEDIUM \| LOW  | 
|  nonCompliantResource  |  NonCompliantResource  |  The resource that was found to be noncompliant with the audit check\.  | 
|  resourceType  |  string  |  The type of the noncompliant resource\.  enum: DEVICE\_CERTIFICATE \| CA\_CERTIFICATE \| IOT\_POLICY \| COGNITO\_IDENTITY\_POOL \| CLIENT\_ID \| ACCOUNT\_SETTINGS  | 
|  resourceIdentifier  |  ResourceIdentifier  |  Information that identifies the noncompliant resource\.  | 
|  deviceCertificateId  |  string  length\- max:64 min:64  pattern: \(0x\)?\[a\-fA\-F0\-9\]\+  |  The ID of the certificate attached to the resource\.  | 
|  caCertificateId  |  string  length\- max:64 min:64  pattern: \(0x\)?\[a\-fA\-F0\-9\]\+  |  The ID of the CA certificate used to authorize the certificate\.  | 
|  cognitoIdentityPoolId  |  string  |  The ID of the Amazon Cognito identity pool\.  | 
|  clientId  |  string  |  The client ID\.  | 
|  policyVersionIdentifier  |  PolicyVersionIdentifier  |  The version of the policy associated with the resource\.  | 
|  policyName  |  string  length\- max:128 min:1  pattern: \[w\+=,\.@\-\]\+  |  The name of the policy\.  | 
|  policyVersionId  |  string  pattern: \[0\-9\]\+  |  The ID of the version of the policy associated with the resource\.  | 
|  account  |  string  length\- max:12 min:12  pattern: \[0\-9\]\+  |  The account with which the resource is associated\.  | 
|  additionalInfo  |  map  |  Other information about the noncompliant resource\.  | 
|  relatedResources  |  list  member: RelatedResource  |  The list of related resources\.  | 
|  resourceType  |  string  |  The type of resource\.  enum: DEVICE\_CERTIFICATE \| CA\_CERTIFICATE \| IOT\_POLICY \| COGNITO\_IDENTITY\_POOL \| CLIENT\_ID \| ACCOUNT\_SETTINGS  | 
|  resourceIdentifier  |  ResourceIdentifier  |  Information that identifies the resource\.  | 
|  deviceCertificateId  |  string  length\- max:64 min:64  pattern: \(0x\)?\[a\-fA\-F0\-9\]\+  |  The ID of the certificate attached to the resource\.  | 
|  caCertificateId  |  string  length\- max:64 min:64  pattern: \(0x\)?\[a\-fA\-F0\-9\]\+  |  The ID of the CA certificate used to authorize the certificate\.  | 
|  cognitoIdentityPoolId  |  string  |  The ID of the Amazon Cognito identity pool\.  | 
|  clientId  |  string  |  The client ID\.  | 
|  policyVersionIdentifier  |  PolicyVersionIdentifier  |  The version of the policy associated with the resource\.  | 
|  iamRoleArn  |  string  length\- max:2048 min:20  |  The ARN of the IAM role that has overly permissive actions\.  | 
|  policyName  |  string  length\- max:128 min:1  pattern: \[w\+=,\.@\-\]\+  |  The name of the policy\.  | 
|  policyVersionId  |  string  pattern: \[0\-9\]\+  |  The ID of the version of the policy associated with the resource\.  | 
|  roleAliasArn  |  string  length\- max:2048 min:1  |  The ARN of the role alias that has overly permissive actions\.  | 
|  account  |  string  length\- max:12 min:12  pattern: \[0\-9\]\+  |  The account with which the resource is associated\.  | 
|  additionalInfo  |  map  |  Other information about the resource\.  | 
|  reasonForNonCompliance  |  string  |  The reason the resource was noncompliant\.  | 
|  reasonForNonComplianceCode  |  string  |  A code that indicates the reason that the resource was noncompliant\.  | 
|  nextToken  |  string  |  A token that can be used to retrieve the next set of results, or `null` if there are no additional results\.  | 

 **Errors**

`InvalidRequestException`  
The contents of the request were invalid\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.