# Create a custom authorizer for AWS IoT Core<a name="custom-auth-tutorial"></a>

This tutorial demonstrates the steps to create, validate, and use Custom Authentication by using the AWS CLI\. Optionally, using this tutorial, you can use Postman to send data to AWS IoT Core by using the HTTP Publish API\.

This tutorial show you how to create a sample Lambda function which implements the authorization and authentication logic and a custom authorizer using the create\-authorizer call with token signing enabled\. The authorizer is then validated using the test\-invoke\-authorizer, and finally you can send data to AWS IoT Core by using the HTTP Publish API to a test MQTT topic\. Sample request will specify the authorizer to invoke by using the `x-amz-customauthorizer-name` header and pass the token\-key\-name and `x-amz-customauthorizer-signature` in request headers\.

**What you'll learn in this tutorial:**
+ How to create a Lambda function to be a custom authorizer handler
+ How to create a custom authorizer using the AWS CLI with token signing enabled
+ How to test your custom authorizer using the test\-invoke\-authorizer command
+ How to publish an MQTT topic by using [Postman](https://www.postman.com/) and validate the request with your custom authorizer

This tutorial takes about 60 minutes to complete\.

**Topics**
+ [Create a Lambda function for your custom authorizer](#custom-auth-tutorial-define)
+ [Create a public and private key pair for your custom authorizer](#custom-auth-tutorial-keys)
+ [Create a customer authorizer resource and its authorization](#custom-auth-tutorial-authorizer)
+ [Test the authorizer by calling test\-invoke\-authorizer](#custom-auth-tutorial-test)
+ [Test publishing MQTT message using Postman](#custom-auth-tutorial-postman)
+ [View messages in MQTT test client](#custom-auth-tutorial-testclient)
+ [Review the results and next steps](#custom-auth-tutorial-review)
+ [Clean up after running this tutorial](#custom-auth-tutorial-cleanup)

**Before you start this tutorial, make sure that you have:**
+ 

**[Set up your AWS account](setting-up.md)**  
You'll need your AWS account and AWS IoT console to complete this tutorial\. 

  The account you use for this tutorial works best when it includes at least these AWS managed policies:
  + [https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/IAMFullAccess$jsonEditor](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/IAMFullAccess$jsonEditor)
  + [https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIoTFullAccess$jsonEditor](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIoTFullAccess$jsonEditor)
  + [https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSLambda_FullAccess$jsonEditor](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSLambda_FullAccess$jsonEditor)
**Important**  
The IAM policies used in this tutorial are more permissive than you should follow in a production implementations\. In a production environment, make sure that your account and resource policies grant only the necessary permissions\.  
When you create IAM policies for production, determine what access users and roles need, and then design the policies that allow them to perform only those tasks\.  
For more information, see [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
+ 

**Installed the AWS CLI**  
For information about how to install the AWS CLI, see [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)\. This tutorial requires AWS CLI version `aws-cli/2.1.3 Python/3.7.4 Darwin/18.7.0 exe/x86_64` or later\.
+ 

**OpenSSL tools**  
The examples in this tutorial use [LibreSSL 2\.6\.5](https://www.libressl.org/)\. You can also use [OpenSSL v1\.1\.1i](https://www.openssl.org/) tools for this tutorial\.
+ 

**Reviewed the [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) overview**  
If you haven't used AWS Lambda before, review [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) and [Getting started with Lambda](https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html) to learn its terms and concepts\.
+ 

**Reviewed how to build requests in Postman**  
For more information, see [Building requests](https://learning.postman.com/docs/sending-requests/requests/)\.
+ 

**Removed custom authorizers from previous tutor**  
Your AWS account can have only a limited number of custom authorizers configured at one time\. For information about how to remove a custom authorizer, see [Clean up after running this tutorial](#custom-auth-tutorial-cleanup)\.

## Create a Lambda function for your custom authorizer<a name="custom-auth-tutorial-define"></a>

Custom authentication in AWS IoT Core uses [authorizer resources](https://docs.aws.amazon.com/iot/latest/apireference/API_AuthorizerDescription.html) that you create to authenticate and authorize clients\. The function you'll create in this section authenticates and authorizes clients as they connect to AWS IoT Core and access AWS IoT resources\.

The Lambda function does the following:
+ If a request comes from test\-invoke\-authorizer, it returns an IAM policy with a `Deny` action\.
+ If a request comes from Passport using HTTP and the `actionToken` parameter has a value of `allow`, it returns an IAM policy with an `Allow` action\. Otherwise, it returns an IAM policy with a `Deny` action\.

**To create the Lambda function for your custom authorizer:**

1. In the [Lambda](https://console.aws.amazon.com/lambda/home#) console, open [Functions](https://console.aws.amazon.com/lambda/home#/functions)\.

1. Choose **Create function**\.

1. Confirm **Author from scratch** is selected\.

1. Under **Basic information**:

   1. In **Function name**, enter **custom\-auth\-function**\.

   1. In **Runtime**, confirm **Node\.js 14\.x** 

1. Choose **Create function**\.

   Lambda creates a Node\.js function and an [execution role](https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html) that grants the function permission to upload logs\. The Lambda function assumes the execution role when you invoke your function and uses the execution role to create credentials for the AWS SDK and to read data from event sources\.

1. To see the function's code and configuration in the [AWS Cloud9 ](https://docs.aws.amazon.com/cloud9/latest/user-guide/welcome.html) editor, choose **custom\-auth\-function** in the designer window, and then choose **index\.js** in the navigation pane of the editor\.

   For scripting languages such as Node\.js, Lambda includes a basic function that returns a success response\. You can use the [AWS Cloud9](https://docs.aws.amazon.com/cloud9/latest/user-guide/welcome.html) editor to edit your function as long as your source code doesn't exceed 3MB\.

1. Replace the **index\.js** code in the editor with the following code:

   ```
   // A simple Lambda function for an authorizer. It demonstrates
   // How to parse a CLI and Http password to generate a response.
   
   exports.handler = function(event, context, callback) {
   
       //Http parameter to initiate allow/deny request
       const HTTP_PARAM_NAME='actionToken';
       const ALLOW_ACTION = 'Allow';
       const DENY_ACTION = 'Deny';
   
       //Event data passed to Lambda function
       var event_str = JSON.stringify(event);
       console.log('Complete event :'+ event_str);
   
       //Read protocolData from the event json passed to Lambda function
       var protocolData = event.protocolData;
       console.log('protocolData value---> ' + protocolData);
   
       //Get the dynamic account ID from function's ARN to be used
       // as full resource for IAM policy
       var ACCOUNT_ID = context.invokedFunctionArn.split(":")[4];
       console.log("ACCOUNT_ID---"+ACCOUNT_ID);
   
       //protocolData data will be undefined if testing is done via CLI.
       // This will help to test the set up.
       if (protocolData === undefined) {
   
           //If CLI testing, pass deny action as this is for testing purpose only.
           console.log('Using the test-invoke-authorizer cli for testing only');
           callback(null, generateAuthResponse(DENY_ACTION,ACCOUNT_ID));
   
       } else{
   
           //Http Testing from Postman
           //Get the query string from the request
           var queryString = event.protocolData.http.queryString;
           console.log('queryString values -- ' + queryString);
           /*         global URLSearchParams       */
           const params = new URLSearchParams(queryString);
           var action = params.get(HTTP_PARAM_NAME);
   
           if(action!=null && action.toLowerCase() === 'allow'){
   
               callback(null, generateAuthResponse(ALLOW_ACTION,ACCOUNT_ID));
   
           }else{
   
               callback(null, generateAuthResponse(DENY_ACTION,ACCOUNT_ID));
   
           }
   
       }
   
   };
   
   // Helper function to generate the authorization IAM response.
   var generateAuthResponse = function(effect,ACCOUNT_ID) {
   
       var full_resource = "arn:aws:iot:us-west-2:"+ACCOUNT_ID+":*";
   
       var authResponse = {};
       authResponse.isAuthenticated = true;
       authResponse.principalId = 'principalId';
   
       var policyDocument = {};
       policyDocument.Version = '2012-10-17';
       policyDocument.Statement = [];
       var statement = {};
       statement.Action = 'iot:*';
       statement.Effect = effect;
       statement.Resource = full_resource;
       policyDocument.Statement[0] = statement;
       authResponse.policyDocuments = [policyDocument];
       authResponse.disconnectAfterInSeconds = 3600;
       authResponse.refreshAfterInSeconds = 600;
   
       console.log('custom auth policy function called from http');
       console.log('authResponse --> ' + JSON.stringify(authResponse));
       console.log(authResponse.policyDocuments[0]);
   
       return authResponse;
   }
   ```

1. Choose **Deploy**\.

1. After **Changes deployed** appears above the editor:

   1. Scroll to the **Function overview** section above the editor\.

   1. Copy the **Function ARN** and save it to use later in this tutorial\.

1. Test your function\.

   1. Choose the **Test** tab\.

   1. Using the default test settings, choose **Invoke**\.

   1. If the test succeeded, in the **Execution results**, open the **Details** view\. You should see the policy document that the function returned\.

      If the test failed or you don't see a policy document, review the code to find and correct the errors\.

## Create a public and private key pair for your custom authorizer<a name="custom-auth-tutorial-keys"></a>

Your custom authorizer requires a public and private key to authenticate it\. The commands in this section use openssl tools to create this key pair\.

**To create the public and private key pair for your custom authorizer:**

1. Create the private key file\.

   ```
   openssl genrsa -out private-key.pem 4096
   ```

1. Verify the private key file you just created\.

   ```
   openssl rsa -check -in private-key.pem -noout
   ```

   If the command doesn't display any errors, the private key file is valid\.

1. Create the public key file\.

   ```
   openssl rsa -in private-key.pem -pubout -out public-key.pem
   ```

1. Verify the public key file\.

   ```
   openssl pkey -inform PEM -pubin -in public-key.pem -noout
   ```

   If the command doesn't display any errors, the public key file is valid\.

## Create a customer authorizer resource and its authorization<a name="custom-auth-tutorial-authorizer"></a>

The AWS IoT custom authorizer is the resource that ties together all the elements created in the previous steps\. In this section, you'll create a custom authorizer resource and give it permission to run the Lambda function you created earlier\. You can create a custom authorizer resource by using the AWS IoT console, the AWS CLI, or the AWS API\. 

For this tutorial, you only need to create one custom authorizer\. This section describes how to create by using the AWS IoT console and the AWS CLI, so you can use the method that is most convenient for you\. There's no difference between the custom authorizer resources created by either method\.

### Create a customer authorizer resource<a name="custom-auth-tutorial-authorizer-resource"></a>

**Choose one of these options to create your custom authorizer resource:**
+ [Create a custom authorizer by using the AWS IoT console](#create-custom-auth-in-console)
+ [Create a custom authorizer using the AWS CLI](#create-custom-auth-in-cli)

**To create a custom authorizer by using the AWS IoT console:**

1. Open the [Custom authorizer page of the AWS IoT console](https://console.aws.amazon.com/iot/home#/authorizerhub), and choose **Create**\.

1. In **Create custom authorizer**:

   1. In **Name your custom authorizer**, enter **my\-new\-authorizer**\.

   1. In **Authorizer function**, choose the Lambda function you created earlier\.

   1. In **Token validation \- optional**:

      1. Check **Enable token signing**\.

      1. In **Token header name \(optional\)**, enter **tokenKeyName**\.

      1. In **Key name**, enter: **FirstKey**\.

      1. In **Value**, enter the contents of the `public-key.pem` file\. Be sure to include the lines from the file with `-----BEGIN PUBLIC KEY-----` and `-----END PUBLIC KEY-----` and don't add or remove any line feeds, carriage returns or other characters from the file contents\. The string that you enter should look something like this example\.

         ```
         -----BEGIN PUBLIC KEY-----
         MIICIjANBgkqhkiG9w0BAQEFAAOCAg8AMIICCgKCAgEAvEBzOk4vhN+3LgslvEWt
         sLCqNmt5Damas3bmiTRvq2gjRJ6KXGTGQChqArAJwL1a9dkS9+maaXC3vc6xzx9z
         QPu/vQOe5tyzz1MsKdmtFGxMqQ3qjEXAMPLEOmqyUKPP5mff58k6ePSfXAnzBH0q
         lg2HioefrpU5OSAnpuRAjYKofKjbc2Vrn6N2G7hV+IfTBvCElf0csalS/Rk4phD5
         oa4Y0GHISRnevypg5C8n9Rrz91PWGqP6M/q5DNJJXjMyleG92hQgu1N696bn5Dw8
         FhedszFa6b2x6xrItZFzewNQkPMLMFhNrQIIyvshtT/F1LVCS5+v8AQ8UGGDfZmv
         QeqAMAF7WgagDMXcfgKSVU8yid2sIm56qsCLMvD2Sq8Lgzpey9N5ON1o1Cvldwvc
         KrJJtgwW6hVqRGuShnownLpgG86M6neZ5sRMbVNZO8OzcobLngJ0Ibw9KkcUdklW
         gvZ6HEJqBY2XE70iEXAMPLETPHzhqvK6Ei1HGxpHsXx6BNft582J1VpgYjXha8oa
         /NN7l7Zbj/euAb41IVtmX8JrD9z613d1iM5L8HluJlUzn62Q+VeNV2tdA7MfPfMC
         8btGYladFAnitThaz6+F0VSBJPu7pZQoLnqyEp5zLMtF+kFl2yOBmGAP0RBivRd9
         JWBUCG0bqcLQPeQyjbXSOfUCAwEAAQ==
         -----END PUBLIC KEY-----
         ```

1. Check **Activate authorizer**\.

1. Choose **Create authorizer**\.

1. If the custom authorizer resource was created, you'll see the list of custom authorizers and your new custom authorizer should appear in the list and you can continue to the next section to test it\.

   If you see an error, review the error and try to create your custom authorizer again and double\-check the entries\. Note that each custom authorizer resource must have a unique name\.

**To create a custom authorizer using the AWS CLI:**

1. Substitute your values for `authorizer-function-arn` and `token-signing-public-keys`, and then run the following command:

   ```
   aws iot create-authorizer \
   --authorizer-name "my-new-authorizer" \
   --token-key-name "tokenKeyName" \
   --status ACTIVE \
   --no-signing-disabled \
   --authorizer-function-arn "arn:aws:lambda:us-west-2:57EXAMPLE833:function:custom-auth-function" \
   --token-signing-public-keys FirstKey="-----BEGIN PUBLIC KEY-----
   MIICIjANBgkqhkiG9w0BAQEFAAOCAg8AMIICCgKCAgEAvEBzOk4vhN+3LgslvEWt
   sLCqNmt5Damas3bmiTRvq2gjRJ6KXGTGQChqArAJwL1a9dkS9+maaXC3vc6xzx9z
   QPu/vQOe5tyzz1MsKdmtFGxMqQ3qjEXAMPLEOmqyUKPP5mff58k6ePSfXAnzBH0q
   lg2HioefrpU5OSAnpuRAjYKofKjbc2Vrn6N2G7hV+IfTBvCElf0csalS/Rk4phD5
   oa4Y0GHISRnevypg5C8n9Rrz91PWGqP6M/q5DNJJXjMyleG92hQgu1N696bn5Dw8
   FhedszFa6b2x6xrItZFzewNQkPMLMFhNrQIIyvshtT/F1LVCS5+v8AQ8UGGDfZmv
   QeqAMAF7WgagDMXcfgKSVU8yid2sIm56qsCLMvD2Sq8Lgzpey9N5ON1o1Cvldwvc
   KrJJtgwW6hVqRGuShnownLpgG86M6neZ5sRMbVNZO8OzcobLngJ0Ibw9KkcUdklW
   gvZ6HEJqBY2XE70iEXAMPLETPHzhqvK6Ei1HGxpHsXx6BNft582J1VpgYjXha8oa
   /NN7l7Zbj/euAb41IVtmX8JrD9z613d1iM5L8HluJlUzn62Q+VeNV2tdA7MfPfMC
   8btGYladFAnitThaz6+F0VSBJPu7pZQoLnqyEp5zLMtF+kFl2yOBmGAP0RBivRd9
   JWBUCG0bqcLQPeQyjbXSOfUCAwEAAQ==
   -----END PUBLIC KEY-----"
   ```

**Where:**
   + The `authorizer-function-arn` value is the Amazon resource name \(ARN\) of the Lambda function you created for your custom authorizer\.
   + The `token-signing-public-keys` value includes the name of the key, **FirstKey**, and the contents of the `public-key.pem` file\. Be sure to include the lines from the file with `-----BEGIN PUBLIC KEY-----` and `-----END PUBLIC KEY-----` and don't add or remove any line feeds, carriage returns, or other characters from the file contents\. 

     Note: be very careful entering the public key as any alteration to the public key value makes it unusable\.

1. If the custom authorizer is created, the command returns the name and ARN of the new resource, such as the following\.

   ```
   {
       "authorizerName": "my-new-authorizer",
       "authorizerArn": "arn:aws:iot:us-west-2:57EXAMPLE833:authorizer/my-new-authorizer"
   }
   ```

   Save the `authorizerArn` value for use in the next step\.

   Remember that each custom authorizer resource must have a unique name\.

### Authorize the custom authorizer resource<a name="custom-auth-tutorial-authorizer-permission"></a>

In this section, you'll grant permission the custom authorizer resource that you just created permission to run the Lambda function

**Grant permission to your Lambda function using the AWS CLI**

1. After inserting your values, enter the following command\. Note that the `statement-id` value must be unique\. Replace *`Id-1234`* with another value if you have run this tutorial before or if you get a `ResourceConflictException` error\.

   ```
   aws lambda add-permission  \
   --function-name "custom-auth-function" \
   --principal "iot.amazonaws.com" \
   --action "lambda:InvokeFunction" \
   --statement-id "Id-1234" \
   --source-arn authorizerArn
   ```

1. If the command succeeds, it returns a permission statement, such as this example\. You can continue to the next section to test the custom authorizer\.

   ```
   {
       "Statement": "{\"Sid\":\"Id-1234\",\"Effect\":\"Allow\",\"Principal\":{\"Service\":\"iot.amazonaws.com\"},\"Action\":\"lambda:InvokeFunction\",\"Resource\":\"arn:aws:lambda:us-west-2:57EXAMPLE833:function:custom-auth-function\",\"Condition\":{\"ArnLike\":{\"AWS:SourceArn\":\"arn:aws:lambda:us-west-2:57EXAMPLE833:function:custom-auth-function\"}}}"
   }
   ```

   If the command doesn't succeed, it returns an error, such as this example\. You'll need to review and correct the error before you continue\.

   ```
   An error occurred (AccessDeniedException) when calling the AddPermission operation: User: arn:aws:iam::57EXAMPLE833:user/EXAMPLE-1 is not authorized to perform: lambda:AddPer
   mission on resource: arn:aws:lambda:us-west-2:57EXAMPLE833:function:custom-auth-function
   ```

## Test the authorizer by calling test\-invoke\-authorizer<a name="custom-auth-tutorial-test"></a>

With all the resources defined, in this section, you'll call test\-invoke\-authorizer from the command line to test the authorization pass\.

Note that when invoking the authorizer from the command line, `protocolData` is not defined, so the authorizer will always return a DENY document\. This test does, however, confirm that your custom authorizer and Lambda function are configured correctly\-\-even if it doesn't fully test the Lambda function\.

**To test your custom authorizer and its Lambda function by using the AWS CLI:**

1. In the directory that has the `private-key.pem` file you created in a previous step, run the following command\.

   ```
   echo -n "tokenKeyValue" | openssl dgst -sha256 -sign private-key.pem | openssl base64 -A
   ```

   This command creates a signature string to use in the next step\. The signature string looks something like this:

   ```
   dBwykzlb+fo+JmSGdwoGr8dyC2qB/IyLefJJr+rbCvmu9Jl4KHAA9DG+V+MMWu09YSA86+64Y3Gt4tOykpZqn9mn
   VB1wyxp+0bDZh8hmqUAUH3fwi3fPjBvCa4cwNuLQNqBZzbCvsluv7i2IMjEg+CPY0zrWt1jr9BikgGPDxWkjaeeh
   bQHHTo357TegKs9pP30Uf4TrxypNmFswA5k7QIc01n4bIyRTm90OyZ94R4bdJsHNig1JePgnuOBvMGCEFE09jGjj
   szEHfgAUAQIWXiVGQj16BU1xKpTGSiTAwheLKUjITOEXAMPLECK3aHKYKY+d1vTvdthKtYHBq8MjhzJ0kggbt29V
   QJCb8RilN/P5+vcVniSXWPplyB5jkYs9UvG08REoy64AtizfUhvSul/r/F3VV8ITtQp3aXiUtcspACi6ca+tsDuX
   f3LzCwQQF/YSUy02u5XkWn+sto6KCkpNlkD0wU8gl3+kOzxrthnQ8gEajd5Iylx230iqcXo3osjPha7JDyWM5o+K
   EWckTe91I1mokDr5sJ4JXixvnJTVSx1li49IalW4en1DAkc1a0s2U2UNm236EXAMPLELotyh7h+flFeloZlAWQFH
   xRlXsPqiVKS1ZIUClaZWprh/orDJplpiWfBgBIOgokJIDGP9gwhXIIk7zWrGmWpMK9o=
   ```

   Copy this signature string to use in the next step\. Be careful not to include any extra characters or leave any out\.

1. In this command, replace the `token-signature` value with the signature string from the previous step and run this command to test your authorizer\.

   ```
   aws iot test-invoke-authorizer \
   --authorizer-name my-new-authorizer \
   --token tokenKeyValue \
   --token-signature dBwykzlb+fo+JmSGdwoGr8dyC2qB/IyLefJJr+rbCvmu9Jl4KHAA9DG+V+MMWu09YSA86+64Y3Gt4tOykpZqn9mnVB1wyxp+0bDZh8hmqUAUH3fwi3fPjBvCa4cwNuLQNqBZzbCvsluv7i2IMjEg+CPY0zrWt1jr9BikgGPDxWkjaeehbQHHTo357TegKs9pP30Uf4TrxypNmFswA5k7QIc01n4bIyRTm90OyZ94R4bdJsHNig1JePgnuOBvMGCEFE09jGjjszEHfgAUAQIWXiVGQj16BU1xKpTGSiTAwheLKUjITOEXAMPLECK3aHKYKY+d1vTvdthKtYHBq8MjhzJ0kggbt29VQJCb8RilN/P5+vcVniSXWPplyB5jkYs9UvG08REoy64AtizfUhvSul/r/F3VV8ITtQp3aXiUtcspACi6ca+tsDuXf3LzCwQQF/YSUy02u5XkWn+sto6KCkpNlkD0wU8gl3+kOzxrthnQ8gEajd5Iylx230iqcXo3osjPha7JDyWM5o+KEWckTe91I1mokDr5sJ4JXixvnJTVSx1li49IalW4en1DAkc1a0s2U2UNm236EXAMPLELotyh7h+flFeloZlAWQFHxRlXsPqiVKS1ZIUClaZWprh/orDJplpiWfBgBIOgokJIDGP9gwhXIIk7zWrGmWpMK9o=
   ```

   If the command is successful, it returns the information generated by your customer authorizer function, such as this example\.

   ```
   {
       "isAuthenticated": true,
       "principalId": "principalId",
       "policyDocuments": [
           "{\"Version\":\"2012-10-17\",\"Statement\":[{\"Action\":\"iot:*\",\"Effect\":\"Deny\",\"Resource\":\"arn:aws:iot:us-west-2:57EXAMPLE833:*\"}]}"
       ],
       "refreshAfterInSeconds": 600,
       "disconnectAfterInSeconds": 3600
   }
   ```

   If the command returns an error, review the error and double\-check the commands you used in this section\.

## Test publishing MQTT message using Postman<a name="custom-auth-tutorial-postman"></a>

1. To get your device data endpoint from the command line, call [describe\-endpoint](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/describe-endpoint.html) as shown here

   ```
   aws iot describe-endpoint --output text --endpoint-type iot:Data-ATS
   ```

   Save this address for use as the *device\_data\_endpoint\_address* in a later step\.

1. Open a new Postman window and create a new HTTP POST request\.

   1. From your computer, open the Postman app\.

   1. In Postman, in the **File** menu, choose **New\.\.\.**\.

   1. In the **New** dialog, choose **Request**\.

   1. In Save request,

      1. In **Request name** enter **Custom authorizer test request**\.

      1. In **Select a collection or folder to save to:** choose or create a collection into which to save this request\.

      1. Choose **Save to *collection\_name***\.

1. Create the POST request to test your custom authorizer\.

   1. In the request method selector next to the URL field, choose **POST**\. 

   1. In the URL field, create the URL for your request by using the following URL with the *device\_data\_endpoint\_address* from the [describe\-endpoint](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/describe-endpoint.html) command in a previous step\.

      ```
      https://device_data_endpoint_address:443/topics/test/cust-auth/topic?qos=0&actionToken=allow
      ```

      Note that this URL includes the `actionToken=allow` query parameter that will tell your Lambda function to return a policy document that allows access to AWS IoT\. After you enter the URL, the query parameters also appear in the **Params** tab of Postman\.

   1. In the **Auth** tab, in the **Type** field, choose **No Auth**\.

   1. In the Headers tab:

      1. If there's a **Host** key that's checked, uncheck this one\.

      1. At the bottom of the list of headers add these new headers and confirm they are checked\. Replace the **Host** value with your *device\_data\_endpoint\_address* and the **x\-amz\-customauthorizer\-signature** value with the signature string that you used with the test\-invoke\-authorize command in the previous section\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/custom-auth-tutorial.html)

   1. In the Body tab:

      1. In the data format option box, choose **Raw**\.

      1. In the data type list, choose **JavaScript**\.

      1. In the text field, enter this JSON message payload for your test message:

         ```
         {
             "data_mode": "test",
             "vibration": 200,
             "temperature": 40
         }
         ```

1. Choose **Send** to send the request\.

   If the request was successful, it returns:

   ```
   {
       "message": "OK",
       "traceId": "ff35c33f-409a-ea90-b06f-fbEXAMPLE25c"
   }
   ```

   The successful response indicates that your custom authorizer allowed the connection to AWS IoT and that the test message was delivered to broker in AWS IoT Core\. 

   If it returns an error, review error message, the *device\_data\_endpoint\_address*, the signature string, and the other header values\.

Keep this request in Postman for use in the next section\.

## View messages in MQTT test client<a name="custom-auth-tutorial-testclient"></a>

In the previous step, you sent simulated device messages to AWS IoT by using Postman\. The successful response indicated that your custom authorizer allowed the connection to AWS IoT and that the test message was delivered to broker in AWS IoT Core\. In this section, you'll use the MQTT test client in the AWS IoT console to see the message contents from that message as other devices and services might\.

**To see the test messages authorized by your custom authorizer:**

1. In the AWS IoT console, open the [MQTT test client](https://console.aws.amazon.com/iot/home#/test)\.

1. In the **Subscribe to topic** tab, in **Topic filter**, enter **test/cust\-auth/topic**, which is the message topic used in the Postman example from the previous section\.

1. Choose **Subscribe**\.

   Keep this window visible for the next step\.

1. In Postman, in the request you created for the previous section, choose **Send**\.

   Review the response to make sure it was successful\. If not, troubleshoot the error as the previous section describes\.

1. In the **MQTT test client**, you should see a new entry that shows the message topic and, if expanded, the message payload from the request you sent from Postman\.

   If you don't see your messages in the **MQTT test client**, here are some things to check:
   + Make sure your Postman request returned successfully\. If AWS IoT rejects the connection and returns an error, the message in the request doesn't get passed to the message broker\.
   + Make sure the AWS account and AWS Region used to open the AWS IoT console are the same as you're using in the Postman URL\.
   + Make sure you've entered the topic correctly in the **MQTT test client**\. The topic filter is case\-sensitive\. If in doubt, you can also subscribe to the **\#** topic, which subscribes to all MQTT messages that pass through the message broker the AWS account and AWS Region used to open the AWS IoT console\.

## Review the results and next steps<a name="custom-auth-tutorial-review"></a>

**In this tutorial:**
+ You created a Lambda function to be a custom authorizer handler
+ You created a custom authorizer with token signing enabled
+ You tested your custom authorizer using the test\-invoke\-authorizer command
+ You published an MQTT topic by using [Postman](https://www.postman.com/) and validate the request with your custom authorizer
+ You used the **MQTT test client** to view the messages sent from your Postman test

**Next steps**  
After you send some messages from Postman to verify the custom authorizer is working, try experimenting to see how changing different aspects of this tutorial affect the results\. Here are some examples to get you started\.
+ Change the signature string so that it's no longer valid to see how unauthorized connection attempts are handled\. You should get an error response, such as this one, and the message should not appear in the **MQTT test client**\. 

  ```
  {
      "message": "Forbidden",
      "traceId": "15969756-a4a4-917c-b47a-5433e25b1356"
  }
  ```
+ To learn more about how to find errors that might occur while you're developing and using AWS IoT rules, see [Monitoring AWS IoT](monitoring_overview.md)\.

## Clean up after running this tutorial<a name="custom-auth-tutorial-cleanup"></a>

If you'd like repeat this tutorial you might need to remove some of your custom authorizers\. Your AWS account can have only a limited number of custom authorizers configured at one time and you can get a `LimitExceededException` when you try to add a new one without removing an existing custom authorizers\.

**To remove a custom authorizer by using the [AWS IoT console](https://console.aws.amazon.com/iot/home#/authorizerhub)**

1. Open the [Custom authorizer page of the AWS IoT console](https://console.aws.amazon.com/iot/home#/authorizerhub), and in the list of custom authorizers, find the custom authorizer to remove\.

1. Open the Custom authorizer details page and, from the **Actions** menu, choose **Edit**\.

1. Uncheck the **Activate authorizer**, and then choose **Update**\.

   You can't delete a custom authorizer while it's active\.

1. From the Custom authorizer details page, open the **Actions** menu, and choose **Delete**\.

**To remove a custom authorizer by using the AWS CLI**

1. List the custom authorizers that you have installed and find the name of the custom authorizer you want to delete\.

   ```
   aws iot list-authorizers 
   ```

1. Set the custom authorizer to `inactive` by running this command after replacing *`Custom_Auth_Name`* with the `authorizerName` of the custom authorizer to delete\.

   ```
   aws iot update-authorizer --status INACTIVE --authorizer-name Custom_Auth_Name
   ```

1. Delete the custom authorizer by running this command after replacing *`Custom_Auth_Name`* with the `authorizerName` of the custom authorizer to delete\.

   ```
   aws iot delete-authorizer --authorizer-name Custom_Auth_Name
   ```