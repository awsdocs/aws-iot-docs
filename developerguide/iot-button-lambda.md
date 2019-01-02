# Configure Your AWS IoT Button with a Lambda Blueprint<a name="iot-button-lambda"></a>

The AWS IoT button wizard is a Lambda blueprint, so you must sign in to the AWS Lambda console to use it\. If you do not have an AWS account, you can create one by following these steps\. 

**To create an AWS account**

1. Open the [AWS home page](https://aws.amazon.com/) and choose **Create an AWS Account**\.

1. Follow the online instructions\. Part of the sign\-up procedure involves receiving a phone call and entering a PIN using your phone's keypad\.

**To configure the AWS IoT Button**

1. Sign in to the AWS Management Console and open the [AWS Lambda console](https://console.aws.amazon.com/lambda/home)\.

1. If this is your first time in the AWS Lambda console, you see the following page\. Choose the **Create a function** button\.   
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/get-started-lambda.png)

   If you have used the AWS Lambda console before, you see the following page\. Choose the **Create function** button\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/create-lambda-function-2.png)

1. On the **Create function** page, choose **Blueprints**\. In the filter text box, type **button**\. Choose the **iot\-button\-email** blueprint, and then choose the **Configure** button\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/iot-button-email.png)

1. In **Basic information**, enter a name for your Lambda function and for the role that you want to use to define permissions for the function\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/configure-lambda-func-basic-info.png)

1. In **AWS IoT trigger**, enter the serial number for your device\. You can find the device serial number \(DSN\) on the back of the button\.

   Choose **Generate certificate and keys**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/configure-triggers.png)
**Note**  
You only need to generate a certificate and private key once\. If you generated them previously, you can instead, in a browser, navigate to [http://192\.168\.0\.1/index\.html](http://192.168.0.1/index.html) to configure your button\.

   Use the links on the page to download the device certificate and private key\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/generate-certificate-and-keys.png)

   In step 3 of the on\-screen instructions, choose a link to open a webpage that allows you to connect the AWS IoT button to your network\. Under **Wi\-Fi Configuration**, enter the network ID \(SSID\) and network password for your Wi\-Fi network\. Under **AWS IoT Configuration**, choose the certificate and private key you downloaded earlier\. This copies your certificate and private key to your AWS IoT button\. Select the check box to agree to the AWS IoT button terms and conditions, and then choose the **Configure** button\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/configure-iot-button.png)

   A configuration confirmation page is displayed\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/button-configure-complete.png)

1. Go back to the AWS Lambda console page, and choose **Enable trigger**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/enable-lambda-trigger.png)

   In the Lambda function code, the email address is retrieved from an environment variable\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/lambda-email-function-code.png)

   In **Environment variables**, enter your email address, as shown here\.

   Review the settings for the Lambda function, and then choose **Create function**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/lambda-email-environment-variable.png)

   You should see a page that confirms your Lambda function has been created:  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/function-created.png)

1. To test your Lambda function, choose the **Test** button\. After about a minute, you should receive an email message with `AWS Notification - Subscription Confirmation` in the subject line\. Choose the link in the email message to confirm the subscription to an SNS topic created by the Lambda function\. When AWS IoT receives a message from your button, it sends a message to Amazon SNS\. The Lambda function created a subscription to the Amazon SNS topic using the email address you added in the code\. When Amazon SNS receives a message on this Amazon SNS topic, it forwards the message to your subscribed email address\.

Press your button to send a message to AWS IoT\. The message causes your Lambda rule to be triggered, and then your Lambda function is invoked\. The Lambda function checks if your SNS topic exists\. The Lambda function then sends the contents of the message to the Amazon SNS topic\. Amazon SNS then forwards the message to the email address you specified in the Lambda function code\.