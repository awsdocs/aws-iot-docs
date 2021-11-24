# Step 2: Provision your Raspberry Pi in AWS IoT<a name="iot-dc-install-provision"></a>

The procedures in this section start with the saved microSD image that has the AWS CLI and AWS IoT Device Client installed and create the AWS IoT resources and device certificates that provision your Raspberry Pi in AWS IoT\.

## Install the microSD card in your Raspberry Pi<a name="iot-dc-install-dc-restore"></a>

This procedure installs the microSD card with the necessary software loaded and configured into the Raspberry Pi and configures your AWS account so that you can continue with the tutorials in this learning path\.

Use a microSD card from [\(Optional\) Save the microSD card image](iot-dc-install-dc-save.md) that has the necessary software for the exercises and tutorials in this learning path\.

**To install the microSD card in your Raspberry Pi**

1. With the power disconnected from the Raspberry Pi, insert the microSD card into the Raspberry Pi\.

1. Apply power to the Raspberry Pi\.

1. After about a minute, on the local host computer, restart the terminal window session and log in to the Raspberry Pi\.

1. On your local host computer, in the terminal window, and with the **Access Key ID** and **Secret Access Key** credentials for your Raspberry Pi:

   1. Run the AWS configure app with this command:

      ```
      aws configure
      ```

   1. Enter your AWS account credentials and configuration information when prompted:

      ```
      AWS Access Key ID [****************YXYX]: your Access Key ID
      AWS Secret Access Key [****************YXYX]: your Secret Access Key
      Default region name [us-west-2]: your AWS Region code
      Default output format [json]: json
      ```

After you have restored your AWS account credentials, you're ready to continue to [Provision your device in AWS IoT Core](#iot-dc-install-dc-provision)\.

## Provision your device in AWS IoT Core<a name="iot-dc-install-dc-provision"></a>

The procedures in this section create the AWS IoT resources that provision your Raspberry Pi in AWS IoT\. As you create these resources, you'll be asked to record various pieces of information\. This information is used by the AWS IoT Device Client configuration in the next procedure\.

For your Raspberry Pi to work with AWS IoT, it must be provisioned\. Provisioning is the process of creating and configuring the AWS IoT resources that are necessary to support your Raspberry Pi as an IoT device\.

With your Raspberry Pi powered up and restarted, connect the terminal window on your local host computer to the Raspberry Pi and complete these procedures\.

**Topics**
+ [Create and download device certificate files](#iot-dc-install-dc-provision-certs)
+ [Create AWS IoT resources](#iot-dc-install-dc-provision-resources)

### Create and download device certificate files<a name="iot-dc-install-dc-provision-certs"></a>

This procedure creates the device certificate files for this demo\.

**To create and download the device certificate files for your Raspberry Pi**

1. In the terminal window on your local host computer, enter these commands to create the device certificate files for your device\.

   ```
   mkdir ~/certs/testconn
   aws iot create-keys-and-certificate \
   --set-as-active \
   --certificate-pem-outfile "~/certs/testconn/device.pem.crt" \
   --public-key-outfile "~/certs/testconn/public.pem.key" \
   --private-key-outfile "~/certs/testconn/private.pem.key"
   ```

   The command returns a response like the following\. Record the `certificateArn` value for later use\.

   ```
   {
       "certificateArn": "arn:aws:iot:us-west-2:57EXAMPLE833:cert/76e7e4edb3e52f52334be2f387a06145b2aa4c7fcd810f3aea2d92abc227d269",
       "certificateId": "76e7e4edb3e52f5233EXAMPLE7a06145b2aa4c7fcd810f3aea2d92abc227d269",
       "certificatePem": "-----BEGIN CERTIFICATE-----\nMIIDWTCCAkGgAwIBAgI_SHORTENED_FOR_EXAMPLE_Lgn4jfgtS\n-----END CERTIFICATE-----\n",
       "keyPair": {
           "PublicKey": "-----BEGIN PUBLIC KEY-----\nMIIBIjANBgkqhkiG9w0BA_SHORTENED_FOR_EXAMPLE_ImwIDAQAB\n-----END PUBLIC KEY-----\n",
           "PrivateKey": "-----BEGIN RSA PRIVATE KEY-----\nMIIEowIBAAKCAQE_SHORTENED_FOR_EXAMPLE_T9RoDiukY\n-----END RSA PRIVATE KEY-----\n"
       }
   }
   ```

1. Enter the following commands to set the permissions on the certificate directory and its files\.

   ```
   chmod 745 ~
   chmod 700 ~/certs/testconn
   chmod 644 ~/certs/testconn/*
   chmod 600 ~/certs/testconn/private.pem.key
   ```

1. Run this command to review the permissions on your certificate directories and files\.

   ```
   ls -l ~/certs/testconn
   ```

   The output of the command should be the same as what you see here, except the file dates and times will be different\.

   ```
   -rw-r--r-- 1 pi pi 1220 Oct 28 13:02 device.pem.crt
   -rw------- 1 pi pi 1675 Oct 28 13:02 private.pem.key
   -rw-r--r-- 1 pi pi  451 Oct 28 13:02 public.pem.key
   ```

At this point, you have the device certificate files installed on your Raspberry Pi and you can continue to [Create AWS IoT resources](#iot-dc-install-dc-provision-resources)\.

### Create AWS IoT resources<a name="iot-dc-install-dc-provision-resources"></a>

This procedure provisions your device in AWS IoT by creating the resources that your device needs to access AWS IoT features and services\.

**To provision your device in AWS IoT**

1. In the terminal window on your local host computer, enter the following command to get the address of the device data endpoint for your AWS account\.

   ```
   aws iot describe-endpoint --endpoint-type IoT:Data-ATS
   ```

   The command from the previous steps returns a response like the following\. Record the `endpointAddress` value for later use\.

   ```
   {
       "endpointAddress": "a3qjEXAMPLEffp-ats.iot.us-west-2.amazonaws.com"
   }
   ```

1. Enter this command to create an AWS IoT thing resource for your Raspberry Pi\.

   ```
   aws iot create-thing --thing-name "DevCliTestThing"
   ```

   If your AWS IoT thing resource was created, the command returns a response like this\.

   ```
   {
       "thingName": "DevCliTestThing",
       "thingArn": "arn:aws:iot:us-west-2:57EXAMPLE833:thing/DevCliTestThing",
       "thingId": "8ea78707-32c3-4f8a-9232-14bEXAMPLEfd"
   }
   ```

1. In the terminal window:

   1. Open a text editor, such as `nano`\.

   1. Copy this JSON policy document and paste it into your open text editor\.

      ```
      {
          "Version": "2012-10-17",
          "Statement": [
              {
                  "Effect": "Allow",
                  "Action": [
                      "iot:Publish",
                      "iot:Subscribe",
                      "iot:Receive",
                      "iot:Connect"
                  ],
                  "Resource": [
                      "*"
                  ]
              }
          ]
      }
      ```
**Note**  
This policy document generously grants every resource permission to connect, receive, publish, and subscribe\. Normally policies grant only permission to specific resources to perform specific actions\. However, for the initial device connectivity test, this overly general and permissive policy is used to minimize the chance of an access problem during this test\. In the subsequent tutorials, more narrowly scoped policy documents will be use to demonstrate better practices in policy design\.

   1. Save the file in your text editor as **\~/policies/dev\_cli\_test\_thing\_policy\.json**\. 

1. Run this command to use the policy document from the previous steps to create an AWS IoT policy\.

   ```
   aws iot create-policy \
   --policy-name "DevCliTestThingPolicy" \
   --policy-document "file://~/policies/dev_cli_test_thing_policy.json"
   ```

   If the policy is created, the command returns a response like this\.

   ```
   {
       "policyName": "DevCliTestThingPolicy",
       "policyArn": "arn:aws:iot:us-west-2:57EXAMPLE833:policy/DevCliTestThingPolicy",
       "policyDocument": "{\n    \"Version\": \"2012-10-17\",\n    \"Statement\": [\n        {\n            \"Effect\": \"Allow\",\n            \"Action\": [\n                \"iot:Publish\",\n                \"iot:Subscribe\",\n                \"iot:Receive\",\n                \"iot:Connect\"\n            ],\n            \"Resource\": [\n                \"*\"\n            ]\n        }\n    ]\n}\n",
       "policyVersionId": "1"
   }
   ```

1. Run this command to attach the policy to the device certificate\. Replace `certificateArn` with the `certificateArn` value you saved earlier\.

   ```
   aws iot attach-policy \
   --policy-name "DevCliTestThingPolicy" \
   --target "certificateArn"
   ```

   If successful, this command returns nothing\.

1. Run this command to attach the device certificate to the AWS IoT thing resource\. Replace `certificateArn` with the `certificateArn` value you saved earlier\.

   ```
   aws iot attach-thing-principal \
   --thing-name "DevCliTestThing" \
   --principal "certificateArn"
   ```

   If successful, this command returns nothing\.

After you successfully provisioned your device in AWS IoT, you're ready to continue to [Step 3: Configure the AWS IoT Device Client to test connectivity](iot-dc-install-configure.md)\.