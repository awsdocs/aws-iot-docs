# Set up your AWS account<a name="setting-up"></a>

Before you use AWS IoT Core for the first time, complete the following tasks:
+ [Sign up for an AWS account](#aws-registration)
+ [Create a user and grant permissions](#create-iam-user)
+ [Open the AWS IoT console](#iot-console-signin)

If you already have an AWS account and an IAM user for yourself, you can use them and skip ahead to [Open the AWS IoT console](#iot-console-signin)\.

## Sign up for an AWS account<a name="aws-registration"></a>

When you sign up for AWS, your account is automatically signed up for all services in AWS\. If you have an AWS account already, skip this procedure\. If you don't have an AWS account, use the following procedure to create one\.

You can expect to spend about 5 minutes setting up your AWS account\.

If you do not have an AWS account, complete the following steps to create one\.

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

**Note**  
Save your AWS account number, because you need it for the next task\.

## Create a user and grant permissions<a name="create-iam-user"></a>

This procedure describes how to create an IAM user for yourself and add that user to a group that has administrative permissions from an attached managed policy\. IAM is the AWS service that manages users of and access to AWS resources\. You must do this so that you can create the AWS IoT resources in your account and grant them permission to do what they need to do\.

**To create an administrator user for yourself and add the user to an administrators group \(console\)**

1. Sign in to the [IAM console](https://console.aws.amazon.com/iam/) as the account owner by choosing **Root user** and entering your AWS account email address\. On the next page, enter your password\.
**Note**  
We strongly recommend that you adhere to the best practice of using the **Administrator** IAM user below and securely lock away the root user credentials\. Sign in as the root user only to perform a few [account and service management tasks](https://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html)\.

1. In the navigation pane, choose **Users** and then choose **Add user**\.

1. For **User name**, enter **Administrator**\.

1. Select the check box next to **AWS Management Console access**\. Then select **Custom password**, and then enter your new password in the text box\.

1. \(Optional\) By default, AWS requires the new user to create a new password when first signing in\. You can clear the check box next to **User must create a new password at next sign\-in** to allow the new user to reset their password after they sign in\.

1. Choose **Next: Permissions**\.

1. Under **Set permissions**, choose **Add user to group**\.

1. Choose **Create group**\.

1. In the **Create group** dialog box, for **Group name** enter **Administrators**\.

1. Choose **Filter policies**, and then select **AWS managed \-job function** to filter the table contents\.

1. In the policy list, select the check box for **AdministratorAccess**\. Then choose **Create group**\.
**Note**  
You must activate IAM user and role access to Billing before you can use the `AdministratorAccess` permissions to access the AWS Billing and Cost Management console\. To do this, follow the instructions in [step 1 of the tutorial about delegating access to the billing console](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_billing.html)\.

1. Back in the list of groups, select the check box for your new group\. Choose **Refresh** if necessary to see the group in the list\.

1. Choose **Next: Tags**\.

1. \(Optional\) Add metadata to the user by attaching tags as key\-value pairs\. For more information about using tags in IAM, see [Tagging IAM Entities](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html) in the *IAM User Guide*\.

1. Choose **Next: Review** to see the list of group memberships to be added to the new user\. When you are ready to proceed, choose **Create user**\.

You can use this same process to create more groups and users and to give your users access to your AWS account resources\. To learn about using policies that restrict user permissions to specific AWS resources, see [Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) and [Example Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_examples.html)\.

## Open the AWS IoT console<a name="iot-console-signin"></a>

Most of the console\-oriented topics in this section start from the [AWS IoT console](https://console.aws.amazon.com/iot/home)\. If you aren't already signed in to your AWS account, sign in, then open the [AWS IoT console](https://console.aws.amazon.com/iot/home) and continue to the next section to continue getting started with AWS IoT\.