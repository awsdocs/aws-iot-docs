# Audit finding suppressions<a name="audit-finding-suppressions"></a>

When you run an audit, it reports findings for all non\-compliant resources\. This means your audit reports include findings for resources where you're working toward mitigating issues and also for resources that are known to be non\-compliant, such as test or broken devices\. The audit continues to report findings for resources that remain non\-compliant in successive audit runs, which may add unwanted information to your reports\. Audit finding suppressions enable you to suppress or filter out findings for a defined period of time until the resource is fixed, or indefinitely for a resource associated with a test or broken device\.

**Note**  
Mitigation actions won't be available for suppressed audit findings\. For more information about mitigation actions, see [Mitigation actions](dd-mitigation-actions.md)\.

For information about audit finding suppression quotas, see [AWS IoT Device Defender endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/iot_device_defender.html)\.

## How audit finding suppressions work<a name="how-suppressions-work"></a>

When you create an audit finding suppression for a non\-compliant resource, your audit reports and notifications behave differently\.

Your audit reports will include a new section that lists all the suppressed findings associated with the report\. Suppressed findings won't be considered when we evaluate whether an audit check is compliant or not\. A suppressed resource count is also returned for each audit check when you use the [describe\-audit\-task](https://docs.aws.amazon.com/cli/latest/reference/iot/describe-audit-task.html) command in the command line interface \(CLI\)\.

For audit notifications, suppressed findings aren't considered when we evaluate whether an audit check is compliant or not\. A suppressed resource count is also included in each audit check notification AWS IoT Device Defender publishes to Amazon CloudWatch and Amazon Simple Notification Service \(Amazon SNS\)\.

## How to use audit finding suppressions in the console<a name="audit-finding-suppressions-console"></a>

**To suppress a finding from an audit report**

The following procedure shows you how to create an audit finding suppression in the AWS IoT console\.

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend**, and then choose **Audit**, **Results**\.

1. Select an audit report you'd like to review\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/audit-results.png)

1. In the **Non\-compliant checks** section, under **Check name**, choose the audit check that you're interested in\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/audit-results-details.png)

1. On the audit check details screen, if there are findings you don't want to see, select the option button next to the finding\. Next, choose **Actions**, and then choose the amount of time you'd like your audit finding suppression to persist\.
**Note**  
In the console, you can select *1 week*, *1 month*, *3 months*, *6 months*, or *Indefinitely* as expiration dates for your audit finding suppression\. If you want to set a specific expiration date, you can do so only in the CLI or API\. Audit finding suppressions can also be canceled anytime regardless of expiration date\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/non-compliant-check.png)

1. Confirm the suppression details, and then choose **Enable suppression**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/confirm-suppression.png)

1. After you've created the audit finding suppression, a banner appears confirming your audit finding suppression was created\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/suppression-created-successfully.png)

**To view your suppressed findings in an audit report**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend**, and then choose **Audit**, **Results**\.

1. Select an audit report you'd like to review\.

1. In the **Suppressed findings** section, view which audit findings have been suppressed for your chosen audit report\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/audit-report-findings.png)

**To list your audit finding suppressions**
+ In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend**, and then choose **Audit**, **Finding suppressions**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/list-suppressions.png)

**To edit your audit finding suppression**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend**, and then choose **Audit**, **Finding suppressions**\.

1. Select the option button next to the audit finding suppression you'd like to edit\. Next, choose **Actions**, **Edit**\.

1. On the **Edit audit finding suppression** window, you can change the **Suppression duration** or **Description \(optional\)**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/edit-suppression.png)

1. After you've made your changes, choose **Save**\. The **Finding suppressions** window opens\.

**To delete an audit finding suppression**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend**, and then choose **Audit**, **Finding suppressions**\.

1. Select the option button next to the audit finding suppression you'd like to delete, and then choose **Actions**, **Delete**\.

1. On the **Delete audit finding suppression** window, enter `delete` in the text box to confirm your deletion, and then choose **Delete**\. The **Finding suppressions** window opens\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/delete-suppression.png)

## How to use audit finding suppressions in the CLI<a name="audit-finding-suppressions-cli"></a>

You can use the following CLI commands to create and manage audit finding suppressions\.
+ [create\-audit\-suppression](https://docs.aws.amazon.com/cli/latest/reference/iot/create-audit-suppression.html)
+ [describe\-audit\-suppression](https://docs.aws.amazon.com/cli/latest/reference/iot/describe-audit-suppression.html)
+ [update\-audit\-suppression](https://docs.aws.amazon.com/cli/latest/reference/iot/update-audit-suppression.html)
+ [delete\-audit\-suppression](https://docs.aws.amazon.com/cli/latest/reference/iot/delete-audit-suppression.html)
+ [list\-audit\-suppressions](https://docs.aws.amazon.com/cli/latest/reference/iot/list-audit-suppressions.html)

The `resource-identifier` you input depends on the `check-name` you're suppressing findings for\. The following table details which checks require which `resource-identifier` for creating and editing suppressions\.

**Note**  
The suppression commands do not indicate turning off an audit\. Audits will still run on your AWS IoT devices\. Suppressions are only applicable to the audit findings\.


| `check-name` | `resource-identifier` | 
| --- | --- | 
| AUTHENTICATE\_COGNITO\_ROLE\_OVERLY\_PERMISSIVE\_CHECK | cognitoIdentityPoolId | 
| CA\_CERT\_APPROACHING\_EXPIRATION\_CHECK | caCertificateId | 
| CA\_CERTIFICATE\_KEY\_QUALITY\_CHECK | caCertificateId | 
| CONFLICTING\_CLIENT\_IDS\_CHECK | clientId | 
| DEVICE\_CERT\_APPROACHING\_EXPIRATION\_CHECK | deviceCertificateId | 
| DEVICE\_CERTIFICATE\_KEY\_QUALITY\_CHECK | deviceCertificateId | 
| DEVICE\_CERTIFICATE\_SHARED\_CHECK | deviceCertificateId | 
| IOT\_POLICY\_OVERLY\_PERMISSIVE\_CHECK | policyVersionIdentifier | 
| IOT\_ROLE\_ALIAS\_ALLOWS\_ACCESS\_TO\_UNUSED\_SERVICES\_CHECK | roleAliasArn | 
| IOT\_ROLE\_ALIAS\_OVERLY\_PERMISSIVE\_CHECK | roleAliasArn | 
| LOGGING\_DISABLED\_CHECK | account | 
| REVOKED\_CA\_CERT\_CHECK | caCertificateId | 
| REVOKED\_DEVICE\_CERT\_CHECK | deviceCertificateId | 
| UNAUTHENTICATED\_COGNITO\_ROLE\_OVERLY\_PERMISSIVE\_CHECK | cognitoIdentityPoolId | 

**To create and apply an audit finding suppression**

The following procedure shows you how to create an audit finding suppression in the AWS CLI\.
+ Use the `create-audit-suppression` command to create an audit finding suppression\. The following example creates an audit finding suppression for AWS account *123456789012* on the basis of the check **Logging disabled**\.

  ```
  aws iot create-audit-suppression \
      --check-name LOGGING_DISABLED_CHECK \
      --resource-identifier account=123456789012 \
      --client-request-token 28ac32c3-384c-487a-a368-c7bbd481f554 \
      --suppress-indefinitely \
      --description "Suppresses logging disabled check because I don't want to enable logging for now."
  ```

  There is no output for this command\.

## Audit finding suppressions APIs<a name="audit-finding-suppressions-apis"></a>

The following APIs can be used to create and manage audit finding suppressions\.
+ [CreateAuditSuppression](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateAuditSuppression.html)
+ [DescribeAuditSuppression](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeAuditSuppression.html)
+ [UpdateAuditSuppression](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateAuditSuppression.html)
+ [DeleteAuditSuppression](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteAuditSuppression.html)
+ [ListAuditSuppressions](https://docs.aws.amazon.com/iot/latest/apireference/API_ListAuditSuppressions.html)

To filter *for* specific audit findings, you can use the [ListAuditFindings](https://docs.aws.amazon.com/iot/latest/apireference/API_ListAuditFindings.html) API\.