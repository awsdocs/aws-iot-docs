# Step 4: Set Up Email Alerts for Low Moisture Readings<a name="iot-plant-step4"></a>

In this step, you set up Amazon Simple Notification Service \(Amazon SNS\) to automatically send an email alert to the houseplant’s owner as a reminder to water it whenever the soil moisture level is too low\.

1. Create an AWS IoT rule to trigger the email alert through Amazon SNS\. To do this, with the [ AWS IoT console](https://console.aws.amazon.com/iot/home) open, in the service navigation pane, choose **Act**\.  
![\[AWS IoT navigation menu with Act highlighted.\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/console-act.png)

1. If a **You don’t have any rules yet** dialog box appears, choose **Create a rule**\. Otherwise, choose **Create**\.

1. On the **Create a rule** page, enter a **Name** for this rule, for example, **MyRPiLowMoistureAlertRule**\. If you use a different name, be sure to substitute it throughout this sample\.

1. For **Description**, provide a meaningful description for this rule, for example, **Sends an alert whenever soil moisture level readings are too low**\.

1. For **Rule query statement**, with **Using SQL version** set to **2016\-03\-23**, in the **Rule query statement** box, enter the following AWS IoT SQL statement as a single line, without any line breaks:

   ```
   SELECT * FROM '$aws/things/MyRPi/shadow/update/accepted' WHERE state.reported.moisture = 'low'
   ```  
![\[Rule query statement with the SQL statement
                highlighted.\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/console-rule-query-statement.png)

   This statement triggers the rule whenever the `moisture` value is reported as `low` for the specified MQTT topic\.
**Important**  
If you named your thing something other than **MyRPi**, be sure to substitute your thing’s name in the preceding AWS IoT SQL statement\. Otherwise, the rule might never be triggered\.

1. For **Set one or more actions**, choose **Add action**\.

1. On the **Select an action** page, choose **Send a message as an SNS push notification**\.  
![\[Select an action page with Send a message as an
                  SNS push notification highlighted.\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/console-select-sns-action.png)

1. Choose **Configure action**\.

1. On the **Configure action** page, for **SNS target**, choose **Create**\. Enter a name for the SNS topic, for example, **MyRPiLowMoistureTopic**, and then choose **Create**\. If you choose to use a different name, be sure to substitute it throughout this sample\.

1. For **Message format**, choose **RAW**\.

1. For **IAM role name**, choose **Create a new role**, and then enter a name for the new role, for example, **MyRPiLowMoistureTopicRole**\. If you choose to use a different name, be sure to substitute it throughout this sample\.

1. Choose **Create a new role**\.

1. For **IAM role name**, choose **MyRPiLowMoistureTopicRole**\.

1. Choose **Add action**\.  
![\[Configure action page with five inputs
                highlighted.\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/console-configure-sns-rule.png)

1. Choose **Create rule**\.

1. Set up Amazon SNS to send the messages through your Amazon SNS topic to your email inbox\. On the AWS navigation bar, choose **Services**\. In the **Find a service by name or feature** box, enter **SNS**, and then press **Enter**\.

1. In the service navigation pane, choose **Topics**\.  
![\[Navigation pane with Topics highlighted.\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/console-sns-topics.png)

1. Select the check box next to **MyRPiLowMoistureTopic**\.  
![\[Topics list with a check box selected for
                  MyRPiLowMoistureTopic.\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/console-sns-choose-topic.png)

1. For **Actions**, choose **Subscribe to topic**\.  
![\[Topics list with the Subscribe to topic action
                selected.\]](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/images/console-sns-subscribe.png)

1. In the **Create subscription** dialog box, for **Protocol**, choose **Email**\.

1. For **Endpoint**, enter your email address\.

1. Choose **Create subscription**\.

1. Watch your inbox for a subscription confirmation email titled **AWS Notification – Subscription Confirmation from no\-reply@sns\.amazonaws\.com**\. When it arrives, open it, and then click the **Confirm subscription** link\. After you click the link, a confirmation page is displayed in your web browser\. You can close this confirmation page\.
**Important**  
Until you confirm this subscription, you won’t receive any email alerts from this Amazon SNS topic, even though AWS IoT might be sending email alerts to it\.