# Step 1: Create the AWS IoT policy<a name="iot-moisture-policy"></a>

Create an AWS IoT policy that allows your Raspberry Pi to connect and send messages to AWS IoT\.

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), if a **Get started** button appears, choose it\. Otherwise, in the navigation pane, expand ** Secure**, and then choose **Policies**\.

1. If a **You don't have any policies yet** dialog box appears, choose **Create a policy**\. Otherwise, choose **Create**\.

1. Enter a name for the AWS IoT policy \(for example, **MoistureSensorPolicy**\)\.

1. In the **Add statements** section, replace the existing policy with the following JSON\. Replace *region* and *account* with your AWS Region and AWS account number\.

   ```
   {
      "Version": "2012-10-17",
      "Statement": [{
            "Effect": "Allow",
            "Action": "iot:Connect",
            "Resource": "arn:aws:iot:region:account:client/RaspberryPi"
         },
         {
            "Effect": "Allow",
            "Action": "iot:Publish",
            "Resource": [
               "arn:aws:iot:region:account:topic/$aws/things/RaspberryPi/shadow/update",
               "arn:aws:iot:region:account:topic/$aws/things/RaspberryPi/shadow/delete",
               "arn:aws:iot:region:account:topic/$aws/things/RaspberryPi/shadow/get"
            ]
         },
         {
            "Effect": "Allow",
            "Action": "iot:Receive",
            "Resource": [
               "arn:aws:iot:region:account:topic/$aws/things/RaspberryPi/shadow/update/accepted",
               "arn:aws:iot:region:account:topic/$aws/things/RaspberryPi/shadow/delete/accepted",
               "arn:aws:iot:region:account:topic/$aws/things/RaspberryPi/shadow/get/accepted",
               "arn:aws:iot:region:account:topic/$aws/things/RaspberryPi/shadow/update/rejected",
               "arn:aws:iot:region:account:topic/$aws/things/RaspberryPi/shadow/delete/rejected"
            ]
         },
         {
            "Effect": "Allow",
            "Action": "iot:Subscribe",
            "Resource": [
               "arn:aws:iot:region:account:topicfilter/$aws/things/RaspberryPi/shadow/update/accepted",
               "arn:aws:iot:region:account:topicfilter/$aws/things/RaspberryPi/shadow/delete/accepted",
               "arn:aws:iot:region:account:topicfilter/$aws/things/RaspberryPi/shadow/get/accepted",
               "arn:aws:iot:region:account:topicfilter/$aws/things/RaspberryPi/shadow/update/rejected",
               "arn:aws:iot:region:account:topicfilter/$aws/things/RaspberryPi/shadow/delete/rejected"
            ]
         },
         {
            "Effect": "Allow",
            "Action": [
               "iot:GetThingShadow",
               "iot:UpdateThingShadow",
               "iot:DeleteThingShadow"
            ],
            "Resource": "arn:aws:iot:region:account:thing/RaspberryPi"
   
         }
      ]
   }
   ```

1. Choose **Create**\.