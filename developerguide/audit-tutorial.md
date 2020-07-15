# Getting started with AWS IoT Device Defender Audit<a name="audit-tutorial"></a>

This tutorial provides instructions on how to configure a recurring audit, setting up alarms, reviewing audit results and mitigating audit issues\.

**Topics**
+ [Prerequisites](#audit-tutorial-prerequisites)
+ [Enable audit checks](#audit-tutorial-enable-checks)
+ [View audit results](#audit-tutorial-view-audit)
+ [Creating audit mitigation actions](#audit-tutorial-mitigation)
+ [Apply mitigation actions to your audit findings](#apply-mitigation-actions)
+ [Enable SNS notifications \(optional\)](#audit-tutorial-enable-sns)
+ [Enable logging \(optional\)](#enable-logging)
+ [Creating a AWS IoT Device Defender Audit IAM role \(optional\)](#audit-iam)

## Prerequisites<a name="audit-tutorial-prerequisites"></a>

To complete this tutorial, you need the following:
+ An AWS account\. If you don't have this, see [Setting up](https://docs.aws.amazon.com/iot/latest/developerguide/dd-setting-up.html)\.

## Enable audit checks<a name="audit-tutorial-enable-checks"></a>

In the following procedure, you enable audit checks that look at account and device settings and policies to ensure security measures are in place\. In this tutorial we instruct you to enable all audit checks, but you're able to select whichever checks you wish\.

Audit pricing is per device count per month \(fleet devices connected to AWS IoT\)\. Therefore, adding or removing audit checks would not affect your monthly bill when using this feature\.

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend** and select **Get started with an audit**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/audit-get-started.png)

1. The **Get started with Device Defender Audit** screen gives an overview of the steps required to enable the audit checks\. Once you've reviewed the screen, select **Next**\.

1. If you already have a role to use, you can select it\. Otherwise select **Create Role** and name it *AWSIoTDeviceDefenderAudit*\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/audit-review-perms.png)

   You should see the required permissions automatically attached to the role\. Select the triangles next to **Permissions** and **Trust relationships** to see what permissions are granted\. Select **Next** when you're ready to move on\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/audit-role-attached.png)

1. On the **Select checks** screen you will see all audit checks you can select\. For this tutorial, we instruct you to select all checks but you can select whichever checks you want\. Next to each audit check is a help icon that describes what the audit check does\. For more information about audit checks, see [Audit Checks](https://docs.aws.amazon.com/iot/latest/developerguide/device-defender-audit-checks.html)\.

   Select **Next** once you've selected your checks\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/audit-select-checks.png)

   You can always change your configured Audit checks under **Settings**\.

1. On the **Configure SNS \(optional\)** screen, select **Enable audit**\. If you'd like to enable SNS notifications, see [Enable SNS notifications \(optional\)](#audit-tutorial-enable-sns)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/audit-optional-sns-config.png)

1. You'll be redirected to **Schedules** under **Audit**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/scheduled-audits.png)

## View audit results<a name="audit-tutorial-view-audit"></a>

The following procedure shows you how to view your audit results\. In this tutorial, you see the audit results from the audit checks set up in [Enable audit checks](#audit-tutorial-enable-checks) tutorial\.

**To view audit results**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend**, select **Audit**, and select **Results**\.

1. The **Summary** will tell you if you have any non\-compliant checks\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/audit-results.png)

1. Select the **Name** of the audit check you'd like to investigate\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/audit-findings.png)

1. Use the question marks for guidance on how to make your non\-compliant checks compliant\. For example, you can follow [Enable logging \(optional\)](#enable-logging) to make the "Logging disabled" check compliant\.

## Creating audit mitigation actions<a name="audit-tutorial-mitigation"></a>

In the following procedure, you will create a AWS IoT Device Defender Audit Mitigation Action to enable AWS IoT logging\. Each audit check has mapped mitigation actions that will affect which **Action type** you choose for the audit check you want to fix\. For more information, see [Mitigation actions](https://docs.aws.amazon.com/iot/latest/developerguide/device-defender-mitigation-actions.html#defender-audit-apply-mitigation-actions.html)\.

**To use the AWS IoT console to create mitigation actions**

1. Open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Defend**, and then choose **Mitigation Actions**\.

1. On the **Mitigation Actions** page, choose **Create**\.

1. On the **Create a Mitigation Action** page, in **Action name**, enter a unique name for your mitigation action such as *EnableErrorLoggingAction*\.

1. In **Action type**, choose **Enable IoT logging**\.

1. In **Action execution role**, select **Create Role**\. For **Name**, use *IoTMitigationActionErrorLoggingRole*\. Then, choose **Create role**\.

1. In **Parameters**, under **Role for logging**, select AWSIoTLoggingRole\. For **Log level**, choose Error\.  
![\[Create a new mitigation action window.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/create-mitigation-action.png)

1. Choose **Save** to save your mitigation action to your AWS account\.

1. Once created, you will see the following screen indication that your mitigation action was created successfully\.  
![\[Mitigation action successfully created window.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/mitigation-action-created.png)

## Apply mitigation actions to your audit findings<a name="apply-mitigation-actions"></a>

The following procedure shows you how to apply mitigation actions to your audit results\.

**To mitigate non\-compliant audit findings**

1. Open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Audit**, and then choose **Results**\. Select the name of the audit that you want to respond to\.

1. Check your results\. Notice that Logging disabled is located under **Non\-compliant checks**\.

1. Select **Start mitigation actions**\.  
![\[Audit findings window.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/start-mitigation-action.png)

1. Under **Select actions**, select the appropriate actions for each non\-compliant finding to address the issues\.  
![\[Start a new mitigation action window.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/select-actions.png)

1. Select **Confirm**\.

1. Once the mitigation action is started, it may take a few minutes for it to run\.  
![\[Start a new mitigation action window.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/mitigation-action-started.png)

**To check that the mitigation action worked**

1. In the AWS console, in the navigation pane, select **Settings**\.

1. Confirm that **Logs** are `Enabled` and the **Level of verbosity** is `Error`\.

## Enable SNS notifications \(optional\)<a name="audit-tutorial-enable-sns"></a>

In the following procedure, you enable Simple Notifications Service \(SNS\) notifications to alert you when your audits identifies any non\-compliant resources\. In this tutorial you will set up notifications for the audit checks enabled in the [Enable audit checks](#audit-tutorial-enable-checks) tutorial\.

1. First, you need to create an IAM policy that provides access to Amazon SNS via the AWS Management Console\. You can do this by following the [Creating a AWS IoT Device Defender Audit IAM role \(optional\)](#audit-iam) process, but selecting **AmazonSNSRole** in step 8\.

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend** and select **Settings**\.

1. Under **SNS alerts**, select **Edit**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/sns-edit-alert.png)

1. On the **Edit SNS alerts** screen, select **Enabled**\. Under **Topic**, select **Create**\. Name the topic *IoTDDNotifications* and select **Create**\. Under **Role**, select the role you created called *AWSIoTDeviceDefenderAudit*\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/sns-enable.png)

   Select **Update**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/sns-update.png)

   If you'd like to receive email or text in your Ops platforms through SNS, see [Using Amazon SNS for user notifications](https://docs.aws.amazon.com/sns/latest/dg/sns-user-notifications.html)\.

## Enable logging \(optional\)<a name="enable-logging"></a>

This procedure describes how to enable AWS IoT to log information to CloudWatch Logs\. This will allow you to view your audit results\. Enabling logging may result in incurred charges\.

**To enable logging**

1. In the AWS console, in the navigation pane, select **Settings**\.

1. Under **Logs**, select **Edit**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/logs-edit.png)

1. Under **Level of verbosity**, select **Debug \(most verbose\)**\.

1. Under **Set role**, select **Create Role** and name the role *AWSIoTLoggingRole*\. A policy will automatically be attached\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/logs-create-role.png)

   Select **Update**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/logs-update.png)

## Creating a AWS IoT Device Defender Audit IAM role \(optional\)<a name="audit-iam"></a>

In the following procedure, you create a AWS IoT Device Defender Audit IAM role that provides AWS IoT Device Defender read access to AWS IoT\.

1. Navigate to the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)

1. In the navigation pane, chose **Users** and then choose **Add user**\.

1. For **User name**, enter **Administrator**\.

1. Select the check box next to **AWS Management Console access**\. Then select **Custom password**, and then enter your new password in the text box\.

1. \(Optional\) By default, AWS requires the new user to create a new password when first signing in\. You can clear the check box next to **User must create a new password at next sign\-in** to allow the new user to reset their password after they sign in\.

1. Choose **Next: Permissions**\.

1. Under **Set permissions**, choose **Attach existing policies directly**\.

1. In the policy list, select the check box for **AWSIoTDeviceDefenderAudit**\.

1. Choose **Next: Tags**\.

1. Choose **Next: Review** to see the list of group memberships to be added to the new user\. When you are ready to proceed, choose **Create user**\.