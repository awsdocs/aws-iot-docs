# Troubleshooting your authorizers<a name="custom-auth-troubleshooting"></a>

 This topic walks through common issues that can cause problems in custom authentication workflows and steps for resolving them\. To troubleshoot issues most effectively, enable CloudWatch logs for AWS IoT Core and set the log level to **DEBUG**\. You can enable CloudWatch logs in the AWS IoT Core console \([https://console\.aws\.amazon\.com/iot/](https://console.aws.amazon.com/iot/)\)\. For more information about enabling and configuring logs for AWS IoT Core, see [Configure AWS IoT logging](configure-logging.md)\. 

**Note**  
If you leave the log level at **DEBUG** for long periods of time, CloudWatch might store large amounts of logging data\. This can increase your CloudWatch charges\. Consider using resource\-based logging to increase the verbosity for only devices in a particular thing group\. For more information about resource\-based logging, see [Configure AWS IoT logging](configure-logging.md)\. Also, when you're done troubleshooting, reduce the log level to a less verbose level\.

Before you start troubleshooting, review [Understanding the custom authentication workflow](custom-authorizer.md) for a high\-level view of the custom authentication process\. This helps you understand where to look for the source of a problem\.

This topic discusses the following two areas for you to investigate\.
+ Issues related to your authorizer's Lambda function\.
+ Issues related to your device\.

## Check for issues in your authorizer’s Lambda function<a name="custom-auth-troubleshooting-lambda"></a>

Perform the following steps to make sure that your devices’ connection attempts are invoking your Lambda function\.

1. Verify which Lambda function is associated with your authorizer\.

   You can do this by calling the [DescribeAuthorizer](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeAuthorizer.html) API or by clicking on the desired authorizer in the **Secure** section of the AWS IoT Core console\.

1. Check the invocation metrics for the Lambda function\. Perform the following steps to do this\.

   1. Open the AWS Lambda console \([https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\) and select the function that is associated with your authorizer\.

   1. Choose the **Monitor** tab and view metrics for the time frame that is relevant to your problem\.

1. If you see no invocations, verify that AWS IoT Core has permission to invoke your Lambda function\. If you see invocations, skip to the next step\. Perform the following steps to verify that your Lambda function has the required permissions\.

   1. Choose the **Permissions** tab for your function in the AWS Lambda console\.

   1. Find the **Resource\-based Policy** section at the bottom of the page\. If your Lambda function has the required permissions, the policy looks like the following example\.

      ```
      {
        "Version": "2012-10-17",
        "Id": "default",
        "Statement": [
          {
            "Sid": "Id123",
            "Effect": "Allow",
            "Principal": {
              "Service": "iot.amazonaws.com"
            },
            "Action": "lambda:InvokeFunction",
            "Resource": "arn:aws:lambda:us-east-1:111111111111:function:FunctionName",
            "Condition": {
              "ArnLike": {
                "AWS:SourceArn": "arn:aws:iot:us-east-1:111111111111:authorizer/AuthorizerName"
              },
              "StringEquals": {
                "AWS:SourceAccount": "111111111111"
              }
            }
          }
        ]
      }
      ```

   1. This policy grants the `InvokeFunction` permission on your function to the AWS IoT Core principal\. If you don't see it, you'll have to add it by using the [AddPermission](https://docs.aws.amazon.com/lambda/latest/dg/API_AddPermission.html) API\. The following example shows you how to do this by using the AWS CLI\.

      ```
        aws lambda add-permission --function-name FunctionName --principal iot.amazonaws.com --source-arn AuthorizerARn --statement-id Id-123 --action "lambda:InvokeFunction"
      ```

1. If you see invocations, verify that there are no errors\. An error might indicate that the Lambda function isn't properly handling the connection event that AWS IoT Core sends to it\.

   For information about handling the event in your Lambda function, see [Defining your Lambda function](config-custom-auth.md#custom-auth-lambda)\. You can use the test feature in the AWS Lambda console \([https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\) to hard\-code test values in the function to make sure that the function is handling events correctly\.

1. If you see invocations with no errors, but your devices are not able to connect \(or publish, subscribe, and receive messages\), the issue might be that the policy that your Lambda function returns doesn't give permissions for the actions that your devices are trying to take\. Perform the following steps to determine whether anything is wrong with the policy that the function returns\.

   1. Use an Amazon CloudWatch Logs Insights query to scan logs over a short period of time to check for failures\. The following example query sorts events by timestamp and looks for failures\.

      ```
      display clientId, eventType, status, @timestamp | sort @timestamp desc | filter status = "Failure"    
      ```

   1. Update your Lambda function to log the data that it's returning to AWS IoT Core and the event that triggers the function\. You can use these logs to inspect the policy that the function creates\.

## Investigating device issues<a name="custom-auth-troubleshooting-investigate"></a>

If you find no issues with invoking your Lambda function or with the policy that the function returns, look for problems with your devices' connection attempts\. Malformed connection requests can cause AWS IoT Core not to trigger your authorizer\. Connection problems can occur at both the TLS and application layers\.

**Possible TLS layer issues:**
+ Customers must pass either a hostname header \(HTTP, MQTT over WebSockets\) or the Server Name Indication TLS extension \(HTTP, MQTT over WebSockets, MQTT\) in all custom authentication requests\. In both cases, the value passed must match one of your account’s AWS IoT Core data endpoints\. These are the endpoints that are returned when you perform the following CLI commands\.
  + `aws iot describe-endpoint —endpoint type iot:data-ats`
  + `aws iot describe-endpoint —endpoint type` \(for legacy VeriSign endpoints\)
+ Devices that use custom authentication for MQTT connections must also pass the Application Layer Protocol Negotiation \(ALPN\) TLS extension with a value of `mqtt`\.
+ Custom authentication is currently available only on port 443\.

**Possible application layer issues:**
+ If signing is enabled \(the `signingDisabled` field is false in your authorizer\), look for the following signature issues\.
  + Make sure that you're passing the token signature in either the `x-amz-customauthorizer-signature`header or in a query string parameter\.
  + Make sure that the service isn't signing a value other than the token\.
  + Make sure that you pass the token in the header or query parameter that you specified in the `token-key-name` field in your authorizer\.
+ Make sure that the authorizer name you pass in the `x-amz-customauthorizer-name` header or query string parameter is valid or that you have a default authorizer defined for your account\.