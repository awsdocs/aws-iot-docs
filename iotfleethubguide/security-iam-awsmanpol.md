# AWS managed policies for Fleet Hub for AWS IoT Device Management<a name="security-iam-awsmanpol"></a>







To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the **ReadOnlyAccess** AWS managed policy provides read\-only access to all AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.









## AWS managed policy: AWSIoTFleetHubFederationAccess<a name="security-iam-awsmanpol-AWSIoTFleetHubFederationAccess"></a>





You can attach the `AWSIoTFleetHubFederationAccess` policy to your IAM identities\.



This policy grants Fleet Hub for AWS IoT Device Management federated users the permissions they need to take actions in AWS IoT and other AWS services from Fleet Hub web applications\.

For more information about adding users to Fleet Hub web applications, see [How to add users to Fleet Hub for AWS IoT Device Management applications](aws-iot-monitor-admin-work-with-apps-add-users.md)\.

View this policy in the [AWS console](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/service-role/AWSIoTFleetHubFederationAccess$jsonEditor)\.

**Permissions details**

This policy includes the following permissions:
+ `iot` \- Retrieve AWS IoT device data and perform fleet\-level actions\.
+ `iotfleethub` \- Retrieve Fleet Hub app metadata\.
+ `cloudwatch` \- Retrieve CloudWatch alarm and metric data\. Also allows create and delete actions scoped to Fleet Hub alarms\.
+ `sns` \- Perform create, read, delete, subscribe, and unsubscribe operations\. These operations are scoped to Fleet Hub SNS topics\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iot:DescribeIndex",
                "iot:DescribeThingGroup",
                "iot:GetBucketsAggregation",
                "iot:GetCardinality",
                "iot:GetIndexingConfiguration",
                "iot:GetPercentiles",
                "iot:GetStatistics",
                "iot:SearchIndex",
                "iot:CreateFleetMetric",
                "iot:ListFleetMetrics",
                "iot:DeleteFleetMetric",
                "iot:DescribeFleetMetric",
                "iot:UpdateFleetMetric",
                "iot:ListThingGroups",
                "iot:ListThingsInThingGroup",
                "iot:ListJobTemplates",
                "iot:DescribeJobTemplate",
                "iot:ListJobs",
                "iot:CreateJob",
                "iot:CancelJob",
                "iot:DescribeJob",
                "iot:ListJobExecutionsForJob",
                "iot:ListJobExecutionsForThing",
                "iot:DescribeJobExecution",
                "iot:ListSecurityProfiles",
                "iot:DescribeSecurityProfile",
                "iot:ListActiveViolations",
                "iot:GetThingShadow",
                "iot:ListNamedShadowsForThing",
                "iot:DescribeEndpoint",
                "iot:CancelJobExecution",
                "iotfleethub:DescribeApplication",
                "cloudwatch:DescribeAlarms",
                "cloudwatch:GetMetricData",
                "cloudwatch:ListMetrics",
                "sns:ListTopics"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "sns:CreateTopic",
                "sns:DeleteTopic",
                "sns:ListSubscriptionsByTopic",
                "sns:Subscribe",
                "sns:Unsubscribe"
            ],
            "Resource": "arn:aws:sns:*:*:iotfleethub*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricAlarm",
                "cloudwatch:DeleteAlarms",
                "cloudwatch:DescribeAlarmHistory"
            ],
            "Resource": "arn:aws:cloudwatch:*:*:iotfleethub*"
        }
    ]
}
```





## Fleet Hub updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>



View details about updates to AWS managed policies for Fleet Hub since this service began tracking these changes\. For more information, see the Fleet Hub [Documentation history](documentation-history-aws-iot-monitor.md) page\.




| Change | Description | Date | 
| --- | --- | --- | 
|   [AWSIoTFleetHubFederationAccess](#security-iam-awsmanpol-AWSIoTFleetHubFederationAccess) – Update to an existing policy  |  Fleet Hub added new permissions to allow app users to retrieve additional data sources for indexing\. A permission is also added to allow app users to cancel an AWS IoT job execution within the app\.  | November 15, 2021 | 
|   [AWSIoTFleetHubFederationAccess](#security-iam-awsmanpol-AWSIoTFleetHubFederationAccess) – Update to an existing policy  |  Fleet Hub added new permissions for app users to retrieve Thing Group data and perform CRUD operations on AWS IoT jobs\.  | May 24, 2021 | 
|   [AWSIoTFleetHubFederationAccess](#security-iam-awsmanpol-AWSIoTFleetHubFederationAccess) – Update to an existing policy  |  Fleet Hub removed permissions for unsupported Fleet Hub dashboard APIs\.  | April 12, 2021 | 
|  [AWSIoTFleetHubFederationAccess](#security-iam-awsmanpol-AWSIoTFleetHubFederationAccess) – New policy  |  Fleet Hub added a new policy that grants permissions that are needed for Fleet Hub application users to retrieve device data and perform AWS IoT actions\.  | April 12, 2021 | 
|  Fleet Hub started tracking changes  |  Fleet Hub started tracking changes for its AWS managed policies\.  | April 12, 2021 | 