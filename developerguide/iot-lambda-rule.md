# Creating a rule with an AWS Lambda action<a name="iot-lambda-rule"></a>

You can define a rule that calls a Lambda function, passing in data from the MQTT message that initiated the rule\. This allows you to extract data from the incoming message and then call another AWS or third\-party service\. In this tutorial, we assume you have completed the [AWS IoT Getting Started Tutorial](iot-gs.md) and the [Creating an Amazon SNS rule](iot-sns-rule.md) tutorial in which you create and subscribe to an Amazon SNS topic\. Now you create a Lambda function that publishes a message to the Amazon SNS topic you created in the [Creating an Amazon SNS rule](iot-sns-rule.md) tutorial\. You also create a Lambda rule that calls the Lambda function, passing in some data from the MQTT message that initiated the rule\.

In this tutorial, you use the AWS IoT MQTT client to send a message that initiated the rule\.

## Create a Lambda function<a name="create-lambda-function"></a>

1. In the [AWS Lambda console](https://console.aws.amazon.com/lambda/home), choose **Create function**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-get-started.png)

1. On the **Create function** page, choose **Use a blueprint**\. In the **Blueprints** filter field, enter **hello\-world**, and then press **Enter**\. Choose the **hello\-world\-python** blueprint, and then choose **Configure**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/select-hello-world.png)

1. In **Basic information**, enter a name for your function\.
**Note**  
We do not recommend the use of personally identifiable information in rule names or descriptions\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-basic-info.png)

1. From **Execution Role**, choose **Create a new role from AWS policy templates**\. Enter a name for the role\. From **Policy templates**, choose **Amazon SNS publish policy**\. Click outside of the drop\-down menu to dismiss it\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/enter-role-name.png)

1. Choose **Create function**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/click-create-function.png)

1. In the AWS Lambda console, choose the name of your Lambda function\. Information about your Lambda function is displayed\. Scroll down to the **Function code** section and replace the existing code with the following example\.

   ```
   from __future__ import print_function
     
   import json
   import boto3
     
   print('Loading function')
     
   def lambda_handler(event, context):
     
       # Parse the JSON message 
       eventText = json.dumps(event)
     
       # Print the parsed JSON message to the console. You can view this text in the Monitoring tab in the AWS Lambda console or in the Amazon CloudWatch Logs console.
       print('Received event: ', eventText)
     
       # Create an SNS client
       sns = boto3.client('sns')
     
       # Publish a message to the specified topic
       response = sns.publish (
         TopicArn = 'arn:aws:iam::123456789012:My_IoT_SNS_Topic',
         Message = eventText
       )
     
       print(response)
   ```

1. Replace the value of `TopicArn` with the ARN of the Amazon SNS topic that you created in [Creating an Amazon SNS rule](iot-sns-rule.md)\.

1. Choose **Save**\.  
![\[Edit your Lambda function to specify the SNS topic.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/save-lambda-button.png)

## Test your Lambda function<a name="test-lambda-function"></a>

1. In the upper right of the Lambda function details page, from **Select a test event**, choose **Configure test events**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/click-config-test-events.png)

1. On **Configure test event**, enter a name for your test event and replace the message JSON with the following:

   ```
   { 
     "message" : "Hello, world"
   }
   ```

   Choose **Create**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/create-test-event.png)

1. In the upper right of the Lambda function details page, choose **Test** to test your Lambda function with the message you specified in the test event\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/click-test-button.png)

1. Under your Lambda function code, on the **Execution result** tab, you see the output from the Lambda function\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-results.png)

## Create a rule with a Lambda action<a name="create-lambda-rule"></a>

This section provides steps for creating a rule with a Lambda action and an error action\. The Lambda action calls your Lambda function\. If an error occurs when calling the Lambda function, the error action publishes a message to the `lambda/error` MQTT topic\. This is useful when you are testing the rule\.

1. Browse to the AWS IoT console, and from the navigation pane, choose **Act**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/choose-act.png)

1. Choose **Create** to create an AWS IoT rule\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-create-rule.png)

1. On the **Create a rule** page, enter a name for your rule\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-enter-rule-name.png)

1. In **Rule query statement**, enter the following query:

   ```
   SELECT * FROM "my/lambda/topic"
   ```  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-enter-query.png)

1. In **Set one or more actions**, choose **Add action**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-add-action.png)

1. Under **Select an action**, choose **Send a message to a Lambda function**, and then choose **Configure action**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-select-action.png)

1. On **Configure action**, choose **Select**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-configure-action-1.png)

1. Choose your Lambda function\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-configure-action-2.png)

1. Choose **Add action**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-configure-action-3.png)

1. On the **Create a rule** page, in **Error action**, choose **Add action**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-configure-error.png)

1. On **Set an action as error action**, choose **Republish a message to an AWS IoT topic**, and then choose **Configure action**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-configure-error-2.png)

1. On **Configure action**, under **Topic**, enter `lambda/error`\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-configure-error-3.png)

1. Under **Choose or create a role to grant AWS IoT access to perform this action**, choose **Create Role**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-configure-error-4.png)

1. In **Create a new role**, enter a name for the role, and then choose **Create role**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-configure-error-5.png)

1. In **Configure action**, choose **Add action**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-configure-error-6.png)

1. In **Create a rule**, choose **Create rule**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-configure-error-7.png)

## Test your rule with a Lambda action<a name="iot-test-lambda-rule"></a>

1. To test your Lambda action, open the AWS IoT console, and from the navigation pane, choose **Test**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-mqtt-client.png)

1. In the MQTT client, under **Subscription topic**, enter `lambda/error`, and then choose **Subscribe to topic**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-mqtt-client-2.png)

1. Under **Publish**, enter `my/lambda/topic`, and then choose **Publish to topic** to publish the default JSON message\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/lambda-mqtt-client-3.png)

Publishing this message should initiate the rule and call your Lambda function\. Your Lambda function pushes an Amazon SNS message to a phone number subscribed to your Amazon SNS topic\. If you do not get a text message, in the MQTT client, check to see if any messages were published to `lambda/error`\.

## Troubleshooting a rule with a Lambda action<a name="troubleshooting-lambda-rules"></a>

If your Lambda function is called, but you do not receive a text message, make sure that your phone number is subscribed to your Amazon SNS topic\. If your phone number is subscribed, check the CloudWatch logs for your Lambda function\. AWS Lambda writes logs to CloudWatch, which makes it possible for you to see output from your Lambda function\.

**To view CloudWatch logs**

1. In the Lambda console, from the navigation pane, choose **Functions**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/choose-functions-for-cwl.png)

1. Choose your Lambda function\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/select-lambda-function.png)

1. On the Lambda function details page, choose the **Monitoring** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/choose-monitor-tab.png)

1. Choose **View logs in CloudWatch**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/view-cloud-watch-logs.png)

1. Choose the latest log stream\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/select-cloudwatch-log.png)

1. The log stream displays the logs written by your Lambda function\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/cloudwatch-log.png)