# Creating and managing custom authorizers<a name="config-custom-auth"></a>

 AWS IoT Core implements custom authentication and authorization schemes by using [authorizer resources](https://docs.aws.amazon.com/iot/latest/apireference/API_AuthorizerDescription.html)\. Each authorizer consists of the following components: 
+  *Name*: A unique user\-defined string that identifies the authorizer\. 
+  *Lambda function ARN*: The Amazon Resource Name \(ARN\) of the Lambda function that implements the authorization and authentication logic\.  
+  *Token key name*: The key name used to extract the token from the HTTP headers, query parameters, or MQTT CONNECT user name in order to perform signature validation\. This value is required if signing is enabled in your authorizer\. 
+  *Signing disabled flag \(optional\)*: A Boolean value that specifies whether to disable the signing requirement on credentials\. This is useful for scenarios where signing the credentials doesn't make sense, such as authentication schemes that use MQTT user name and password\. The default value is `false`, so signing is enabled by default\. 
+  *Token signing public key*: The public key that AWS IoT Core uses to validate the token signature\. Its minimum length is 2,048 bits\. This value is required if signing is enabled in your authorizer\.  

Lambda charges you for the number of times your Lambda function runs and for the amount of time it takes for the code in your function to execute\. For more information about Lambda pricing, see [Lambda Pricing](https://aws.amazon.com/lambda/pricing/)\. For more information about creating Lambda functions, see the [Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/)\.

**Note**  
If you leave signing enabled, you can prevent excessive triggering of your Lambda by unrecognized clients\. Consider this before you disable signing in your authorizer\.

## Defining your Lambda function<a name="custom-auth-lambda"></a>

 When AWS IoT Core invokes your authorizer, it triggers the associated Lambda associated with the authorizer with an event that contains the following JSON object\. The example JSON object contains all of the possible fields\. Any fields that aren't relevant to the connection request aren't included\. 

```
{
    "token" :"aToken",
    "signatureVerified": Boolean, // Indicates whether the device gateway has validated the signature.
    "protocols": ["tls", "http", "mqtt"], // Indicates which protocols to expect for the request.
    "protocolData": {
        "tls" : {
            "serverName": "serverName" // The server name indication (SNI) host_name string.
        },
        "http": {
            "headers": {
                "#{name}": "#{value}"
            },
            "queryString": "?#{name}=#{value}"
        },
        "mqtt": {
            "username": "myUserName",
            "password": "myPassword", // A base64-encoded string.
            "clientId": "myClientId" // Included in the event only when the device sends the value.
        }
    },
    "connectionMetadata": {
        "id": UUID // The connection ID. You can use this for logging.
    },
}
```

 The Lambda function should use this information to authenticate the incoming connection and decide what actions are permitted in the connection\. The function should send a response that contains the following values\. 
+  `isAuthenticated`: A Boolean value that indicates whether the request is authenticated\. 
+  `principalId`: An alphanumeric string that acts as an identifier for the token sent by the custom authorization request\. Its minimum length is 1 character\. Its maximum length is 128 characters\. 
+  `policyDocuments`: A list of JSON\-formatted AWS IoT Core policy documents For more information about creating AWS IoT Core policies, see [AWS IoT Core policies](iot-policies.md)\. The maximum number of policy documents is 10 policy documents\. Each policy document can contain a maximum of 2,048 characters\. 
+  `disconnectAfterInSeconds`: An integer that specifies the maximum duration \(in seconds\) of the connection to the AWS IoT Core gateway\. The minimum value is 300 seconds, and the maximum value is 86,400 seconds\. 
+  `refreshAfterInSeconds`: An integer that specifies the interval between policy refreshes\. When this interval passes, AWS IoT Core invokes the Lambda function to allow for policy refreshes\. The minimum value is 300 seconds, and the maximum value is 86,400 seconds\. 

  The following JSON object contains an example of a response that your Lambda function can send\. 

```
{
"isAuthenticated":true, //A Boolean that determines whether client can connect.
"principalId": "xxxxxxxx",  //A string that identifies the connection in logs.
"disconnectAfterInSeconds": 86400, 
"refreshAfterInSeconds": 300, 
 "policyDocuments": [
      {
        "Version": "2012-10-17",
        "Statement": [
           {
              "Action": "iot:Publish",
              "Effect": "Allow",
              "Resource": "arn:aws:iot:us-east-1:<your_aws_account_id>:topic/customauthtesting"
            }
         ]
       }
    ]
}
```

 The `policyDocument` value must contain a valid AWS IoT Core policy document\. For more information about AWS IoT Core policies, see [AWS IoT Core policies](iot-policies.md)\.  In MQTT over TLS and MQTT over WebSockets connections, AWS IoT Core caches this policy for the interval specified in the value of the `refreshAfterInSeconds` field\. During this interval, AWS IoT Core authorizes actions in an established connection against this cached policy without triggering your Lambda function again\. If failures occur during custom authentication, AWS IoT Core terminates the connection\. AWS IoT Core also terminates the connection if it has been open for longer than the value specified in the `disconnectAfterInSeconds`parameter\. 

 The following JavaScript contains a sample Node\.js Lambda function that looks for a password in the MQTT Connect message with a value of `test` and returns a policy that grants permission to connect to AWS IoT Core with a client named `myClientName` and publish to a topic that contains the same client name\. If it doesn't find the expected password, it returns a policy that denies those two actions\. 

```
// A simple Lambda function for an authorizer. It demonstrates 
// how to parse an MQTT password and generate a response.

exports.handler = function(event, context, callback) { 
    var uname = event.protocolData.mqtt.username;
    var pwd = event.protocolData.mqtt.password;
    var buff = new Buffer(pwd, 'base64');
    var passwd = buff.toString('ascii');
    switch (passwd) { 
        case 'test': 
            callback(null, generateAuthResponse(passwd, 'Allow')); 
        default: 
            callback(null, generateAuthResponse(passwd, 'Deny'));  
    }
};

// Helper function to generate the authorization response.
var generateAuthResponse = function(token, effect) { 
    var authResponse = {}; 
    authResponse.isAuthenticated = true; 
    authResponse.principalId = 'TEST123'; 
    
    var policyDocument = {}; 
    policyDocument.Version = '2012-10-17'; 
    policyDocument.Statement = []; 
    var publishStatement = {}; 
    var connectStatement = {};
    connectStatement.Action = ["iot:Connect"];
    connectStatement.Effect = effect;
    connectStatement.Resource = ["arn:aws:iot:us-east-1:123456789012:client/myClientName"];
    publishStatement.Action = ["iot:Publish"]; 
    publishStatement.Effect = effect; 
    publishStatement.Resource = ["arn:aws:iot:us-east-1:123456789012:topic/telemetry/myClientName"]; 
    policyDocument.Statement[0] = connectStatement;
    policyDocument.Statement[1] = publishStatement; 
    authResponse.policyDocuments = [policyDocument]; 
    authResponse.disconnectAfterInSeconds = 3600; 
    authResponse.refreshAfterInSeconds = 600; 
    
    return authResponse; 
}
```

 This Lambda function returns the following values when it receives the expected value of `test` in the MQTT Connect password field\. 

```
{
  "password": "password",
  "isAuthenticated": true,
  "principalId": "principalId",
  "policyDocuments": [
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Action": "iot:Connect",
          "Effect": "Allow",
          "Resource": "*"
        },
        {
          "Action": "iot:Publish",
          "Effect": "Allow",
          "Resource": "arn:aws:region:accountId:topic/telemetry/${iot:ClientId}"
        },
        {
          "Action": "iot:Subscribe",
          "Effect": "Allow",
          "Resource": "arn:aws:iot:region:accountId:topicfilter/telemetry/${iot:ClientId}"
        },
        {
          "Action": "iot:Receive",
          "Effect": "Allow",
          "Resource": "arn:aws:iot:region:accountId:topic/telemetry/${iot:ClientId}"
        }
      ]
    }
  ],
  "disconnectAfterInSeconds": 3600,
  "refreshAfterInSeconds": 600
}
```

## Creating an authorizer<a name="custom-auth-create-authorizer"></a>

 You can create an authorizer by using the [CreateAuthorizer API](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateAuthorizer.html)\. The following example shows how to do this\. 

```
 aws iot create-authorizer
--authorizer-name MyAuthorizer
--authorizer-function-arn arn:aws:lambda:us-west-2:<account_id>:function:MyAuthorizerFunction  //The ARN of the Lambda function.
[--token-key-name MyAuthorizerToken //The key used to extract the token from headers.
[--token-signing-public-keys FirstKey=
 "-----BEGIN PUBLIC KEY-----
  [...insert your public key here...] 
  -----END PUBLIC KEY-----"
[--status ACTIVE]
[--tags <value>]
[--signing-disabled | --no-signing-disabled]
```

 ​ 

 You can use the `signing-disabled` parameter to opt out of signature validation for each invocation of your authorizer\. We strongly recommend that you do not disable signing unless you have to\. Signature validation protects you against excessive invocations of your Lambda function from unknown devices\. You can't update the `signing-disabled` status of an authorizer after you create it\. To change this behavior, you must create another custom authorizer with a different value for the `signing-disabled` parameter\. 

 Values for the `tokenKeyName` and `tokenSigningPublicKeys` parameters are optional if you have disabled signing\. They are required values if signing is enabled\. 

 After you create your Lambda function and the custom authorizer, you must explicitly grant the AWS IoT Core service permission to invoke the function on your behalf\. You can do this with the following command\. 

 `aws lambda add-permission --function-name <lambda_function_name> --principal iot.amazonaws.com --source-arn <authorizer_arn> --statement-id Id-123 --action "lambda:InvokeFunction"` 

## Testing your authorizers<a name="custom-auth-testing"></a>

 You can use the [TestInvokeAuthorizer](https://docs.aws.amazon.com/iot/latest/apireference/API_TestInvokeAuthorizer.html) API to test the invocation and return values of your authorizer\. This API enables you to specify protocol metadata and test the signature validation in your authorizer\.   

The following tabs show how to use the AWS CLI to test your authorizer\.

------
#### [ Unix\-like ]

```
aws iot test-invoke-authorizer --authorizer-name NAME_OF_AUTHORIZER \
--token TOKEN_VALUE --token-signature TOKEN_SIGNATURE
```

------
#### [ Windows CMD ]

```
aws iot test-invoke-authorizer --authorizer-name NAME_OF_AUTHORIZER ^
--token TOKEN_VALUE --token-signature TOKEN_SIGNATURE
```

------
#### [ Windows PowerShell ]

```
aws iot test-invoke-authorizer --authorizer-name NAME_OF_AUTHORIZER `
--token TOKEN_VALUE --token-signature TOKEN_SIGNATURE
```

------

The value of the `token-signature` parameter is the signed token\. To learn how to obtain this value, see [Signing the token](custom-auth.md#custom-auth-token-signature)\.

If your authorizer takes a user name and password, you can pass this information by using the `--mqtt-context` parameter\. The following tabs show how to use the `TestInvokeAuthorizer` API to send a JSON object that contains a user name, password, and client name to your custom authorizer\.

------
#### [ Unix\-like ]

```
aws iot test-invoke-authorizer --authorizer-name NAME_OF_AUTHORIZER  \
--mqtt-context '{"username": "USER_NAME", "password": "dGVzdA==", "clientId":"CLIENT_NAME"}'
```

------
#### [ Windows CMD ]

```
aws iot test-invoke-authorizer --authorizer-name NAME_OF_AUTHORIZER  ^
--mqtt-context '{"username": "USER_NAME", "password": "dGVzdA==", "clientId":"CLIENT_NAME"}'
```

------
#### [ Windows PowerShell ]

```
aws iot test-invoke-authorizer --authorizer-name NAME_OF_AUTHORIZER  `
--mqtt-context '{"username": "USER_NAME", "password": "dGVzdA==", "clientId":"CLIENT_NAME"}'
```

------

The password must be base64\-encoded\. The following example shows how to encode a password in a Unix\-like environment\.

```
echo -n PASSWORD | base64
```

## Managing custom authorizers<a name="custom-auth-manage"></a>

 You can manage your authorizers by using the following APIs\. 
+ [ListAuthorizers](https://docs.aws.amazon.com/iot/latest/apireference/API_ListAuthorizers.html): Show all authorizers in your account\.
+  [DescribeAuthorizer](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeAuthorizer.html): Displays properties of the specified authorizer\. These values include creation date, last modified date, and other attributes\.
+ [SetDefaultAuthorizer](https://docs.aws.amazon.com/iot/latest/apireference/API_SetDefaultAuthorizer.html): Specifies the default authorizer for your AWS IoT Core data endpoints\. AWS IoT Core uses this authorizer if a device doesn't pass AWS IoT Core credentials and doesn't specify an authorizer\. For more information about using AWS IoT Core credentials, see [Client authentication](client-authentication.md)\.
+ [UpdateAuthorizer](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateAuthorizer.html):  Changes the status, token key name, or public keys for the specified authorizer\.
+  [DeleteAuthorizer](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteAuthorizer.html): Deletes the specified authorizer\. 

**Note**  
 You can't update an authorizer's signing requirement\. This means that you can't disable signing in an existing authorizer that requires it\. You also can't require signing in an existing authorizer that doesn't require it\. 
