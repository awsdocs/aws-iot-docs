# AWS managed policies for AWS IoT<a name="security-iam-awsmanpol"></a>







To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the **ReadOnlyAccess** AWS managed policy provides read\-only access to all AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.

**Note**  
AWS IoT works with both AWS IoT and IAM policies\. This topic discusses only IAM policies, which defines a policy action for control plane and data plane API operations\. See also [AWS IoT Core policies](iot-policies.md)\.









## AWS managed policy: AWSIoTConfigAccess<a name="security-iam-awsmanpol-AWSIoTConfigAccess"></a>





You can attach the `AWSIoTConfigAccess` policy to your IAM identities\.



This policy grants the associated identity permissions that allow access to all AWS IoT configuration operations\. This policy can affect data processing and storage\. To view this policy in the AWS Management Console, see [AWSIoTConfigAccess](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIoTConfigAccess$jsonEditor?section=permissions)\.



**Permissions details**

This policy includes the following permissions\.




+ `iot` – Retrieve AWS IoT data and perform IoT configuration actions\.



```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iot:AcceptCertificateTransfer",
                "iot:AddThingToThingGroup",
                "iot:AssociateTargetsWithJob",
                "iot:AttachPolicy",
                "iot:AttachPrincipalPolicy",
                "iot:AttachThingPrincipal",
                "iot:CancelCertificateTransfer",
                "iot:CancelJob",
                "iot:CancelJobExecution",
                "iot:ClearDefaultAuthorizer",
                "iot:CreateAuthorizer",
                "iot:CreateCertificateFromCsr",
                "iot:CreateJob",
                "iot:CreateKeysAndCertificate",
                "iot:CreateOTAUpdate",
                "iot:CreatePolicy",
                "iot:CreatePolicyVersion",
                "iot:CreateRoleAlias",
                "iot:CreateStream",
                "iot:CreateThing",
                "iot:CreateThingGroup",
                "iot:CreateThingType",
                "iot:CreateTopicRule",
                "iot:DeleteAuthorizer",
                "iot:DeleteCACertificate",
                "iot:DeleteCertificate",
                "iot:DeleteJob",
                "iot:DeleteJobExecution",
                "iot:DeleteOTAUpdate",
                "iot:DeletePolicy",
                "iot:DeletePolicyVersion",
                "iot:DeleteRegistrationCode",
                "iot:DeleteRoleAlias",
                "iot:DeleteStream",
                "iot:DeleteThing",
                "iot:DeleteThingGroup",
                "iot:DeleteThingType",
                "iot:DeleteTopicRule",
                "iot:DeleteV2LoggingLevel",
                "iot:DeprecateThingType",
                "iot:DescribeAuthorizer",
                "iot:DescribeCACertificate",
                "iot:DescribeCertificate",
                "iot:DescribeDefaultAuthorizer",
                "iot:DescribeEndpoint",
                "iot:DescribeEventConfigurations",
                "iot:DescribeIndex",
                "iot:DescribeJob",
                "iot:DescribeJobExecution",
                "iot:DescribeRoleAlias",
                "iot:DescribeStream",
                "iot:DescribeThing",
                "iot:DescribeThingGroup",
                "iot:DescribeThingRegistrationTask",
                "iot:DescribeThingType",
                "iot:DetachPolicy",
                "iot:DetachPrincipalPolicy",
                "iot:DetachThingPrincipal",
                "iot:DisableTopicRule",
                "iot:EnableTopicRule",
                "iot:GetEffectivePolicies",
                "iot:GetIndexingConfiguration",
                "iot:GetJobDocument",
                "iot:GetLoggingOptions",
                "iot:GetOTAUpdate",
                "iot:GetPolicy",
                "iot:GetPolicyVersion",
                "iot:GetRegistrationCode",
                "iot:GetTopicRule",
                "iot:GetV2LoggingOptions",
                "iot:ListAttachedPolicies",
                "iot:ListAuthorizers",
                "iot:ListCACertificates",
                "iot:ListCertificates",
                "iot:ListCertificatesByCA",
                "iot:ListIndices",
                "iot:ListJobExecutionsForJob",
                "iot:ListJobExecutionsForThing",
                "iot:ListJobs",
                "iot:ListOTAUpdates",
                "iot:ListOutgoingCertificates",
                "iot:ListPolicies",
                "iot:ListPolicyPrincipals",
                "iot:ListPolicyVersions",
                "iot:ListPrincipalPolicies",
                "iot:ListPrincipalThings",
                "iot:ListRoleAliases",
                "iot:ListStreams",
                "iot:ListTargetsForPolicy",
                "iot:ListThingGroups",
                "iot:ListThingGroupsForThing",
                "iot:ListThingPrincipals",
                "iot:ListThingRegistrationTaskReports",
                "iot:ListThingRegistrationTasks",
                "iot:ListThings",
                "iot:ListThingsInThingGroup",
                "iot:ListThingTypes",
                "iot:ListTopicRules",
                "iot:ListV2LoggingLevels",
                "iot:RegisterCACertificate",
                "iot:RegisterCertificate",
                "iot:RegisterThing",
                "iot:RejectCertificateTransfer",
                "iot:RemoveThingFromThingGroup",
                "iot:ReplaceTopicRule",
                "iot:SearchIndex",
                "iot:SetDefaultAuthorizer",
                "iot:SetDefaultPolicyVersion",
                "iot:SetLoggingOptions",
                "iot:SetV2LoggingLevel",
                "iot:SetV2LoggingOptions",
                "iot:StartThingRegistrationTask",
                "iot:StopThingRegistrationTask",
                "iot:TestAuthorization",
                "iot:TestInvokeAuthorizer",
                "iot:TransferCertificate",
                "iot:UpdateAuthorizer",
                "iot:UpdateCACertificate",
                "iot:UpdateCertificate",
                "iot:UpdateEventConfigurations",
                "iot:UpdateIndexingConfiguration",
                "iot:UpdateRoleAlias",
                "iot:UpdateStream",
                "iot:UpdateThing",
                "iot:UpdateThingGroup",
                "iot:UpdateThingGroupsForThing",
                "iot:UpdateAccountAuditConfiguration",
                "iot:DescribeAccountAuditConfiguration",
                "iot:DeleteAccountAuditConfiguration",
                "iot:StartOnDemandAuditTask",
                "iot:CancelAuditTask",
                "iot:DescribeAuditTask",
                "iot:ListAuditTasks",
                "iot:CreateScheduledAudit",
                "iot:UpdateScheduledAudit",
                "iot:DeleteScheduledAudit",
                "iot:DescribeScheduledAudit",
                "iot:ListScheduledAudits",
                "iot:ListAuditFindings",
                "iot:CreateSecurityProfile",
                "iot:DescribeSecurityProfile",
                "iot:UpdateSecurityProfile",
                "iot:DeleteSecurityProfile",
                "iot:AttachSecurityProfile",
                "iot:DetachSecurityProfile",
                "iot:ListSecurityProfiles",
                "iot:ListSecurityProfilesForTarget",
                "iot:ListTargetsForSecurityProfile",
                "iot:ListActiveViolations",
                "iot:ListViolationEvents",
                "iot:ValidateSecurityProfileBehaviors"
            ],
            "Resource": "*"
        }
    ]
}
```

## AWS managed policy: AWSIoTConfigReadOnlyAccess<a name="security-iam-awsmanpol-AWSIoTConfigReadOnlyAccess"></a>





You can attach the `AWSIoTConfigReadOnlyAccess` policy to your IAM identities\.



This policy grants the associated identity permissions that allow read\-only access to all AWS IoT configuration operations\. To view this policy in the AWS Management Console, see [AWSIoTConfigReadOnlyAccess](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIoTConfigReadOnlyAccess$jsonEditor?section=permissions)\.



**Permissions details**

This policy includes the following permissions\.




+ `iot` – Perform read\-only operations of IoT configuration actions\.



```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iot:DescribeAuthorizer",
                "iot:DescribeCACertificate",
                "iot:DescribeCertificate",
                "iot:DescribeDefaultAuthorizer",
                "iot:DescribeEndpoint",
                "iot:DescribeEventConfigurations",
                "iot:DescribeIndex",
                "iot:DescribeJob",
                "iot:DescribeJobExecution",
                "iot:DescribeRoleAlias",
                "iot:DescribeStream",
                "iot:DescribeThing",
                "iot:DescribeThingGroup",
                "iot:DescribeThingRegistrationTask",
                "iot:DescribeThingType",
                "iot:GetEffectivePolicies",
                "iot:GetIndexingConfiguration",
                "iot:GetJobDocument",
                "iot:GetLoggingOptions",
                "iot:GetOTAUpdate",
                "iot:GetPolicy",
                "iot:GetPolicyVersion",
                "iot:GetRegistrationCode",
                "iot:GetTopicRule",
                "iot:GetV2LoggingOptions",
                "iot:ListAttachedPolicies",
                "iot:ListAuthorizers",
                "iot:ListCACertificates",
                "iot:ListCertificates",
                "iot:ListCertificatesByCA",
                "iot:ListIndices",
                "iot:ListJobExecutionsForJob",
                "iot:ListJobExecutionsForThing",
                "iot:ListJobs",
                "iot:ListOTAUpdates",
                "iot:ListOutgoingCertificates",
                "iot:ListPolicies",
                "iot:ListPolicyPrincipals",
                "iot:ListPolicyVersions",
                "iot:ListPrincipalPolicies",
                "iot:ListPrincipalThings",
                "iot:ListRoleAliases",
                "iot:ListStreams",
                "iot:ListTargetsForPolicy",
                "iot:ListThingGroups",
                "iot:ListThingGroupsForThing",
                "iot:ListThingPrincipals",
                "iot:ListThingRegistrationTaskReports",
                "iot:ListThingRegistrationTasks",
                "iot:ListThings",
                "iot:ListThingsInThingGroup",
                "iot:ListThingTypes",
                "iot:ListTopicRules",
                "iot:ListV2LoggingLevels",
                "iot:SearchIndex",
                "iot:TestAuthorization",
                "iot:TestInvokeAuthorizer",
                "iot:DescribeAccountAuditConfiguration",
                "iot:DescribeAuditTask",
                "iot:ListAuditTasks",
                "iot:DescribeScheduledAudit",
                "iot:ListScheduledAudits",
                "iot:ListAuditFindings",
                "iot:DescribeSecurityProfile",
                "iot:ListSecurityProfiles",
                "iot:ListSecurityProfilesForTarget",
                "iot:ListTargetsForSecurityProfile",
                "iot:ListActiveViolations",
                "iot:ListViolationEvents",
                "iot:ValidateSecurityProfileBehaviors"
            ],
            "Resource": "*"
        }
    ]
}
```

## AWS managed policy: AWSIoTDataAccess<a name="security-iam-awsmanpol-AWSIoTDataAccess"></a>





You can attach the `AWSIoTDataAccess` policy to your IAM identities\.



This policy grants the associated identity permissions that allow access to all AWS IoT data operations\. Data operations send data over MQTT or HTTP protocols\. To view this policy in the AWS Management Console, see [https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIoTDataAccess?section=permissions](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIoTDataAccess?section=permissions)\.



**Permissions details**

This policy includes the following permissions\.




+ `iot` – Retrieve AWS IoT data and allow full access to AWS IoT messaging actions\.



```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iot:Connect",
                "iot:Publish",
                "iot:Subscribe",
                "iot:Receive",
                "iot:GetThingShadow",
                "iot:UpdateThingShadow",
                "iot:DeleteThingShadow",
                "iot:ListNamedShadowsForThing"
            ],
            "Resource": "*"
        }
    ]
}
```

## AWS managed policy: AWSIoTFullAccess<a name="security-iam-awsmanpol-AWSIoTFullAccess"></a>





You can attach the `AWSIoTFullAccess` policy to your IAM identities\.



This policy grants the associated identity permissions that allow access to all AWS IoT configuration and messaging operations\. To view this policy in the AWS Management Console, see [https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIoTFullAccess?section=permissions](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIoTFullAccess?section=permissions)\.



**Permissions details**

This policy includes the following permissions\.




+ `iot` – Retrieve AWS IoT data and allow full access to AWS IoT configuration and messaging actions\.
+ `iotjobsdata` – Retrieve AWS IoT Jobs data and allow full access to AWS IoT Jobs data plane API operations\.



```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iot:*",
                "iotjobsdata:*"
            ],
            "Resource": "*"
        }
    ]
}
```

## AWS managed policy: AWSIoTLogging<a name="security-iam-awsmanpol-AWSIoTLogging"></a>





You can attach the `AWSIoTLogging` policy to your IAM identities\.



This policy grants the associated identity permissions that allow access to create Amazon CloudWatch Logs groups and stream logs to the groups\. This policy is attached to your CloudWatch logging role\. To view this policy in the AWS Management Console, see [https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIoTLogging?section=permissions](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIoTLogging?section=permissions)\.



**Permissions details**

This policy includes the following permissions\.




+ `logs` – Retrieve CloudWatch logs\. Also allows creation of CloudWatch Logs groups and stream logs to the groups\.



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
                "logs:PutRetentionPolicy",
                "logs:GetLogEvents",
                "logs:DeleteLogStream"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

## AWS managed policy: AWSIoTOTAUpdate<a name="security-iam-awsmanpol-AWSIoTOTAUpdate"></a>





You can attach the `AWSIoTOTAUpdate` policy to your IAM identities\.



This policy grants the associated identity permissions that allow access to create AWS IoT jobs, AWS IoT code signing jobs, and to describe AWS code signer jobs\. To view this policy in the AWS Management Console, see [`AWSIoTOTAUpdate`\.](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIoTOTAUpdate?section=permissions)



**Permissions details**

This policy includes the following permissions\.




+ `iot` – Create AWS IoT jobs and code signing jobs\.
+ `signer` – Perform creation of AWS code signer jobs\.



```
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Action": [
            "iot:CreateJob",
            "signer:DescribeSigningJob"
        ],
        "Resource": "*"
    }
}
```

## AWS managed policy: AWSIoTRuleActions<a name="security-iam-awsmanpol-AWSIoTRuleActions"></a>





You can attach the `AWSIoTRuleActions` policy to your IAM identities\.



This policy grants the associated identity permissions that allow access to all AWS services supported in AWS IoT rule actions\. To view this policy in the AWS Management Console, see [https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIoTRuleActions?section=permissions](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIoTRuleActions?section=permissions)\.



**Permissions details**

This policy includes the following permissions\.




+ `iot` \- Perform actions for publishing rule action messages\.
+ `dynamodb` \- Insert a message into a DynamoDB table or split a message into multiple columns of a DynamoDB table\.
+ `s3` \- Store an object in an Amazon S3 bucket\.
+ `kinesis` \- Send a message to an Amazon Kinesis stream object\.
+ `firehose` \- Insert a record in a Kinesis Data Firehose stream object\.
+ `cloudwatch` \- Change CloudWatch alarm state or send message data to CloudWatch metric\.
+ `sns` \- Perform operation to publish a notification using Amazon SNS\. This operation is scoped to AWS IoT SNS topics\.
+ `sqs` \- Insert a message to add to the SQS queue\.
+ `es` \- Send a message to the OpenSearch Service service\.



```
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Action": [
            "dynamodb:PutItem",
            "kinesis:PutRecord",
            "iot:Publish",
            "s3:PutObject",
            "sns:Publish",
            "sqs:SendMessage*",
            "cloudwatch:SetAlarmState",
            "cloudwatch:PutMetricData",
            "es:ESHttpPut",
            "firehose:PutRecord"
        ],
        "Resource": "*"
    }
}
```

## AWS managed policy: AWSIoTThingsRegistration<a name="security-iam-awsmanpol-AWSIoTThingsRegistration"></a>





You can attach the `AWSIoTThingsRegistration` policy to your IAM identities\.



This policy grants the associated identity permissions that allow access to register things in bulk using the `StartThingRegistrationTask` API\. This policy can affect data processing and storage\. To view this policy in the AWS Management Console, see [https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIoTThingsRegistration?section=permissions](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIoTThingsRegistration?section=permissions)\.



**Permissions details**

This policy includes the following permissions\.




+ `iot` \- Perform actions for creating things and attaching policies and certificates when registering in bulk\.



```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iot:AddThingToThingGroup",
                "iot:AttachPolicy",
                "iot:AttachPrincipalPolicy",
                "iot:AttachThingPrincipal",
                "iot:CreateCertificateFromCsr",
                "iot:CreatePolicy",
                "iot:CreateThing",
                "iot:DescribeCertificate",
                "iot:DescribeThing",
                "iot:DescribeThingGroup",
                "iot:DescribeThingType",
                "iot:DetachPolicy",
                "iot:DetachThingPrincipal",
                "iot:GetPolicy",
                "iot:ListAttachedPolicies",
                "iot:ListPolicyPrincipals",
                "iot:ListPrincipalPolicies",
                "iot:ListPrincipalThings",
                "iot:ListTargetsForPolicy",
                "iot:ListThingGroupsForThing",
                "iot:ListThingPrincipals",
                "iot:RegisterCertificate",
                "iot:RegisterThing",
                "iot:RemoveThingFromThingGroup",
                "iot:UpdateCertificate",
                "iot:UpdateThing",
                "iot:UpdateThingGroupsForThing",
                "iot:AddThingToBillingGroup",
                "iot:DescribeBillingGroup",
                "iot:RemoveThingFromBillingGroup"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```





## AWS IoT updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>



View details about updates to AWS managed policies for AWS IoT since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the AWS IoT Document history page\.




| Change | Description | Date | 
| --- | --- | --- | 
|  [AWSIoTFullAccess](#security-iam-awsmanpol-AWSIoTFullAccess) – Update to an existing policy  |  AWS IoT added new permissions to allow users to access AWS IoT Jobs data plane API operations using the HTTP protocol\. A new IAM policy prefix, `iotjobsdata:`, provides you finer grained access control to access AWS IoT Jobs data plane endpoints\. For control plane API operations, you still use the `iot:` prefix\. For more information, see [AWS IoT Core policies for HTTPS protocol](iot-data-plane-jobs.md#iot-jobs-data-http)\.  | May 11, 2022 | 
|  AWS IoT started tracking changes  |  AWS IoT started tracking changes for its AWS managed policies\.  | May 11, 2022 | 