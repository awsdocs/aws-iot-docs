# Mitigation Action Commands<a name="mitigation-action-commands"></a>

You use these mitigation action commands to define a set of actions for your AWS account that you can later apply to one or more sets of audit findings\. There are two command categories: 
+ Those used to define and manage actions\.
+ Those used to start and manage the application of those actions to audit findings\.


**Mitigation Action Commands**  

|  Define and Manage Actions  |  Start and Manage Execution  | 
| --- | --- | 
|  [CreateMitigationAction](#dd-api-iot-CreateMitigationAction)  |  [CancelAuditMitigationActionsTask](#dd-api-iot-CancelAuditMitigationActionsTask)  | 
|  [DeleteMitigationAction](#dd-api-iot-DeleteMitigationAction)  |  [DescribeAuditMitigationActionsTask](#dd-api-iot-DescribeAuditMitigationActionsTask)  | 
|  [DescribeMitigationAction](#dd-api-iot-DescribeMitigationAction)  |  [ListAuditMitigationActionsTasks](#dd-api-iot-ListAuditMitigationActionsTasks)  | 
|  [ListMitigationActions](#dd-api-iot-ListMitigationActions)  |  [StartAuditMitigationActionsTask](#dd-api-iot-StartAuditMitigationActionsTask)  | 
|  [UpdateMitigationAction](#dd-api-iot-UpdateMitigationAction)  |  [ListAuditMitigationActionsExecutions](#dd-api-iot-ListAuditMitigationActionsExecutions)  | 

## CreateMitigationAction<a name="dd-api-iot-CreateMitigationAction"></a>

Defines an action that can be applied to audit findings by using [StartAuditMitigationActionsTask](#dd-api-iot-StartAuditMitigationActionsTask)\. Each mitigation action can apply only one type of change\. Defining an action does not apply it\.

 **Synopsis**

```
aws iot  create-mitigation-action \
    --action-name <value] \
    --role-arn <value> \
    [--tags <value>] \
    --action-params <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
   "actionParams": { 
      "addThingsToThingGroupParams": { 
         "overrideDynamicGroups": boolean,
         "thingGroupNames": [ "string" ]
      },
      "enableIoTLoggingParams": { 
         "logLevel": "string",
         "roleArnForLogging": "string"
      },
      "publishFindingToSnsParams": { 
         "topicArn": "string"
      },
      "replaceDefaultPolicyVersionParams": { 
         "templateName": "string"
      },
      "updateCACertificateParams": { 
         "action": "string"
      },
      "updateDeviceCertificateParams": { 
         "action": "string"
      }
   },
   "roleArn": "string",
   "tags": [ 
      { 
         "Key": "string",
         "Value": "string"
      }
   ]
}
```


**`cli-input-json` fields**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  `roleArn`  |  string  length\- max:2048 min:20  |  The ARN of the role that grants permission to AWS IoT to access information about your devices, policies, certificates, and other items when applying this action\.  | 
|  `tags`  |  array of tag objects  |  Metadata that can be used to manage the mitigation action\.  | 
|  `actionParams`  |  map  |  Defines the type of action to be applied and the parameters for that mitigation action\. You can include only one type of parameter for each mitigation action\.  | 

You must provide parameters for the type of action that you are defining\. You can provide only one action type and its parameters\. These are the supported action types:
+ `ADD_THINGS_TO_THING_GROUP`
+ `ENABLE_IOT_LOGGING`
+ `PUBLISH_FINDING_TO_SNS`
+ `REPLACE_DEFAULT_POLICY_VERSION`
+ `UPDATE_CA_CERTIFICATE`
+ `UPDATE_DEVICE_CERTIFICATE`


**Parameters for AddThingsToThingGroup**  

| Name | Type | Description | 
| --- | --- | --- | 
|  `overrideDynamicGroups`  |  boolean  |  Optional\. Specifies if this mitigation action can move the things that triggered the mitigation action out of one or more dynamic thing groups\. This setting is used only if the thing is already in the maximum number of groups\.  | 
|  `thingGroupNames`  |  array of strings  |  Required\. The list of groups to which you want to add the things that triggered the mitigation action\. You can add a thing to a maximum of 10 groups, but you cannot add a thing to more than one group in the same hierarchy\. You must provide at least one group name\. Length constraints: Minimum length of 1\. Maximum length of 128\. Pattern: \[a\-zA\-Z0\-9:\_\-\]\+  | 


**Parameters for EnableIoTLogging**  

| Name | Type | Description | 
| --- | --- | --- | 
|  `logLevel`  |  string  |  Required\. Specifies which types of information are logged\. Valid values, from most verbose to least, are `DEBUG`, `INFO`, `ERROR`, and `WARN`\. You cannot specify a logging level of `DISABLED`\.  | 
|  `roleArnForLogging`  |  string  |  Required\. The ARN of the IAM role used for logging\. Minimum length of 20\. Maximum length of 2048\.  | 


**Parameters for PublishingFindingToSns**  

| Name | Type | Description | 
| --- | --- | --- | 
|  `topicArn`  |  string  |  Required\. The ARN of the topic to which you want to publish the findings\. Minimum length of 20\. Maximum length of 2048\.  | 


**Parameters for ReplaceDefaultPolicyVersion**  

| Name | Type | Description | 
| --- | --- | --- | 
|  `templateName`  |  string  |  Required\. The name of the template to be applied\. The only supported value is `BLANK_POLICY`\.  | 


**Parameters for UpdateCACertificate**  

| Name | Type | Description | 
| --- | --- | --- | 
|  `action`  |  string  |  Required\. The action that you want to apply to the CA certificate\. The only supported value is `DEACTIVATE`\.  | 


**Parameters for UpdateDeviceCertificate**  

| Name | Type | Description | 
| --- | --- | --- | 
|  `action`  |  string  |  Required\. The action that you want to apply to the device certificate\. The only supported value is `DEACTIVATE`\.  | 


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  `actionArn`  |  string  |  The ARN for the new mitigation action\.  | 
|  `actionId`  |  string  |  The unique identifier for the new mitigation action\.  | 

 **Errors**

`InvalidRequestException`  
The contents of the request were invalid\.

`LimitExceededException`  
A limit has been exceeded\. For information about mitigation action limits, see [Service Limits](https://docs.aws.amazon.com/iot/latest/developerguide/)\.

`RequestAlreadyExistsException`  
A mitigation action with this name already exists\. This error occurs only if another mitigation action exists with the same name but different parameters\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

**Example**

This example defines a mitigation action that, when applied, deactivates a device certificate\.

```
$ aws iot create-mitigation-action --action-name "UpdateCACertName" --role-arn arn:aws:iam::123456789012:role/MitigationActionsValidRole --action-params "updateDeviceCertificateParams={action=DEACTIVATE}" 
```

The response resembles the following:

```
{
    "actionArn": "arn:aws:iot:us-east-1:123456789012:mitigationaction/UpdateCACertName",
    "actionId": "6a22b98e-0e27-4396-9b25-637d04959429"
}
```

## UpdateMitigationAction<a name="dd-api-iot-UpdateMitigationAction"></a>

Updates the definition of an action that can be applied to audit findings by using [StartAuditMitigationActionsTask](#dd-api-iot-StartAuditMitigationActionsTask)\. Each mitigation action can apply only one type of change\.

 **Synopsis**

```
aws iot update-mitigation-action \
    --action-name <value> \
    --role-arn <value> \
    --action-params <value]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
   "actionParams": { 
      "addThingsToThingGroupParams": { 
         "overrideDynamicGroups": boolean,
         "thingGroupNames": [ "string" ]
      },
      "enableIoTLoggingParams": { 
         "logLevel": "string",
         "roleArnForLogging": "string"
      },
      "publishFindingToSnsParams": { 
         "topicArn": "string"
      },
      "replaceDefaultPolicyVersionParams": { 
         "templateName": "string"
      },
      "updateCACertificateParams": { 
         "action": "string"
      },
      "updateDeviceCertificateParams": { 
         "action": "string"
      }
   },
   "roleArn": "string"
   ]
}
```


**`cli-input-json` fields**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  `roleArn`  |  string length\- max:2048 min:20  |  The ARN of the role that grants permission to AWS IoT to access information about your devices, policies, certificates, and other items when applying this action\.  | 
|  `actionParams`  |  map  |  Defines the type of action to be applied and the parameters for that mitigation action\. You can specify only one type of action and its parameters\.  | 

You must provide parameters for the type of action that you are defining\. You can provide only one action type and its parameters\. These are the supported action types:
+ `ADD_THINGS_TO_THING_GROUP`
+ `ENABLE_IOT_LOGGING`
+ `PUBLISH_FINDING_TO_SNS`
+ `REPLACE_DEFAULT_POLICY_VERSION`
+ `UPDATE_CA_CERTIFICATE`
+ `UPDATE_DEVICE_CERTIFICATE`


**Parameters for AddThingsToThingGroup**  

| Name | Type | Description | 
| --- | --- | --- | 
|  `overrideDynamicGroups`  |  boolean  |  Optional\. Specifies if this mitigation action can move the things that triggered the mitigation action out of one or more dynamic thing groups\. This setting is used only if the thing is already in the maximum number of groups\.  | 
|  `thingGroupNames`  |  array of strings  |  Required\. The list of groups to which you want to add the things that triggered the mitigation action\. You can add a thing to a maximum of 10 groups, but you cannot add a thing to more than one group in the same hierarchy\. You must provide at least one group name\. Length constraints: Minimum length of 1\. Maximum length of 128\. Pattern: \[a\-zA\-Z0\-9:\_\-\]\+  | 


**Parameters for EnableIoTLogging**  

| Name | Type | Description | 
| --- | --- | --- | 
|  `logLevel`  |  string  |  Required\. Specifies what types of information are to be logged\. Valid values, from most verbose to least, are `DEBUG`, `INFO`, `ERROR`, and `WARN`\. You cannot specify a logging level of `DISABLED`\.  | 
|  `roleArnForLogging`  |  string  |  Required\. The ARN of the IAM role used for logging\. Minimum length of 20\. Maximum length of 2048\.  | 


**Parameters for PublishingFindingToSns**  

| Name | Type | Description | 
| --- | --- | --- | 
|  `topicArn`  |  string  |  Required\. The ARN of the topic to which you want to publish the findings\. Minimum length of 20\. Maximum length of 2048\.  | 


**Parameters for ReplaceDefaultPolicyVersion**  

| Name | Type | Description | 
| --- | --- | --- | 
|  `templateName`  |  string  |  Required\. The name of the template to be applied\. The only supported value is `BLANK_POLICY`\.  | 


**Parameters for UpdateCACertificate**  

| Name | Type | Description | 
| --- | --- | --- | 
|  `action`  |  string  |  Required\. The action that you want to apply to the CA certificate\. The only supported value is `DEACTIVATE`\.  | 


**Parameters for UpdateDeviceCertificate**  

| Name | Type | Description | 
| --- | --- | --- | 
|  `action`  |  string  |  Required\. The action that you want to apply to the device certificate\. The only supported value is `DEACTIVATE`\.  | 


**cli output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  `actionArn`  |  string  |  The ARN for the mitigation action\.  | 
|  `actionId`  |  string  |  The unique identifier for the mitigation action\.  | 

 **Errors**

`InvalidRequestException`  
The contents of the request were invalid\.

`ResourceNotFoundException`  
No mitigation action with the specified name was found\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## ListMitigationActions<a name="dd-api-iot-ListMitigationActions"></a>

You use the ListMitigationActions command to get a list of all mitigation actions in your AWS account\.

 **Synopsis**

```
aws iot list-mitigation-actions \
    [--action-type <value>] \
    [--max-results <value>] \
    [--next-token <value>]
```


**Input parameters**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  `action-type`  |  string  |  Specify a value to limit the results to mitigation actions with a specific action type\. Supported values are `UPDATE_DEVICE_CERTIFICATE`, `UPDATE_CA_CERTIFICATE`, `ADD_THINGS_TO_THING_GROUP`, `REPLACE_DEFAULT_POLICY_VERSION`, `ENABLE_IOT_LOGGING`, or `PUBLISH_FINDING_TO_SNS`\.  | 
|  `max-results`  |  integer  |  The maximum number of results to return at one time\. The default is 25\. Valid range is from 1 to 250\.  | 
|  `next-token`  |  string  |  The token to retrieve the next set of results\.  | 

Output JSON:

```
{
   "actionIdentifiers": [ 
      { 
         "actionArn": "string",
         "actionName": "string",
         "creationDate": number
      }
   ],
   "nextToken": "string"
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  `actionIdentifiers`  |  map  |  Information about the collection of mitigation actions that match the specified parameters\.  | 
|  `actionArn`  |  string  |  The ARN for the mitigation action\.  | 
|  `actionName`  |  string  |  The unique identifier for the new mitigation action\. Maximum length is 128\. Pattern: \[a\-zA\-Z0\-9\_\-\]\+  | 
|  `creationDate`  |  timestamp  |  The date and time when the mitigation action was created\.  | 
|  `nextToken`  |  string  |  The token to retrieve the next set of results\.  | 

 **Errors**

`InvalidRequestException`  
The contents of the request were invalid\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

**Example**

This example lists all mitigation actions defined for your account in the region\.

```
$ aws iot list-mitigation-actions 
```

The response resembles the following:

```
{
    "actionIdentifiers": [
        {
            "actionName": "UpdateCACertName",
            "actionArn": "arn:aws:iot:us-east-1:123456789012:mitigationaction/UpdateCACertName",
            "creationDate": 1560173584.521
        }
    ]
}
```

## DescribeMitigationAction<a name="dd-api-iot-DescribeMitigationAction"></a>

You use the DescribeMitigationAction command to view details for a mitigation action in your AWS account\.

 **Synopsis**

```
aws iot describe-mitigation-action --action-name <value>
```


**Input parameters**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  `action-name`  |  string  |  The friendly name that uniquely identifies the mitigation action\. Maximum length is 128\. Pattern: \[a\-zA\-Z0\-9\_\-\]\+  | 

Output JSON:

```
{
   "actionArn": "string",
   "actionId": "string",
   "actionName": "string",
   "actionParams": { 
      "addThingsToThingGroupParams": { 
         "overrideDynamicGroups": boolean,
         "thingGroupNames": [ "string" ]
      },
      "enableIoTLoggingParams": { 
         "logLevel": "string",
         "roleArnForLogging": "string"
      },
      "publishFindingToSnsParams": { 
         "topicArn": "string"
      },
      "replaceDefaultPolicyVersionParams": { 
         "templateName": "string"
      },
      "updateCACertificateParams": { 
         "action": "string"
      },
      "updateDeviceCertificateParams": { 
         "action": "string"
      }
   },
   "actionType": "string",
   "creationDate": number,
   "lastModifiedDate": number,
   "roleArn": "string"
}
```

Each mitigation action has only one type, so only one of the sets of parameters appears\.


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  `actionArn`  |  string  |  The ARN for the mitigation action\.  | 
|  `actionId`  |  string  |  A unique identifier for the mitigation action\.  | 
|  `actionName`  |  string  |  The unique friendly name for the new mitigation action\. Maximum length is 128\. Pattern: \[a\-zA\-Z0\-9\_\-\]\+  | 
|  `actionParams`  |  map  |  Defines the type of action to be applied and the parameters for that mitigation action\.  | 
|  `addThingsToThingGroupParams`  |  map  |  Parameters to define a mitigation action that moves devices associated with a certificate to one or more specified thing groups, typically for quarantine\.  | 
|  `overrideDynamicGroups`  |  boolean  |  Specifies if this mitigation action can move the things that triggered the mitigation action even if they are part of one or more dynamic thing groups\.  | 
|  `thingGroupNames`  |  array of strings  |  The list of groups to which you want to add the things that triggered the mitigation action\. You can add a thing to a maximum of 10 groups, but you cannot add a thing to more than one group in the same hierarchy\. You must provide at least one group name\. Minimum length of 1\. Maximum length of 128\. Pattern: \[a\-zA\-Z0\-9:\_\-\]\+  | 
|  `enableIoTLoggingParams`  |  map  |  Parameters to define a mitigation action that enables AWS IoT logging at a specified level of detail\.  | 
|  `logLevel`  |  string  |  Specifies the types of information to log\. Valid values, from most verbose to least, are `DEBUG`, `INFO`, `ERROR`, `WARN`, and `DISABLED`\.  | 
|  `roleArnForLogging`  |  string  |  The ARN of the IAM role used for logging\. Minimum length of 20\. Maximum length of 2048\.  | 
|  `publishFindingToSnsParams`  |  map  |  Parameters to define a mitigation action that publishes findings to Amazon SNS\. You can implement your own custom actions in response to the Amazon SNS messages\.  | 
|  `topicArn`  |  string  |  The ARN of the topic to which you want to publish the findings\. Minimum length of 20\. Maximum length of 2048\.  | 
|  `replaceDefaultPolicyVersionParams`  |  map  |  Parameters to define a mitigation action that adds a blank policy to restrict permissions\.  | 
|  `templateName`  |  string  |  The name of the template to be applied\. The only supported value is `BLANK_POLICY`\.  | 
|  `updateCACertificateParams`  |  map  |  Parameters to define a mitigation action that changes the state of the CA certificate to inactive\.  | 
|  `action`  |  string  |  The action that you want to apply to the CA certificate\. The only supported value is `DEACTIVATE`\.  | 
|  `updateDeviceCertificateParams`  |  map  |  Parameters to define a mitigation action that changes the state of the device certificate to inactive\.  | 
|  `action`  |  string  |  The action that you want to apply to the device certificate\. The only supported value is `DEACTIVATE`\.  | 
|  `actionType`  |  string  |  The type of action being applied\.  | 
|  `creationDate`  |  timestamp  |  The date and time when the mitigation action was created\.  | 
|  `lastModifiedDate`  |  timestamp  |  The date and time when the mitigation action was most recently changed\.  | 
|  `roleArn`  |  string  |  The ARN for the role to use when applying this mitigation action\.  | 

 **Errors**

`ResourceNotFoundException`  
No mitigation action with the specified name was found\.

`InvalidRequestException`  
The contents of the request were invalid\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

**Example**

This example gets the definition for the mitigation action named `UpdateCACertName`\.

```
$ aws iot describe-mitigation-action --action-name UpdateCACertName 
```

The response resembles the following:

```
{
    "actionName": "UpdateCACertName",
    "actionType": "UPDATE_DEVICE_CERTIFICATE",
    "actionArn": "arn:aws:iot:us-east-1:123456789012:mitigationaction/UpdateCACertName",
    "actionId": "6a22b98e-0e27-4396-9b25-637d04959429",
    "roleArn": "arn:aws:iam::123456789012:role/MitigationActionsValidRole",
    "actionParams": {
        "updateDeviceCertificateParams": {
            "action": "DEACTIVATE"
        }
    },
    "creationDate": 1560173584.521,
    "lastModifiedDate": 1560173584.521
}
```

## DeleteMitigationAction<a name="dd-api-iot-DeleteMitigationAction"></a>

You use the DeleteMitigationAction command to remove a mitigation action from your AWS account\. To make the DeleteMitigationAction command idempotent, it does not throw a `ResourceNotFoundException` if you try to delete a mitigation action that doesn't exist\. Instead, DeleteMitigationAction returns success when the action name does not exist\.

 **Synopsis**

```
aws iot delete-mitigation-action --action-name <value>
```


**Input parameters**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  `action-name`  |  string  |  The friendly name that uniquely identifies the mitigation action\. Maximum length is 128\. Pattern: \[a\-zA\-Z0\-9\_\-\]\+  | 

 **Errors**

`InvalidRequestException`  
The contents of the request were invalid\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## StartAuditMitigationActionsTask<a name="dd-api-iot-StartAuditMitigationActionsTask"></a>

You use the StartAuditMitigationActionsTask command to apply a set of mitigation actions to findings from one or more audits\.

 **Synopsis**

```
aws iot  start-audit-mitigation-actions-task \
    --task-id <value> \
    --audit-task-id <value> \
    --audit-checks-to-action-mapping <value> \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

 `cli-input-json` format

```
{
   "auditCheckToActionsMapping": { 
      "string" : [ "string" ]
   },
   "clientRequestToken": "string",
   "target": { 
      "auditTaskId": "string",
      "findingIds": [ "string" ]
   }
}
```


**Input parameters**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  `taskId`  |  string  |  A unique identifier for the audit mitigations task\. Maximum length is 128\. Pattern: \[a\-zA\-Z0\-9\_\-\]\+  | 
|  `auditCheckToActionsMapping`  |  string to array of strings map  |  Specifies the list of actions to apply to a particular audit check\. Array must contain at least 1 but not more than 5 members\. Maximum length is 128\. Pattern: \[a\-zA\-Z0\-9\_\-\]\+  | 
|  `clientRequestToken`  |  string  |  Each audit mitigation task must have a unique client request token\. If you try to start a new task with the same task ID as an existing task, but with a different token, an exception occurs\. If you omit this value, a unique client request token is generated automatically\. Minimum length of 1\. Maximum length of 64\. Pattern: \[a\-zA\-Z0\-9\_\-\]\+$  | 
|  `target`  |  map  |  Specifies the audit findings to which the mitigation actions are applied\. You can apply them to the results of an audit task or a set of findings\.  | 
|  `auditTaskId`  |  string  |  A unique identifier for the audit to whose findings you want to apply the set of actions\. The `auditCheckToReasonCodeFilter` can further filter the results from the audit\. If you want to restrict the actions to a specific set of findings, use `findingIds` instead\. Maximum length is 40\. Pattern: \[a\-zA\-Z0\-9\\\-\]\+  | 
|  `findingIds`  |  array of strings  |  If the task applies a mitigation action to one or more listed findings, this value uniquely identifies those findings\. Array members: Minimum of 1 item\. Maximum of 25 items\. Maximum length of each item is 128\. Pattern: \[a\-zA\-Z0\-9\_\-\]\+  | 

Output JSON:

```
{
   "taskId": "string"
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  `taskId`  |  string  |  The unique identifier for the newly created audit mitigation actions task\. Use this identifier if you need to cancel the task \(`CancelAuditMitigationActionsTask`\) or if you want to view details of the task with `DesribeAuditMitigationAtionsTask`\.  | 

 **Errors**

`TaskAlreadyExistsException`  
This exception occurs if you attempt to start a task with the same task\-id as an existing task but with a different clientRequestToken\.

`InvalidRequestException`  
The contents of the request were invalid\.

`LimitExceededException`  
This exception occurs if you try to start more than 10 mitigation action tasks\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

**Example**

This example starts a task to apply the `UpdateCACertAction` that you defined to the audit findings from the audit whose taskId is `aef320b958891041e0c60c088afac64c` and the audit check is `CA_CERTIFICATE_EXPIRING_CHECK.`\.

```
$ aws iot start-audit-mitigation-actions-task --task-id "myActionsTaskId" --target "auditTaskId=aef320b958891041e0c60c088afac64c" --audit-check-to-actions-mapping "CA_CERTIFICATE_EXPIRING_CHECK=UpdateCACertAction" --client-request-token "adhadhahda"  
```

The response looks like the following\.

```
{
    "taskId": "myActionsTaskId"
}
```

## CancelAuditMitigationActionsTask<a name="dd-api-iot-CancelAuditMitigationActionsTask"></a>

You use the CancelAuditMitigationActionsTask command to stop execution for an audit mitigation task if it is not already complete\. 

 **Synopsis**

```
aws iot cancel-audit-mitigation-actions-task --task-id <value>
```


**Input parameters**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  `task-id`  |  string  |  A unique identifier for the audit mitigations task that you want to cancel\. Maximum length is 128\. Pattern: \[a\-zA\-Z0\-9\_\-\]\+  | 

 **Errors**

`ResourceNotFoundException`  
An audit mitigation actions task with the specified task ID was not found\.

`InvalidRequestException`  
The contents of the request were invalid\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## ListAuditMitigationActionsExecutions<a name="dd-api-iot-ListAuditMitigationActionsExecutions"></a>

You use the ListAuditMitigationActionsExecutions command to display details about an audit mitigation task that has been started\. 

 **Synopsis**

```
aws iot list-audit-mitigation-actions-executions \
    [--action-status <value>] \
    --finding-id <value> \
    [--max-results <value>] \
    [--next-token <value>]  \
    --task-id <value>  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```


**Input parameters**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  `action-status`  |  string  |  Specify this filter to limit results to those executions with a specific status\. Supported values are `IN_PROGRESS`, `COMPLETED`, `FAILED`, `CANCELED`, `SKIPPED `, and `PENDING`\.  | 
|  `finding-id`  |  string  |  Specify this filter to limit results to those that were applied to a specific audit finding\. Maximum length is 128\. Pattern: \[a\-zA\-Z0\-9\_\-\]\+  | 
|  `max-results`  |  number  |  The maximum number of results to return at one time\. The default is 25\. Valid range: minimum of 1\. maximum of 250\.  | 
|  `next-token`  |  string  |  The token for the next set of results\.  | 
|  `task-id`  |  string  |  A unique identifier for the audit mitigations task whose details you want to display\. Maximum length is 128\. Pattern: \[a\-zA\-Z0\-9\_\-\]\+  | 

Output JSON:

```
{
   "actionsExecutions": [ 
      { 
         "actionId": "string",
         "actionName": "string",
         "endTime": number,
         "errorCode": "string",
         "findingId": "string",
         "message": "string",
         "startTime": number,
         "status": "string",
         "taskId": "string"
      }
   ],
   "nextToken": "string"
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  `actionsExecutions`  |  map  |  A collection of information about the audit mitigation task executions that have been started for your AWS account\.  | 
|  `actionId`  |  string  |  The unique identifier for the mitigation action being applied by the task\.  | 
|  `actionName`  |  string  |  The friendly name of the mitigation action being applied by the task\.  | 
|  `endTime`  |  timestamp  |  The date and time when the task was completed or canceled\.  | 
|  `errorCode`  |  string  |  If an error occurred, the code that indicates the error type\.  | 
|  `findingId`  |  string  |  The unique identifier for the finding to which the task and associated mitigation actions are applied\.  | 
|  `message`  |  string  |  If an error occurred, a message that describes the error\.  | 
|  `startTime`  |  timestamp  |  The date and time when the task was started\.  | 
|  `status`  |  string  |  The current status of the task being executed\. Supported values are `IN_PROGRESS`, `COMPLETED`, `FAILED`, `CANCELED`, `SKIPPED `, and `PENDING`  | 
|  `taskId`  |  string  |  The unique identifier for the task that applies the mitigation action\.  | 
|  `nextToken`  |  string  |  The token for the next set of results\.  | 

The status field can have the following values:


****  

| Status | What It Means | 
| --- | --- | 
| IN\_PROGRESS | AWS IoT Device Defender is applying the mitigation action to the finding\. | 
| COMPLETED | The mitigation action was applied successfully to the finding\. | 
| FAILED | The mitigation action failed to be applied to the finding\. The error code provides more information\. | 
| CANCELED | The action execution was canceled because the user canceled the task\. | 
| SKIPPED | The action execution was skipped because one of the actions in the list failed\. | 
| PENDING | The action execution has not started yet\. | 

The following error codes can be returned:

**INSUFFICIENT\_PERMISSIONS**  
+ The roleArn that was provided for the mitigation action does not have permissions to apply the action\.

**INVALID\_STATE\_OF\_RESOURCE**  
+ The resource in the finding is not in a state that allows the mitigation action to be applied successfully\. This error occurs for the ADD\_THINGS\_TO\_THING\_GROUP action type if the thing is already in the maximum number of allowed groups\. The error occurs for the UPDATE\_DEVICE\_CERTIFICATE and UPDATE\_CA\_CERTIFICATE mitigation action types if the certificate has already been revoked\.

 **Errors**

`InvalidRequestException`  
The contents of the request were invalid\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

## ListAuditMitigationActionsTasks<a name="dd-api-iot-ListAuditMitigationActionsTasks"></a>

You use the ListAuditMitigationActionsTasks command to get a list of audit mitigation action tasks that match the specified filters\.

 **Synopsis**

```
aws iot list-audit-mitigation-actions-tasks
    [--audit-task-id <value>] \
    --end-time <value> \
    [--finding-id <value>] \
    [--max-results <value>] \
    [--next-token <value>] \
    --start-time <value> \ 
    [--task-status <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```


**Input parameters**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  `audit-task-id`  |  string  |  A unique identifier for the audit mitigations task that you want to display\. Minimum length is 1\. Maximum length is 40\. Pattern: \[a\-zA\-Z0\-9\_\-\]\+  | 
|  `end-time`  |  timestamp  |  Required\. Specify this filter to list tasks that ended on or before this date\. This date should not be more than 90 days in the past\.  | 
|  `finding-id`  |  string  |  Specify this filter to list tasks that applied to a particular finding\. Maximum length is 128\. Pattern: \[a\-zA\-Z0\-9\_\-\]\+  | 
|  `max-results`  |  number  |  The maximum number of results to return at one time\. The default is 25\. Valid range: Minimum value is 1\. Maximum value is 250\.  | 
|  `next-token`  |  string  |  The token for the next set of results\.  | 
|  `start-time`  |  timestamp  |  Required\. Specify this filter to limit the results to tasks that were started on or after this date and time\. This date should not be more than 90 days in the past\.  | 
|  `task-status`  |  string  |  Specify this filter to limit the results to tasks that are in a specified state\. Supported values are: `IN_PROGRESS`, `COMPLETED`, `FAILED`, and `CANCELED`\.  | 

Output JSON:

```
{
   "nextToken": "string",
   "tasks": [ 
      { 
         "startTime": number,
         "taskId": "string",
         "taskStatus": "string"
      }
   ]
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  `nextToken`  |  string  |  The token for the next set of results\.  | 
|  `tasks`  |  map  |  The collection of audit mitigation action tasks that matched the filter criteria\.  | 
|  `startTime`  |  timestamp  |  Parameters to define a mitigation action that moves devices associated with a certificate to one or more specified thing groups, typically for quarantine\.  | 
|  `taskId`  |  string  |  The unique identifier for the task\.  | 
|  `taskStatus`  |  string  |  The current state of the task\.  | 

 **Errors**

`InvalidRequestException`  
The contents of the request were invalid\. This error can occur if you specify dates more than 90 days in the past\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.

**Example**

This example lists the audit mitigation tasks that ran during the specified timeframe\.

```
$ aws iot list-audit-mitigation-actions-tasks --start-time 1560001663 --end-time 1560174463 
```

The response looks like the following\.

```
{
    "tasks": [
        {
            "taskId": "actionsTaskId",
            "startTime": 1560174232.07,
            "taskStatus": "CANCELED"
        },
        {
            "taskId": "4e8acaf8-b7f0-4484-9b0d-01b979e61d8d",
            "startTime": 1560060994.965,
            "taskStatus": "COMPLETED"
        },
        {
            "taskId": "0e8f5a95-e43f-43fa-9aa4-57ff3efb946d",
            "startTime": 1560060860.243,
            "taskStatus": "COMPLETED"
        },
        {
            "taskId": "92d60009-33a1-45a7-a3c6-b28e938ec68a",
            "startTime": 1560060707.653,
            "taskStatus": "COMPLETED"
        },
        {
            "taskId": "bdea63c8-58bd-4798-9cb3-e949c7f1969a",
            "startTime": 1560060531.123,
            "taskStatus": "COMPLETED"
        },
        {
            "taskId": "61e146ef-69f4-41bf-9d37-5d667f72ef3d",
            "startTime": 1560060388.035,
            "taskStatus": "COMPLETED"
        },
        {
            "taskId": "2ef289dc-9bbd-422b-95ba-d96b7e626a60",
            "startTime": 1560060256.695,
            "taskStatus": "COMPLETED"
        },
        {
            "taskId": "a6cdc16b-6be3-4800-bafa-2b86d3f5d639",
            "startTime": 1560060097.613,
            "taskStatus": "COMPLETED"
        },
        {
            "taskId": "7ccf351b-e560-4eb2-8796-1dc1bcb25396",
            "startTime": 1560059925.477,
            "taskStatus": "COMPLETED"
        },
        {
            "taskId": "8f69ee5d-3e35-4c91-88e8-3c5f84c96fde",
            "startTime": 1560059345.473,
            "taskStatus": "COMPLETED"
        }
    ]
}
```

## DescribeAuditMitigationActionsTask<a name="dd-api-iot-DescribeAuditMitigationActionsTask"></a>

You use the DescribeAuditMitigationActionsTask command to display details about an audit mitigation task that has been started\. 

 **Synopsis**

```
aws iot describe-audit-mitigation-actions-task --taskId <value>
```


**Input parameters**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  `taskId`  |  string  |  Required\. A unique identifier for the audit mitigations task whose details you want to display\. Maximum length is 128\. Pattern: \[a\-zA\-Z0\-9\_\-\]\+  | 

Output JSON:

```
{
   "actionsDefinition": [ 
      { 
         "actionParams": { 
            "addThingsToThingGroupParams": { 
               "overrideDynamicGroups": boolean,
               "thingGroupNames": [ "string" ]
            },
            "enableIoTLoggingParams": { 
               "logLevel": "string",
               "roleArnForLogging": "string"
            },
            "publishFindingToSnsParams": { 
               "topicArn": "string"
            },
            "replaceDefaultPolicyVersionParams": { 
               "templateName": "string"
            },
            "updateCACertificateParams": { 
               "action": "string"
            },
            "updateDeviceCertificateParams": { 
               "action": "string"
            }
         },
         "id": "string",
         "name": "string",
         "roleArn": "string"
      }
   ],
   "auditCheckToActionsMapping": { 
      "string" : [ "string" ]
   },
   "endTime": number,
   "startTime": number,
   "target": { 
      "auditCheckToReasonCodeFilter": { 
         "string" : [ "string" ]
      },
      "auditTaskId": "string",
      "findingIds": [ "string" ]
   },
   "taskStatistics": { 
      "string" : { 
         "canceledFindingsCount": number,
         "failedFindingsCount": number,
         "skippedFindingsCount": number,
         "succeededFindingsCount": number,
         "totalFindingsCount": number
      }
   },
   "taskStatus": "string"
}
```


**CLI output fields:**  

|  Name  |  Type  |  Description  | 
| --- | --- | --- | 
|  `>actionsDefinition`  |  map  |  The set of actions \(and their metadata\) applied by this audit mitigation action task\.  | 
|  `actionParams`  |  map  |  The parameters used when applying the mitigation action\.  | 
|  `addThingsToThingGroupParam`  |  map  |  Parameters to define a mitigation action that moves devices associated with a certificate to one or more specified thing groups, typically for quarantine\.  | 
|  `overrideDynamicGroups`  |  boolean  |  Specifies if this mitigation action can move the things that triggered the mitigation action even if they are part of one or more dynamic thing groups\.  | 
|  `thingGroupNames`  |  array of strings  |  The list of groups to which this mitigation action adds the things in the target for this audit mitigation actions task\.  | 
|  `enableIoTLoggingParams`  |  map  |  Parameters to define a mitigation action that enables AWS IoT logging at a specified level of detail\.  | 
|  `logLevel`  |  string  |  Specifies the types of information to log\.  | 
|  `roleArnForLogging`  |  string  |  The ARN of the IAM role used for logging\.  | 
|  `publishFindingToSnsParams`  |  map  |  Parameters to define a mitigation action that publishes findings to Amazon SNS\. You can implement your own custom actions in response to the Amazon SNS messages\.  | 
|  `topicArn`  |  string  |  The ARN of the topic to which you want to publish the findings\.  | 
|  `replaceDefaultPolicyVersionParams`  |  map  |  Parameters to define a mitigation action that adds a blank policy to restrict permissions\.  | 
|  `templateName`  |  string  |  The name of the template to be applied\. The only supported value is `BLANK_POLICY`\.  | 
|  `updateCACertificateParams`  |  map  |  Parameters to define a mitigation action that changes the state of the CA certificate to inactive\.  | 
|  `action`  |  string  |  The action that you want to apply to the CA certificate\. The only supported value is `DEACTIVATE`\.  | 
|  `updateDeviceCertificateParams`  |  map  |  Parameters to define a mitigation action that changes the state of the device certificate to inactive\.  | 
|  `action`  |  string  |  The action that you want to apply to the device certificate\. The only supported value is `DEACTIVATE`\.  | 
|  `id`  |  string  |  The unique identifier for the mitigation action applied as part of this audit mitigation task\.  | 
|  `name`  |  string  |  The friendly name for the mitigation action applied as part of this audit mitigation task\.  | 
|  `roleArn`  |  string  |  The ARN for the role used when applying this mitigation task\.  | 
|  `auditCheckToActionsMapping`  |  map  |  For a type of audit check, specifies which mitigation actions are applied by this audit mitigation task\.  | 
|  `endTime`  |  timestamp  |  If the audit mitigation action task is complete or was canceled, the date and time when execution stopped\.  | 
|  `startTime`  |  timestamp  |  The date and time when execution of this audit mitigation action task began\.  | 
|  `target`  |  map  |  Information about the targets to which the mitigation actions are applied as part of this audit mitigation actions task\.  | 
|  `auditCheckToReasonCodeFilter`  |  map  |  Specifies a set of audit checks and reason codes used to identify the audit findings to which this audit mitigation actions task applies\.  | 
|  `auditTaskId`  |  string  |  Specifies an audit on whose findings this audit mitigation actions task applies\.  | 
|  `findingIds`  |  array of strings  |  Specifies a set of audit findings on which this audit mitigation actions task applies\.  | 
|  `taskStatistics`  |  map  |  The set of execution statistics for this audit mitigation actions task\.  | 
|  `canceledFindingsCount`  |  number  |  If the task was canceled, the number of findings in the target for this audit mitigation actions task to which mitigation actions were not applied\.  | 
|  `failedFindingsCount`  |  number  |  The number of findings in the target for this audit mitigation actions task to which mitigation actions failed when applied\.  | 
|  `skippedFindingsCount`  |  number  |  The number of findings in the target for this audit mitigation actions task to which mitigation actions were not applied because a they were excluded by a filter applied to the task\.  | 
|  `succeededFindingsCount`  |  number  |  The number of findings in the target for this audit mitigation actions task to which mitigation actions were applied without error or cancellation\.  | 
|  `totalFindingsCount`  |  number  |  The total number of findings in the target for this audit mitigation actions task\.  | 
|  `taskStatus`  |  string  |  The current state of the audit mitigation actions task\. The status is failed if one or more of the actions for the finding failed to be applied\.  | 

 **Errors**

`ResourceNotFoundException`  
No audit mitigation task was found with the specified `taskId`\.

`InvalidRequestException`  
The contents of the request were invalid\.

`ThrottlingException`  
The rate exceeds the limit\.

`InternalFailureException`  
An unexpected error has occurred\.