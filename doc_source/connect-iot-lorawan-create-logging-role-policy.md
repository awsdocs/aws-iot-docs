# Create logging role and policy for AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-create-logging-role-policy"></a>

The following shows how to create a logging role for only AWS IoT Core for LoRaWAN resources\. If you want to also create a logging role for AWS IoT Core, see [Create a logging role](configure-logging.md#create-logging-role)\.

## Create a logging role for AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-logging-role"></a>

Before you can enable logging, you must create an IAM role and a policy that gives AWS permission to monitor AWS IoT Core for LoRaWAN activity on your behalf\.

**Create IAM role for logging**  
To create a logging role for AWS IoT Core for LoRaWAN, open the [Roles hub of the IAM console](https://console.aws.amazon.com/iam/home#/roles) and choose **Create role**\.

1. Under **Select type of trusted entity**, choose **Another AWS account**\.

1. In **Account ID**, enter your AWS account ID, and then choose **Next: Permissions**\.

1. In the search box, enter **AWSIoTWirelessLogging**\.

1. Select the box next to the policy named **AWSIoTWirelessLogging**, and then choose **Next: Tags**\.

1. Choose **Next: Review**\.

1. In **Role name**, enter **IoTWirelessLogsRole**, and then choose **Create role**\.

**Edit trust relationship of the IAM role**  
In the confirmation message displayed after you ran the previous step, choose the name of the role you created, **IoTWirelessLogsRole**\. Next, you'll edit the role to add the following trust relationship\.

1. In the **Summary** section of the role **IoTWirelessLogsRole**, choose the **Trust relationships** tab, and then choose **Edit trust relationship**\.

1. In **Policy Document**, change the `Principal` property to look like this example\.

   ```
   "Principal": { 
       "Service": "iotwireless.amazonaws.com" 
   },
   ```

   After you change the `Principal` property, the complete policy document should look like this example\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": "iotwireless.amazonaws.com"
         },
         "Action": "sts:AssumeRole",
         "Condition": {}
       }
     ]
   }
   ```

1. To save your changes and exit, choose **Update Trust Policy**\.

## Logging policy for AWS IoT Core for LoRaWAN<a name="connect-iot-lorawan-logging-policy"></a>

The following policy document provides the role policy and trust policy that allows AWS IoT Core for LoRaWAN to submit log entries to CloudWatch on your behalf\.

**Note**  
This AWS managed policy document was automatically created for you when you created the logging role, **IoTWirelessLogsRole**\.

**Role policy**  
The following shows the role policy document\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:DescribeLogGroups",
                "logs:DescribeLogStreams",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:*:*:log-group:/aws/iotwireless*"
        }
    ]
}
```

**Trust policy to log only AWS IoT Core for LoRaWAN activity**  
The following shows the trust policy for logging only AWS IoT Core for LoRaWAN activity\.

```
{
     "Version": "2012-10-17",
      "Statement": [
        {
          "Sid": "",
          "Effect": "Allow",
          "Principal": {
            "Service": [
            "iotwireless.amazonaws.com"
            ]
          },
          "Action": "sts:AssumeRole"
        }
      ]
    }
```

If you created the IAM role to also log AWS IoT Core activity, then the policy documents allow you to log both activities\. For information about creating a logging role for AWS IoT Core, see [Create a logging role](configure-logging.md#create-logging-role)\.

## Next steps<a name="connect-iot-lorawan-logging-role-next-steps"></a>

You've learned how to create a logging role to log your AWS IoT Core for LoRaWAN resources\. By default, logs have a log level of `ERROR`, so if you want to see only error information, go to [View CloudWatch AWS IoT Core for LoRaWAN log entries](connect-iot-lorawan-cwl-format.md) to monitor your wireless resources by viewing the log entries\.

If you want more information in the log entries, you can configure the default log level for your resources or for different event types, such as setting the log level to `INFO`\. For information about configuring logging for your resources, see [Configure logging for AWS IoT Core for LoRaWAN resources](connect-iot-lorawan-configure-resource-logging.md)\.