# Create an AWS IoT Core policy<a name="create-iot-policy"></a>

X\.509 certificates are used to authenticate your device with AWS IoT Core\. AWS IoT Core policies are used to authorize your device to perform AWS IoT Core operations, such as subscribing or publishing to MQTT topics\. Your device presents its certificate when sending messages to AWS IoT Core\. To allow your device to perform AWS IoT Core operations, you must create an AWS IoT Core policy and attach it to your device certificate\.

**To create an AWS IoT Core policy**

1. In the left navigation pane, choose **Secure**, and then choose **Policies**\. On the **You don't have a policy yet** page, choose **Create a policy**\.

1. On the **Create a policy** page, in the **Name** field, enter a name for the policy \(for example, **MyIotPolicy**\)\. Do not use personally identifiable information in your policy names\.  
![\[Create policy\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/gs-create-policy.png)

1. In the **Action** field, enter **iot:Connect**\. In the **Resource ARN** field, enter **\***\. Select the **Allow** check box\. This allows all clients to connect to AWS IoT Core\.
**Note**  
You can restrict which clients \(devices\) can connect by specifying a client ARN as the resource\. The client ARNs follow this format:  
 `arn:aws:iot:your-region:your-aws-account:client/<my-client-id>`

1. Choose the **Add Statement** button to add another policy statement\. In the **Action** field, enter **iot:Publish**\. In the **Resource ARN** field, enter the ARN of the topic to which your device publishes\.
**Note**  
The topic ARN follows this format:  
 `arn:aws:iot:your-region:your-aws-account:topic/<your/topic>`   
For example:  
`arn:aws:iot:us-east-1:123456789012:topic/my/topic`

1. Finally, select the **Allow** check box\. This allows your device to publish messages to the specified topic\.

1. After you have entered the information for your policy, choose **Create**\.

For more information, see [IAM policies](iam-policies.md)\. 