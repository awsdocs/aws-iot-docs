--------

 The Fleet Hub service is currently in public preview\. This service is subject to change\.

--------

# Logging Fleet Hub for AWS IoT Device Management API calls with AWS CloudTrail<a name="logging-using-cloudtrail"></a>

Fleet Hub for AWS IoT Device Management is integrated with AWS CloudTrail\. The CloudTrail service provides a record of actions that a user, role, or an AWS service takes in Fleet Hub\. CloudTrail captures all API calls for Fleet Hub as events\. Captured calls include those from the Fleet Hub console and code calls to the Fleet Hub API operations\. 

If you create a trail, you can enable continuous delivery of CloudTrail events to an Amazon S3 bucket, including events for Fleet Hub\. If you don't configure a trail, you can still view the most recent events in the CloudTrail console in **Event history**\. 

Using the information that CloudTrail collects, you can determine the request that was made to Fleet Hub, the IP address from which the request was made, who made the request and when, and more details\. 

To learn more about CloudTrail, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## Fleet Hub information in CloudTrail<a name="service-name-info-in-cloudtrail"></a>

AWS CloudTrail is enabled on your AWS account when you create the account\. When activity occurs in Fleet Hub, that activity is recorded in a CloudTrail event with other AWS service events in **Event history**\. You can view, search, and download recent events in your AWS account\. For more information, see [Viewing events with CloudTrail event history](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)\. 

For an ongoing record of events in your AWS account, including events for Fleet Hub, create a trail\. A *trail* enables CloudTrail to deliver log files to an Amazon Simple Storage Service \(Amazon S3\) bucket\. By default, when you create a trail in the console, the trail applies to all AWS Regions\. The trail logs events from all Regions in the AWS partition and delivers the log files to the Amazon S3 bucket that you specify\. 

You can also configure other AWS services to further analyze and act on the event data collected in CloudTrail logs\. For more information, see the following: 
+ [Overview for creating a trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html)
+ [CloudTrail supported services and integrations](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-aws-service-specific-topics.html#cloudtrail-aws-service-specific-topics-integrations)
+ [Configuring Amazon SNS notifications for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)
+ [Receiving CloudTrail log files from multiple Regions](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/receive-cloudtrail-log-files-from-multiple-regions.html)
+ [Receiving CloudTrail log files from multiple accounts](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)

CloudTrail logs all Fleet Hub actions\. They're documented in the [AWS IoT API Reference](https://docs.aws.amazon.com/iot/latest/apireference/API_Operations_AWS_IoT_Fleet_Hub.html)\. For example, calls to the `CreateApplication` and `UpdateApplication` actions generate entries in the CloudTrail log files\. 

Every event or log entry contains information about who generated the request\. The identity information helps you determine the following: 
+ Whether the request was made with root or AWS Identity and Access Management \(IAM\) user credentials
+ Whether the request was made with temporary security credentials for a role or federated user
+ Whether the request was made by another AWS service

For more information, see the [CloudTrail userIdentity element](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\.

## Understanding Fleet Hub for AWS IoT Device Management log file entries<a name="understanding-service-name-entries"></a>

A trail is a configuration that enables delivery of events as log files to an Amazon S3 bucket that you specify\. 

CloudTrail log files contain one or more log entries\. An event represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters, and so on\. 

CloudTrail log files aren't an ordered stack trace of the public API calls, so they don't appear in any specific order\. 

**Example**  
The following CloudTrail log entry shows information about the `CreateApplication` action\.  

```
    {
        "eventVersion": "1.08",
        "userIdentity": {
            "type": "AssumedRole",
            "principalId": "principal-id",
            "arn": "arn:aws:sts::123456789012:assumed-role/Admin/test-user-name",
            "accountId": "123456789012",
            "accessKeyId": "access-key",
            "sessionContext": {
                "sessionIssuer": {
                    "type": "Role",
                    "principalId": "principal-id",
                    "arn": "arn:aws:iam::123456789012:role/Admin",
                    "accountId": "123456789012",
                    "userName": "Admin"
                },
                "webIdFederationData": {},
                "attributes": {
                    "mfaAuthenticated": "false",
                    "creationDate": "2020-12-04T19:59:53Z"
                }
            }
        },
        "eventTime": "2020-12-04T20:02:38Z",
        "eventSource": "iotfleethub.amazonaws.com",
        "eventName": "CreateApplication",
        "awsRegion": "us-east-1",
        "sourceIPAddress": "72.22.186.61",
        "userAgent": "console.amazonaws.com",
        "requestParameters": {
            "applicationDescription": "Test application description",
            "applicationName": "Test application name",
            "clientToken": "c9bc7f45-3737-4ee9-9b0f-5de1aab169b2"
        },
        "responseElements": {
            "applicationUrl": "https://application-id.app.iotfleethub.aws",
            "applicationArn": "arn:aws:iotfleethub:us-east-1:123456789012:application/application-id",
            "applicationId": "application-id"
        },
        "requestID": "5456304e-31c5-4336-9bbe-a375e3728eee",
        "eventID": "9ffb5d72-9267-4f4e-88e6-d26051133c8c",
        "readOnly": false,
        "eventType": "AwsApiCall",
        "recipientAccountId": "123456789012"
    }
```