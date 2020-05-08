# Monitoring with CloudWatch Logs<a name="cloud-watch-logs"></a>

AWS IoT sends progress events about each message as it passes from your devices through the message broker and rules engine\. To view these logs, you must configure AWS IoT to generate the logs used by CloudWatch\. 

For more information about CloudWatch Logs, see [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatchLogs.html)\. For information about supported AWS IoT CloudWatch Logs, see [CloudWatch log entry format](cwl-format.md)\.

To enable AWS IoT logging, you must create an IAM role, register the role with AWS IoT, and then configure AWS IoT logging\. In the CloudWatch console, CloudWatch logs appear in a log group named **AWSIotLogs**\.

**Note**  
Before you enable AWS IoT logging, make sure you understand the CloudWatch Logs access permissions\. Users with access to CloudWatch Logs can see debugging information from your devices\. For more information, see [Authentication and Access Control for Amazon CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/auth-and-access-control-cw.html)\.

## Create a logging role<a name="create-logging-role"></a>

Use the [IAM console](https://console.aws.amazon.com/iam/) to create a logging role\.

1. From the navigation pane, choose **Roles**, and then choose **Create role**\.

1. Under **Choose the service that will use this role**, choose **AWS Service**\.

1. Under **Select your use case**, choose **IoT**, and then choose **Next: Permissions**\.

1. On the page that displays the policies that are automatically attached to the service role, choose **Next: Tags**\.

1. Choose **Next: Review**\.

1. Enter a name and description for the role, and then choose **Create role**\.

### Logging role policy<a name="logging-role-policy"></a>

The following policy documents provide the role policy and trust policy that allow AWS IoT to submit logs to CloudWatch on your behalf\. 

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

Trust policy:

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

## Log level<a name="log-level"></a>

The log level specifies which types of logs are generated\.

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

## Configure AWS IoT logging<a name="configure-logging"></a>

You can use the AWS IoT console, the [set\-v2\-logging\-options](https://docs.aws.amazon.com/cli/latest/reference/iot/set-v2-logging-options.html) CLI command, or the [SetV2LoggingOptions](https://docs.aws.amazon.com/iot/latest/apireference/API_SetV2LoggingOptions.html) API to enable logging\. The principal used to make the API call must have [Pass role permissions](pass-role.md) for your logging role\. The logging role is passed to [set\-v2\-logging\-options](https://docs.aws.amazon.com/cli/latest/reference/iot/set-v2-logging-options.html) or [SetV2LoggingOptions](https://docs.aws.amazon.com/iot/latest/apireference/API_SetV2LoggingOptions.html) as the `roleARN` parameter\. 

You can configure logging to be global or fine\-grained\. Global logging sets one logging level for all logs no matter what resource triggered the logs\. Fine\-grained logging allows you to set a logging level for a specific resource or set of resources\. Currently, only thing groups are supported\. You can use the AWS IoT console, the CLI, or the API to enable global logging\. You must use the CLI or API to enable fine\-grained logging\.

### Global logging<a name="global-logging"></a>

Use the `set-v2-logging-options` CLI command to set the logging options for your account\. `set-v2-logging-options` takes three arguments:

`--role-arn`  
Your logging role ARN\. The logging role grants AWS IoT permission to write to your logs in CloudWatch Logs\.

`--default-log-level`  
The log level to use\. Valid values are: ERROR, WARN, INFO, DEBUG, or DISABLED

`--disable-all-logs | --no-disable-all-logs`  
When set to true \(`--disable-all-logs`\) disables all logs\. The default \(parameter not used\) is false\.

For example: 

```
aws iot set-v2-logging-options \
    --role-arn arn:aws:iam::<your-aws-account-num>:role/<IoTLoggingRole> \
    --default-log-level <INFO>
```

You can use the `get-v2-logging-options` CLI command to get the current logging options\.

**Note**  
AWS IoT continues to support older commands \(`set-logging-options` and `get-logging-options`\) to set and get global logging on your account\. Be aware that when these commands are used, the resulting logs contain plain\-text, rather than JSON payloads and logging latency is generally higher\. No further improvements will be made to the implementation of these older commands\. We recommend that you use the "v2" versions to configure your logging options and, when possible, change legacy applications that use the older versions\.

**To use the AWS IoT console to configure global logging**

1. Sign in to the AWS IoT console\. For more information, see [Sign in to the AWS IoT console](iot-console-signin.md)\.

1. In the left navigation pane, choose **Settings**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/configure-cloudwatch-logging-1.png)

1. In the **Logs** section of the **Settings** page, choose **Edit**\. The **Logs** section displays your settings for role and level of verbosity\.

1. On the **Configure role setting** page, choose the level of verbosity that describes the level of detail that you want to appear in the CloudWatch logs\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/configure-cloudwatch-logging-2.png)

1. Choose **Select** to specify a role that you created previously, or **Create Role** to create a role to use for logging\.

1. Choose **Update** to save your changes\.

   Review your CloudWatch logs to see if you are satisfied with the level of collected information\. If not, you can always change the logging level later\.

### Fine\-grained logging<a name="fine-grained-logging"></a>

Fine\-grained logging allows you to specify a logging level for a target\. A target is defined by a resource type and a resource name\. Currently, AWS IoT supports thing groups as targets\. Fine\-grained logging allows you to set a logging level for a thing group\. For example, you might have a thing group named "Phones" that contains things that represent different kinds of phones\. You might then create another thing group named "MobilePhones" and make it a child of the "Phones" thing group\. Fine\-grained logging allows you to configure one logging level for all things in the "Phones" group \(and any child groups\) and another logging level for things in the "MobilePhones" group\. In this example, there are two different logging levels assigned to things in the "MobilePhones" group — one from the logging level for the "Phones" thing group and another from the "MobilePhones" thing group — but the logging level specified for the child thing group overrides the logging level specified for the parent thing group\.

Use the `set-v2-logging-options` CLI command to enable fine\-grained logging and set the default logging level\. It takes the following optional arguments: 

\-\-role\-arn  
An IAM role that allows AWS IoT to write to your CloudWatch Logs\. If not specified, AWS IoT uses the logging role associated with your account\. The logging role is associated with your account when it is created\. For more information, see [Create a logging role](#create-logging-role)\.

\-\-default\-log\-level  
The logging level used if not specified\. Valid values are: `DEBUG`, `INFO`, `ERROR`, `WARN`, and `DISABLED`\.

\-\-disable\-all\-logs \| \-\-no\-disable\-all\-logs  
When set to true \(`--disable-all-logs`\), disables all logs\. The default \(parameter not used\) is false\.

The `get-v2-logging-options` CLI command returns the configured IAM logging role, the default logging level, and the `disableAllLogs` value\.

Use the `set-v2-logging-level` CLI command to configure fine\-grained logging for a target\. It takes the following arguments:

\-\-log\-target  
A JSON object that contains the resource type \(field `targetType`\) and name \(field `targetName`\) of the entity for which you are configuring logging\. AWS IoT currently supports `THING_GROUP` for the resource type\. You can configure up to 10 logging targets\.

\-\-log\-level  
The logging level used when generating logs for the specified resource\. Valid values are: `DEBUG`, `INFO`, `ERROR`, `WARN`, and `DISABLED` 

Use the `list-v2-logging-levels` CLI command to get a list of the currently configured fine\-grained logging levels\. Call the `delete-v2-logging-level` CLI command to delete a logging level\. Use the `delete-v2-logging-level` command to delete a fine\-grained logging level\.