# Configure AWS IoT logging<a name="configure-logging"></a>

You must enable logging by using the AWS IoT console, CLI, or API before you can monitor and log AWS IoT activity\. 

You can enable logging for all of AWS IoT or only specific thing groups\. You can configure AWS IoT logging by using the AWS IoT console, CLI, or API; however, you must use the CLI or API to configure logging for specific thing groups\. 

When considering how to configure your AWS IoT logging, the default logging configuration determines how AWS IoT activity will be logged unless specified otherwise\. Starting out, you might want to obtain detailed logs with a default [log level](#log-level) of `INFO` or `DEBUG`\. After reviewing the initial logs, you can change the default log level to a less verbose level such as `WARN` or `ERROR` and set a more verbose resource\-specific log level on resources that might need more attention\. Log levels can be changed whenever you want\.

## Configure logging role and policy<a name="configure-logging-role-and-policy"></a>

 Before you can enable logging in AWS IoT, you must create an IAM role and a policy that gives AWS permission to monitor AWS IoT activity on your behalf\. 

**Note**  
Before you enable AWS IoT logging, make sure you understand the CloudWatch Logs access permissions\. Users with access to CloudWatch Logs can see debugging information from your devices\. For more information, see [Authentication and Access Control for Amazon CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/auth-and-access-control-cw.html)\.  
If you expect high traffic patterns in AWS IoT Core due to load testing, consider turning off IoT logging to prevent throttling\. If high traffic is detected, our service may disable logging in your account\.

### Create a logging role<a name="create-logging-role"></a>

Use the [IAM console](https://console.aws.amazon.com/iam/) to create a logging role\.

**To create a logging role for AWS IoT Core using the [IAM console](https://console.aws.amazon.com/iam/home#/roles)**

1. Open the [Roles hub of the IAM console](https://console.aws.amazon.com/iam/home#/roles), and then choose **Create role**\.

1. To create a logging role for AWS IoT Core:

   1. Under **Select type of trusted entity**, choose **AWS Service**, **IoT**\.

   1. Under **Select your use case**, choose **IoT**, and then choose **Next: Permissions**\.

   1. On the page that displays the policies that are automatically attached to the service role\.

   1. Choose **Next: Tags**, and then choose **Next: Review**\.

   1. Enter a **Role name** and **Role description** for the role, and then choose **Create role**\.

   1. In the list of **Roles**, find the role you created, open it, and copy the **Role ARN** \(*logging\-role\-arn*\) to use when you [Configure default logging in the AWS IoT \(console\)](#configure-logging-console)\.

1. To create a logging role for AWS IoT Core for LoRaWAN:

   1. Under **Select type of trusted entity**, choose **Another AWS account**\.

   1. In **Account ID**, enter your AWS account ID, and then choose **Next: Permissions**\.

   1. In the search box, enter **AWSIoTWirelessLogging**\.

   1. Select the box next to the policy named **AWSIoTWirelessLogging**, and then choose **Next: Tags**\.

   1. Choose **Next: Review**\.

   1. In **Role name**, enter **IoTWirelessLogsRole**, and then choose **Create role**\.

   1.  In the confirmation message, choose **IoTWirelessLogsRole**\. 

   1.  In the role's **Summary** page, choose the **Trust relationships** tab, and choose **Edit trust relationship**\. 

   1.  Edit the `Principal` entry so that it contains

      ```
      "Service": "iotwireless.amazonaws.com"
      ```

      as this example shows:

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

   1. Choose **Update Trust Policy**\. Logging for AWS IoT Core for LoRaWAN is now configured\.

### Logging role policy<a name="logging-role-policy"></a>

The following policy documents provide the role policy and trust policy that allow AWS IoT, and optionally, AWS IoT Core for LoRaWAN, to submit log entries to CloudWatch on your behalf\. 

**Note**  
These documents were created for you when you created the logging role\.

Role policy:

```
{
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "logs:CreateLogGroup",
                    "logs:CreateLogStream",
                    "logs:PutLogEvents",
                    "logs:PutMetricFilter",
                    "logs:PutRetentionPolicy"
                 ],
                "Resource": [
                    "*"
                ]
            }
        ]
    }
```

Role policy for logging only AWS IoT Core for LoRaWAN activity:

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

Trust policy to log only AWS IoT Core activity:

```
{
     "Version": "2012-10-17",
      "Statement": [
        {
          "Sid": "",
          "Effect": "Allow",
          "Principal": {
            "Service": "iot.amazonaws.com"
          },
          "Action": "sts:AssumeRole"
        }
      ]
    }
```

Trust policy to log AWS IoT Core and AWS IoT Core for LoRaWAN activity:

```
{
     "Version": "2012-10-17",
      "Statement": [
        {
          "Sid": "",
          "Effect": "Allow",
          "Principal": {
            "Service": [
            "iot.amazonaws.com",
            "iotwireless.amazonaws.com"
            ]
          },
          "Action": "sts:AssumeRole"
        }
      ]
    }
```

## Configure default logging in the AWS IoT \(console\)<a name="configure-logging-console"></a>

This section describes how use the AWS IoT console to configure logging for all of AWS IoT\. To configure logging for only specific thing groups, you must use the CLI or API\. For information about configuring logging for specific thing groups, see [Configure resource\-specific logging in AWS IoT \(CLI\)](#fine-logging-cli)\.

**To use the AWS IoT console to configure default logging for all of AWS IoT**

1. Sign in to the AWS IoT console\. For more information, see [Open the AWS IoT console](setting-up.md#iot-console-signin)\.

1. In the left navigation pane, choose **Settings**\. In the **Logs** section of the **Settings** page, choose **Edit**\. 

   The **Logs** section displays the logging role and level of verbosity used by all of AWS IoT\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/configure-cloudwatch-logging-1.png)

1. On the **Configure role setting** page, choose the **Level of verbosity** that describes the [level of detail](#log-level) of the log entries that you want to appear in the CloudWatch logs\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/configure-cloudwatch-logging-2.png)

1. Choose **Select** to specify a role that you created in [Create a logging role](#create-logging-role), or **Create Role** to create a new role to use for logging\.

1. Choose **Update** to save your changes\.

After you've enabled logging, visit [Viewing AWS IoT logs in the CloudWatch console](cloud-watch-logs.md#viewing-logs) to learn more about viewing the log entries\.

## Configure default logging in AWS IoT \(CLI\)<a name="global-logging-cli"></a>

This section describes how to configure global logging for AWS IoT by using the CLI\.

**Note**  
You need the Amazon Resource Name \(ARN\) of the role that you want to use\. If you need to create a role to use for logging, see [Create a logging role](#create-logging-role) before continuing\.  
 The principal used to call the API must have [Pass role permissions](pass-role.md) for your logging role\. 

You can also perform this procedure with the API by using the methods in the AWS API that correspond to the CLI commands shown here\.

**To use the CLI to configure default logging for AWS IoT**

1. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/set-v2-logging-options.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/set-v2-logging-options.html) command to set the logging options for your account\.

   ```
   aws iot set-v2-logging-options \
       --role-arn logging-role-arn \
       --default-log-level log-level
   ```

   where:  
\-\-role\-arn  
The role ARN that grants AWS IoT permission to write to your logs in CloudWatch Logs\.  
\-\-default\-log\-level  
The [log level](#log-level) to use\. Valid values are: `ERROR`, `WARN`, `INFO`, `DEBUG`, or `DISABLED`  
\-\-no\-disable\-all\-logs  
An optional parameter that enables all AWS IoT logging\. Use this parameter to enable logging when it is currently disabled\.  
\-\-disable\-all\-logs  
An optional parameter that disables all AWS IoT logging\. Use this parameter to disable logging when it is currently enabled\.

1. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/get-v2-logging-options.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/get-v2-logging-options.html) command to get your current logging options\.

   ```
   aws iot get-v2-logging-options
   ```

After you've enabled logging, visit [Viewing AWS IoT logs in the CloudWatch console](cloud-watch-logs.md#viewing-logs) to learn more about viewing the log entries\.

**Note**  
AWS IoT continues to support older commands \(set\-logging\-options and get\-logging\-options\) to set and get global logging on your account\. Be aware that when these commands are used, the resulting logs contain plain\-text, rather than JSON payloads and logging latency is generally higher\. No further improvements will be made to the implementation of these older commands\. We recommend that you use the "v2" versions to configure your logging options and, when possible, change legacy applications that use the older versions\.

## Configure resource\-specific logging in AWS IoT \(CLI\)<a name="fine-logging-cli"></a>

This section describes how to configure resource\-specific logging for AWS IoT by using the CLI\. Resource\-specific logging allows you to specify a logging level for a specific [thing group](thing-groups.md)\. 

Thing groups can contain other thing groups to create a hierarchical relationship\. This procedure describes how to configure the logging of a single thing group\. You can apply this procedure to the parent thing group in a hierarchy to configure the logging for all thing groups in the hierarchy\. You can also apply this procedure to a child thing group to override the logging configuration of its parent\.

**Note**  
You need the Amazon Resource Name \(ARN\) of the role you want to use\. If you need to create a role to use for logging, see [Create a logging role](#create-logging-role) before continuing\.  
The principal used to call the API must have [Pass role permissions](pass-role.md) for your logging role\. 

You can also perform this procedure with the API by using the methods in the AWS API that correspond to the CLI commands shown here\. 

**To use the CLI to configure resource\-specific logging for AWS IoT**

1. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/set-v2-logging-options.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/set-v2-logging-options.html) command to set the logging options for your account\.

   ```
   aws iot set-v2-logging-options \
       --role-arn logging-role-arn \
       --default-log-level log-level
   ```

   where:  
\-\-role\-arn  
The role ARN that grants AWS IoT permission to write to your logs in CloudWatch Logs\.  
\-\-default\-log\-level  
The [log level](#log-level) to use\. Valid values are: `ERROR`, `WARN`, `INFO`, `DEBUG`, or `DISABLED`  
\-\-no\-disable\-all\-logs  
An optional parameter that enables all AWS IoT logging\. Use this parameter to enable logging when it is currently disabled\.  
\-\-disable\-all\-logs  
An optional parameter that disables all AWS IoT logging\. Use this parameter to disable logging when it is currently enabled\.

1. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/set-v2-logging-level.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/set-v2-logging-level.html) command to configure resource\-specific logging for a thing group\.

   ```
   aws iot set-v2-logging-level \
                 --log-target targetType=THING_GROUP,targetName=thing_group_name \
                 --log-level log_level
   ```  
\-\-log\-target  
The type and name of the resource for which you are configuring logging\. The `target_type` value must be `THING_GROUP`\. The log\-target parameter value can be text, as shown in the preceding command example, or a JSON string, such as the following example\.  

   ```
   aws iot set-v2-logging-level \
                 --log-target '{"targetType": "THING_GROUP","targetName": "thing_group_name"}' \
                 --log-level log_level
   ```  
\-\-log\-level  
The logging level used when generating logs for the specified resource\. Valid values are: DEBUG, INFO, ERROR, WARN, and DISABLED 

1. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/list-v2-logging-levels.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/list-v2-logging-levels.html) command to list the currently configured logging levels\.

   ```
   aws iot list-v2-logging-levels
   ```

1. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/delete-v2-logging-level.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iot/delete-v2-logging-level.html) command to delete a resource\-specific logging level\.

   ```
   aws iot delete-v2-logging-level \
                 --targetType "THING_GROUP" \
                 --targetName "thing_group_name"
   ```  
\-\-targetType  
The `target_type` value must be `THING_GROUP`  
\-\-targetName  
The name of the thing group for which to remove the logging level\.

After you've enabled logging, visit [Viewing AWS IoT logs in the CloudWatch console](cloud-watch-logs.md#viewing-logs) to learn more about viewing the log entries\.

## Log levels<a name="log-level"></a>

These log levels determine the events that are logged and apply to default and resource\-specific log levels\.

ERROR  
Any error that causes an operation to fail\.  
Logs include ERROR information only\.

WARN  
Anything that can potentially cause inconsistencies in the system, but might not cause the operation to fail\.  
Logs include ERROR and WARN information\.

INFO  
High\-level information about the flow of things\.  
Logs include INFO, ERROR, and WARN information\.

DEBUG  
Information that might be helpful when debugging a problem\.  
Logs include DEBUG, INFO, ERROR, and WARN information\.

DISABLED  
All logging is disabled\.