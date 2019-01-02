# AWS IoT Button Quickstarts<a name="iot-button-quickstart"></a>

The two quickstarts in this section show you how to configure and use the AWS IoT button\. You can use the AWS IoT button wizard in the AWS Lambda console to easily and quickly configure your AWS IoT button\. The AWS Lambda console contains a blueprint that automates the process of setting up your AWS IoT button by:

+ Creating and activating an X\.509 certificate and private key for authenticating with AWS IoT\. 

+ Walking you through the configuration of your AWS IoT button to connect to your Wi\-Fi network\.

+ Walking you through the copying of your certificate and private key to your AWS IoT button\.

+ Creating and attaching to the certificate an AWS IoT policy that gives the button permission to make calls to AWS IoT\.

+ Creating an AWS IoT rule that invokes a Lambda function when your AWS IoT button is pressed\.

+ Creating an IAM role and policy that allows the Lambda function to send email messages using Amazon SNS\.

+ Creating a Lambda function that sends an email message to the address specified in the Lambda function code\.

 The second quickstart shows you how to use an AWS CloudFormation template to configure the AWS IoT resources required to process the MQTT messages that are sent when the AWS IoT button is pressed\.

![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/iot-button.png)

If you do not have a button, you can purchase one [here](https://www.amazon.com/All-New-AWS-IoT-Button-Generation/dp/B01KW6YCIM/ref=dp_ob_title_def)\. For more information about AWS IoT, see What Is AWS IoT?


+ [Configure Your AWS IoT Button with a Lambda Blueprint](iot-button-lambda.md)
+ [Configure Your AWS IoT Button with a AWS CloudFormation Template](iot-button-cloudformation.md)
+ [Next Steps](#next-steps)

## Next Steps<a name="next-steps"></a>

To learn more about the Lambda blueprint used to set up your button, see [Getting Started with AWS IoT](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/iot-gs.html)\. To learn how to use AWS CloudFormation with the AWS IoT button, see [AWS IoT Button with AWS CloudFormation Quickstart](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/iot-button-cloud-formation.html)\.