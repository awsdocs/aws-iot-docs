# Step 3: Configure the AWS IoT Device Client to test connectivity<a name="iot-dc-install-configure"></a>

The procedures in this section configure the AWS IoT Device Client to publish an MQTT message from your Raspberry Pi\.

**Topics**
+ [Create the config file](#iot-dc-install-dc-configure-step1)
+ [Open MQTT test client](#iot-dc-install-dc-configure-step2)
+ [Run AWS IoT Device Client](#iot-dc-install-dc-configure-step3)

## Create the config file<a name="iot-dc-install-dc-configure-step1"></a>

This procedure creates the config file to test the AWS IoT Device Client\.

**To create the config file to test the AWS IoT Device Client**
+ In the terminal window on your local host computer that's connected to your Raspberry Pi:

  1. Enter these commands to create a directory for the config files and set the permission on the directory:

     ```
     mkdir ~/dc-configs
     chmod 745 ~/dc-configs
     ```

  1. Open a text editor, such as `nano`\.

  1. Copy this JSON document and paste it into your open text editor\.

     ```
     {
       "endpoint": "a3qEXAMPLEaffp-ats.iot.us-west-2.amazonaws.com",
       "cert": "~/certs/testconn/device.pem.crt",
       "key": "~/certs/testconn/private.pem.key",
       "root-ca": "~/certs/AmazonRootCA1.pem",
       "thing-name": "DevCliTestThing",
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

  1. Replace the *endpoint* value with device data endpoint for your AWS account that you found in [Provision your device in AWS IoT Core](iot-dc-install-provision.md#iot-dc-install-dc-provision)\.

  1. Save the file in your text editor as **\~/dc\-configs/dc\-testconn\-config\.json**\.

  1. Run this command to set the permissions on the new config file\.

     ```
     chmod 644 ~/dc-configs/dc-testconn-config.json
     ```

After you save the file, you're ready to continue to [Open MQTT test client](#iot-dc-install-dc-configure-step2)\.

## Open MQTT test client<a name="iot-dc-install-dc-configure-step2"></a>

This procedure prepares the **MQTT test client** in the AWS IoT console to subscribe to the MQTT message that the AWS IoT Device Client publishes when it runs\.

**To prepare the **MQTT test client** to subscribe to all MQTT messages**

1. On your local host computer, in the [AWS IoT console](https://console.aws.amazon.com/iot/home#/test), choose **MQTT test client**\.

1. In the **Subscribe to a topic** tab, in **Topic filter**, enter **\#** \(a single pound sign\), and choose **Subscribe** to subscribe to every MQTT topic\.

1. Below the **Subscriptions** label, confirm that you see **\#** \(a single pound sign\)\.

Leave the window with the **MQTT test client** open as you continue to [Run AWS IoT Device Client](#iot-dc-install-dc-configure-step3)\.

## Run AWS IoT Device Client<a name="iot-dc-install-dc-configure-step3"></a>

This procedure runs the AWS IoT Device Client so that it publishes a single MQTT message that the **MQTT test client** receives and displays\.

**To send an MQTT message from the AWS IoT Device Client**

1. Make sure that both the terminal window that's connected to your Raspberry Pi and the window with the **MQTT test client** are visible while you perform this procedure\.

1. In the terminal window, enter these commands to run the AWS IoT Device Client using the config file created in [Create the config file](#iot-dc-install-dc-configure-step1)\.

   ```
   cd ~/aws-iot-device-client/build
   ./aws-iot-device-client --config-file ~/dc-configs/dc-testconn-config.json
   ```

   In the terminal window, the AWS IoT Device Client displays information messages and any errors that occur when it runs\.

   If no errors are displayed in the terminal window, review the **MQTT test client**\.

1. In the **MQTT test client**, in the Subscriptions window, see the *Hello World\!* message sent to the `test/dc/pubtopic` message topic\.

1. If the AWS IoT Device Client displays no errors and you see *Hello World\!* sent to the `test/dc/pubtopic` message in the **MQTT test client**, you've demonstrated a successful connection\.

1. In the terminal window, enter **^C** \(Ctrl\-C\) to stop the AWS IoT Device Client\.

After you've demonstrated that the AWS IoT Device Client is running correctly on your Raspberry Pi and can communicate with AWS IoT, you can continue to the [Tutorial: Demonstrate MQTT message communication with the AWS IoT Device Client](iot-dc-testconn.md)\.