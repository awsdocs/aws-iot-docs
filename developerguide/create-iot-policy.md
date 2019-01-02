# Create an AWS IoT Policy<a name="create-iot-policy"></a>

X\.509 certificates are used to authenticate your device with AWS IoT\. AWS IoT policies are used to authorize your device to perform AWS IoT operations, such as subscribing or publishing to MQTT topics\. Your device presents its certificate when sending messages to AWS IoT\. To allow your device to perform AWS IoT operations, you must create an AWS IoT policy and attach it to your device certificate\.

**To create an AWS IoT policy:**

1. In the left navigation pane, choose **Secure**, and then **Policies**\. On the **You don't have a policy yet** page, choose **Create a policy**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/policy-create.png)

1. On the **Create a policy** page, in the **Name** field, type a name for the policy \(for example, **MyIoTButtonPolicy**\)\. In the **Action** field, type **iot:Connect**\. In the **Resource ARN** field, type `*`\. Select the **Allow** check box\. This allows all clients to connect to AWS IoT\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/iot-connect-policy.png)
**Note**  
You can restrict which clients \(devices\) are able to connect by specifying a client ARN as the resource\. The client ARNs follow this format:  
 `arn:aws:iot:your-region:your-aws-account:client/<my-client-id>`

   Select the **Add Statement** button to add another policy statement\. In the **Action** field, type **iot:Publish**\. In the **Resource ARN** field, type the ARN of the topic to which your device will publish\.
**Note**  
The topic ARN follows this format:  
 `arn:aws:iot:your-region:your-aws-account:topic/iotbutton/your-button-serial-number`   
For example:  
`arn:aws:iot:us-east-1:123456789012:topic/iotbutton/G030JF055364XVRB`  
You can find the serial number on the bottom of your button\.  
If you are not using an AWS IoT button, after `topic/` in the ARN, place the topic to which your device publishes\. For example:  
`arn:aws:iot:us-east-1:123456789012:topic/my/topic/here`

   Finally, select the **Allow** check box\. This allows your device to publish messages to the specified topic\.

1. After you have entered the information for your policy, choose **Create**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/create-policy-2.png)

For more information, see [Managing AWS IoT Policies](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/authorization.html)\. 