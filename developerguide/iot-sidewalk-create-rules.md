# Create rules to process Sidewalk device messages<a name="iot-sidewalk-create-rules"></a>

AWS IoT rules can receive the messages from Sidewalk devices and route them to other services\. [AWS IoT Core for LoRaWAN destinations](connect-iot-lorawan-create-destinations.md) associate a Sidewalk device with the rule that processes the device's message data to send to other services\.

You can use an existing rule for your destination\. In this section, we'll create the rule, `SidewalkRule`, that you specified when creating the Sidewalk destination, as described in [Add a destination for your Sidewalk device](iot-sidewalk-add-destination.md)\. When creating the rule, we'll create an AWS Lambda action to republish the message to an AWS IoT topic\.

## Create a Sidewalk destination rule<a name="iot-sidewalk-create-rules-destination"></a>

Navigate to the [ Rules](https://console.aws.amazon.com/iot/home#/create/rule) Hub of the AWS IoT console and perform the following steps\.

1. Choose **Create a rule** to create a new rule for the destination\. 

1. Enter the name **SidewalkRule** for the **Name** and specify an optional **Description** for the rule \(for example, **Sidewalk rule for lambda action to republish a topic**\)\.

1. Change the default query statement to **SELECT \*** so that any actions associated with the rule will be performed\. Keep the SQL version to `2016-03-23`\.

1. Under **Set one or more actions**, choose **Add action**\.

1. For the rule action, choose **Send a message to a Lambda function** and then choose **Configure action**\.

1. You can choose an existing Lambda function or create a new one\. In this example, we'll create a Lambda function\. Choose **Create a new Lambda function**\.

**Create your function using AWS Lambda**  
Choosing **Create a new Lambda function** opens the [Functions](https://console.aws.amazon.com/lambda/home#/create/function) page of the Lambda console\. Perform the following steps\.

1. To create your function, choose **Author from scratch**\.

1. For **Function name**, enter a name \(for example, **Sidewalk\_Handler**\), choose `Python 3.8` as **Runtime**, and then choose **Create function**\.

1. Choose the **lambda\.py** function in the **Code source** section of the console\. 

1. In the function body, delete any code inside the function body, and add a print statement for your Lambda function\. You can also use base64 to decode the `PayloadData` to receive the application data that your device sends to AWS IoT\. The following shows an example Lambda function\. 

   ```
   import json
   import base64
   
   def lambda_handler(event, context):
   
       message = json.dumps(event)
       print (message)
   
       payload_data = base64.b64decode(event["PayloadData"])
       print(payload_data)
       print(int(payload_data,16))
   ```

1. To deploy your function code, choose **deploy**\.

1. Go back to the [Rules](https://console.aws.amazon.com/iot/home#/create/rule) Hub of the console and refresh the page\. Choose the Lambda function that you created and choose **Add action**\.

**Republish a message to an AWS IoT topic**  
You can add a second action to republish a message to an AWS IoT topic from the [Rules](https://console.aws.amazon.com/iot/home#/create/rule) Hub of the console\.

1. Choose **Add action**\.

1. Choose **Republish a message to an AWS IoT topic** and choose **Configure action**\.

1. Enter **project/sensor/observed** for the **Topic** and make sure the **Quality of Service** is set to **0 \- The message is delivered zero or more times**\. 

1. Choose **Create Role**\. Enter **SidewalkRepublishRole** for the role name and choose **Create Role**\.

1. Choose **Add action**\.

   Both actions appear in the **Rules** Hub of the AWS IoT console\.

1. Choose **Create rule**\.

   The rule appears on the **Rules** page that shows the list of rules\.

## Next steps<a name="iot-sidewalk-rules-next-steps"></a>

Now that you've created the destination rule for your Sidewalk device, you can connect your device and observe messages on the topic that you subscribed to\. For more information, see [Connect your Sidewalk device and view uplink metadata format](iot-sidewalk-connect-uplink-metadata.md)\.