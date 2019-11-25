# Configure a Custom Authorizer<a name="config-custom-auth"></a>

1. Create a Lambda function that implements your authentication/authorization logic \(for example, the `MyAuthorizerFunction` in the following step\)\. The following is an example of what a custom authorizing Lambda function might return:

   ```
   {
           "isAuthenticated": true,
           "principalId": "xxxxxxxx",
           "disconnectAfterInSeconds": 86400,
           "refreshAfterInSeconds": 300,
           "policyDocuments": [
               "{\"Version\":\"2012-10-17\",\"Statement\":[{\"Action\":\"...\",\"Effect\":\"Allow|Deny\",\"Resource\":\"...\"}]}"
           ],
           "context": {
               "username": "johnDoe123",
               "city": "Seattle",
               "country": "USA"
           }
       }
   ```

   The return value of the Lambda function should be similar to this response\. It can be either a JSON serialized or non\-serialized object\.

1. Use the `create-authorizer` API to register a custom authorizer with AWS IoT\.

   ```
   aws iot create-authorizer --authorizer-name MyAuthorizer    
           --authorizer-function-arn arn:aws:lambda:us-west-2:<account_id>:function:MyAuthorizerFunction   // Lambda ARN
           --token-key-name MyAuthorizerToken                               // Key use to extract token from headers 
           --token-signing-public-keys FIRST_KEY=                           // Public key used to verify token signature
       "-----BEGIN PUBLIC KEY-----
       [...insert your public key here...]
       -----END PUBLIC KEY-----" 
           --status ACTIVE                                                  // Authorizer status - must be ACTIVE
           --region us-west-2                                               // AWS region
           --endpoint https://us-west-2.iot.amazonaws.com                   // IoT endpoint
   ```

   You can use the `test-invoke-authorizer` API to test if the custom authorizer Lambda function has been configured correctly, as shown:

   ```
   aws iot test-invoke-authorizer --authorizer-name <NAME_OF_AUTHORIZER> --token <TOKEN_VALUE> --token-signature <TOKEN_SIGNATURE>
   ```
**Note**  
*<TOKEN\_SIGNATURE>* must be signed with the private key of the public\-private key pair uploaded to AWS IoT used in the `create-authorizer` call\. One method of locally creating *<TOKEN\_SIGNATURE>* from a UNIX\-like command line is as follows:  

   ```
   echo -n <TOKEN_VALUE> | openssl dgst -sha256 -sign <PRIVATE_KEY> | openssl base64
   ```
You must trim all newline characters from the result of this command before passing the *<TOKEN\_SIGNATURE>* value to the `test-invoke-authorizer` API\.