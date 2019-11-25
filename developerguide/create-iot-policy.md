# Create an AWS IoT Policy<a name="create-iot-policy"></a>

X\.509 certificates are used to authenticate your device with AWS IoT\. AWS IoT policies are used to authorize your device to perform AWS IoT operations, such as subscribing or publishing to MQTT topics\. Your device presents its certificate when sending messages to AWS IoT\. To allow your device to perform AWS IoT operations, you must create an AWS IoT policy and attach it to your device certificate\.

**To create an AWS IoT policy**

1. In the left navigation pane, choose **Secure**, and then choose **Policies**\. On the **You don't have a policy yet** page, choose **Create a policy**\.  
![\[Policies\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/create-first-policy.png)

1. On the **Create a policy** page, in the **Name** field, enter a name for the policy \(for example, **MyIotPolicy**\)\. 
**Note**  
We do not recommend using personally identifiable information in your policy names\.

   In the **Action** field, enter **iot:Connect**\. In the **Resource ARN** field, enter **\***\. Select the **Allow** check box\. This allows all clients to connect to AWS IoT\.  
![\[Create policy\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/gs-create-policy.png)
**Note**  
You can restrict which clients \(devices\) can connect by specifying a client ARN as the resource\. The client ARNs follow this format:  
 `arn:aws:iot:your-region:your-aws-account:client/<my-client-id>`

   Choose the **Add Statement** button to add another policy statement\. In the **Action** field, enter **iot:Publish**\. In the **Resource ARN** field, enter the ARN of the topic to which your device will publish\.
**Note**  
The topic ARN follows this format:  
 `arn:aws:iot:your-region:your-aws-account:topic/<your/topic>`   
For example:  
`arn:aws:iot:us-east-1:123456789012:topic/my/topic`

   Finally, select the **Allow** check box\. This allows your device to publish messages to the specified topic\.

1. After you have entered the information for your policy, choose **Create**\.  
![\[Create policy\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/gs-create-policy.png)

For more information, see [Managing AWS IoT Policies](https://docs.aws.amazon.com/iot/latest/developerguide/authorization.html)\. 