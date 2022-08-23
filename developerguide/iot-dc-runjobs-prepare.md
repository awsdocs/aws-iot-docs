# Step 1: Prepare the Raspberry Pi to run jobs<a name="iot-dc-runjobs-prepare"></a>

The procedures in this section describe how to prepare your Raspberry Pi to run jobs by using the AWS IoT Device Client\.

**Note**  
These procedures are device specific\. If you want to perform the procedures in this section with more than one device at the same time, each device will need its own policy and unique, device\-specific certificate and thing name\. To give each device its unique resources, perform this procedure one time for each device while changing the device\-specific elements as described in the procedures\.

**Topics**
+ [Provision your Raspberry Pi to demonstrate jobs](#iot-dc-runjobs-prepare-provision)
+ [Configure the AWS IoT Device Client to run the jobs agent](#iot-dc-runjobs-prepare-config)

## Provision your Raspberry Pi to demonstrate jobs<a name="iot-dc-runjobs-prepare-provision"></a>

The procedures in this section provision your Raspberry Pi in AWS IoT by creating AWS IoT resources and device certificates for it\. 

### Create and download device certificate files to demonstrate AWS IoT jobs<a name="iot-dc-runjobs-prepare-cert"></a>

This procedure creates the device certificate files for this demo\.

If you are preparing more than one device, this procedure must be performed on each device\.

**To create and download the device certificate files for your Raspberry Pi:**

In the terminal window on your local host computer that's connected to your Raspberry Pi, enter these commands\.

1. Enter the following command to create the device certificate files for your device\.

   ```
   aws iot create-keys-and-certificate \
   --set-as-active \
   --certificate-pem-outfile "~/certs/jobs/device.pem.crt" \
   --public-key-outfile "~/certs/jobs/public.pem.key" \
   --private-key-outfile "~/certs/jobs/private.pem.key"
   ```

   The command returns a response like the following\. Save the `certificateArn` value for later use\.

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
   chmod 700 ~/certs/jobs
   chmod 644 ~/certs/jobs/*
   chmod 600 ~/certs/jobs/private.pem.key
   ```

1. Run this command to review the permissions on your certificate directories and files\.

   ```
   ls -l ~/certs/jobs
   ```

   The output of the command should be the same as what you see here, except the file dates and times will be different\.

   ```
   -rw-r--r-- 1 pi pi 1220 Oct 28 13:02 device.pem.crt
   -rw------- 1 pi pi 1675 Oct 28 13:02 private.pem.key
   -rw-r--r-- 1 pi pi  451 Oct 28 13:02 public.pem.key
   ```

After you have downloaded the device certificate files to your Raspberry Pi, you're ready to continue to [Provision your Raspberry Pi to demonstrate jobs](#iot-dc-runjobs-prepare-provision)\.

### Create AWS IoT resources to demonstrate AWS IoT jobs<a name="iot-dc-runjobs-prepare-iot"></a>

Create the AWS IoT resources for this device\.

If you are preparing more than one device, this procedure must be performed for each device\.



**To provision your device in AWS IoT:**

In the terminal window on your local host computer that's connected to your Raspberry Pi:

1. Enter the following command to get the address of the device data endpoint for your AWS account\.

   ```
   aws iot describe-endpoint --endpoint-type IoT:Data-ATS
   ```

   The endpoint value hasn’t changed since the last time you ran this command\. Running the command again here makes it easy to find and paste the data endpoint value into the config file used in this tutorial\.

   The describe\-endpoint command returns a response like the following\. Record the `endpointAddress` value for later use\.

   ```
   {
   "endpointAddress": "a3qjEXAMPLEffp-ats.iot.us-west-2.amazonaws.com"
   }
   ```

1. Replace *uniqueThingName* with a unique name for your device\. If you want to perform this tutorial with multiple devices, give each device its own name\. For example, **TestDevice01**, **TestDevice02**, and so on\.

   Enter this command to create a new AWS IoT thing resource for your Raspberry Pi\.

   ```
   aws iot create-thing --thing-name "uniqueThingName"
   ```

   Because an AWS IoT thing resource is a *virtual* representation of your device in the cloud, we can create multiple thing resources in AWS IoT to use for different purposes\. They can all be used by the same physical IoT device to represent different aspects of the device\.
**Note**  
When you want to secure the policy for multiple devices, you can use `${iot:Thing.ThingName}` instead of the static thing name, `uniqueThingName`\.

   These tutorials will only use one thing resource at a time per device\. This way, in these tutorials, they represent the different demos so that after you create the AWS IoT resources for a demo, you can go back and repeat the demos using the resources you created specifically for each\.

   If your AWS IoT thing resource was created, the command returns a response like this\. Record the `thingArn` value for use later when you create the job to run on this device\.

   ```
   {
   "thingName": "uniqueThingName",
   "thingArn": "arn:aws:iot:us-west-2:57EXAMPLE833:thing/uniqueThingName",
   "thingId": "8ea78707-32c3-4f8a-9232-14bEXAMPLEfd"
   }
   ```

1. In the terminal window:

   1. Open a text editor, such as `nano`\.

   1. Copy this JSON document and paste it into your open text editor\.

      ```
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Effect": "Allow",
            "Action": [
              "iot:Connect"
            ],
            "Resource": [
              "arn:aws:iot:us-west-2:57EXAMPLE833:client/uniqueThingName"
            ]
          },
          {
            "Effect": "Allow",
            "Action": [
              "iot:Publish"
            ],
            "Resource": [
              "arn:aws:iot:us-west-2:57EXAMPLE833:topic/test/dc/pubtopic",
              "arn:aws:iot:us-west-2:57EXAMPLE833:topic/$aws/events/job/*",
              "arn:aws:iot:us-west-2:57EXAMPLE833:topic/$aws/events/jobExecution/*",
              "arn:aws:iot:us-west-2:57EXAMPLE833:topic/$aws/things/uniqueThingName/jobs/*"
            ]
          },
          {
            "Effect": "Allow",
            "Action": [
              "iot:Subscribe"
            ],
            "Resource": [
              "arn:aws:iot:us-west-2:57EXAMPLE833:topicfilter/test/dc/subtopic",
              "arn:aws:iot:us-west-2:57EXAMPLE833:topic/$aws/events/jobExecution/*",
              "arn:aws:iot:us-west-2:57EXAMPLE833:topicfilter/$aws/things/uniqueThingName/jobs/*"
            ]
          },
          {
            "Effect": "Allow",
            "Action": [
              "iot:Receive"
            ],
            "Resource": [
              "arn:aws:iot:us-west-2:57EXAMPLE833:topic/test/dc/subtopic",
              "arn:aws:iot:us-west-2:57EXAMPLE833:topic/$aws/things/uniqueThingName/jobs/*"
            ]
          },
          {
            "Effect": "Allow",
            "Action": [
              "iot:DescribeJobExecution",
              "iot:GetPendingJobExecutions",
              "iot:StartNextPendingJobExecution",
              "iot:UpdateJobExecution"
            ],
            "Resource": [
              "arn:aws:iot:us-west-2:57EXAMPLE833:topic/$aws/things/uniqueThingName"
            ]
          }
        ]
      }
      ```

   1. In the editor, in the `Resource` section of every policy statement, replace *us\-west\-2:57EXAMPLE833* with your AWS Region, a colon character \(:\), and your 12\-digit AWS account number\.

   1. In the editor, in every policy statement, replace *uniqueThingName* with the thing name you gave this thing resource\.

   1. Save the file in your text editor as **\~/policies/jobs\_test\_thing\_policy\.json**\.

      If you are running this procedure for multiple devices, save the file to this file name on each device\.

1. Replace *uniqueThingName* with the thing name for the device, and then run this command to create an AWS IoT policy that is tailored for that device\.

   ```
   aws iot create-policy \
   --policy-name "JobTestPolicyForuniqueThingName" \
   --policy-document "file://~/policies/jobs_test_thing_policy.json"
   ```

   If the policy is created, the command returns a response like this\.

   ```
   {
       "policyName": "JobTestPolicyForuniqueThingName",
       "policyArn": "arn:aws:iot:us-west-2:57EXAMPLE833:policy/JobTestPolicyForuniqueThingName",
       "policyDocument": "{\n\"Version\": \"2012-10-17\",\n\"Statement\": [\n{\n\"Effect\": \"Allow\",\n\"Action\": [\n\"iot:Connect\"\n],\n\"Resource\": [\n\"arn:aws:iot:us-west-2:57EXAMPLE833:client/PubSubTestThing\"\n]\n},\n{\n\"Effect\": \"Allow\",\n\"Action\": [\n\"iot:Publish\"\n],\n\"Resource\": [\n\"arn:aws:iot:us-west-2:57EXAMPLE833:topic/test/dc/pubtopic\"\n]\n},\n{\n\"Effect\": \"Allow\",\n\"Action\": [\n\"iot:Subscribe\"\n],\n\"Resource\": [\n\"arn:aws:iot:us-west-2:57EXAMPLE833:topicfilter/test/dc/subtopic\"\n]\n},\n{\n\"Effect\": \"Allow\",\n\"Action\": [\n\"iot:Receive\"\n],\n\"Resource\": [\n\"arn:aws:iot:us-west-2:57EXAMPLE833:topic/test/dc/*\"\n]\n}\n]\n}\n",
       "policyVersionId": "1"
   ```

1. Replace *uniqueThingName* with the thing name for the device and `certificateArn` with the `certificateArn` value you saved earlier in this section for this device, and then run this command to attach the policy to the device certificate\. 

   ```
   aws iot attach-policy \
   --policy-name "JobTestPolicyForuniqueThingName" \
   --target "certificateArn"
   ```

   If successful, this command returns nothing\.

1.  Replace *uniqueThingName* with the thing name for the device, replace `certificateArn` with the `certificateArn` value that you saved earlier in this section, and then run this command to attach the device certificate to the AWS IoT thing resource\.

   ```
   aws iot attach-thing-principal \
   --thing-name "uniqueThingName" \
   --principal "certificateArn"
   ```

   If successful, this command returns nothing\.

After you successfully provisioned your Raspberry Pi, you're ready to repeat this section for another Raspberry Pi in your test or, if all devices have been provisioned, continue to [Configure the AWS IoT Device Client to run the jobs agent](#iot-dc-runjobs-prepare-config)\.

## Configure the AWS IoT Device Client to run the jobs agent<a name="iot-dc-runjobs-prepare-config"></a>

This procedure creates a config file for the AWS IoT Device Client to run the jobs agent:\.

Note: if you are preparing more than one device, this procedure must be performed on each device\.

**To create the config file to test the AWS IoT Device Client:**

1. In the terminal window on your local host computer that's connected to your Raspberry Pi:

   1. Open a text editor, such as `nano`\.

   1. Copy this JSON document and paste it into your open text editor\.

      ```
      {
        "endpoint": "a3qEXAMPLEaffp-ats.iot.us-west-2.amazonaws.com",
        "cert": "~/certs/jobs/device.pem.crt",
        "key": "~/certs/jobs/private.pem.key",
        "root-ca": "~/certs/AmazonRootCA1.pem",
        "thing-name": "uniqueThingName",
        "logging": {
          "enable-sdk-logging": true,
          "level": "DEBUG",
          "type": "STDOUT",
          "file": ""
        },
        "jobs": {
          "enabled": true,
          "handler-directory": ""
        },
        "tunneling": {
          "enabled": false
        },
        "device-defender": {
          "enabled": false,
          "interval": 300
        },
        "fleet-provisioning": {
          "enabled": false,
          "template-name": "",
          "template-parameters": "",
          "csr-file": "",
          "device-key": ""
        },
        "samples": {
          "pub-sub": {
            "enabled": false,
            "publish-topic": "",
            "publish-file": "",
            "subscribe-topic": "",
            "subscribe-file": ""
          }
        },
        "config-shadow": {
          "enabled": false
        },
        "sample-shadow": {
          "enabled": false,
          "shadow-name": "",
          "shadow-input-file": "",
          "shadow-output-file": ""
        }
      }
      ```

   1. Replace the *endpoint* value with device data endpoint value for your AWS account that you found in [Provision your device in AWS IoT Core](iot-dc-install-provision.md#iot-dc-install-dc-provision)\.

   1. Replace *uniqueThingName* with the thing name that you used for this device\.

   1. Save the file in your text editor as **\~/dc\-configs/dc\-jobs\-config\.json**\.

1. Run this command to set the file permissions of the new config file\.

   ```
   chmod 644 ~/dc-configs/dc-jobs-config.json
   ```

You won't use the **MQTT test client** for this test\. While the device will exchange jobs\-related MQTT messages with AWS IoT, job progress messages are only exchanged with the device running the job\. Because job progress messages are only exchanged with the device running the job, you can't subscribe to them from another device, such as the AWS IoT console\.

After you save the config file, you're ready to continue to [Step 2: Create and run the job in AWS IoT](iot-dc-runjobs-prepare-define.md)\.