# Setting Up CloudWatch Logs with AWS IoT<a name="cloud-watch-logs"></a>

AWS IoT sends progress events about each message as it passes from your devices through the message broker and the rules engine\. To view these logs, you must configure AWS IoT to generate the logs used by CloudWatch\. 

For more information about CloudWatch Logs in general, see [CloudWatch Logs](http://alpha-docs-aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatchLogs.html)\. For information on supported AWS IoT CloudWatch Logs, see [CloudWatch Log Entry Format](#cwl-format) below\.

To enable AWS IoT logging, you must create an IAM role, register the role with AWS IoT, and then configure AWS IoT logging\.

**Note**  
Before you enable AWS IoT logging, make sure you understand the CloudWatch Logs access permissions\. Users with access to CloudWatch Logs can see debugging information from your devices\. For more information see [Authentication and Access Control for Amazon CloudWatch Logs](http://alpha-docs-aws.amazon.com/AmazonCloudWatch/latest/monitoring/auth-and-access-control-cw.html)\. 

## Create a Logging Role<a name="create-logging-role"></a>

Use the [IAM console ](https://console.aws.amazon.com/iam/) to create a logging role\.

1. From the navigation pane, choose **Roles**, and then choose **Create new role**\.

1. Choose **AWS Service Role** and for service role type, choose **AWS IoT**\.

1. Choose the **AWSIoTLogging** role, and then choose **Next Step**\.

1. Type a name and description for the role, and then choose **Create role**\.

### Logging Role Policy<a name="logging-role-policy"></a>

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

## Log Level<a name="log-level"></a>

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

## Configure AWS IoT Logging<a name="configure-logging"></a>

You can configure logging in two ways, global logging and fine grained logging\. Global logging sets one logging level for all logs no matter what resource triggered the logs\. Fine\-grained logging allows you to set a logging level for a specific resource or set of resources\. Currently only thing groups are supported\.

### Global Logging<a name="global-logging"></a>

Use the `set-v2-logging-options` CLI command to set the logging options for your account\. `set-v2-logging-options` takes three arguments:

`--role-arn`  
Your logging role ARN\. The logging role grants AWS IoT permission to write to your CloudWatch Logs\.

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
AWS IoT continues to support older commands to set and get global logging on your account: `set-logging-options` and `get-logging-options`\. Be aware that when these commands are used, the resulting logs will contain plain\-text, rather than JSON payloads and logging latency will generally be higher\. Also, no further improvements will be made to the implementation of these older commands\. We recommend using the "v2" versions to configure your logging options and, when possible, changing any legacy applications which use the older versions\.

### Fine\-Grained Logging<a name="fine-grained-logging"></a>

Fine\-grained logging allows you to specify a logging level for a target\. A target is defined by a resource type and a resource name\. Currently, AWS IoT supports thing groups as targets\. Fine\-grained logging allows you to set a logging level for a specific thing group\. Say we have a thing group called "Phones" that contains things that represent different kinds of phones\. We then create another thing group called "MobilePhones" and make it a child of the "Phones" thing group\. Fine\-grained logging allows you to configure one logging level for all things in the "Phones" group \(and any child groups\) and another logging level for things in the "MobilePhones" group\. In this example, we have assigned two different logging levels to things in the "MobilePhones" group — one from the logging level for the "Phones" thing group and another from the "MobilePhones" thing group — but the logging level specified for the child thing group will override the logging level specified for the parent thing group\.

Use the `set-v2-logging-options` CLI command to enable fine\-grained logging and set the default logging level\. It takes the following optional arguments: 

\-\-role\-arn  
An IAM role that allows AWS IoT to write to your CloudWatch Logs\. If not specified, AWS IoT uses the logging role associated with your account\. The logging role is associated with your account when it is created\. For more information, see [Create a Logging Role](#create-logging-role)

\-\-default\-log\-level  
The logging level used if not specified\. Valid values are: `DEBUG`, `INFO`, `ERROR`, `WARN`, and `DISABLED`

\-\-disable\-all\-logs | \-\-no\-disable\-all\-logs  
When set to true \(`--disable-all-logs`\) disables all logs\. The default \(parameter not used\) is false\.

The `get-v2-logging-options` CLI command returns the configured IAM logging role, the default logging level, and the `disableAllLogs` value\.

Use the `set-v2-logging-level` CLI command to configure fine\-grained logging for a target\. It takes the following arguments:

\-\-log\-target  
A JSON object that contains the resource type \(field `targetType`\) and name \(field `targetName`\) of the entity for which you are configuring logging\. AWS IoT currently supports `THING_GROUP` for the resource type\. You can configure up to 10 logging targets\.

\-\-log\-level  
The logging level used when generating logs for the specified resource\. Valid values are: `DEBUG`, `INFO`, `ERROR`, `WARN`, and `DISABLED` 

Use the `list-v2-logging-levels` CLI command to get a list of the currently configured fine\-grained logging levels\. Call the `delete-v2-logging-level` CLI command to delete a logging level\. Use the `delete-v2-logging-level` command to delete a fine\-grained logging level\.

## CloudWatch Log Entry Format<a name="cwl-format"></a>

Each component of AWS IoT generates its own logs\. Each log entry has an `eventType` that indicates which operation caused the log to be generated\. This section describes the logs generated by the following AWS IoT components:

+ Message Broker

+ Device Shadow service

+ Rules Engine

+ Jobs

All CloudWatch Logs have the following common attributes:

timestamp  
The UNIX timestamp of when the client connected to the AWS IoT message broker\.

logLevel  
The log level being used\. For more information, see [[ERROR] BAD/MISSING LINK TEXT](#log-level)\.

traceId  
A randomly generated identifier that can be used to correlate all logs for a specific request\.

accountId  
Your AWS account ID\.

status  
The status of the request\.

eventType  
The event type for which the log was generated\. The value of the event type for each event is listed in the following sections\.

### Message Broker Logs<a name="message-broker-logs"></a>

The AWS IoT message broker generates logs for the following events:

------
#### [ Connect Log ]

The AWS IoT message broker generates a `Connect` log when an MQTT client connects\.

------
#### [ more info \(1\) ]

For example:

```
{
    "timestamp": "2017-08-10 15:37:23.476", 
    "logLevel": "INFO", 
    "traceId": "20b23f3f-d7f1-feae-169f-82263394fbdb", 
    "accountId": "123456789012", 
    "status": "Success", 
    "eventType": "Connect", 
    "protocol": "MQTT", 
    "clientId": "abf27092886e49a8a5c1922749736453", 
    "principalId": "145179c40e2219e18a909d896a5340b74cf97a39641beec2fc3eeafc5a932167",
    "sourceIp": "205.251.233.181", 
    "sourcePort": 13490 
}
```

In addition to the attributes common to all CloudWatch Logs, `Connect` log entries contain the following attributes:

eventType  
`Connect` for connection logs\.

protocol  
The protocol used when making the request\. Valid values are `MQTT` or `HTTP`\.

clientId  
The ID of the client making the request\.

principalId  
The ID of the principal making the request\.

sourceIp  
The IP address where the request originated\.

sourcePort  
The port where the request originated\.

------

------
#### [ Subscribe Log ]

The AWS IoT message broker generates a `Subscribe` log when an MQTT client subscribes to a topic\.

------
#### [ more info \(2\) ]

For example:

```
{ 
    "timestamp": "2017-08-10 15:39:04.413", 
    "logLevel": "INFO", 
    "traceId": "7aa5c38d-1b49-3753-15dc-513ce4ab9fa6", 
    "accountId": "123456789012", 
    "status": "Success", 
    "eventType": "Subscribe", 
    "protocol": "MQTT", 
    "topicName": "$aws/things/MyThing/shadow/#", 
    "clientId": "abf27092886e49a8a5c1922749736453", 
    "principalId": "145179c40e2219e18a909d896a5340b74cf97a39641beec2fc3eeafc5a932167", 
    "sourceIp": "205.251.233.181", 
    "sourcePort": 13490 
}
```

In addition to the attributes common to all CloudWatch Logs, `Subscribe` log entries contain the following attributes:

eventType  
`Subscribe` for subscription logs\.

protocol  
The protocol used when making the request\. Valid values are `MQTT` or `HTTP`\.

topicName  
The name of the subscribed topic\. 

clientId  
The ID of the client making the request\.

principalId  
The ID of the principal making the request\.

sourceIp  
The IP address where the request originated\.

sourcePort  
The port where the request originated\.

------

------
#### [ Publish\-In Log ]

The AWS IoT message broker generates a `Publish-In` log when the AWS IoT message broker receives an MQTT message\.

------
#### [ more info \(3\) ]

For example:

```
{ 
    "timestamp": "2017-08-10 15:39:30.961", 
    "logLevel": "INFO", 
    "traceId": "672ec480-31ce-fd8b-b5fb-22e3ac420699", 
    "accountId": "123456789012", 
    "status": "Success", 
    "eventType": "Publish-In", 
    "protocol": "MQTT", 
    "topicName": "$aws/things/MyThing/shadow/get", 
    "clientId": "abf27092886e49a8a5c1922749736453", 
    "principalId": "145179c40e2219e18a909d896a5340b74cf97a39641beec2fc3eeafc5a932167", 
    "sourceIp": "205.251.233.181", 
    "sourcePort": 13490 
}
```

In addition to the attributes common to all CloudWatch Logs, `Publish-In` log entries contain the following attributes:

eventType  
`Publish-In` when the message broker receives a message\.

status  
The status of the request\.

protocol  
The protocol used when making the request\. Valid values are `MQTT` or `HTTP`\.

topicName  
The name of the subscribed topic\. 

clientId  
The ID of the client making the request\.

principalId  
The ID of the principal making the request\.

sourceIp  
The IP address where the request originated\.

sourcePort  
The port where the request originated\.

------

------
#### [ Publish\-Out Log ]

The AWS IoT message broker generates a `Publish-Out` log when the message broker publishes an MQTT message\.

------
#### [ more info \(4\) ]

For example:

```
{ 
    "timestamp": "2017-08-10 15:39:30.961", 
    "logLevel": "INFO", 
    "traceId": "672ec480-31ce-fd8b-b5fb-22e3ac420699", 
    "accountId": "123456789012", 
    "status": "Success", 
    "eventType": "Publish-Out", 
    "protocol": "MQTT", 
    "topicName": "$aws/things/MyThing/shadow/get", 
    "clientId": "abf27092886e49a8a5c1922749736453", 
    "principalId": "145179c40e2219e18a909d896a5340b74cf97a39641beec2fc3eeafc5a932167", 
    "sourceIp": "205.251.233.181", 
    "sourcePort": 13490 
}
```

In addition to the attributes common to all CloudWatch Logs, `Publish-Out` log entries contain the following attributes:

eventType  
`Publish-Out` when the message broker publishes a message\.

status  
The status of the request\.

protocol  
The protocol used when making the request\. Valid values are `MQTT` or `HTTP`\.

topicName  
The name of the subscribed topic\. 

clientId  
The ID of the client making the request\.

principalId  
The ID of the principal making the request\.

sourceIp  
The IP address where the request originated\.

sourcePort  
The port where the request originated\.

------

------
#### [ Disconnect Log ]

The AWS IoT message broker generates a `Disconnect` log when an MQTT client disconnects\.

------
#### [ more info \(5\) ]

For example:

```
{ 
    "timestamp": "2017-08-10 15:37:23.476", 
    "logLevel": "INFO", 
    "traceId": "20b23f3f-d7f1-feae-169f-82263394fbdb", 
    "accountId": "123456789012", 
    "status": "Success", 
    "eventType": "Disconnect", 
    "protocol": "MQTT", 
    "clientId": "abf27092886e49a8a5c1922749736453", 
    "principalId": "145179c40e2219e18a909d896a5340b74cf97a39641beec2fc3eeafc5a932167",
    "sourceIp": "205.251.233.181", 
    "sourcePort": 13490 
}
```

In addition to the attributes common to all CloudWatch Logs, `Disconnect` log entries contain the following attributes:

eventType  
`Disconnect` for connection logs\.

protocol  
The protocol used when making the request\. Valid values are `MQTT` or `HTTP`\.

clientId  
The ID of the client making the request\.

principalId  
The ID of the principal making the request\.

sourceIp  
The IP address where the request originated\.

sourcePort  
The port where the request originated\.

------

### Device Shadow Logs<a name="device-shadow-logs"></a>

The AWS IoT Device Shadow service generates logs for the following events:

------
#### [ GetThingShadow Logs ]

The Device Shadow service generates a `GetThingShadow` log when a get request for a shadow is received\.

------
#### [ more info \(6\) ]

For example:

```
{ 
    "timestamp": "2017-08-09 17:56:30.941", 
    "logLevel": "INFO", 
    "traceId": "b575f19a-97a2-cf72-0ed0-c64a783a2504", 
    "accountId": "123456789012", 
    "status": "Success", 
    "eventType": "GetThingShadow", 
    "protocol": "MQTT", 
    "deviceShadowName": "MyThing", 
    "topicName": "$aws/things/MyThing/shadow/get" 
}
```

In addition to the attributes common to all CloudWatch Logs, `GetThingShadow` log entries contain the following attributes:

eventType  
`GetThingShadow` for GetThingShadow logs\.

protocol  
The protocol used when making the request\. Valid values are `MQTT` or `HTTP`\.

deviceShadowName  
The name of the requested shadow\.

topicName  
The name of the topic on which the request was published\. 

------

------
#### [ UpdateThingShadow Logs ]

The Device Shadow service generates a `UpdateThingShadow` log when a request to update a device's shadow is received\.

------
#### [ more info \(7\) ]

For example:

```
{ 
    "timestamp": "2017-08-07 18:43:59.436", 
    "logLevel": "INFO", 
    "traceId": "d0074ba8-0c4b-a400-69df-76326d414c28", 
    "accountId": "123456789012", 
    "status": "Success", 
    "eventType": "UpdateThingShadow", 
    "protocol": "MQTT", 
    "deviceShadowName": "Jack",
    "topicName": "$aws/things/Jack/shadow/update" 
}
```

In addition to the attributes common to all CloudWatch Logs, `UpdateThingShadow` log entries contain the following attributes:

eventType  
`UpdateThingShadow` for update shadow logs\.

protocol  
The protocol used when making the request\. Valid values are `MQTT` or `HTTP`\.

deviceShadowName  
The name of the shadow to update\.

topicName  
The name of the topic on which the request was published\. 

------

------
#### [ DeleteThingShadow Logs ]

The Device Shadow service generates a `DeleteThingShadow` log when a request to delete a device's shadow is received\.

------
#### [ more info \(8\) ]

For example:

```
{ 
    "timestamp": "2017-08-07 18:47:56.664", 
    "logLevel": "INFO", 
    "traceId": "1a60d02e-15b9-605b-7096-a9f584a6ad3f", 
    "accountId": "123456789012", 
    "status": "Success", 
    "eventType": "DeleteThingShadow", 
    "protocol": "MQTT", 
    "deviceShadowName": "Jack", 
    "topicName": "$aws/things/Jack/shadow/delete" 
}
```

In addition to the attributes common to all CloudWatch Logs, `DeleteThingShadow` log entries contain the following attributes:

eventType  
`DeleteThingShadow` for DeleteThingShadow logs\.

protocol  
The protocol used when making the request\. Valid values are `MQTT` or `HTTP`\.

deviceShadowName  
The name of the shadow to update\.

topicName  
The name of the topic on which the request was published\. 

------

### Rules Engine Logs<a name="rule-engine-logs"></a>

The AWS IoT Rules Engine service generates logs for the following events:

------
#### [ Rule Match Logs ]

The AWS IoT rules engine generates a `RuleMatch` log when the message broker receives a message that matches a rule\.

------
#### [ more info \(9\) ]

For example:

```
{ 
    "timestamp": "2017-08-10 16:32:46.002", 
    "logLevel": "INFO", 
    "traceId": "30aa7ccc-1d23-0b97-aa7b-76196d83537e", 
    "accountId": "123456789012", 
    "status": "Success", 
    "eventType": "RuleMatch", 
    "clientId": "abf27092886e49a8a5c1922749736453", 
    "topicName": "rules/test", 
    "ruleName": "JSONLogsRule", 
    "principalId": "145179c40e2219e18a909d896a5340b74cf97a39641beec2fc3eeafc5a932167" 
}
```

In addition to the attributes common to all CloudWatch Logs, `RuleMatch` log entries contain the following attributes:

eventType  
`RuleMatch` for rule match logs\.

clientId  
The ID of the client making the request\.

topicName  
The name of the subscribed topic\. 

ruleName  
The name of the matching rule\.

principalId  
The ID of the principal making the request\.

------

------
#### [ Function Execution Logs ]

The rules engine generates a `FunctionExecution` log when a rule's SQL query calls an external function\. An external function is called when a rule's action makes an HTTP request to AWS IoT or another web service \(for example, calling `get_thing_shadow` or `machinelearning_predict`\)\. 

------
#### [ more info \(10\) ]

A `FunctionExecution` log will look like the following:

```
{
    "timestamp": "2017-07-13 18:33:51.903",
    "logLevel": "DEBUG",
    "traceId": "180532b7-0cc7-057b-687a-5ca1824838f5",
    "status": "Success",
    "eventType": "FunctionExecution",
    "clientId": "N/A",
    "topicName":"rules/test",
    "ruleName": "ruleTestPredict",
    "ruleAction": "MachinelearningPredict",
    "resources": {
        "ModelId": "predict-model"
    },
    "principalId": "145179c40e2219e18a909d896a5340b74cf97a39641beec2fc3eeafc5a932167"
}
```

In addition to the attributes common to all CloudWatch Logs, `FunctionExecution` log entries contain the following attributes:

eventType  
`FunctionExecution` for rule match logs\.

clientId  
`N/A` for `FunctionExecution` logs\.

topicName  
The name of the subscribed topic\. 

ruleName  
The name of the matching rule\.

resources  
A collection of resources used by the rule's actions\.

principalId  
The ID of the principal making the request\.

------

------
#### [ Starting Execution Logs ]

The AWS IoT Rules Engine generates a `StartingExecution` log when the Rules Engine starts to trigger a rule's action\.

------
#### [ more info \(11\) ]

For example:

```
{ 
    "timestamp": "2017-08-10 16:32:46.002", 
    "logLevel": "DEBUG", 
    "traceId": "30aa7ccc-1d23-0b97-aa7b-76196d83537e", 
    "accountId": "123456789012",
    "status": "Success", 
    "eventType": "StartingRuleExecution", 
    "clientId": "abf27092886e49a8a5c1922749736453", 
    "topicName": "rules/test", 
    "ruleName": "JSONLogsRule", 
    "ruleAction": "RepublishAction", 
    "principalId": "145179c40e2219e18a909d896a5340b74cf97a39641beec2fc3eeafc5a932167" 
}
```

In addition to the attributes common to all CloudWatch Logs, `StartingExecution` log entries contain the following attributes:

eventType  
`StartingRuleExecution` for starting rule execution logs\.

clientId  
The ID of the client making the request\.

topicName  
The name of the subscribed topic\. 

ruleName  
The name of the matching rule\.

ruleAction  
The name of the action triggered\.

principalId  
The ID of the principal making the request\.

------

------
#### [ Rule Execution Logs ]

The AWS IoT rules engine generates a `RuleExecution` log when the rules engine triggers a rule's action\.

------
#### [ more info \(12\) ]

For example:

```
{ 
    "timestamp": "2017-08-10 16:32:46.070", 
    "logLevel": "INFO", 
    "traceId": "30aa7ccc-1d23-0b97-aa7b-76196d83537e", 
    "accountId": "123456789012", 
    "status": "Success", 
    "eventType": "RuleExecution", 
    "clientId": "abf27092886e49a8a5c1922749736453", 
    "topicName": "rules/test", 
    "ruleName": "JSONLogsRule", 
    "ruleAction": "RepublishAction", 
    "resources": { 
        "RepublishTopic": "rules/republish" 
    }, 
    "principalId": "145179c40e2219e18a909d896a5340b74cf97a39641beec2fc3eeafc5a932167" 
}
```

In addition to the attributes common to all CloudWatch Logs, `RuleExecution` log entries contain the following attributes:

eventType  
`RuleExecution` for rule execution logs\.

clientId  
The ID of the client making the request\.

topicName  
The name of the subscribed topic\. 

ruleName  
The name of the matching rule\.

ruleAction  
The name of the action triggered\.

resources  
A collection of resources used by the rule's actions\.

principalId  
The ID of the principal making the request\.

------

------
#### [ Rule Not Found Logs ]

The AWS IoT Rules Engine generates a `RuleNotFound` error log when it cannot find a rule with a given name\.

------
#### [ more info \(13\) ]

For example:

```
{ 
    "timestamp": "2017-10-04 19:25:46.070", 
    "logLevel": "ERROR", 
    "traceId": "30aa7ccc-1d23-0b97-aa7b-76196d83537e", 
    "accountId": "123456789012", 
    "status": "Failure", 
    "eventType": "RuleNotFound", 
    "clientId": "abf27092886e49a8a5c1922749736453", 
    "topicName": "$aws/rules/example_rule",
    "ruleName": "example_rule", 
    "principalId": "145179c40e2219e18a909d896a5340b74cf97a39641beec2fc3eeafc5a932167",
    "reason": "RuleNotFound",
    "details": "Rule example_rule not found"
}
```

In addition to the attributes common to all CloudWatch Logs, `RuleNotFound` log entries contain the following attributes:

eventType  
`RuleNotFound` for rule not found logs\.

clientId  
The ID of the client making the request\.

topicName  
The name of the topic that was published\.

ruleName  
The name of the rule which could not be found\.

principalId  
The ID of the principal making the request\.

reason  
The string "RuleNotFound"\.

details  
A brief explanation of the error\.

------

------
#### [ Rule Message Throttled Logs ]

The AWS IoT Rules Engine generates a `RuleMessageThrottled` error log when a message is throttled\.

------
#### [ more info \(14\) ]

For example:

```
{ 
    "timestamp": "2017-10-04 19:25:46.070", 
    "logLevel": "ERROR", 
    "traceId": "30aa7ccc-1d23-0b97-aa7b-76196d83537e", 
    "accountId": "123456789012", 
    "status": "Failure", 
    "eventType": "RuleMessageThrottled", 
    "clientId": "abf27092886e49a8a5c1922749736453", 
    "topicName": "$aws/rules/example_rule",
    "ruleName": "example_rule", 
    "principalId": "145179c40e2219e18a909d896a5340b74cf97a39641beec2fc3eeafc5a932167",
    "reason": "RuleExecutionThrottled",
    "details": "Message for Rule example_rule throttled"
}
```

In addition to the attributes common to all CloudWatch Logs, `RuleMessageThrottled` log entries contain the following attributes:

eventType  
`RuleMessageThrottled` for rule message throttled logs\.

clientId  
The ID of the client making the request\.

topicName  
The name of the topic that was published\.

ruleName  
The name of the rule to be triggered\.

principalId  
The ID of the principal making the request\.

reason  
The string "RuleMessageThrottled"\.

details  
A brief explanation of the error\.

------

### Job Logs<a name="job-logs"></a>

The AWS IoT Job service generates logs for the following events\. Note that logs are generated when an MQTT or HTTP request is received from the device\.

------
#### [ Get Pending Job Execution Logs ]

The AWS IoT Jobs service generates a `GetJobExecution` log when the service receives a job execution request\.

------
#### [ more info \(16\) ]

For example:

```
{
  "timestamp": "2018-06-13 17:45:17.197",
  "logLevel": "DEBUG",
  "accountId": "123456789012",
  "status": "Success",
  "eventType": "GetPendingJobExecution",
  "protocol": "MQTT",
  "clientId": "299966ad-54de-40b4-99d3-4fc8b52da0c5",
  "topicName": "$aws/things/299966ad-54de-40b4-99d3-4fc8b52da0c5/jobs/get",
  "clientToken": "24b9a741-15a7-44fc-bd3c-1ff2e34e5e82",
  "details": "The request status is SUCCESS."
}
```

In addition to the attributes common to all CloudWatch Logs, `GetPendingJobExecution` log entries contain the following attributes:

eventType  
`GetPendingJobExecution` for get pending job execution logs\.

protocol  
The protocol used when making the request\. Valid values are `MQTT` or `HTTP`\.

clientId  
The ID of the client making the request\.

topicName  
The name of the subscribed topic\. 

clientToken  
A unique, case\-sensitive, identifier to ensure the idempotency of the request\. For more information, see [How to Ensure Idempotency](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/Run_Instance_Idempotency.html)\.

details  
Additional information from the Jobs service\.

------

------
#### [ Describe Job Execution Logs ]

The AWS IoT Jobs service generates a `DescribeJobExecution` log when the service receives a request to describe a job execution\.

------
#### [ more info \(17\) ]

For example:

```
{ 
    "timestamp": "2017-08-10 19:13:22.841", 
    "logLevel": "DEBUG", 
    "accountId": "123456789012", 
    "status": "Success", 
    "eventType": "DescribeJobExecution", 
    "protocol": "MQTT", 
    "clientId": "thingOne", 
    "jobId": "002", 
    "topicName": "$aws/things/thingOne/jobs/002/get", 
    "clientToken": "myToken", 
    "details": "The request status is SUCCESS." 
}
```

In addition to the attributes common to all CloudWatch Logs, `GetJobExecution` log entries contain the following attributes:

eventType  
`DescribeJobExecution` for describe job execution logs\.

protocol  
The protocol used when making the request\. Valid values are `MQTT` or `HTTP`\.

clientId  
The ID of the client making the request\.

jobId  
The job ID for the job execution\.

topicName  
The topic used to make the request\. 

clientToken  
A unique, case\-sensitive, identifier to ensure the idempotency of the request\. For more information, see [How to Ensure Idempotency](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/Run_Instance_Idempotency.html)\.

details  
Additional information from the Jobs service\.

------

------
#### [ Update Job Execution Logs ]

The AWS IoT Jobs service generates an `UpdateJobExecution` log when the service receives a request to update a job execution\.

------
#### [ more info \(18\) ]

For example:

```
{ 
    "timestamp": "2017-08-10 19:25:14.758", 
    "logLevel": "DEBUG", 
    "accountId": "123456789012", 
    "status": "Success", 
    "eventType": "UpdateJobExecution", 
    "protocol": "MQTT", 
    "clientId": "thingOne", 
    "jobId": "002", 
    "topicName": "$aws/things/thingOne/jobs/002/update", 
    "clientToken": "myClientToken", 
    "versionNumber": "1", 
    "details": "The destination status is IN_PROGRESS. The request status is SUCCESS." 
}
```

In addition to the attributes common to all CloudWatch Logs, `UpdateJobExecution` log entries contain the following attributes:

eventType  
`UpdateJobExecution` for update job execution logs\.

protocol  
The protocol used when making the request\. Valid values are `MQTT` or `HTTP`\.

clientId  
The ID of the client making the request\.

jobId  
The job ID for the job execution\.

topicName  
The topic used to make the request\. 

clientToken  
A unique, case\-sensitive, identifier to ensure the idempotency of the request\. For more information, see [How to Ensure Idempotency](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/Run_Instance_Idempotency.html)\.

versionNumber  
The version of the job execution\.

details  
Additional information from the Jobs service\.

------

------
#### [ Start Next Pending Job Execution Logs ]

The AWS IoT Jobs service generates a `StartNextPendingJobExecution` log when the service receives a request to start the next pending job execution\.

------
#### [ more info \(19\) ]

For example:

```
{
  "timestamp": "2018-06-13 17:49:51.036",
  "logLevel": "DEBUG",
  "accountId": "123456789012",
  "status": "Success",
  "eventType": "StartNextPendingJobExecution",
  "protocol": "MQTT",
  "clientId": "95c47808-b1ca-4794-bc68-a588d6d9216c",
  "topicName": "$aws/things/95c47808-b1ca-4794-bc68-a588d6d9216c/jobs/start-next",
  "clientToken": "bd7447c4-3a05-49f4-8517-dd89b2c68d94",
  "details": "The request status is SUCCESS."
}
```

In addition to the attributes common to all CloudWatch Logs, `StartNextPendingJobExecution` log entries contain the following attributes:

eventType  
`StartNextPendingJobExecution` for start next pending job execution logs\.

protocol  
The protocol used when making the request\. Valid values are `MQTT` or `HTTP`\.

clientId  
The ID of the client making the request\.

topicName  
The topic used to make the request\. 

clientToken  
A unique, case\-sensitive, identifier to ensure the idempotency of the request\. For more information, see [How to Ensure Idempotency](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/Run_Instance_Idempotency.html)\.

details  
Additional information from the Jobs service\.

------

------
#### [ Report Final Job Execution Count Logs ]

The AWS IoT Jobs service generates a `ReportFinalJobExecutionCount` log when a job is completed\.

------
#### [ more info \(20\) ]

For example:

```
{
  "timestamp": "2017-08-10 19:44:16.776",
  "logLevel": "INFO",
  "accountId": "123456789012",
  "status": "Success",
  "eventType": "ReportFinalJobExecutionCount",
  "protocol": "MQTT",
  "jobId": "002",
  "details": "Job 002 completed. QUEUED job execution count: 0 IN_PROGRESS job execution count: 0 FAILED job execution count: 0 SUCCEEDED job execution count: 1 CANCELED job execution count: 0 REJECTED job execution count: 0 REMOVED job execution count: 0"
}
```

In addition to the attributes common to all CloudWatch Logs, `ReportFinalJobExecutionCount` log entries contain the following attributes:

eventType  
`ReportFinalJobExecutionCount` for report final job execution count logs\.

protocol  
The protocol used when making the request\. The valid value is `MQTT` \(not `HTTP`\)\.

jobId  
The job ID for the job execution\.

details  
Additional information from the Jobs service\.

------