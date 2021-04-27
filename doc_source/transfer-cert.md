# Transfer a certificate to another account<a name="transfer-cert"></a>

X\.509 certificates that belong to one AWS account can be transferred to another AWS account\.

**To transfer an X\.509 certificate from one AWS account to another**

1. [Begin a certificate transfer](#transfer-cert-init)

   The certificate must be deactivated and detached from all policies and things before initiating the transfer\.

1. [Accept or reject a certificate transfer](#transfer-cert-accept)

   The receiving account must explicitly accept or reject the transferred certificate\. After the receiving account accepts the certificate, the certificate must be activated before use\.

1. [Cancel a certificate transfer](#transfer-cert-cancel)

   The originating account can cancel a transfer, if the certificate has not been accepted\.

## Begin a certificate transfer<a name="transfer-cert-init"></a>

You can begin to transfer a certificate to another AWS account by using the [AWS IoT console](https://console.aws.amazon.com/iot/home) or the AWS CLI\.

### Begin a certificate transfer \(console\)<a name="transfer-cert-init-console"></a>

To complete this procedure, you'll need the ID of the certificate that you want to transfer\.

Do this procedure from the account with the certificate to transfer\.

**To begin to transfer a certificate to another AWS account**

1. Sign in to the AWS Management Console and open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Secure**, choose **Certificates**\.

   Choose the certificate with an **Active** or **Inactive** status that you want to transfer and open its details page\.

1. On the certificate's **Details** page, in the **Actions** menu, if the **Deactivate** option is available, choose the **Deactivate** option to deactivate the certificate\.

1. On the certificate's **Details** page, in the left menu, choose **Policies**\.

1. On the certificate's **Policies** page, if there are any policies attached to the certificate, detach each one by opening the policy's options menu and choosing **Detach**\.

   The certificate must not have any attached policies before you continue\.

1. On the certificate's **Policies** page, in the left menu, choose **Things**\.

1. On the certificate's **Things** page, if there are any things attached to the certificate, detach each one by opening the thing's options menu and choosing **Detach**\.

   The certificate must not have any attached things before you continue\.

1. On the certificate's **Things** page, in the left menu, choose **Details**\.

1. On the certificate's **Details** page, in the **Actions** menu, choose **Start transfer** to open the **Start transfer** dialog box\.

1. In the **Start transfer** dialog box, enter the AWS account number of the account to receive the certificate and an optional short message\.

1. Choose **Start transfer** to transfer the certificate\.

The console should display a message that indicates the success or failure of the transfer\. If the transfer was started, the certificate's status is updated to **Transferred**\.

### Begin a certificate transfer \(CLI\)<a name="transfer-cert-init-cli"></a>

To complete this procedure, you'll need the *certificateId* and the *certificateArn* of the certificate that you want to transfer\.

Do this procedure from the account with the certificate to transfer\.

**To begin to transfer a certificate to another AWS account**

1. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/update-certificate.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/update-certificate.html) command to deactivate the certificate\.

   ```
   aws iot update-certificate --certificate-id certificateId --new-status INACTIVE
   ```

1. Detach all policies\.

   1. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/list-attached-policies.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/list-attached-policies.html) command to list the policies attached to the certificate\.

      ```
      aws iot list-attached-policies --target certificateArn
      ```

   1. For each attached policy, use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/detach-policy.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/detach-policy.html) command to detach the policy\.

      ```
      aws iot detach-policy --target certificateArn --policy-name policy-name
      ```

1. Detach all things\.

   1. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/list-principal-things.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/list-principal-things.html) command to list the things attached to the certificate\.

      ```
      aws iot list-principal-things --principal certificateArn
      ```

   1. For each attached thing, use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/detach-thing-principal.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/detach-thing-principal.html) command to detach the thing\.

      ```
      aws iot detach-thing-principal --principal certificateArn --thing-name thing-name
      ```

1. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/transfer-certificate.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/transfer-certificate.html) command to start the certificate transfer\.

   ```
   aws iot transfer-certificate --certificate-id certificateId --target-aws-account account-id
   ```

## Accept or reject a certificate transfer<a name="transfer-cert-accept"></a>

You can accept or reject a certificate transferred to you AWS account from another AWS account by using the [AWS IoT console](https://console.aws.amazon.com/iot/home) or the AWS CLI\.

### Accept or reject a certificate transfer \(console\)<a name="transfer-cert-accept-console"></a>

To complete this procedure, you'll need the ID of the certificate that was transferred to your account\.

Do this procedure from the account receiving the certificate that was transferred\.

**To accept or reject a certificate that was transferred to your AWS account**

1. Sign in to the AWS Management Console and open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Secure**, choose **Certificates**\.

   Choose the certificate with a status of **Pending transfer** that you want to accept or reject and open its details page\.

1. On the certificate's **Details** page, in the **Actions** menu,
   + To accept the certificate, choose **Accept transfer**\.
   + To not accept the certificate, choose **Reject transfer**\.

### Accept or reject a certificate transfer \(CLI\)<a name="transfer-cert-accept-cli"></a>

To complete this procedure, you'll need the *certificateId* of the certificate transfer that you want to accept or reject\.

Do this procedure from the account receiving the certificate that was transferred\.

**To accept or reject a certificate that was transferred to your AWS account**

1. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/accept-certificate-transfer.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/accept-certificate-transfer.html) command to accept the certificate\.

   ```
   aws iot accept-certificate-transfer --certificate-id certificateId
   ```

1. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/reject-certificate-transfer.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/reject-certificate-transfer.html) command to reject the certificate\.

   ```
   aws iot reject-certificate-transfer --certificate-id certificateId
   ```

## Cancel a certificate transfer<a name="transfer-cert-cancel"></a>

You can cancel a certificate transfer before it has been accepted by using the [AWS IoT console](https://console.aws.amazon.com/iot/home) or the AWS CLI\.

### Cancel a certificate transfer \(console\)<a name="transfer-cert-cancel-console"></a>

To complete this procedure, you'll need the ID of the certificate transfer that you want to cancel\.

Do this procedure from the account that initiated the certificate transfer\.

**To cancel a certificate transfer**

1. Sign in to the AWS Management Console and open the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

1. In the left navigation pane, choose **Secure**, choose **Certificates**\.

   Choose the certificate with **Transferred** status whose transfer you want to cancel and open its options menu\.

1. On the certificate's options menu, choose the **Revoke transfer** option to cancel the certificate transfer\.
**Important**  
Be careful not to mistake the **Revoke transfer** option with the **Revoke** option\.  
The **Revoke transfer** option cancels the certificate transfer, while the **Revoke** option makes the certificate irreversibly unusable by AWS IoT\. 

### Cancel a certificate transfer \(CLI\)<a name="transfer-cert-cancel-cli"></a>

To complete this procedure, you'll need the *certificateId* of the certificate transfer that you want to cancel\.

Do this procedure from the account that initiated the certificate transfer\.

Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/cancel-certificate-transfer.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/cancel-certificate-transfer.html) command to cancel the certificate transfer\.

```
aws iot cancel-certificate-transfer --certificate-id certificateId
```