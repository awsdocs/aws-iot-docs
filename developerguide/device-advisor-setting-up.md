# Setting up<a name="device-advisor-setting-up"></a>

Before you use Device Advisor for the first time, complete the following tasks\.

**Topics**
+ [Create an IoT thing](#da-create-thing-certificate)
+ [Create an IAM role to be used as your device role](#da-iam-role)
+ [Create a custom\-managed policy for an IAM user to use Device Advisor](#da-managed-policy)
+ [Create an IAM user to use Device Advisor](#da-iam-user)
+ [Configure your device](#da-configure-device)

## Create an IoT thing<a name="da-create-thing-certificate"></a>

First you will need to create a thing and attach a certificate to the thing\. Use the following tutorial to create a thing: [Create a thing object](https://docs.aws.amazon.com/iot/latest/developerguide/create-iot-resources.html#create-aws-thing)\.

## Create an IAM role to be used as your device role<a name="da-iam-role"></a>

**Note**  
You can quickly create the device role using the Device Advisor console\. See [ Getting started with the Device Advisor in the console](https://docs.aws.amazon.com/iot/latest/developerguide/da-console-guide.html) for the steps to set up your device role using the Device Advisor console\.

1. Go to the [AWS IAM console](https://console.aws.amazon.com/iam/home?region=us-west-2#/home) and log in to the account you use for Device Advisor testing\.

1. In the left navigation pane, chose **Policies**\.

1. Choose **Create policy**\.

1. Under **Create policy**, do the following:

   1. For **Service**, choose **IoT**\.

   1. Under **Actions**, either select actions based on the policy attached to the IoT thing or certificate created in the previous section \(recommended\), or search for the following actions in the **Filter** action box and select them\.
      + Connect
      + Publish
      + Subscribe
      + Receive

   1. Under **Resources**, or best security practices, we recommend you restrict the **client**, **topic**, and **topicfilter** resources using the following steps:

      1. Choose **Specify client resource ARN for the Connect action**\.

         1. Choose **Add ARN**\.

         1. Specify the **region**, **accountId**, and **clientId** in the visual ARN editor, or manually specify the Amazon Resource Names \(ARNs\) of the IoT topics you want to use to run test cases\. The **clientId** is the MQTT **clientId** your device uses to interact with Device Advisor\.

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

1. Under **Or select a service to view its use cases**, choose **IoT**\.

1. Under **Select your use case**, choose **IoT**\.

1. Choose **Next: Permissions**\.

1. ****\(Optional\) Under **Set permissions boundary**, Choose **Use a permissions boundary to control the maximum role permissions**, and then choose the policy you just created\.

1. Choose **Next: Tags**\.

1. Choose **Next: Review**\.

1. Enter a **Role name** and a **Role description**\.

1. Choose **Create role**\.

1. Navigate to the role you created\.

1. In the **Permissions** tab, choose **Attach policies** and then choose the policy you created in Step 4

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

## Create a custom\-managed policy for an IAM user to use Device Advisor<a name="da-managed-policy"></a>

1. Navigate to the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\. If prompted, enter your AWS credentials to sign in\.

1. In the left navigation pane, choose **Policies**\.

1. Choose **Create Policy**, then choose the **JSON** tab\. 

1. Add the necessary permissions to use Device Advisor\. The policy document can be found n the topic [Security best practices](https://docs.aws.amazon.com/iot/latest/developerguide/security-best-practices.html#device-advisor-perms)\. 

1. Choose **Review Policy**\.

1. Enter a **Name** and **Description**\.

1. Choose **Create Policy**\.

## Create an IAM user to use Device Advisor<a name="da-iam-user"></a>

**Note**  
We recommend that you create an IAM user to use when you run Device Advisor tests\. Using an IAM admin user to run Device Advisor tests, while allowed, is not recommended\.

1. Navigate to the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/) If prompted, enter your AWS credentials to sign in\.

1. In the left navigation pane, Choose **Users**\.

1. Choose **Add User**\.

1. Enter a **User name**\.

1. Select **Programmatic access**\.

1. Choose **Next: Permissions**\.

1. Choose **Attach existing policies directly**\.

1. Enter the name of the custom\-managed policy the you created in the search box, and then select the check box for **Policy name**\.

1. Choose **Next: Tags**\.

1. Choose **Next: Review**\.

1. Choose **Create user**\.

1. Choose **Close**\.

Device Advisor requires access to your AWS resources \(things, certificates, and endpoints\) on your behalf\. Your IAM user must have the necessary permissions\. Device Advisor will also publish logs to Amazon CloudWatch if you attach the necessary permissions policy to your IAM user\.

## Configure your device<a name="da-configure-device"></a>

Device Advisor uses the server name indication \(SNI\) TLS extension to apply TLS configurations\. Devices must use this extension when connecting and pass a server name that is identical to the Device Advisor test endpoint\.

Device Advisor allows the TLS connection when a test is in the `Running` state and denies the TLS connection before and after each test run\. For this reason, we also recommend using the device connect retry mechanism to have a fully automated testing experience with Device Advisor\. If you run a test suite with more than one test case \(for instance TLS connect, MQTT connect, and MQTT publish\) then we recommend that you have a mechanism built for your device to try connecting to our test endpoint every five seconds\. You can then run multiple test cases, in sequence, in an automated manner\.

**Note**  
To make your device software ready for testing, we recommend that you have an SDK that can connect to AWS IoT Core and update the SDK with the Device Advisor test endpoint provided for your account\. 

Device Advisor supports two types of endpoints: Account\-level endpoints and Device\-level endpoints\. Choose the endpoint that best fits your use case\. To simultaneously run multiple test suites using different devices, use a Device\-level endpoint\. Run the following command to get the Device\-level endpoint:

```
aws iotdeviceadvisor get-endpoint --thing-arn your-thing-arn
```

or

```
aws iotdeviceadvisor get-endpoint --certificate-arn your-certificate-arn
```

To run one test suite at a time, choose an Account\-level endpoint\. Run the following command to get the Account\-level endpoint:

```
aws iotdeviceadvisor get-endpoint
```