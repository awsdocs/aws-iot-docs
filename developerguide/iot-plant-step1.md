# Step 1: Create the AWS IoT Policy<a name="iot-plant-step1"></a>

In this step, to allow the Raspberry Pi, or your development computer as a substitute simulator, to perform AWS IoT operations, you create an AWS IoT policy\.

X\.509 certificates are used to authenticate devices with AWS IoT \. AWS IoT policies are used to authorize devices to perform AWS IoT operations, such as subscribing or publishing to MQTT topics\.

If you use the Raspberry Pi hardware for this sample, the Raspberry Pi presents its certificate when sending messages to AWS IoT \. If you use your development computer to simulate the soil moisture readings, your computer presents its certificate when sending messages to AWS IoT instead\.

Later on, you attach the policy to a device certificate\.

1. Use your operating system’s web browser to sign in to the AWS Management Console, at [https://aws\.amazon\.com](https://aws.amazon.com)\. 
**Note**  
For this sample walkthrough, we recommend that you sign in using the credentials of an [IAM administrator user](http://alpha-docs-aws.amazon.com/IAM/latest/UserGuide//best-practices.html#create-iam-users) in your AWS account\.

1. On the AWS navigation bar, choose the AWS Region where you want to create the AWS IoT resources in your AWS account\. This sample was tested with the *US East \(N\. Virginia\)* Region\.

1. Open the [ AWS IoT console](https://console.aws.amazon.com/iot/home)\. To do this, on the AWS navigation bar, choose **Services**\. In the **Find a service by name or feature** box, enter **IoT**, and then press **Enter**\.

1. In the AWS IoT console, if a **Get started** button appears, choose it\.

1. In the service navigation pane, expand **Secure**, and then choose **Policies**\.

1.   
![\[Select Policies in the Secure
                section of the AWS IoT console navigation pane.\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/console-secure-policies.png)

1. If a **You don’t have any policies yet** dialog box appears, choose **Create a policy**\. Otherwise, choose **Create**\.

1. Provide a **Name** that represents this policy, for example **PlantWateringPolicy**\.
**Note**  
If you choose to use a different name, be sure to substitute it throughout this sample\.

1. For **Action**, enter **iot:\***\. 

1. For **Resource ARN**, replace the suggested value with an asterisk \(\*\)\.

1. For **Effect**, choose **Allow**\.

1. Choose **Create**\.  
![\[Create a Policy page, with the fields used in this step
                highlighted.\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/console-create-policy.png)