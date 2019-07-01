# Configure and Test Rules<a name="config-and-test-rules"></a>

The AWS IoT rules engine listens for incoming MQTT messages that match a rule\. When a matching message is received, the rule takes some action with the data in the MQTT message \(for example, writing data to an Amazon S3 bucket, invoking a Lambda function, or sending a message to an Amazon SNS topic\)\. In this step, you will create and configure a rule to send the data received from a device to an Amazon SNS topic\. Specifically, you will:
+ Create an Amazon SNS topic\.
+ Subscribe to the Amazon SNS topic using a cell phone number\.
+ Create a rule that will send a message to the Amazon SNS topic when a message is received from your device\.
+ Test the rule using the MQTT client\.

## Create an SNS Topic<a name="create-sns-topic"></a>

Use the Amazon SNS console to create an Amazon SNS topic\.

**Note**  
Amazon SNS is not available in all AWS regions\. 

1. Open the [Amazon SNS console](https://console.aws.amazon.com/sns/v2/home)\.

1. On the left pane, choose **Topics**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/sns-dashboard.png)

1. Choose **Create new topic**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/sns-topics.png)

1. Type a topic name and a display name, and then choose **Create topic**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/create-sns-topic.png)
**Note**  
We do not recommend using personally identifiable information in Amazon SNS topic names\.

1. Make a note of the ARN for the topic you just created\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/sns-topic-arn.png)

## Subscribe to an Amazon SNS Topic<a name="subscribe-sns-topic"></a>

To receive SMS messages on your cell phone, subscribe to the Amazon SNS topic\.

1. In the Amazon SNS console, select the check box next to the topic you just created\. From the **Actions** menu, choose **Subscribe to topic**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/sns-subscribe-to-topic.png)

1. On **Create subscription**, from the **Protocol** drop\-down list, choose **SMS**\.

   In the **Endpoint** field, type the phone number of an SMS\-enabled cell phone, and then choose **Create subscription**\.
**Note**  
Enter the phone number using numbers and dashes only\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/create-sns-subscription.png)

The Amazon SNS console will display the following message, but you may not receive a confirmation message\.

```
A confirmation message will be sent to the subscribed endpoint. 
Once the subscription has been confirmed, the endpoint will receive notifications from this topic.
```

## Create a Rule<a name="create-rule"></a>

AWS IoT rules consist of a topic filter, a rule action, and, in most cases, an IAM role\. Messages published on topics that match the topic filter trigger the rule\. The rule action defines which action to take when the rule is triggered\. The IAM role contains one or more IAM policies that determine which AWS services the rule can access\. You can create multiple rules that listen on a single topic\. Likewise, you can create a single rule that is triggered by multiple topics\. The AWS IoT rules engine continuously processes messages published on topics that match the topic filters defined in the rules\. 

In this example, you will create a rule that uses Amazon SNS to send an SMS notification to a cell phone number\.

1. In the AWS IoT console, in the left navigation pane, choose **Act**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/choose-rules.png)

1. On the **Act** page, choose **Create a rule**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/create-a-rule-button.png)

1. On the **Create a rule** page, in the **Name** field, type a name for your rule\.
**Note**  
We do not recommend using personally identifiable information in your rule name\.

   In the **Description** field, type a description for the rule\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/create-a-rule.png)

1. Scroll down to **Rule query statement**\. Choose the latest version from the **Using SQL version** drop\-down list\. In the **Rule query statement** field, type **SELECT \* FROM 'my/topic'**\.

    `SELECT *` specifies that you want to send the entire MQTT message that triggered the rule\. `FROM 'my/topic'` is the topic filter\. The rules engine uses the topic filter to determine which rules to trigger when an MQTT message is received\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/message-source.png)

1. In **Set one or more actions**, choose **Add action**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/rule-add-action.png)

1. On the **Select an action** page, select **Send a message as an SNS push notification**, and then choose **Configure action**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/select-an-action.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/choose-configure-action.png)

1. On the **Configure action** page, under **SNS target**, choose **Select** to expand the SNS topic drop down\. Then choose **Select** next to the Amazon SNS topic you created earlier\. Under **Message format** select **JSON**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/configure-action.png)

1. Now give AWS IoT permission to publish to the Amazon SNS topic on your behalf when the rule is triggered\. Choose **Create a new role**\. Enter a name for your new role in the **IAM role name ** field\. After you have entered the name, choose **Create a new role**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/configure-action-2.png)

1. Under **IAM role name**, choose **Update role** to apply the permissions to the newly created role, choose the newly created role, and choose **Add action**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/configure-action-3.png)

1. On the **Create a Rule** page, choose **Create rule**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/final-create-rule.png)

For more information about creating rules, see [AWS IoT Rules ](https://docs.aws.amazon.com/iot/latest/developerguide/iot-rules.html)\.

## Test the Amazon SNS Rule<a name="test-rule"></a>

You can test your rule by using the AWS IoT MQTT client:

1. In the [AWS IoT console](https://console.aws.amazon.com/iot/home), in the left navigation pane, choose **Test**\.

1. On the MQTT client page, in the **Publish** section, in the **Specify a topic and a message to publish**… field, type **my/topic** or the topic you used in the rule\. In the message payload section, type the following JSON:

   ```
   {
       "default": "Hello, from AWS IoT console",
       "message": "Hello, from AWS IoT console"
   }
   ```

1. Choose **Publish to topic**\. You should receive an Amazon SNS message on your cell phone\.

Congratulations\! You have successfully created and configured a rule that sends data received from a device to an Amazon SNS topic\.

## Next Steps<a name="more-rules-info"></a>

For more information about AWS IoT rules, see [AWS IoT Rule Tutorials ](iot-rules-tutorial.md) and [AWS IoT Rules](iot-rules.md)\.