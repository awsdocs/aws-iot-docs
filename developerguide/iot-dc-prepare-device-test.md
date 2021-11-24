# Step 3: Test your device and save the Amazon CA cert<a name="iot-dc-prepare-device-test"></a>

The procedures in this section continue from [the previous section](iot-dc-prepare-device-sw.md) to install the AWS Command Line Interface and the Certificate Authority certificate used to authenticate your connections with AWS IoT Core\.

After you complete this section, you'll know that your Raspberry Pi has the necessary system software to install the AWS IoT Device Client and that it has a working connection to the internet\.

**Required equipment:**
+ Your local development and testing environment from [the previous section](iot-dc-prepare-device-sw.md)
+ The Raspberry Pi that you used in [the previous section](iot-dc-prepare-device-sw.md)
+ The microSD memory card from [the previous section](iot-dc-prepare-device-sw.md)

**Topics**
+ [Install the AWS Command Line Interface](#iot-dc-prepare-device-test-step1)
+ [Configure your AWS account credentials](#iot-dc-prepare-device-test-step2)
+ [Download the Amazon Root CA certificate](#iot-dc-prepare-device-test-step3)
+ [\(Optional\) Save the microSD card image](#iot-dc-prepare-device-test-step4)

## Install the AWS Command Line Interface<a name="iot-dc-prepare-device-test-step1"></a>

This procedure installs the AWS CLI onto your Raspberry Pi\.

If you are using a Raspberry Pi or if you can compile software on your IoT device, perform these steps in the terminal window on your local host computer\. If you must compile software for your IoT device on your local host computer, review the software documentation for your IoT device for information about the libraries it requires\.

**To install the AWS CLI on your Raspberry Pi**

1. Run these commands to download and install the AWS CLI\.

   ```
   export PATH=$PATH:~/.local/bin # configures the path to include the directory with the AWS CLI
   git clone https://github.com/aws/aws-cli.git # download the AWS CLI code from GitHub
   cd aws-cli && git checkout v2 # go to the directory with the repo and checkout version 2
   pip3 install -r requirements.txt # install the prerequisite software
   ```

1. Run this command to install the AWS CLI\. This command can take up to 15 minutes to complete\.

   ```
   pip3 install . # install the AWS CLI 
   ```

1. Run this command to confirm that the correct version of the AWS CLI was installed\.

   ```
   aws --version
   ```

   The version of the AWS CLI should be 2\.2 or later\.

If the AWS CLI displayed its current version, you're ready to continue to [Configure your AWS account credentials](#iot-dc-prepare-device-test-step2)\.

## Configure your AWS account credentials<a name="iot-dc-prepare-device-test-step2"></a>

In this procedure, you'll obtain AWS account credentials and add them for use on your Raspberry Pi\.

**To add your AWS account credentials to your device**

1. Obtain an **Access Key ID** and **Secret Access Key** from your AWS account to authenticate the AWS CLI on your device\. 

   If you’re new to AWS IAM, [ https://aws\.amazon\.com/premiumsupport/knowledge\-center/create\-access\-key/ ](https://aws.amazon.com/premiumsupport/knowledge-center/create-access-key/) describes the process to run in the AWS console to create AWS IAM credentials to use on your device\. 

1. In the terminal window on your local host computer that's connected to your Raspberry Pi\. and with the **Access Key ID** and **Secret Access Key** credentials for your device:

   1. Run the AWS configure app with this command:

      ```
      aws configure
      ```

   1. Enter your credentials and configuration information when prompted:

      ```
      AWS Access Key ID: your Access Key ID
      AWS Secret Access Key: your Secret Access Key
      Default region name: your AWS Region code
      Default output format: json
      ```

1. Run this command to test your device's access to your AWS account and AWS IoT Core endpoint\.

   ```
   aws iot describe-endpoint --endpoint-type iot:Data-ATS
   ```

   It should return your AWS account\-specific AWS IoT data endpoint, such as this example:

   ```
   {
       "endpointAddress": "a3EXAMPLEffp-ats.iot.us-west-2.amazonaws.com"
   }
   ```

If you see your AWS account\-specific AWS IoT data endpoint, your Raspberry Pi has the connectivity and permissions to continue to [Download the Amazon Root CA certificate](#iot-dc-prepare-device-test-step3)\. 

**Important**  
Your AWS account credentials are now stored on the microSD card in your Raspberry Pi\. While this makes future interactions with AWS easy for you and the software you’ll create in these tutorials, they will also be saved and duplicated in any microSD card images you make after this step by default\.  
To protect the security of your AWS account credentials, before you save any more microSD card images, consider erasing the credentials by running `aws configure` again and entering random characters for the **Access Key ID** and **Secret Access Key** to prevent your AWS account credentials from compromised\.  
If you find that you have saved your AWS account credentials inadvertently, you can deactivate them in the AWS IAM console\. 

## Download the Amazon Root CA certificate<a name="iot-dc-prepare-device-test-step3"></a>

This procedure downloads and saves a copy of a certificate of the Amazon Root Certificate Authority \(CA\)\. Downloading this certificate saves it for use in the subsequent tutorials and it also tests your device's connectivity with AWS services\.

**To download and save the Amazon Root CA certificate**

1. Run this command to create a directory for the certificate\.

   ```
   mkdir ~/certs
   ```

1. Run this command to download the Amazon Root CA certificate\.

   ```
   curl -o ~/certs/AmazonRootCA1.pem https://www.amazontrust.com/repository/AmazonRootCA1.pem
   ```

1. Run these commands to set the access to the certificate directory and its file\.

   ```
   chmod 745 ~
   chmod 700 ~/certs
   chmod 644 ~/certs/AmazonRootCA1.pem
   ```

1. Run this command to see the CA certificate file in the new directory\.

   ```
   ls -l ~/certs
   ```

   You should see an entry like this\. The date and time will be different; however, the file size and all other info should be the same as shown here\.

   ```
   -rw-r--r-- 1 pi pi 1188 Oct 28 13:02 AmazonRootCA1.pem
   ```

   If the file size is not `1188`, check the curl command parameters\. You might have downloaded an incorrect file\.

## \(Optional\) Save the microSD card image<a name="iot-dc-prepare-device-test-step4"></a>

At this point, your Raspberry Pi's microSD card has an updated OS and the basic application software loaded\. 

**To save the microSD card image to a file**

1. In the terminal window on your local host computer, clear your AWS credentials\.

   1. Run the AWS configure app with this command:

      ```
      aws configure
      ```

   1. Replace your credentials when prompted\. You can leave **Default region name** and **Default output format** as they are by pressing **Enter**\.

      ```
      AWS Access Key ID [****************YT2H]: XYXYXYXYX
      AWS Secret Access Key [****************9plH]: XYXYXYXYX
      Default region name [us-west-2]: 
      Default output format [json]:
      ```

1. Enter this command to shut down the Raspberry Pi\.

   ```
   sudo shutdown -h 0
   ```

1. After the Raspberry Pi shuts down completely, remove its power connector\.

1. Remove the microSD card from your device\.

1. On your local host computer: 

   1. Insert the microSD card\.

   1. Using your SD card imaging tool, save the microSD card’s image to a file\.

   1. After the microSD card’s image has been saved, eject the card from the local host computer\.

1. With the power disconnected from the Raspberry Pi, insert the microSD card into the Raspberry Pi\.

1. Apply power to the device\.

1. After about a minute, on the local host computer, restart the terminal window session and log in to the device\.

   **Don't reenter your AWS account credentials yet\.**

After you have restarted and logged in to your Raspberry Pi, you're ready to continue to [Tutorial: Installing and configuring the AWS IoT Device Client](iot-dc-install-dc.md)\.