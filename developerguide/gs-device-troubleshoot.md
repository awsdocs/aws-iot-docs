# Troubleshooting problems with the sample app<a name="gs-device-troubleshoot"></a>

If you encounter an error when you try to run the sample app, here are some things to check\. 

## Check the certificate<a name="gs-device-ts-step-1"></a>

If the certificate is not active, AWS IoT will not accept any connection attempts that use it for authorization\. When creating your certificate, it's easy to overlook the **Activate** button\. Fortunately, you can activate your certificate from the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

**To check your certificate's activation**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot/home), in the left menu, choose **Secure**, and then choose **Certificates**\.

1. In the list of certificates, find the certificate you created for the exercise and check its status in the **Status** column\.

   If you don't remember the certificate's name, check for any that are **Inactive** to see if they might be the one you're using\.

   Choose the certificate in the list to open its detail page\. In the detail page, you can see its **Create date** to help you identify the certificate\.

1. **To activate an inactive certificate**, from the certificate's detail page, choose **Actions** and then choose **Activate**\. 

If you found the correct certificate and its active, but you're still having problems running the sample app, check its policy as the next step describes\.

You can also try to create a new thing and a new certificate by following the steps in [Create a thing object](create-iot-resources.md#create-aws-thing)\. If you create a new thing, you will need to give it new thing name and download the new certificate files to your device\.

## Check the policy attached to the certificate<a name="gs-device-ts-step-2"></a>

Policies authorize actions in AWS IoT\. If the certificate used to connect to AWS IoT does not have a policy, or does not have a policy that allows it to connect, the connection will be refused, even if the certificate is active\.

**To check the policies attached to a certificate**

1. Find the certificate as described in the previous item and open its details page\.

1. In the left menu of the certificate's details page, choose **Policies** to see the policies attached to the certificate

1. If there are no policies attached to the certificate, add one by choosing the **Actions** menu, and then choosing **Attach policy**\. 

   Choose the policy that you created earlier in [Create AWS IoT resources](create-iot-resources.md)\.

1. If there is a policy attached, choose the policy tile to open its details page\.

   In the details page, review the **Policy document** to make sure it contains the same information as the one you created in [Create an AWS IoT policy](create-iot-resources.md#create-iot-policy)\.

## Check the command line<a name="gs-device-ts-step-3"></a>

Make sure you used the correct command line for your system\. The commands used on Linux/macOS systems are often different from those used on Windows systems\.

## Check the endpoint address<a name="gs-device-ts-step-4"></a>

Review the command you entered and double\-check the endpoint address in your command to the one in your [AWS IoT console](https://console.aws.amazon.com/iot/home)\.

## Check the filenames of the certificate files<a name="gs-device-ts-step-5"></a>

Compare the filenames in the command you entered to the filenames of the certificate files in the `certs` directory\.

Some systems might require the filenames to be in quotes to work correctly\.

## Check the SDK installation<a name="gs-device-ts-step-6"></a>

Make sure that your SDK installation is complete and correct\.

If in doubt, reinstall the SDK on your device\. In most cases, that's a matter of finding the section of the tutorial titled **Install the AWS IoT Device SDK for **SDK language**** and following the procedure again\.

If you are using the **AWS IoT Device SDK for JavaScript**, remember to install the sample apps before you try to run them\. Installing the SDK doesn't automatically install the sample apps\. The sample apps must be installed manually after the SDK has been installed\.