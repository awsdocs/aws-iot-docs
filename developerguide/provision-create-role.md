# Creating IAM policies and roles for a user installing a device<a name="provision-create-role"></a>

**Note**  
These procedures are for use only when directed by the AWS IoT console\.  
To go to this page from the console, open [create a new provisioning template](https://console.aws.amazon.com/iot/home#/provisioningtemplate/create/provisioningmethods/trustedUser)\.

## Why can't this be done in the AWS IoT console?<a name="provision-create-role-why"></a>

For the most secure experience, IAM actions are performed in the IAM console\. The procedures in this section walk you through the steps to create the IAM roles and policies that are needed to use the provisioning template\.

## Creating an IAM policy for the user who will install a device<a name="provision-create-role-policy"></a>

This procedure describes how to create an IAM policy that authorizes a user to install a device using a provisioning template\.

While performing this procedure, you'll be switching between the IAM console and the AWS IoT console\. We recommend having both consoles open at the same time while you complete this procedure\.

**To create an IAM policy for the user who will install a device**

1. Open the [Policies hub in the IAM console](https://console.aws.amazon.com/iamv2/home#/policies)\.

1. Choose **Create Policy**\.

1. On the **Create policy** page, choose the **JSON** tab\.

1. Switch to the page in the AWS IoT console where you chose **Configure user policy and role**\.

1. In the **Sample provisioning policy**, choose **Copy**\.

1. Switch back to the IAM console\.

1. In the **JSON** editor, paste the policy you copied from the AWS IoT console\. This policy is specific to the template you're creating in the AWS IoT console\.

1. To continue, choose **Next: Tags**\.

1. On the **Add tags \(Optional\)** page, choose **Add tag** for each tag you want to add to this policy\. You can skip this step if you don't have any tags to add\.

1. To continue, choose **Next: Review**\.

1. On the **Review policy** page, do the following:

   1. For **Name\***, enter a name for the policy that will help you remember the policy's purpose\.

      Note the name you give this policy because you'll use it in the next procedure\.

   1. You can choose to enter an optional description for the policy you're creating\.

   1. Review the rest of this policy and its tags\.

1. To finish creating the new policy, choose **Create policy**\.

After you create your new policy, continue to [Creating an IAM role for the user who will install a device](#provision-create-role-role) to create the user's role entry that you'll attach this policy\.

## Creating an IAM role for the user who will install a device<a name="provision-create-role-role"></a>

These steps describe how to create an IAM role that authenticates the user who will install a device using a provisioning template\.

**To create an IAM policy for the user who will install a device**

1. Open the [Role hub in the IAM console](https://console.aws.amazon.com/iamv2/home#/roles)\.

1. Choose **Create role**\.

1. In **Select trusted entity**, choose the type of trusted entity that you want to give access to the template you're creating\.

1. Choose or enter the identification of the trusted entity that you want to grant access to, and then choose **Next**\.

1. On the **Add permissions** page, in **Permission policies**, in the search box, enter the name of the policy you created in the [previous procedure](#provision-create-role-policy)\.

1. For the policy list, choose the policy that you created in the previous procedure, and then choose **Next**\.

1. In the **Name, review, and create** section, do the following:

   1. For **Role name**, enter a role name that will help you remember the role's purpose\.

   1. For **Description**, you can choose to enter an optional description of the Role\. This isn't required to continue\.

   1. Review the values in **Step 1** and **Step 2**\.

   1. For **Add tags \(Optional\)**, you can choose to add tags to this role\. This isn't required to continue\.

   1. Verify the information on this page is complete and correct, and then choose **Create role**\. 

After you create the new role, return to the AWS IoT console to continue creating the template\.

## Updating an existing policy to authorize a new template<a name="provision-create-role-update"></a>

The following steps describe how to add a new template to an IAM policy that authorizes a user to install a device using a provisioning template\.

**To add a new template to an existing IAM policy**

1. Open the [Policies hub in the IAM console](https://console.aws.amazon.com/iamv2/home#/policies)\.

1. In the search box, enter the name of the policy to update\.

1. For the list below the search box, find the policy you want to update and choose the policy name\.

1. For **Policy summary**, choose the **JSON** tab, if that panel isn't already visible\.

1. To modify the policy document, choose **Edit policy**\.

1. In the editor, choose the **JSON** tab, if that panel isn't already visible\.

1. In the policy document, find the policy statement that contains the `iot:CreateProvisioningClaim` action\.

   If the policy document doesn't contain a policy statement with the `iot:CreateProvisioningClaim` action, copy the following statement snippet and paste it as an additional entry in the `Statement` array in the policy document\. 
**Note**  
This snippet must be placed before the closing `]` character in the `Statement` array\. You might need to add a comma before or after this snippet to correct any syntax errors\.

   ```
   {
       "Effect": "Allow",
       "Action": [
           "iot:CreateProvisioningClaim"
       ],
       "Resource": [
           "--PUT YOUR NEW TEMPLATE ARN HERE--"
       ]
   }
   ```

1. Switch to the page in the AWS IoT console where you chose **Modify user role permissions**\.

1. Find the **Resource ARN** of the template and choose **Copy**\.

1. Switch back to the IAM console\.

1. Paste the copied Amazon Resource Name \(ARN\) at the top of the list of template ARNs in the `Statement` array so that it's the first entry\.

   If this is the only ARN in the array, remove the comma at end of the value you just pasted\.

1. Review the updated policy statement and correct any errors indicated by the editor\.

1. To save the updated policy document, choose **Review policy**\.

1. Review the policy and then choose **Save changes**\.

1. Return to the AWS IoT console\.