# Create the AWS IoT Thing, Certificate, and Private Key<a name="iot-moisture-create-thing"></a>

Create a thing in the AWS IoT registry to represent your Raspberry Pi\.

1. In the [AWS IoT console](https://console.aws.amazon.com/iot/home), in the navigation pane, choose **Manage**, and then choose **Things**\.

1. If a **You don't have any things yet** dialog box is displayed, choose **Register a thing**\. Otherwise, choose **Create**\.

1. On the **Creating AWS IoT things** page, choose **Create a single thing**\.

1. On the **Add your device to the device registry** page, enter a name for your IoT thing \(for example, **RaspberryPi**\), and then choose **Next**\.

1. On the **Add a certificate for your thing** page, choose **Create certificate**\.

1. Choose the **Download** links to download the certificate, private key, and root CA certificate\.
**Important**  
This is the only time you can download your certificate and private key\.

1. Choose **Activate**\.

1. Choose **Attach a policy**\.

1. For **Add a policy for your thing**, choose **MoistureSensorPolicy**, and then choose **Register Thing**\.