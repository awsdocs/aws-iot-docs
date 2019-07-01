# Attach a Certificate to a Thing<a name="attach-cert-thing"></a>

A device must have a certificate, private key and root CA certificate to authenticate with AWS IoT\. We recommend that you also attach the device certificate to the IoT thing that represents your device in AWS IoT\. This allows you to create AWS IoT policies that grant permissions based on certificates attached to your things\. For more information\. see [Thing Policy Variables](thing-policy-variables.md)

**To attach a certificate to the thing representing your device in the registry:**

1. In the box for the certificate you created, choose **\.\.\.** to open a drop\-down menu, and then choose **Attach thing**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/certificates-dashboard-2.png)

1. In the **Attach things to certificate\(s\)** dialog box, select the check box next to the thing you registered, and then choose **Attach**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/attach-thing-to-cert.png)

1. To verify the thing is attached, select the box representing the certificate\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/certificate-box.png)

1. On the **Details** page for the certificate, in the left navigation pane, choose **Things**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/confirm-attach-thing-result.png)

1. To verify the policy is attached, on the **Details** page for the certificate, in the left navigation pane, choose **Policies**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/confirm-attach-policy-result.png)