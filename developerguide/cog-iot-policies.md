# Authorization with Amazon Cognito identities<a name="cog-iot-policies"></a>

There are two types of Amazon Cognito identities: unauthenticated and authenticated\. If your app supports unauthenticated Amazon Cognito identities, no authentication is performed so you don't know who the user is\. For these users, you grant permission by attaching an IAM role to an unauthenticated identity pool\. We recommend you only grant access to those resources you want available to unknown users\.

When your app supports authenticated Amazon Cognito identities, in order to authenticate users, you need to specify a policy in two places\. Attach an IAM policy to the authenticated Amazon Cognito Identity pool and attach an AWS IoT Core policy to the Amazon Cognito Identity\.

Authenticated and unauthenticated users are different identity types\. If you don't attach an AWS IoT policy to the Amazon Cognito Identity, an authenticated user fails authorization in AWS IoT and doesn't have access to AWS IoT resources and actions\. For more information about creating policies for Amazon Cognito identities, see [Publish/Subscribe policy examples](pub-sub-policy.md)\.

The following example web applications on GitHub show how to incorporate policy attachment to authenticated users into the user signup and authentication process\.
+ [MQTT publish/subscribe React web application using AWS Amplify and the AWS IoT Device SDK for JavaScript](https://github.com/aws-samples/aws-amplify-react-iot-pub-sub-using-cp)
+ [MQTT publish/subscribe React web application using AWS Amplify, the AWS IoT Device SDK for JavaScript, and a Lambda function](https://github.com/aws-samples/aws-amplify-react-iot-pub-sub-using-lambda)

Amplify is a set of tools and services that help you build web and mobile applications that integrate with AWS services\. For more information about Amplify, see [Amplify Framework Documentation,](https://docs.amplify.aws/)\.

Both examples perform the following steps\.

1. When a user signs up for an account, the application creates an Amazon Cognito user pool and identity\.

1. When a user authenticates, the application creates and attaches a policy to the identity\. This gives the user publish and subscribe permissions\.

1. The user can use the application to publish and subscribe to MQTT topics\.

The first example uses the `AttachPolicy` API directly inside the authentication operation\. The following example demonstrates how to implement this API inside a React web application that uses Amplify and the AWS IoT Device SDK for JavaScript\.

```
function attachPolicy(id, policyName) {
    var Iot = new AWS.Iot({region: AWSConfiguration.region, apiVersion: AWSConfiguration.apiVersion, endpoint: AWSConfiguration.endpoint});
    var params = {policyName: policyName, target: id};

    console.log("Attach IoT Policy: " + policyName + " with cognito identity id: " + id);
    Iot.attachPolicy(params, function(err, data) {
         if (err) {
               if (err.code !== 'ResourceAlreadyExistsException') {
                  console.log(err);
               }
          }
         else  {
            console.log("Successfully attached policy with the identity", data);
         }
     });
}
```

This code appears in the [AuthDisplay\.js](https://github.com/aws-samples/aws-amplify-react-iot-pub-sub-using-cp/blob/d1c307b36357be934db9dda020140fa337709cd9/src/AuthDisplay.js#L45) file\.

The second example implements the `AttachPolicy` API in a Lambda function\. The following example shows how the Lambda uses this API\.

```
iot.attachPolicy(params, function(err, data) {
     if (err) {
           if (err.code !== 'ResourceAlreadyExistsException') {
              console.log(err);
              res.json({error: err, url: req.url, body: req.body});
           }
      }
     else  {
        console.log(data);
        res.json({success: 'Create and attach policy call succeed!', url: req.url, body: req.body});
     }
 });
```

This code appears inside the `iot.GetPolicy` function in the [app\.js](https://github.com/aws-samples/aws-amplify-react-iot-pub-sub-using-lambda/blob/e493039581d2aff0faa3949086deead20a2c5385/amplify/backend/function/amplifyiotlambda/src/app.js#L50) file\.

**Note**  
When you call the function with AWS credentials that you obtain through Amazon Cognito Identity pools, the context object in your Lambda function contains a value for `context.cognito_identity_id`\. For more information, see the following\.   
[AWS Lambda context object in Node\.js](https://docs.aws.amazon.com/lambda/latest/dg/nodejs-context.html)
[AWS Lambda context object in Python](https://docs.aws.amazon.com/lambda/latest/dg/python-context.html)
[AWS Lambda context object in Ruby](https://docs.aws.amazon.com/lambda/latest/dg/ruby-context.html)
[AWS Lambda context object in Java](https://docs.aws.amazon.com/lambda/latest/dg/java-context.html)
[AWS Lambda context object in Go](https://docs.aws.amazon.com/lambda/latest/dg/golang-context.html)
[AWS Lambda context object in C\#](https://docs.aws.amazon.com/lambda/latest/dg/csharp-context.html)
[AWS Lambda context object in PowerShell](https://docs.aws.amazon.com/lambda/latest/dg/powershell-context.html)