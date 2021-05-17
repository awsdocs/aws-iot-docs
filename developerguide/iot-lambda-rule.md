# Format a notification by using an AWS Lambda function<a name="iot-lambda-rule"></a>

This tutorial demonstrates how to send MQTT message data to an AWS Lambda action for formatting and sending to another AWS service\. In this tutorial, the AWS Lambda action uses the AWS SDK to send the formatted message to the Amazon SNS topic you created in the tutorial about how to [Send an Amazon SNS notification](iot-sns-rule.md)\.

In the tutorial about how to [Send an Amazon SNS notification](iot-sns-rule.md), the JSON document that resulted from the rule's query statement was sent as the body of the text message\. The result was a text message that looked something like this example:

```
{"device_id":"32","reported_temperature":38,"max_temperature":30}
```

In this tutorial, you'll use an AWS Lambda rule action to call an AWS Lambda function that formats the data from the rule query statement into a friendlier format, such as this example:

```
Device 32 reports a temperature of 38, which exceeds the limit of 30.
```

The AWS Lambda function you'll create in this tutorial formats the message string by using the data from the rule query statement and calls the [SNS publish](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sns.html#SNS.Client.publish) function of the AWS SDK to create the notification\.

**What you'll learn in this tutorial**
+ How to create and test an AWS Lambda function
+ How to use the AWS SDK in an AWS Lambda function to publish an Amazon SNS notification
+ How to use simple SQL queries and functions in a rule query statement
+ How to use the MQTT client to test an AWS IoT rule

This tutorial takes about 45 minutes to complete\.

**Topics**
+ [Create an AWS Lambda function that sends a text message](#iot-lambda-rule-create-lambda)
+ [Create an AWS IoT rule with an AWS Lambda rule action](#iot-lambda-rule-create-rule)
+ [Test the AWS IoT rule and AWS Lambda rule action](#iot-lambda-rule-test-rule)
+ [Review the results and next steps](#iot-lambda-rule-next-steps)

**Before you start this tutorial, make sure that you have:**
+ 

**[Set up your AWS account](setting-up.md)**  
You'll need your AWS account and AWS IoT console to complete this tutorial\.
+ 

**Reviewed [View MQTT messages with the AWS IoT MQTT client](view-mqtt-messages.md)**  
Be sure you can use the MQTT client to subscribe and publish to a topic\. You'll use the MQTT client to test your new rule in this procedure\.
+ 

**Completed the other rules tutorials in this section**  
This tutorial requires the SNS notification topic you created in the tutorial about how to [Send an Amazon SNS notification](iot-sns-rule.md)\. It also assumes that you've completed the other rules\-related tutorials in this section\.
+ 

**Reviewed the [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) overview**  
If you haven't used AWS Lambda before, review [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) and [Getting started with Lambda](https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html) to learn its terms and concepts\.

## Create an AWS Lambda function that sends a text message<a name="iot-lambda-rule-create-lambda"></a>

The AWS Lambda function in this tutorial receives the result of the rule query statement, inserts the elements into a text string, and sends the resulting string to Amazon SNS as the message in a notification\.

Unlike the tutorial about how to [Send an Amazon SNS notification](iot-sns-rule.md), which used an AWS IoT rule action to send the notification, this tutorial sends the notification from the Lambda function by using a function of the AWS SDK\. The actual Amazon SNS notification topic used in this tutorial, however, is the same one that you used in the tutorial about how to [Send an Amazon SNS notification](iot-sns-rule.md)\.

**To create an AWS Lambda function that sends a text message**

1. Create a new AWS Lambda function\.

   1. In the [AWS Lambda console](https://console.aws.amazon.com/lambda/home), choose **Create function**\.

   1. In **Create function**, select **Use a blueprint**\.

      Search for and select the **hello\-world\-python** blueprint, and then choose **Configure**\.

   1. In **Basic information**:

      1. In **Function name**, enter the name of this function, **format\-high\-temp\-notification**\. 

      1. In **Execution role**, choose **Create a new role from AWS policy templates**\.

      1. In Role name, enter the name of the new role, **format\-high\-temp\-notification\-role**\.

      1. In **Policy templates \- *optional***, search for and select **Amazon SNS publish policy**\.

      1. Choose **Create function**\.

1. Modify the blueprint code to format and send an Amazon SNS notification\.

   1. After you created your function, you should see the **format\-high\-temp\-notification** details page\. If you don't, open it from the [Lambda **Functions**](https://console.aws.amazon.com/lambda/home#/functions) page\.

   1. In the **format\-high\-temp\-notification** details page, choose the **Configuration** tab and scroll to the **Function code** panel\.

   1. In the **Function code** window, in the **Environment** pane, choose the Python file, `lambda_function.py`\.

   1. In the **Function code** window, delete all of the original program code from the blueprint and replace it with this code\.

      ```
      import boto3
      #
      #   expects event parameter to contain:
      #   {
      #       "device_id": "32",
      #       "reported_temperature": 38,
      #       "max_temperature": 30,
      #       "notify_topic_arn": "arn:aws:sns:us-east-1:57EXAMPLE833:high_temp_notice"
      #   }
      # 
      #   sends a plain text string to be used in a text message
      #
      #      "Device {0} reports a temperature of {1}, which exceeds the limit of {2}."
      #   
      #   where:
      #       {0} is the device_id value
      #       {1} is the reported_temperature value
      #       {2} is the max_temperature value
      #
      def lambda_handler(event, context):
      
          # Create an SNS client to send notification
          sns = boto3.client('sns')
      
          # Format text message from data
          message_text = "Device {0} reports a temperature of {1}, which exceeds the limit of {2}.".format(
                  str(event['device_id']),
                  str(event['reported_temperature']),
                  str(event['max_temperature'])
              )
      
          # Publish the formatted message
          response = sns.publish(
                  TopicArn = event['notify_topic_arn'],
                  Message = message_text
              )
      
          return response
      ```

   1. Choose **Deploy**\.

1. In a new window, look up the Amazon Resource Name \(ARN\) of your Amazon SNS topic from the tutorial about how to [Send an Amazon SNS notification](iot-sns-rule.md)\.

   1. In a new window, open the [Topics page of the Amazon SNS console](https://console.aws.amazon.com/sns/v3/home#/topics)\. 

   1. In the **Topics** page, find the **high\_temp\_notice** notification topic in the list of Amazon SNS topics\.

   1. Find the **ARN** of the **high\_temp\_notice** notification topic to use in the next step\.

1. Create a test case for your Lambda function\.

   1. In the [Lambda **Functions**](https://console.aws.amazon.com/lambda/home#/functions) page of the console, on the **format\-high\-temp\-notification** details page, choose **Select a test event** in the upper right corner of the page \(even though it looks disabled\), and then choose **Configure test events**\.

   1. In **Configure test event**, choose **Create new test event**\.

   1. In **Event name**, enter **SampleRuleOutput**\.

   1. In the JSON editor below **Event name**, paste this sample JSON document\. This is an example of what your AWS IoT rule will send to the Lambda function\.

      ```
      {
        "device_id": "32",
        "reported_temperature": 38,
        "max_temperature": 30,
        "notify_topic_arn": "arn:aws:sns:us-east-1:57EXAMPLE833:high_temp_notice"
      }
      ```

   1. Refer to the window that has the **ARN** of the **high\_temp\_notice** notification topic and copy the ARN value\.

   1. Replace the `notify_topic_arn` value in the JSON editor with the ARN from your notification topic\.

      Keep this window open so you can use this ARN value again when you create the AWS IoT rule\.

   1. Choose **Create**\.

1. Test the function with sample data\.

   1. In the **format\-high\-temp\-notification** details page, in the upper\-right corner of the page, confirm that **SampleRuleOutput** appears next to the **Test** button\. If it doesn't, choose it from the list of available test events\.

   1. To send the sample rule output message to your function, choose **Test**\.

If the function and the notification both worked, you will get a text message on the phone that subscribed to the notification\.

If you didn't get a text message on the phone, check the result of the operation\. In the **Function code** panel, in the **Execution result** tab, review the response to find any errors that occurred\. Don't continue to the next step until your function can send the notification to your phone\.

## Create an AWS IoT rule with an AWS Lambda rule action<a name="iot-lambda-rule-create-rule"></a>

In this step, you'll use the rule query statement to format the data from the imaginary weather sensor device to send to a Lambda function, which will format and send a text message\.

A sample message payload received from the weather devices looks like this:

```
{
  "temperature": 28,
  "humidity": 80,
  "barometer": 1013,
  "wind": {
    "velocity": 22,
    "bearing": 255
  }
}
```

In this rule, you'll use the rule query statement to create a message payload for the Lambda function that looks like this:

```
{
  "device_id": "32",
  "reported_temperature": 38,
  "max_temperature": 30,
  "notify_topic_arn": "arn:aws:sns:us-east-1:57EXAMPLE833:high_temp_notice"
}
```

This contains all the information the Lambda function needs to format and send the correct text message\.

**To create the AWS IoT rule to call a Lambda function**

1. Open the [**Rules** hub of the AWS IoT console](https://console.aws.amazon.com/iot/home#/rulehub)\.

1. To start creating your new rule in **Rules**, choose **Create**\.

1. In the top part of **Create a rule**:

   1. In **Name**, enter the rule's name, **wx\_friendly\_text**\.

      Remember that a rule name must be unique within your AWS Account and Region, and it can't have any spaces\. We've used an underscore character in this name to separate the two words in the rule's name\.

   1.  In **Description**, describe the rule\. 

      A meaningful description makes it easier to remember what this rule does and why you created it\. The description can be as long as needed, so be as detailed as possible\. 

1. In **Rule query statement** of **Create a rule**:

   1.  In **Using SQL version**, select **2016\-03\-23**\. 

   1. In the **Rule query statement** edit box, enter the statement: 

      ```
      SELECT 
        cast(topic(2) AS DECIMAL) as device_id, 
        temperature as reported_temperature,
        30 as max_temperature,
        'arn:aws:sns:us-east-1:57EXAMPLE833:high_temp_notice' as notify_topic_arn
      FROM 'device/+/data' WHERE temperature > 30
      ```

      This statement:
      + Listens for MQTT messages with a topic that matches the `device/+/data` topic filter and that have a `temperature` value greater than 30\. 
      + Selects the second element from the topic string, converts it to a decimal number, and then assigns it to the `device_id` field\.
      + Selects the value of the `temperature` field from the message payload and assigns it to the `reported_temperature` field\. 
      + Creates a constant value, `30`, to represent the limit value and assigns it to the `max_temperature` field\. 
      + Creates a constant value for the `notify_topic_arn` field\.

   1. Refer to the window that has the **ARN** of the **high\_temp\_notice** notification topic and copy the ARN value\.

   1. Replace the ARN value \(*arn:aws:sns:us\-east\-1:57EXAMPLE833:high\_temp\_notice*\) in the rule query statement editor with the ARN of your notification topic\.

1. In **Set one or more actions**:

   1. To open up the list of rule actions for this rule, choose **Add action**\.

   1. In **Select an action**, choose **Send a message to a Lambda function**\.

   1. To open the selected action's configuration page, at the bottom of the action list, choose **Configure action**\.

1. In **Configure action**:

   1. In **Function name**, choose **Select**\.

   1. Choose **format\-high\-temp\-notification**\.

   1. At the bottom of **Configure action**, choose **Add action**\.

   1. To create the rule, at the bottom of **Create a rule**, choose **Create rule**\.

## Test the AWS IoT rule and AWS Lambda rule action<a name="iot-lambda-rule-test-rule"></a>

To test your new rule, you'll use the MQTT client to publish and subscribe to the MQTT messages used by this rule\.

Open the [MQTT client in the AWS IoT console](https://console.aws.amazon.com/iot/home#/test) in a new window\. Now you can edit the rule without losing the configuration of your MQTT client\. If you leave the MQTT client to go to another page in the console, you'll lose your subscriptions or message logs\.

**To use the MQTT client to test your rule**

1. In the [MQTT client in the AWS IoT console](https://console.aws.amazon.com/iot/home#/test), subscribe to the input topics, in this case, `device/+/data`\.

   1. In the MQTT client, under **Subscriptions**, choose **Subscribe to a topic**\.

   1. In **Subscription topic**, enter the topic of the input topic filter, **device/\+/data**\.

   1. Keep the rest of the fields at their default settings\.

   1. Choose **Subscribe to topic**\.

      In the **Subscriptions** column, under **Publish to a topic**, **device/\+/data** appears\. 

1. Publish a message to the input topic with a specific device ID, **device/32/data**\. You can't publish to MQTT topics that contain wildcard characters\.

   1. In the MQTT client, under **Subscriptions**, choose **Publish to topic**\.

   1. In the **Publish** field, enter the input topic name, **device/32/data**\.

   1. Copy the sample data shown here and, in the edit box below the topic name, paste the sample data\.

      ```
      {
        "temperature": 38,
        "humidity": 80,
        "barometer": 1013,
        "wind": {
          "velocity": 22,
          "bearing": 255
        }
      }
      ```

   1. To publish your MQTT message, choose **Publish to topic**\.

1. Confirm that the text message was sent\.

   1. In the MQTT client, under **Subscriptions**, there is a green dot next to the topic to which you subscribed earlier\.

      The green dot indicates that one or more new messages have been received since the last time you looked at them\.

   1. Under **Subscriptions**, choose **device/\+/data** to check that the message payload matches what you just published and looks like this:

      ```
      {
        "temperature": 38,
        "humidity": 80,
        "barometer": 1013,
        "wind": {
          "velocity": 22,
          "bearing": 255
        }
      }
      ```

   1. Check the phone that you used to subscribe to the SNS topic and confirm the contents of the message payload look like this:

      ```
      Device 32 reports a temperature of 38, which exceeds the limit of 30.
      ```

      If you change the topic ID element in the message topic, remember that casting the `topic(2)` value to a numeric value will only work if that element in the message topic contains only numeric characters\.

1. Try sending an MQTT message in which the temperature does not exceed the limit\.

   1. In the MQTT client, under **Subscriptions**, choose **Publish to topic**\.

   1. In the **Publish** field, enter the input topic name, **device/33/data**\.

   1. Copy the sample data shown here and, in the edit box below the topic name, paste the sample data\.

      ```
      {
        "temperature": 28,
        "humidity": 80,
        "barometer": 1013,
        "wind": {
          "velocity": 22,
          "bearing": 255
        }
      }
      ```

   1. To send your MQTT message, choose **Publish to topic**\.

   You should see the message that you sent in the **device/\+/data** subscription; however, because the temperature value is below the max temperature in the rule query statement, you shouldn't receive a text message\.

   If you don't see the correct behavior, check the troubleshooting tips\.

### Troubleshooting your AWS Lambda rule and notification<a name="iot-lambda-rule-troubleshoot"></a>

Here are some things to check, in case you're not seeing the results you expect\.
+ 

**You got an error banner**  
If an error appeared when you published the input message, correct that error first\. The following steps might help you correct that error\.
+ 

**You don't see the input message in the MQTT client**  
Every time you publish your input message to the `device/32/data` topic, that message should appear in the MQTT client, if you subscribed to the `device/+/data` topic filter as described in the procedure\.

**Things to check**
  + 

**Check the topic filter you subscribed to**  
If you subscribed to the input message topic as described in the procedure, you should see a copy of the input message every time you publish it\.

    If you don't see the message, check the topic name you subscribed to and compare it to the topic to which you published\. Topic names are case sensitive and the topic to which you subscribed must be identical to the topic to which you published the message payload\.
  + 

**Check the message publish function**  
In the MQTT client, under **Subscriptions**, choose **device/\+/data**, check the topic of the publish message, and then choose **Publish to topic**\. You should see the message payload from the edit box below the topic appear in the message list\. 
+ 

**You don't receive an SMS message**  
For your rule to work, it must have the correct policy that authorizes it to receive a message and send an SNS notification, and it must receive the message\.

**Things to check**
  + 

**Check the AWS Region of your MQTT client and the rule that you created**  
The console in which you're running the MQTT client must be in the same AWS Region as the rule you created\.
  + 

**Check that the temperature value in the message payload exceeds the test threshold**  
If the temperature value is less than or equal to 30, as defined in the rule query statement, the rule will not perform any of its actions\.
  + 

**Check the input message topic in the rule query statement**  
For the rule to work, it must receive a message with the topic name that matches the topic filter in the FROM clause of the rule query statement\.

    Check the spelling of the topic filter in the rule query statement with that of the topic in the MQTT client\. Topic names are case sensitive and the message's topic must match the topic filter in the rule query statement\.
  + 

**Check the contents of the input message payload**  
For the rule to work, it must find the data field in the message payload that is declared in the SELECT statement\.

    Check the spelling of the `temperature` field in the rule query statement with that of the message payload in the MQTT client\. Field names are case sensitive and the `temperature` field in the rule query statement must be identical to the `temperature` field in the message payload\.

    Make sure that the JSON document in the message payload is correctly formatted\. If the JSON has any errors, such as a missing comma, the rule will not be able to read it\.
  + 

**Check the Amazon SNS notification**  
In [Create an Amazon SNS topic that sends an SMS text message](iot-sns-rule.md#iot-sns-rule-create-sns-topic), refer to step 3 that describes how to test the Amazon SNS notification and test the notification to make sure the notification works\.
  + 

**Check the Lambda function**  
In [Create an AWS Lambda function that sends a text message](#iot-lambda-rule-create-lambda), refer to step 5 that describes how to test the Lambda function using test data and test the Lambda function\.
  + 

**Check the role being used by the rule**  
The rule action must have permission to receive the original topic and publish the new topic\. 

    The policies that authorize the rule to receive message data and republish it are specific to the topics used\. If you change the topic used to republish the message data, you must update the rule action's role to update its policy to match the current topic\.

    If you suspect this is the problem, edit the Republish rule action and create a new role\. New roles created by the rule action receive the authorizations necessary to perform these actions\.

## Review the results and next steps<a name="iot-lambda-rule-next-steps"></a>

**In this tutorial:**
+ You created an AWS IoT rule to call a Lambda function that sent an Amazon SNS notification that used your customized message payload\.
+ You used a simple SQL query and functions in a rule query statement to create a new message payload for your Lambda function\.
+ You used the MQTT client to test your AWS IoT rule\.

**Next steps**  
After you send a few text messages with this rule, try experimenting with it to see how changing some aspects of the tutorial affect the message and when it's sent\. Here are some ideas to get you started\.
+ Change the *device\_id* in the input message's topic and observe the effect in the text message contents\.
+ Change the fields selected in the rule query statement, update the Lambda function to use them in a new message, and observe the effect in the text message contents\.
+ Change the test in the rule query statement to test for a minimum temperature instead of a maximum temperature\. Update the Lambda function to format a new message and remember to change the name of `max_temperature`\.
+ To learn more about how to find errors that might occur while you're developing and using AWS IoT rules, see [Monitoring AWS IoT](monitoring_overview.md)\.