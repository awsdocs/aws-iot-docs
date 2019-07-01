# Amazon Cognito Identities<a name="cognito-identities"></a>

Amazon Cognito Identity allows you to use your own identity provider or other popular identity providers, such as Login with Amazon, Facebook, or Google\. You exchange a token from your identity provider for AWS security credentials\. The credentials represent an IAM role and can be used with AWS IoT\.

You can attach an AWS IoT policy to an Amazon Cognito identity using the [AttachPrincipalPolicy](https://docs.aws.amazon.com//iot/latest/apireference/API_AttachPrincipalPolicy.html) API and give fine\-grained permissions to an individual user of your AWS IoT application\. In this way, you can assign permissions between specific customers and their devices\. For more information, see [Amazon Cognito Identity](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-identity.html)\.

![\[App accessing a device with Amazon Cognito Identity\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/device-cognito.png)