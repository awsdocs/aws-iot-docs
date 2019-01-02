# Creating an AWS Lambda Rule<a name="iot-lambda-rule"></a>

You can define a rule that calls a Lambda function, passing in data from the MQTT message that triggered the rule\. This allows you to process the incoming message and then call another AWS or third\-party service\.

In this tutorial, we assume you have completed the AWS IoT Getting Started Tutorial in which you create and subscribe to an Amazon SNS topic using your cell phone number\. Now you will create a Lambda function that publishes a message to the Amazon SNS topic you created in the AWS IoT Getting Started Tutorial\. You will also create a Lambda rule that calls the Lambda function, passing in some data from the MQTT message that triggered the rule\. 

In this tutorial, we also assume you are using an AWS IoT button to trigger the Lambda rule\. If you don't have an AWS IoT button, [you can buy one here](https://www.amazon.com/dp/B01C7WE5WM) or you can use an MQTT client to send an MQTT message that triggers the rule\.

## Create the Lambda Function<a name="iot-create-lambda-function"></a>

1. In the [AWS Lambda console](https://console.aws.amazon.com/lambda/home), choose **Get Started Now**\. Or, if you have created a Lambda function before, choose **Create a Lambda function**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/lambda-get-started.png)

1. On the **Create function** page, choose **Blueprints**\. In the **Blueprints** filter field, type **hello\-world** then press **Enter**\. Choose the **hello\-world** blueprint\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/select-hello-world.png)

1. Choose the **Configure** button\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/select-hello-world-b.png)

1. On the **Basic information** page, enter a name for your function\. Under **Role**, choose **Create a custom role**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/create-custom-role.png)

   The IAM console opens, enabling you to create an IAM role that Lambda can assume when executing the Lambda function\. 

   **To edit the role's policy to give it permission to publish to your Amazon SNS topic**

   1. Choose **View Policy Document**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/view-policy-document.png)

   1. Choose **Edit** to edit the role's policy\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/edit-lambda-role.png)

   1. Replace the policy document with the following\.

      ```
      {
          "Version": "2012-10-17",
          "Statement": [
              {
                  "Effect": "Allow",
                  "Action": [
                      "logs:CreateLogGroup",
                      "logs:CreateLogStream",
                      "logs:PutLogEvents"
                  ],
                  "Resource": "arn:aws:logs:*:*:*"
              },
              {
                  "Effect": "Allow",
                  "Action": [
                      "sns:Publish"
                  ],
                  "Resource": "arn:aws:sns:us-east-1:123456789012:MyIoTButtonSNSTopic"
              }
          ]
      }
      ```

      This policy document adds permission to publish to your Amazon SNS topic\.
**Note**  
Replace the value of the second `Resource` with the ARN of the Amazon SNS topic you created previously\.

1. Choose **Allow**\. The IAM console closes\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/allow-sns-publish.png)

1. Return to the Lambda console\. Choose **Create function**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/review-create-function.png)

1. On the function **Configuration** page, under **Add Triggers**, choose **AWS IoT**\.   
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/configure-trigger.png)

1. In the **Configure triggers** section that appears, for **Device Serial Number**, enter your button's device serial number \(DSN\)\. The DSN is printed on the back of your AWS IoT button\. If you have not already generated a certificate and private key for your AWS IoT button, choose **Generate certificate and keys**\. Otherwise, skip to step 10\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/lambda-generate-cert.png)

1. Choose the links to download your certificate PEM and private key\. Save these files in a secure location on your computer\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/lambda-download-cert.png)

   Follow the online instructions to configure your AWS IoT button\.

1. Be sure to select the **Enable trigger** box, and then choose **Add**\. Choose **Save** in the upper\-right corner to save this change\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/lambda-enable-trigger.png)  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/lambda-save-changes.png)

1. On the function **Configuration** page, select the name of your Lambda function\. Scroll down to the **Function code** section of the page\. Replace the existing code with the following\.

   ```
   console.log('Loading function');
       // Load the AWS SDK
       var AWS = require("aws-sdk");
       
       // Set up the code to call when the Lambda function is invoked
       exports.handler = (event, context, callback) => {
           // Load the message passed into the Lambda function into a JSON object 
           var eventText = JSON.stringify(event, null, 2);
           
           // Log a message to the console; you can view this text in the Monitoring tab in the Lambda console or in the CloudWatch Logs console
           console.log("Received event:", eventText);
           
           // Create a string, extracting the click type and serial number from the message sent by the AWS IoT button
           var messageText = "Received  " + event.clickType + " message from button ID: " + event.serialNumber;
           
           // Write the string to the console
           console.log("Message to send: " + messageText);
           
           // Create an SNS object
           var sns = new AWS.SNS();
           
           // Populate the parameters for the publish operation
           // - Message : the text of the message to send
           // - TopicArn : the ARN of the Amazon SNS topic to which you want to publish 
           var params = {
               Message: messageText,
               TopicArn: "arn:aws:sns:us-east-1:123456789012:MyIoTButtonSNSTopic"
            };
            sns.publish(params, context.done);
       };
   ```
**Note**  
Replace the value of `TopicArn` with the ARN of the Amazon SNS topic you created previously\.

   Choose **Save** in the upper\-right corner to save this change\.

## Test Your Lambda Function<a name="test-lambda-function"></a>

1. In the upper\-right corner of the function **Configuration** page, choose **Test**\. If you haven't created a test, the **Configure test event** page appears\. Otherwise, that test will appear as an option in the menu between **Actions** and the **Test** button\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/lambda-configure-test-event.png)

1. Copy and paste the following JSON into the **Configure test event** page, and then choose **Save**\.

   ```
   {
       "serialNumber": "ABCDEFG12345",
       "clickType": "SINGLE",
       "batteryVoltage": "2000 mV"
   }
   ```  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/input-test-event.png)

1. Choose **Test** again to run the test\. In the AWS Lambda console, scroll to the bottom of the page\. The **Log output** section displays the output the Lambda function has written to the console\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/lambda-output-log.png)

## Create a Lambda Rule to Invoke the Lambda Function<a name="iot-create-lambda-rule"></a>

1. In the [AWS IoT console](https://console.aws.amazon.com/iot/home), in the left navigation pane, choose **Act**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/choose-rules.png)

1. On the **Rules** page, choose **Create**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/dashboard-rules.png)

1. Enter a **Name** and **Description** for the rule\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/create-lambda-rule.png)

1. In the **Rule query statement** editor, enter:

   ```
   SELECT * FROM 'iotbutton/+'
   ```  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/lambda-rule-settings-1.png)

1. In **Set one or more actions**, choose **Add action**\.   
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/rule-add-action.png)

1. On the **Select an action** page, choose **Invoke a Lambda function passing the message data**, and then choose **Configure action**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/select-an-action-lambda.png)  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/choose-configure-action.png)

1. In the **Function name**, choose your Lambda function name, and then choose **Add action**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/lambda-rule-add-action.png)

1. Choose **Create rule** to create your Lambda function\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/lambda-rule-create.png)

## Test Your Lambda Rule<a name="iot-test-lambda-rule"></a>

Now that your button is configured and connected to Wi\-Fi and you have configured an Amazon SNS topic, you can press the button to test your Lambda rule\. You should receive an SMS text message on your phone that contains:

+ The serial number of your button\.

+ The type of button press \(SINGLE or DOUBLE\)\.

+ The battery voltage\. 

The message should look like the following\.

```
IOT BUTTON> {
    "serialNumber" : "ABCDEFG12345",
    "clickType" : "SINGLE",
    "batteryVoltage" : "2000 mV"
}
```

If you don't have a button, [you can buy one here](https://www.amazon.com/dp/B01C7WE5WM) or you can use the AWS IoT MQTT client instead:

1. In the [AWS IoT console](https://console.aws.amazon.com/iot/home), choose **Test**\.  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/choose-test.png)

1. On the **MQTT client** page, in the **Publish** section, in **Specify a topic**, enter **iotbutton/ABCDEFG12345**\. 

   In **Payload**, enter the following JSON, and then choose **Publish to topic**\. 

   ```
   {
       "serialNumber" : "ABCDEFG12345",
       "clickType" : "SINGLE",
       "batteryVoltage" : "2000 mV"
   }
   ```  
![\[Image NOT FOUND\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/publish-to-topic.png)

1. You should receive a message on your cell phone\.