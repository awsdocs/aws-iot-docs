# Monitoring AWS IoT<a name="monitoring_overview"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of AWS IoT and your AWS solutions\. You should collect monitoring data from all parts of your AWS solution so that you can more easily debug a multi\-point failure if one occurs\. Before you start monitoring AWS IoT, you should create a monitoring plan that includes answers to the following questions:

+ What are your monitoring goals?

+ Which resources will you monitor?

+ How often will you monitor these resources?

+ Which monitoring tools will you use?

+ Who will perform the monitoring tasks?

+ Who should be notified when something goes wrong?

The next step is to establish a baseline for normal AWS IoT performance in your environment, by measuring performance at various times and under different load conditions\. As you monitor AWS IoT, store historical monitoring data so that you can compare it with current performance data, identify normal performance patterns and performance anomalies, and devise methods to address issues\.

For example, if you're using Amazon EC2, you can monitor CPU utilization, disk I/O, and network utilization for your instances\. When performance falls outside your established baseline, you might need to reconfigure or optimize the instance to reduce CPU utilization, improve disk I/O, or reduce network traffic\.

To establish a baseline you should, at a minimum, monitor the following items:

+ PublishIn\.Success

+ PublishOut\.Success

+ Subscribe\.Success

+ Ping\.Success

+ Connect\.Success

+ GetThingShadow\.Accepted

+ UpdateThingShadow\.Accepted

+ DeleteThingShadow\.Accepted

+ RulesExecuted


+ [Monitoring Tools](monitoring_automated_manual.md)
+ [Monitoring with Amazon CloudWatch](monitoring-cloudwatch.md)
+ [Logging AWS IoT API Calls with AWS CloudTrail](#iot-using-cloudtrail)

## Logging AWS IoT API Calls with AWS CloudTrail<a name="iot-using-cloudtrail"></a>

AWS IoT is integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or an AWS service in AWS IoT\. CloudTrail captures all API calls for AWS IoT as events, including calls from the AWS IoT console and from code calls to the AWS IoT APIs\. If you create a trail, you can enable continuous delivery of CloudTrail events to an Amazon S3 bucket, including events for AWS IoT\. If you don't configure a trail, you can still view the most recent events in the CloudTrail console in **Event history**\. Using the information collected by CloudTrail, you can determine the request that was made to AWS IoT, the IP address from which the request was made, who made the request, when it was made, and additional details\.

To learn more about CloudTrail, see the [AWS CloudTrail User Guide](http://alpha-docs-aws.amazon.com/awscloudtrail/latest/userguide/)\.

### AWS IoT Information in CloudTrail<a name="aws-iot-info-in-cloudtrail"></a>

CloudTrail is enabled on your AWS account when you create the account\. When activity occurs in AWS IoT, that activity is recorded in a CloudTrail event along with other AWS service events in **Event history**\. You can view, search, and download recent events in your AWS account\. For more information, see [Viewing Events with CloudTrail Event History](http://alpha-docs-aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)\. 

For an ongoing record of events in your AWS account, including events for AWS IoT, create a trail\. A trail enables CloudTrail to deliver log files to an Amazon S3 bucket\. By default, when you create a trail in the console, the trail applies to all regions\. The trail logs events from all regions in the AWS partition and delivers the log files to the Amazon S3 bucket that you specify\. Additionally, you can configure other AWS services to further analyze and act upon the event data collected in CloudTrail logs\. For more information, see: 

+  [Overview for Creating a Trail](http://alpha-docs-aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html) 

+  [CloudTrail Supported Services and Integrations](http://alpha-docs-aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-aws-service-specific-topics.html#cloudtrail-aws-service-specific-topics-integrations) 

+  [Configuring Amazon SNS Notifications for CloudTrail](http://alpha-docs-aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html) 

+  [Receiving CloudTrail Log Files from Multiple Regions](http://alpha-docs-aws.amazon.com/awscloudtrail/latest/userguide/receive-cloudtrail-log-files-from-multiple-regions.html) and [Receiving CloudTrail Log Files from Multiple Accounts](http://alpha-docs-aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html) 

**Note**  
AWS IoT data plane actions \(device side\) are not logged by CloudTrail\. Use CloudWatch to monitor these\.

AWS IoT control plane actions *are* logged by CloudTrail\. For example, calls to the **CreateThing**, **ListThings**, and **ListTopicRules** sections generate entries in the CloudTrail log files\.

Every event or log entry contains information about who generated the request\. The identity information helps you determine the following: 

+ Whether the request was made with root or IAM user credentials\.

+ Whether the request was made with temporary security credentials for a role or federated user\.

+ Whether the request was made by another AWS service\.

For more information, see the [CloudTrail userIdentity Element](http://alpha-docs-aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\. AWS IoT actions are documented in the [AWS IoT API Reference](http://alpha-docs-aws.amazon.com/iot/latest/apireference/)\. 

### Understanding AWS IoT Log File Entries<a name="understanding-aws-iot-entries"></a>

A trail is a configuration that enables delivery of events as log files to an Amazon S3 bucket that you specify\. CloudTrail log files contain one or more log entries\. An event represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters, and so on\. CloudTrail log files are not an ordered stack trace of the public API calls, so they do not appear in any specific order\. 

The following example shows a CloudTrail log entry that demonstrates the `AttachPolicy` action\.

```
{
    "timestamp":"1460159496",
    "AdditionalEventData":"",
    "Annotation":"",
    "ApiVersion":"",
    "ErrorCode":"",
    "ErrorMessage":"",
    "EventID":"8bff4fed-c229-4d2d-8264-4ab28a487505",
    "EventName":"AttachPolicy",
    "EventTime":"2016-04-08T23:51:36Z",
    "EventType":"AwsApiCall",
    "ReadOnly":"",
    "RecipientAccountList":"",
    "RequestID":"d4875df2-fde4-11e5-b829-23bf9b56cbcd",
    "RequestParamters":{
        "principal":"arn:aws:iot:us-east-1:123456789012:cert/528ce36e8047f6a75ee51ab7beddb4eb268ad41d2ea881a10b67e8e76924d894",
        "policyName":"ExamplePolicyForIoT"
    },
    "Resources":"",
    "ResponseElements":"",
    "SourceIpAddress":"52.90.213.26",
    "UserAgent":"aws-internal/3",
    "UserIdentity":{
        "type":"AssumedRole",
        "principalId":"AKIAI44QH8DHBEXAMPLE",
        "arn":"arn:aws:sts::12345678912:assumed-role/iotmonitor-us-east-1-beta-InstanceRole-1C5T1YCYMHPYT/i-35d0a4b6",
        "accountId":"222222222222",
        "accessKeyId":"access-key-id",
        "sessionContext":{
            "attributes":{
                "mfaAuthenticated":"false",
                "creationDate":"Fri Apr 08 23:51:10 UTC 2016"
            },
            "sessionIssuer":{
                "type":"Role",
                "principalId":"AKIAI44QH8DHBEXAMPLE",
                "arn":"arn:aws:iam::123456789012:role/executionServiceEC2Role/iotmonitor-us-east-1-beta-InstanceRole-1C5T1YCYMHPYT",
                "accountId":"222222222222",
                "userName":"iotmonitor-us-east-1-InstanceRole-1C5T1YCYMHPYT"
            }
        },
        "invokedBy":{
            "serviceAccountId":"111111111111"
        }
    },
    "VpcEndpointId":""
}
```