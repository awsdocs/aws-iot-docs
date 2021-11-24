# Step 1: Prepare the Raspberry Pi to demonstrate MQTT message communication<a name="iot-dc-testconn-provision"></a>

This procedure creates the resources in AWS IoT and in the Raspberry Pi to demonstrate MQTT message communication using the AWS IoT Device Client\.

**Topics**
+ [Create the certificate files to demonstrate MQTT communication](#iot-dc-testconn-provision-certs)
+ [Provision your device to demonstrate MQTT communication](#iot-dc-testconn-provision-aws)
+ [Configure the AWS IoT Device Client config file and MQTT test client to demonstrate MQTT communication](#iot-dc-testconn-provision-dc-config)

## Create the certificate files to demonstrate MQTT communication<a name="iot-dc-testconn-provision-certs"></a>

This procedure creates the device certificate files for this demo\.

**To create and download the device certificate files for your Raspberry Pi**



1. In the terminal window on your local host computer, enter the following command to create the device certificate files for your device\.

   ```
   mkdir ~/certs/pubsub
   aws iot create-keys-and-certificate \
   --set-as-active \
   --certificate-pem-outfile "~/certs/pubsub/device.pem.crt" \
   --public-key-outfile "~/certs/pubsub/public.pem.key" \
   --private-key-outfile "~/certs/pubsub/private.pem.key"
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
   chmod 700 ~/certs/pubsub
   chmod 644 ~/certs/pubsub/*
   chmod 600 ~/certs/pubsub/private.pem.key
   ```

1. Run this command to review the permissions on your certificate directories and files\.

   ```
   ls -l ~/certs/pubsub
   ```

   The output of the command should be the same as what you see here, except the file dates and times will be different\.

   ```
   -rw-r--r-- 1 pi pi 1220 Oct 28 13:02 device.pem.crt
   -rw------- 1 pi pi 1675 Oct 28 13:02 private.pem.key
   -rw-r--r-- 1 pi pi  451 Oct 28 13:02 public.pem.key
   ```

1. Enter these commands to create the directories for the log files\.

   ```
   mkdir ~/.aws-iot-device-client
   mkdir ~/.aws-iot-device-client/log
   chmod 745 ~/.aws-iot-device-client/log
   echo " " > ~/.aws-iot-device-client/log/aws-iot-device-client.log
   echo " " > ~/.aws-iot-device-client/log/pubsub_rx_msgs.log
   chmod 600 ~/.aws-iot-device-client/log/*
   ```

## Provision your device to demonstrate MQTT communication<a name="iot-dc-testconn-provision-aws"></a>

This section creates the AWS IoT resources that provision your Raspberry Pi in AWS IoT\. 

**To provision your device in AWS IoT:**

1. In the terminal window on your local host computer, enter the following command to get the address of the device data endpoint for your AWS account\.

   ```
   aws iot describe-endpoint --endpoint-type IoT:Data-ATS
   ```

   The endpoint value hasn’t changed since the time you ran this command for the previous tutorial\. Running the command again here is done to make it easy to find and paste the data endpoint value into the config file used in this tutorial\.

   The command from the previous steps returns a response like the following\. Record the `endpointAddress` value for later use\.

   ```
   {
   "endpointAddress": "a3qjEXAMPLEffp-ats.iot.us-west-2.amazonaws.com"
   }
   ```

1. Enter this command to create a new AWS IoT thing resource for your Raspberry Pi\.

   ```
   aws iot create-thing --thing-name "PubSubTestThing"
   ```

   Because an AWS IoT thing resource is a *virtual* representation of your device in the cloud, we can create multiple thing resources in AWS IoT to use for different purposes\. They can all be used by the same physical IoT device to represent different aspects of the device\.

   These tutorials will only use one thing resource at a time to represent the Raspberry Pi\. This way, in these tutorials, they represent the different demos so that after you create the AWS IoT resources for a demo, you can go back and repeat the demo using the resources you created specifically for each\.

   If your AWS IoT thing resource was created, the command returns a response like this\.

   ```
   {
   "thingName": "PubSubTestThing",
   "thingArn": "arn:aws:iot:us-west-2:57EXAMPLE833:thing/PubSubTestThing",
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
              "arn:aws:iot:us-west-2:57EXAMPLE833:client/PubSubTestThing"
            ]
          },
          {
            "Effect": "Allow",
            "Action": [
              "iot:Publish"
            ],
            "Resource": [
              "arn:aws:iot:us-west-2:57EXAMPLE833:topic/test/dc/pubtopic"
            ]
          },
          {
            "Effect": "Allow",
            "Action": [
              "iot:Subscribe"
            ],
            "Resource": [
              "arn:aws:iot:us-west-2:57EXAMPLE833:topicfilter/test/dc/subtopic"
            ]
          },
          {
            "Effect": "Allow",
            "Action": [
              "iot:Receive"
            ],
            "Resource": [
              "arn:aws:iot:us-west-2:57EXAMPLE833:topic/test/dc/subtopic"
            ]
          }
        ]
      }
      ```

   1. In the editor, in each `Resource` section of the policy document, replace *us\-west\-2:57EXAMPLE833* with your AWS Region, a colon character \(:\), and your 12\-digit AWS account number\.

   1. Save the file in your text editor as **\~/policies/pubsub\_test\_thing\_policy\.json**\. 

1. Run this command to use the policy document from the previous steps to create an AWS IoT policy\.

   ```
   aws iot create-policy \
   --policy-name "PubSubTestThingPolicy" \
   --policy-document "file://~/policies/pubsub_test_thing_policy.json"
   ```

   If the policy is created, the command returns a response like this\.

   ```
   {
       "policyName": "PubSubTestThingPolicy",
       "policyArn": "arn:aws:iot:us-west-2:57EXAMPLE833:policy/PubSubTestThingPolicy",
       "policyDocument": "{\n\"Version\": \"2012-10-17\",\n\"Statement\": [\n{\n\"Effect\": \"Allow\",\n\"Action\": [\n\"iot:Connect\"\n],\n\"Resource\": [\n\"arn:aws:iot:us-west-2:57EXAMPLE833:client/PubSubTestThing\"\n]\n},\n{\n\"Effect\": \"Allow\",\n\"Action\": [\n\"iot:Publish\"\n],\n\"Resource\": [\n\"arn:aws:iot:us-west-2:57EXAMPLE833:topic/test/dc/pubtopic\"\n]\n},\n{\n\"Effect\": \"Allow\",\n\"Action\": [\n\"iot:Subscribe\"\n],\n\"Resource\": [\n\"arn:aws:iot:us-west-2:57EXAMPLE833:topicfilter/test/dc/subtopic\"\n]\n},\n{\n\"Effect\": \"Allow\",\n\"Action\": [\n\"iot:Receive\"\n],\n\"Resource\": [\n\"arn:aws:iot:us-west-2:57EXAMPLE833:topic/test/dc/*\"\n]\n}\n]\n}\n",
       "policyVersionId": "1"
   ```

1. Run this command to attach the policy to the device certificate\. Replace `certificateArn` with the `certificateArn` value you saved earlier in this section\.

   ```
   aws iot attach-policy \
   --policy-name "PubSubTestThingPolicy" \
   --target "certificateArn"
   ```

   If successful, this command returns nothing\.

1. Run this command to attach the device certificate to the AWS IoT thing resource\. Replace `certificateArn` with the `certificateArn` value you saved earlier in this section\.

   ```
   aws iot attach-thing-principal \
   --thing-name "PubSubTestThing" \
   --principal "certificateArn"
   ```

   If successful, this command returns nothing\.

After you successfully provision your device in AWS IoT, you're ready to continue to [Configure the AWS IoT Device Client config file and MQTT test client to demonstrate MQTT communication](#iot-dc-testconn-provision-dc-config)\.

## Configure the AWS IoT Device Client config file and MQTT test client to demonstrate MQTT communication<a name="iot-dc-testconn-provision-dc-config"></a>

This procedure creates a config file to test the AWS IoT Device Client\.

**To create the config file to test the AWS IoT Device Client**

1. In the terminal window on your local host computer that's connected to your Raspberry Pi:

   1. Open a text editor, such as `nano`\.

   1. Copy this JSON document and paste it into your open text editor\.

      ```
      {
        "endpoint": "a3qEXAMPLEaffp-ats.iot.us-west-2.amazonaws.com",
        "cert": "~/certs/pubsub/device.pem.crt",
        "key": "~/certs/pubsub/private.pem.key",
        "root-ca": "~/certs/AmazonRootCA1.pem",
        "thing-name": "PubSubTestThing",
        "logging": {
          "enable-sdk-logging": true,
          "level": "DEBUG",
          "type": "STDOUT",
          "file": ""
        },
        "jobs": {
          "enabled": false,
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
            "enabled": true,
            "publish-topic": "test/dc/pubtopic",
            "publish-file": "",
            "subscribe-topic": "test/dc/subtopic",
            "subscribe-file": "~/.aws-iot-device-client/log/pubsub_rx_msgs.log"
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

   1. Replace the *endpoint* value with device data endpoint for your AWS account that you found in [Provision your device in AWS IoT Core](iot-dc-install-provision.md#iot-dc-install-dc-provision)\.

   1. Save the file in your text editor as **\~/dc\-configs/dc\-pubsub\-config\.json**\.

   1. Run this command to set the permissions on the new config file\.

      ```
      chmod 644 ~/dc-configs/dc-pubsub-config.json
      ```

1. To prepare the **MQTT test client** to subscribe to all MQTT messages:

   1. On your local host computer, in the [AWS IoT console](https://console.aws.amazon.com/iot/home#/test), choose **MQTT test client**\.

   1. In the **Subscribe to a topic** tab, in **Topic filter**, enter **\#** \(a single pound sign\), and choose **Subscribe**\.

   1. Below the **Subscriptions** label, confirm that you see **\#** \(a single pound sign\)\.

   Leave the window with the **MQTT test client** open while you continue this tutorial\.

After you save the file and configure the **MQTT test client**, you're ready to continue to [Step 2: Demonstrate publishing messages with the AWS IoT Device Client](iot-dc-testconn-publish.md)\.