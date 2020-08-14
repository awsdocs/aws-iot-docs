# Mitigation actions<a name="device-defender-mitigation-actions"></a>

You can use AWS IoT Device Defender to take actions to mitigate issues that were found during an audit\. AWS IoT Device Defender provides predefined actions for the different audit checks\. You configure those actions for your AWS account and then apply them to a set of findings\. Those findings can be:

**Note**  
Mitigation actions won't be performed on suppressed audit findings\. For more information about audit finding suppressions, see [Audit finding suppressions](audit-finding-suppressions.md)\.
+ All findings from an audit\. This option is available in both the AWS IoT console and by using the AWS CLI\.
+ A list of individual findings\. This option is only available by using the AWS CLI\.
+ A filtered set of findings from an audit\.

The following table lists the types of audit checks and the supported mitigation actions for each:


**Audit check to mitigation action mapping**  

| Audit check | Supported mitigation actions | 
| --- | --- | 
| REVOKED\_CA\_CERT\_CHECK | PUBLISH\_FINDING\_TO\_SNS, UPDATE\_CA\_CERTIFICATE | 
| DEVICE\_CERTIFICATE\_SHARED\_CHECK | PUBLISH\_FINDING\_TO\_SNS, UPDATE\_DEVICE\_CERTIFICATE, ADD\_THINGS\_TO\_THING\_GROUP | 
| UNAUTHENTICATED\_COGNITO\_ROLE\_OVERLY\_PERMISSIVE\_CHECK | PUBLISH\_FINDING\_TO\_SNS | 
| AUTHENTICATED\_COGNITO\_ROLE\_OVERLY\_PERMISSIVE\_CHECK | PUBLISH\_FINDING\_TO\_SNS | 
| IOT\_POLICY\_OVERLY\_PERMISSIVE\_CHECK | PUBLISH\_FINDING\_TO\_SNS, REPLACE\_DEFAULT\_POLICY\_VERSION | 
| CA\_CERT\_APPROACHING\_EXPIRATION\_CHECK | PUBLISH\_FINDING\_TO\_SNS, UPDATE\_CA\_CERTIFICATE | 
| CONFLICTING\_CLIENT\_IDS\_CHECK | PUBLISH\_FINDING\_TO\_SNS | 
| DEVICE\_CERT\_APPROACHING\_EXPIRATION\_CHECK | PUBLISH\_FINDING\_TO\_SNS, UPDATE\_DEVICE\_CERTIFICATE, ADD\_THINGS\_TO\_THING\_GROUP | 
| REVOKED\_DEVICE\_CERT\_CHECK | PUBLISH\_FINDING\_TO\_SNS, UPDATE\_DEVICE\_CERTIFICATE, ADD\_THINGS\_TO\_THING\_GROUP | 
| LOGGING\_DISABLED\_CHECK | PUBLISH\_FINDING\_TO\_SNS, ENABLE\_IOT\_LOGGING | 
| DEVICE\_CERTIFICATE\_KEY\_QUALITY\_CHECK | PUBLISH\_FINDING\_TO\_SNS, UPDATE\_DEVICE\_CERTIFICATE, ADD\_THINGS\_TO\_THING\_GROUP | 
| CA\_CERTIFICATE\_KEY\_QUALITY\_CHECK | PUBLISH\_FINDING\_TO\_SNS, UPDATE\_CA\_CERTIFICATE | 
| IOT\_ROLE\_ALIAS\_OVERLY\_PERMISSIVE\_CHECK | PUBLISH\_FINDING\_TO\_SNS | 
| IOT\_ROLE\_ALIAS\_ALLOWS\_ACCESS\_TO\_UNUSED\_SERVICES\_CHECK | PUBLISH\_FINDING\_TO\_SNS | 

All audit checks support publishing the audit findings to Amazon SNS so you can take custom actions in response to the notification\. Each type of audit check can support additional mitigation actions:

**REVOKED\_CA\_CERT\_CHECK**  
+ Change the state of the certificate to mark it as inactive in AWS IoT\.

**DEVICE\_CERTIFICATE\_SHARED\_CHECK**  
+ Change the state of the device certificate to mark it as inactive in AWS IoT\.
+ Add the devices that use that certificate to a thing group\.

**UNAUTHENTICATED\_COGNITO\_ROLE\_OVERLY\_PERMISSIVE\_CHECK**  
+ No additional supported actions\.

**AUTHENTICATED\_COGNITO\_ROLE\_OVERLY\_PERMISSIVE\_CHECK**  
+ No additional supported actions\.

**IOT\_POLICY\_OVERLY\_PERMISSIVE\_CHECK**  
+ Add a blank AWS IoT policy version to restrict permissions\.

**CA\_CERT\_APPROACHING\_EXPIRATION\_CHECK**  
+ Change the state of the certificate to mark it as inactive in AWS IoT\.

**CONFLICTING\_CLIENT\_IDS\_CHECK**  
+ No additional supported actions\.

**DEVICE\_CERT\_APPROACHING\_EXPIRATION\_CHECK**  
+ Change the state of the device certificate to mark it as inactive in AWS IoT\.
+ Add the devices that use that certificate to a thing group\.

**DEVICE\_CERTIFICATE\_KEY\_QUALITY\_CHECK**  
+ Change the state of the device certificate to mark it as inactive in AWS IoT\.
+ Add the devices that use that certificate to a thing group\.

**CA\_CERTIFICATE\_KEY\_QUALITY\_CHECK**  
+ Change the state of the certificate to mark it as inactive in AWS IoT\.

**REVOKED\_DEVICE\_CERT\_CHECK**  
+ Change the state of the device certificate to mark it as inactive in AWS IoT\.
+ Add the devices that use that certificate to a thing group\.

**LOGGING\_DISABLED\_CHECK**  
+ Enable logging\.

AWS IoT Device Defender supports the following types of mitigation actions:


****  

|  Action type  | Notes | 
| --- | --- | 
| ADD\_THINGS\_TO\_THING\_GROUP | You specify the group to which you want to add the devices\. You also specify whether membership in one or more dynamic groups should be overridden if that would exceed the maximum number of groups to which the thing can belong\. | 
| ENABLE\_IOT\_LOGGING | You specify the logging level and the role with permissions for logging\. You cannot specify a logging level of DISABLED\. | 
| PUBLISH\_FINDING\_TO\_SNS | You specify the topic to which the finding should be published\. | 
| REPLACE\_DEFAULT\_POLICY\_VERSION | You specify the template name\. Replaces the policy version with a default or blank policy\. Only a value of BLANK\_POLICY is currently supported\. | 
| UPDATE\_CA\_CERTIFICATE | You specify the new state for the CA certificate\. Only a value of DEACTIVATE is currently supported\. | 
| UPDATE\_DEVICE\_CERTIFICATE | You specify the new state for the device certificate\. Only a value of DEACTIVATE is currently supported\. | 

By configuring standard actions when issues are found during an audit, you can respond to those issues consistently\. Using these defined mitigation actions also helps you resolve the issues more quickly and with less chance of human error\.

**Important**  
Applying mitigation actions that change certificates, add things to a new thing group, or replace the policy can have an impact on your devices and applications\. For example, devices might be unable to connect\. Consider the implications of the mitigation actions before you apply them\. You might need to take other actions to correct the problems before your devices and applications can function normally\. For example, you might need to provide updated device certificates\. Mitigation actions can help you quickly limit your risk, but you must still take corrective actions to address the underlying issues\.

Some actions, such as reactivating a device certificate, can only be performed manually\. AWS IoT Device Defender does not provide a mechanism to automatically roll back mitigation actions that have been applied\. 

## How to define and manage mitigation actions<a name="defender-manage-mitigation-actions"></a>

You can use the AWS IoT console or the AWS CLI to define and manage mitigation actions for your AWS account\.

### Create mitigation actions<a name="defender-create-mitigation-action"></a>

Each mitigation action that you define is a combination of a predefined action type and parameters specific to your account\.

**To use the AWS IoT console to create mitigation actions**

1. Open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Defend**, and then choose **Mitigation Actions**\.

1. On the **Mitigation Actions** page, choose **Create**\.  
![\[Create a new mitigation action window.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/create-mitigation-action.png)

1. On the **Create a Mitigation Action** page, in **Action name**, enter a unique name for your mitigation action\.

1. In **Action type**, specify the type of action that you want to define\.

1. Each action type requests a different set of parameters\. Enter the parameters for the action\. For example, if you choose the **Add things to thing group** action type, choose the destination group and select or clear **Override dynamic groups**\.

1. In **Action execution role**, choose the role under whose permissions the action is applied\.

1. Choose **Save** to save your mitigation action to your AWS account\.

**To use the AWS CLI to create mitigation actions**
+ Use the [CreateMitigationAction](mitigation-action-commands.md#dd-api-iot-CreateMitigationAction) command to create your mitigation action\. The unique name that you give the action is used when you apply that action to audit findings\. Choose a meaningful name\.

**To use the AWS IoT console to view and modify mitigation actions**

1. Open the [ AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Defend**, and then choose **Mitigation Actions**\.

   The **Mitigation Actions** page displays a list of all of the mitigation actions that are defined for your AWS account\.  
![\[List of mitigation actions.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/mitigation-actions.png)

1. Choose the action name link for the mitigation action that you want to change\.

1. Make your changes to the mitigation action\. Because the name of the mitigation action is used to identify it, you cannot change the name\.  
![\[Edit mitigation action page.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/update-mitigation-action.png)

1. Choose **Save** to save the changes to the mitigation action to your AWS account\.

**To use the AWS CLI to list a mitigation action**
+ Use the [ListMitigationActions](mitigation-action-commands.md#dd-api-iot-ListMitigationActions) command to list your mitigation actions\. If you want to change or delete a mitigation action, make a note of the name\.

**To use the AWS CLI to update a mitigation action**
+ Use the [UpdateMitigationAction](mitigation-action-commands.md#dd-api-iot-UpdateMitigationAction) command to change your mitigation action\.

**To use the AWS IoT console to delete a mitigation action**

1. Open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Defend**, and then choose **Mitigation Actions**\.

   The **Mitigation Actions** page displays all of the mitigation actions that are defined for your AWS account\.

1. Choose the ellipsis \(**\.\.\.**\) for the mitigation action that you want to delete, and then choose **Delete**\.

**To use the AWS CLI to delete mitigation actions**
+ Use the [UpdateMitigationAction](mitigation-action-commands.md#dd-api-iot-UpdateMitigationAction) command to change your mitigation action\.

**To use the AWS IoT console to view mitigation action details**

1. Open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Defend**, and then choose **Mitigation Actions**\.  
![\[List of mitigation actions.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/mitigation-actions.png)

   The **Mitigation Actions** page displays all of the mitigation actions that are defined for your AWS account\.

1. Choose the action name link for the mitigation action that you want to change\.

1. In the **Are you sure you want to delete the mitigation action** window, choose **Confirm**\.  
![\[Are you sure you want to delete the mitigation action confirmation window.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/delete-mitigation-action-confirmation.png)

**To use the AWS CLI to view mitigation action details**
+ Use the [DescribeMitigationAction](mitigation-action-commands.md#dd-api-iot-DescribeMitigationAction) command to view details for your mitigation action\.

## Apply mitigation actions<a name="defender-audit-apply-mitigation-actions"></a>

After you have defined a set of mitigation actions, you can apply those actions to the findings from an audit\. When you apply actions, you start an audit mitigation actions task\. This task might take some time to complete, depending on the set of findings and the actions that you apply to them\. For example, if you have a large pool of devices whose certificates have expired, it might take some time to deactivate all of those certificates or to move those devices to a quarantine group\. Other actions, such as enabling logging, can be completed quickly\.

You can view the list of action executions and cancel an execution that has not yet been completed\. Actions already performed as part of the canceled action execution are not rolled back\. If you are applying multiple actions to a set of findings and one of those actions failed, the subsequent actions are skipped for that finding \(but are still applied to other findings\)\. The task status for the finding is FAILED\. The `taskStatus` is set to failed if one or more of the actions failed when applied to the findings\. Actions are applied in the order in which they are specified\.

Each action execution applies a set of actions to a target\. That target can be a list of findings or it can be all findings from an audit\.

The following diagram shows how you can define an audit mitigation task that takes all findings from one audit and applies a set of actions to those findings\. A single execution applies one action to one finding\. The audit mitigation actions task outputs an execution summary\.

![\[Conceptual image shows an audit mitigation actions task.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/conceptual-mitigation-actions-audit.png)

The following diagram shows how you can define an audit mitigation task that takes a list of individual findings from one or more audits and applies a set of actions to those findings\. A single execution applies one action to one finding\. The audit mitigation actions task outputs an execution summary\.

![\[Conceptual image shows an audit mitigation actions task.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/conceptual-mitigation-actions-findings.png)

You can use the AWS IoT console or the AWS CLI to apply mitigation actions\.

**To use the AWS IoT console to apply mitigation actions by starting an action execution**

1. Open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Defend**, choose **Audit**, and then choose **Results**\.  
![\[List of audit results that shows the Start mitigation actions button.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/audit-results-start-mitigation-actions.png)

1. Choose the name for the audit to which you want to apply actions\.  
![\[Are you sure you want to start mitigation action task window.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/start-mitigation-action-task.png)

1. Choose **Start Mitigation Actions**\. This button is not available if all of your checks are compliant\.

1. In **Are you sure that you want to start mitigation action task**, the task name defaults to the audit ID, but you can change it to something more meaningful\.

1. For each type of check that had one or more noncompliant findings in the audit, you can choose one or more actions to apply\. Only actions that are valid for the check type are displayed\.
**Note**  
If you have not configured actions for your AWS account, the list of actions is empty\. You can choose the **click here** link to create one or more mitigation actions\.

1. When you have specified all of the actions that you want to apply, choose **Confirm**\.

**To use the AWS CLI to apply mitigation actions by starting an audit mitigation actions execution**

1. If you want to apply actions to all findings for the audit, use the [ ListAuditTasks](AuditCommands.md#dd-api-iot-ListAuditTasks) command to find the task ID\.

1. If you want to apply actions to selected findings only, use the [ ListAuditFindings](AuditCommands.md#dd-api-iot-ListAuditFindings) command to get the finding IDs\.

1. Use the [ListMitigationActions](mitigation-action-commands.md#dd-api-iot-ListMitigationActions) command and make note of the names of the mitigation actions that you want to apply\.

1. Use the [StartAuditMitigationActionsTask](mitigation-action-commands.md#dd-api-iot-StartAuditMitigationActionsTask) command to apply actions to the target\. Make note of the task ID\. You can use the ID to check the state of the action execution, review the details, or cancel it\.

**To use the AWS IoT console to view your action executions**

1. Open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Defend**, and then choose **Action Executions**\.  
![\[List of audit action executions.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/audit-action-executions.png)

   A list of action tasks shows when each was started and the current status\.

1. Choose the **Name** link to see details for the task\. The details include all of the actions that are applied by the task, their target, and their status\.  
![\[Details for the audit mitigation action task.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/audit-action-execution-task-details.png)

   You can use the **Show executions for** filters to focus on types of actions or action states\.

1. To see details for the task, in **Executions**, choose **Show**\.  
![\[Execution details for the audit mitigation action task.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/audit-action-execution-task-executions.png)

**To use the AWS CLI to list your started tasks**

1. Use [ListAuditMitigationActionsTasks](mitigation-action-commands.md#dd-api-iot-ListAuditMitigationActionsTasks) to view your audit mitigation actions tasks\. You can provide filters to narrow the results\. If you want to view details of the task, make note of the task ID\.

1. Use [ListAuditMitigationActionsExecutions](mitigation-action-commands.md#dd-api-iot-ListAuditMitigationActionsExecutions) to view execution details for a particular audit mitigation actions task\.

1. Use [DescribeAuditMitigationActionsTask](mitigation-action-commands.md#dd-api-iot-DescribeAuditMitigationActionsTask) to view details about the task, such as the parameters specified when it was started\.

**To use the AWS CLI to cancel a running audit mitigation actions task**

1. Use the [ListAuditMitigationActionsTasks](mitigation-action-commands.md#dd-api-iot-ListAuditMitigationActionsTasks) command to find the task ID for the task whose execution you want to cancel\. You can provide filters to narrow the results\.

1. Use the [CancelAuditMitigationActionsTask](mitigation-action-commands.md#dd-api-iot-CancelAuditMitigationActionsTask) command, using the task ID, to cancel your audit mitigation actions task\. You cannot cancel tasks that have been completed\. When you cancel a task, remaining actions are not applied, but mitigation actions that were already applied are not rolled back\.

## Permissions<a name="defender-audit-mitigation-permissions"></a>

For each mitigation action that you define, you must provide the role used to apply that action\.


**Permissions for mitigation actions**  

| Action type | Permissions policy template | 
| --- | --- | 
|  UPDATE\_DEVICE\_CERTIFICATE  |  <pre><br />{<br />    "Version":"2012-10-17",<br />    "Statement":[<br />        {<br />            "Effect":"Allow",<br />            "Action":[<br />                "iot:UpdateCertificate"<br />            ],<br />            "Resource":[<br />                "*"<br />            ]<br />        }<br />    ]<br />}</pre>  | 
| UPDATE\_CA\_CERTIFICATE |  <pre><br />{<br />    "Version":"2012-10-17",<br />    "Statement":[<br />        {<br />            "Effect":"Allow",<br />            "Action":[<br />                "iot:UpdateCACertificate"<br />            ],<br />            "Resource":[<br />                "*"<br />            ]<br />        }<br />    ]<br />}</pre>  | 
| ADD\_THINGS\_TO\_THING\_GROUP |  <pre><br />{<br />    "Version":"2012-10-17",<br />    "Statement":[<br />        {<br />            "Effect":"Allow",<br />            "Action":[<br />                "iot:ListPrincipalThings",<br />                "iot:AddThingToThingGroup"<br />            ],<br />            "Resource":[<br />                "*"<br />            ]<br />        }<br />    ]<br />}</pre>  | 
| REPLACE\_DEFAULT\_POLICY\_VERSION |  <pre><br />{<br />    "Version":"2012-10-17",<br />    "Statement":[<br />        {<br />            "Effect":"Allow",<br />            "Action":[<br />                "iot:CreatePolicyVersion"<br />            ],<br />            "Resource":[<br />                "*"<br />            ]<br />        }<br />    ]<br />}</pre>  | 
| ENABLE\_IOT\_LOGGING |  <pre><br />{<br />    "Version":"2012-10-17",<br />    "Statement":[<br />        {<br />            "Effect":"Allow",<br />            "Action":[<br />                "iot:SetV2LoggingOptions"<br />            ],<br />            "Resource":[<br />                "*"<br />            ]<br />        },<br />        {<br />            "Effect":"Allow",<br />            "Action":[<br />                "iam:PassRole"<br />            ],<br />            "Resource":[<br />                "<IAM role ARN used for setting up logging>"<br />            ]<br />        }<br />    ]<br />}</pre>  | 
| PUBLISH\_FINDING\_TO\_SNS |  <pre><br />{<br />    "Version":"2012-10-17",<br />    "Statement":[<br />        {<br />            "Effect":"Allow",<br />            "Action":[<br />                "sns:Publish"<br />            ],<br />            "Resource":[<br />                "<The SNS topic to which the finding is published>"<br />            ]<br />        }<br />    ]<br />}</pre>  | 

For all mitigation action types, use the following trust policy template:

```
{
      "Version": "2012-10-17",
      "Statement": [
            {
                  "Sid": "",
                  "Effect": "Allow",
                  "Principal": {
                        "Service": "iot.amazonaws.com"      
            },
                  "Action": "sts:AssumeRole"    
        }  
    ]
}
```