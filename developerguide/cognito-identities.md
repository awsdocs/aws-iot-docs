# Amazon Cognito Identities<a name="cognito-identities"></a>

Amazon Cognito Identity allows you to use your own identity provider or other popular identity providers, such as Login with Amazon, Facebook, or Google\. You exchange a token from your identity provider for AWS security credentials\. The credentials represent an IAM role and can be used with AWS IoT\.

AWS IoT extends Amazon Cognito and allows policy attachment to Amazon Cognito identities\. You can attach a policy to an Amazon Cognito identity and give fine\-grained permissions to an individual user of your AWS IoT application\. In this way, you can assign permissions between specific customers and their devices\. For more information, see [Amazon Cognito Identity](http://alpha-docs-aws.amazon.com/cognito/latest/developerguide/cognito-identity.html)\.

![\[App accessing a device with Amazon Cognito Identity\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/device-cognito.png)