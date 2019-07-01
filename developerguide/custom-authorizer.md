# Custom Authorizers<a name="custom-authorizer"></a>

Custom authorizers consist of:

Name  
A unique arbitrary string that identifies the authorizer\.

Lambda function ARN  
An ARN of a Lambda function that implements the authentication logic and returns authorization policies\.

Public key  
The public key from a key pair that is used to prevent unauthorized calls to the authorizer's Lambda function\.   
Use the following command to generate a key pair:  

```
openssl genrsa
                                -out myKeyPair.pem 2048
```
Use the following command to extract the public key from the key pair:  

```
openssl rsa -in myKeyPair.pem
                                -pubout > mykey.pub
```

Token key name  
The key name used to extract tokens from the WebSocket connection headers\.

The logic that performs the authentication is implemented in a Lambda function\.

**Note**  
AWS Lambda usage is [billed](https://aws.amazon.com/lambda/pricing/)\. For more information about Lambda, see [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/)\.

This function takes a token presented by a device, authenticates the device, and returns the following information:

isAuthenticated  
A Boolean value that indicates whether the token was authenticated\. If this is `false`, the rest of the response fields should be ignored\. 

principalId  
The principal that is getting this permission\.

policyDocuments  
A list of policies that specifies which operations the token bearer can perform\.

DisconnectAfterInSecs  
The length of time, in seconds, to keep the WebSocket connection open\.

RefreshAfterInSecs  
The length of time, in seconds, after which the Lambda function is invoked to refresh the policies\.

Context  
Additional information derived after validating the token\. This information is made available in [AWS IoT rules engine SQL statements](https://docs.aws.amazon.com/iot/latest/developerguide/iot-substitution-templates.html) and [IAM/AWS IoT policy variables](https://docs.aws.amazon.com/iot/latest/developerguide/policy-variables.html)\. 

You must grant permission to the AWS IoT service principal to invoke the Lambda function that implements the custom authentication/authorization logic\. You can do this with the following CLI command:

```
aws lambda add-permission --function-name <lambda_function_name>
    --statement-id <unique_identifier_string>
    --action 'lambda:InvokeFunction' 
    --principal iot.amazonaws.com 
    --source-arn arn:aws:iot:<your-aws-region>:<account_id>:authorizer/<authorizer-name>
```

`function-name`  
The name of the Lambda function to which you are granting invocation permission\.

`statement-id`  
A statement identifier

`action`  
The Lambda action you are granting permission to perform\.

`principal`  
The principal to which you are granting permission\.

`source-arn`  
The ARN of the custom authorizer, specifying this value ensures your Lambda function can only be invoked by the intended custom authorizer\.

For more information about granting permission to call Lambda functions, see [AWS Lambda Permissions](https://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html#intro-permission-model-access-policy)\.

You can set a default authorizer that is used when authorizer information is not included in a connection request:

```
aws iot set-default-authorizer --authorizer-name <my-authorizer>
```