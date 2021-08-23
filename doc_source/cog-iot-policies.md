# Authorization with Amazon Cognito identities<a name="cog-iot-policies"></a>

There are two types of Amazon Cognito identities: unauthenticated and authenticated\. When your app supports unauthenticated Amazon Cognito identities, no authentication is performed so you do not know who the user is\. For unauthenticated users, you grant permission by attaching an IAM role to an unauthenticated identity pool\. You should grant access to only those resources you want available to unknown users\.

When your app supports authenticated Amazon Cognito identities, you specify a policy in two places\. You attach an IAM policy to the authenticated Amazon Cognito Identity pool and you attach an AWS IoT Core policy to the Amazon Cognito Identity\. You attach an IAM policy to a Amazon Cognito Identity pool using the Amazon Cognito Identity console when you create a Amazon Cognito Identity pool\. To attach an AWS IoT Core policy to a Amazon Cognito Identity, you must define a Lambda function that calls `AttachPolicy`\.

**Note**  
The context object in your Lambda function contains a value for `context.cognito_identity_id` when you call the function with AWS credentials that you obtain through Amazon Cognito Identity pools\. For more information, see the following\.   
[AWS Lambda context object in Node\.js](https://docs.aws.amazon.com/lambda/latest/dg/nodejs-context.html)
[AWS Lambda context object in Python](https://docs.aws.amazon.com/lambda/latest/dg/python-context.html)
[AWS Lambda context object in Ruby](https://docs.aws.amazon.com/lambda/latest/dg/ruby-context.html)
[AWS Lambda context object in Java](https://docs.aws.amazon.com/lambda/latest/dg/java-context.html)
[AWS Lambda context object in Go](https://docs.aws.amazon.com/lambda/latest/dg/golang-context.html)
[AWS Lambda context object in C\#](https://docs.aws.amazon.com/lambda/latest/dg/csharp-context.html)
[AWS Lambda context object in PowerShell](https://docs.aws.amazon.com/lambda/latest/dg/powershell-context.html)