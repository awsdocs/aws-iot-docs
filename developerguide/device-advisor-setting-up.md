# Setting up<a name="device-advisor-setting-up"></a>


|  | 
| --- |
| Device Advisor is in preview and is subject to change\. | 

Before you use Device Advisor for the first time, complete the following tasks\.

**Topics**
+ [Create an IAM role to be used as your device role](#da-iam-role)
+ [Create a custom managed policy for your Device Advisor user account](#da-managed-policy)
+ [Create an IAM user to use to run Device Advisor tests](#da-iam-user)
+ [Create an AWS IoT thing and certificate](#da-create-thing-certificate)
+ [Configure your test device](#da-configure-device)

## Create an IAM role to be used as your device role<a name="da-iam-role"></a>

This section shows you how to create an AWS account and add permissions to an IAM user that you can use to run Device Advisor tests on your devices\.

1. Go to the [AWS IAM console](https://console.aws.amazon.com/iam/home?region=us-west-2#/home) and log in to the account you use for Device Advisor testing\.

1. First, create a policy that restricts the role permissions to the device thing policies\. On the left navigation pane, chose **Policies**\.

1. Choose **Create policy**\.

1. Under **Create policy**, do the following\.

   1. Choose **IoT** for **Service**\.

   1. Under **Action**, check **All IoT actions \(iot:\*\)**\.

   1. Under **Resources**, you can select all resources\. However, for best security practices, restrict **client**, **topic**, and **topicfilter**\. If you choose to restrict these resources, follow the steps below\.

      1. Choose **Specify client resource ARN for the Connect action**\.

         1. Choose **Add ARN**\.

         1. Specify the **region**, **accountId**, and **clientId** in the visual ARN editor or manually specify the Amazon Resource Names \(ARN\) of the IoT topics you want to use to run test cases\. The **clientId** is the MQTT **clientId** your device uses to interact with Device Advisor\.

         1. Choose **Add**\.

      1. Choose **Specify topic resource ARN for the Receive and 1 more action**\.

         1. Choose **Add ARN**\.

         1. Specify the **region**, **accountId**, and **topic name** in the visual ARN editor or manually specify the ARNs of the IoT topics you want to use to run test cases\. The **topic name** is the MQTT **topic** your device use to publish messages to\.

         1. Choose **Add**\.

      1. Choose **Specify topicfilter resource ARN for the Subscribe action**\.

         1. Choose **Add ARN**\.

         1. Specify the **region**, **accountId**, and **topic name** in the visual ARN editor or manually specify the ARNs of the IoT topics you want to use to run test cases\. The **topic name** is the MQTT **topic** your device uses to subscribe to\.

         1. Choose **Add**\.

1. Choose **Review policy**\.

1. Under **Review policy**, enter a **Name**\.

1. Choose **Create policy**\.

1. On the left navigation pane, Choose **Roles**\.

1. Choose **Create Role**\.

1. Under **Or select a service to view its use cases**, select **IoT**\.

1. Under **Select your use case**, select **IoT**\.

1. Choose **Next: Permissions**\.

1. Under **Set permissions boundary**, Choose **Use a permissions boundary to control the maximum role permissions**, and then choose the policy you just created\.

1. Choose **Next: Tags**\.

1. Choose **Next: Review**\.

1. Enter a **Role name** and a **Role description**\.

1. Choose **Create role**\.

1. Navigate to the role you created\.

1. Choose **Attach policies** in the **Permissions** tab, and then choose the policy you created in **Step 4**\.

1. Choose **Attach policy**\.

1. Choose **Trust relationships** tab and choose **Edit trust relationship**\.

1. Enter this policy:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "AllowAwsIoTCoreDeviceAdvisor",
         "Effect": "Allow",
         "Principal": {
           "Service": "iotdeviceadvisor.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. Choose **Update Trust Policy**\.

## Create a custom managed policy for your Device Advisor user account<a name="da-managed-policy"></a>

1. Navigate to the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/) and log in to your account\.

1. In the left navigation pane, choose **Policies**\.

1. Choose **Create Policy**, then choose the **JSON** tab\. 

1. Add the necessary permissions to use Device Advisor\. The policy document can be found under [Security best practices](https://docs.aws.amazon.com/iot/latest/developerguide/security-best-practices.html#device-advisor-perms)\. 

1. Choose **Review Policy**\.

1. Enter a **Name** and **Description**\.

1. Choose **Create Policy**\.

## Create an IAM user to use to run Device Advisor tests<a name="da-iam-user"></a>

**Note**  
We recommend that you create an IAM user to use when you run Device Advisor tests\. Although we don't recommend it, you can also use an IAM Admin user\.

1. Navigate to the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/) and log in to your account\.

1. In the left navigation pane, Choose **Users**\.

1. Choose **Add User**\.

1. Enter a **User name**\.

1. Select **Programmatic access**\.

1. Choose **Next: Permissions**\.

1. Choose **Attach existing policies directly**\.

1. Enter the name of the custom\-managed policy you created in the search box and then select the check box to the left of **Policy name**\.

1. Choose **Next: Tags**\.

1. Choose **Next: Review**\.

1. Choose **Create user**\.

1. Choose **Close**\.

## Create an AWS IoT thing and certificate<a name="da-create-thing-certificate"></a>

1. Go to the [AWS IoT Core console](https://console.aws.amazon.com/iot/)\. Log in to the account you use for Device Advisor testing, and in the left navigation pane, choose **Manage**\.

1. If you have any existing things, choose **Create** to create a new thing\. Otherwise, on the **You don't have any things yet** page, choose **Register a thing**\.

1. On the **Creating AWS IoT things** page, choose **Create a single thing**\.

1. On the **Add your device to the thing registry** page, enter a **Name** for your thing\. Choose **Next**\.

1. On the **Add a certificate for your thing** page, choose **Create certificate**\. Notifications appears confirming that your thing and a certificate for your thing are created\.

1. Copy the certificate you created in the previous step to your device\. The correct location of the certificate depends on references to the certificate in your device's software or firmware\.

Device Advisor requires access to your AWS resources \(things, certificates, endpoint\) on your behalf\. Your IAM user must have the necessary permissions\. Device Advisor will also publish logs to Amazon CloudWatch if you attach the necessary permissions policy to your IAM user\.

## Configure your test device<a name="da-configure-device"></a>

**Configure your test device**

Device Advisor uses the server name indication \(SNI\) TLS extension to apply TLS configurations\. Devices must use this extension when connecting and pass a server name that is identical to the Device Advisor test endpoint\.

Device Advisor allows the TLS connection when test is in ”Running“ state and denies the TLS connection before and after each test run\. Device connect retry mechanism is recommended to have full\-automated testing experience with Device Advisor\. If you run a test suite with more than one test case say \- TLS connect, MQTT connect and MQTT publish then we recommend that you have a mechanism built for your device to try connecting to our test end point every 5 seconds for one to two minutes\. This will enable you to run multiple test cases in sequence in an automated manner\.
+ You must configure the firmware or software on the device that you'll use for testing to connect to the Device Advisor test endpoint for your account\. The command to get the test endpoint is:

  ```
  aws iot describe-endpoint --endpoint-type iot:DeviceAdvisor --region us-east-1
  ```