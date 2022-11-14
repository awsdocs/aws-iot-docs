# Step 2: Create the AWS IoT thing, certificate, and private key<a name="iot-moisture-create-thing"></a>

Create a thing in the AWS IoT registry to represent your Raspberry Pi\.

1. In the [AWS IoT console](https://console.aws.amazon.com/iot/home), in the navigation pane under **Manage**, choose **All Devices**, and then choose **Things**\.

1. Choose **Create things**\.

1. On the **Creating AWS IoT things** page, choose **Create a single thing**\.

1. On the **Specify thing properties** page, enter a name for your IoT thing \(for example, **RaspberryPi**\), and then choose **Next**\. You can't change the name of a thing after you create it\. To change a thing's name, you must create a new thing, give it the new name, and then delete the old thing\.

1. On the **Configure device certificate** page, choose **Auto-generate a new certificate**\.

1. On the **Attach policies to certificate** page, select the policy you created in the previous step (**MoistureSensorPolicy**, per the example).

1. Choose **Create thing**.

1. Choose the **Download** links to download the certificate, private key, public key, and RSA 2048 root CA certificate\.
**Important**  
This is the only time you can download your certificate and private key\.
