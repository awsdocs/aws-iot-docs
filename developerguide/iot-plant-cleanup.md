# Cleaning Up<a name="iot-plant-cleanup"></a>

If you no longer want to use the AWS resources in AWS IoT, Amazon SNS, and IAM that you created for this sample, you can delete those resources by following the instructions in this section\.

**Note**  
If you don’t delete these AWS resources, then any ongoing usage of those resources might start generating charges to your AWS account\.

# To delete the thing in AWS IoT

1. In the AWS Management Console, open the [ AWS IoT console](https://console.aws.amazon.com/iot/home), if it isn’t already open\. To do this, on the AWS navigation bar, choose **Services**\. In the **Find a service by name or feature** box, enter **IoT Core**, and then press **Enter**\.

1. In the service navigation pane, expand **Manage**\.

1. If an **Introducing AWS IoT Device Management** dialog box is displayed, choose **Show me later**, or press **Esc**\.

1. Choose **Things**\.

1. In the list of things, in the card for the name of the thing you want to delete \(for example, **MyRPi**\), choose the ellipsis \(**…**\), and then choose **Delete**\.  
![\[Manage Things page, with Delete highlighted.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-delete-thing.png)

1. When prompted, choose **Yes, continue with delete**\.

# To delete the thing's certificate in AWS IoT

1. In the [ AWS IoT console](https://console.aws.amazon.com/iot/home), in the service navigation pane, expand **Secure**, and then choose **Certificates**\.

1. In the list of certificates, in the card for the thing’s certificate, choose the ellipsis \(**\.\.\.**\), and then choose **Delete**\.  
![\[Secure Certificates page, with Delete highlighted.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-delete-certificate.png)

   If you’re not sure which certificate to delete, then in the list of certificates, choose the ID for the certificate that you think is the correct one\. Then choose **Things**\.

   If your thing’s name is displayed, this is the correct certificate to delete\. Choose **Actions**, and then choose **Delete**\.

   If, however, your thing isn't displayed, this isn’t the correct certificate\. Choose the back button to return to the list of certificates\.  
![\[Certificates page for your Things, with Delete highlighted.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-delete-certificate-alt.png)

1. When prompted, choose **Yes, continue with delete**\.

# To delete the thing's policy in AWS IoT

1. In the [ AWS IoT console](https://console.aws.amazon.com/iot/home), in the service navigation pane, expand **Secure**, and then choose **Policies**\.

1. In the list of policies, in the card for the thing’s policy \(for example, **PlantWateringPolicy**\), choose the ellipsis \(**\.\.\.**\), and then choose **Delete**\.  
![\[Secure Policies page, with Delete highlighted.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-delete-policy.png)

1. When prompted, choose **Yes, continue with delete**\.

# To delete the thing's rule in AWS IoT

1. In the [ AWS IoT console](https://console.aws.amazon.com/iot/home), in the service navigation pane, choose **Act**\.

1. In the list of rules, in the card for the thing’s rule \(for example, **MyRPiLowMoistureAlertRule**\), choose the ellipsis \(**\.\.\.**\), and then choose **Delete**\.  
![\[Act page, with Delete highlighted.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-delete-rule.png)

1. When prompted, choose **Yes, continue with delete**\.

# To delete the related subscription in Amazon SNS

1. Open the Amazon SNS console\. To do this, on the AWS navigation bar, choose **Services**\. In the **Find a service by name or feature** box, enter **SNS**, and then press **Enter**\.

1. In the service navigation pane, choose **Subscriptions**\.

1. In the list of subscriptions, select the box for the row where **Topic ARN** contains the name of the related topic \(for example, **MyRPiLowMoistureTopic**\) and **Endpoint** contains your email address\.

1. Choose **Actions**, **Delete subscriptions**\.  
![\[Amazon SNS Subscriptions page, with the subscription to be deleted highlighted.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-delete-subscription.png)

1. When prompted, choose **Delete**\.

# To delete the related topic in Amazon SNS

1. In the Amazon SNS console, in the service navigation pane, choose **Topics**\.

1. In the list of topics, select the box for the row where **Name** contains the name of the related topic \(for example, **MyRPiLowMoistureTopic**\)\.

1. Choose **Actions**, **Delete topics**\.  
![\[Amazon SNS Topics page, with the topic to be deleted highlighted.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-delete-topic.png)

1. When prompted, choose **Delete**\.

# To delete the related Amazon SNS role in IAM

1. Open the IAM console\. To do this, on the AWS navigation bar, choose **Services**\. In the **Find a service by name or feature** box, enter **IAM**, and then press **Enter**\.

1. In the service navigation pane, choose **Roles**\.

1. In the list of roles, select the box where **Role name** contains the name of the related role \(for example, **MyRPiLowMoistureTopicRole**\)\. If you can’t find the role, then in the **Search** box, enter the name of the role\. Then press **Enter**\.

1. Choose **Delete role**\.  
![\[IAM Roles page, with the role to be deleted highlighted.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/console-delete-role.png)

1. When prompted, choose **Yes, delete**\.