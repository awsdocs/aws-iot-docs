# How to Perform Audits<a name="device-defender-HowToProceed"></a>

1. Configure audit settings for your account\. Use [ UpdateAccountAuditConfiguration](AuditCommands.md#dd-api-iot-UpdateAccountAuditConfiguration) to enable those checks you want to be available for audits, set up optional notifications, and configure permissions\. 

   For some checks, AWS IoT starts collecting data as soon as the check is enabled\.

1. Create one or more audit schedules\. Use [ CreateScheduledAudit](AuditCommands.md#dd-api-iot-CreateScheduledAudit) to specify the checks you want to perform during an audit and how often these audits should be run\. 

   Or, you can run an on\-demand audit when necessary\. Use [ StartOnDemandAuditTask](AuditCommands.md#dd-api-iot-StartOnDemandAuditTask) to specify the checks you want to perform and start an audit running right away\. \(Results might not be ready for some time if you have recently enabled a check that is included in the on\-demand audit\.\)

1. You can use the [ AWS IoT console](https://console.aws.amazon.com/iot/home) to view the results of your audits\.

   Or, you can see the results of your audits with [ ListAuditFindings](AuditCommands.md#dd-api-iot-ListAuditFindings)\. With this command, you can filter the results by the type of check, a specific resource, or the time of the audit\. You can use this information to mitigate any problems found\.

1. You can apply mitigation actions that you defined in your AWS account to any noncompliant findings\. For more information, see [Apply Mitigation Actions](device-defender-mitigation-actions.md#defender-audit-apply-mitigation-actions)\.

## Notifications<a name="device-defender-audit-notifications"></a>

When an audit is completed, an SNS notification can be sent with a summary of the results of each audit check that was performed, including details about the number of noncompliant resources that were found\. Use the `auditNotificationTargetConfigurations` field in the input to the [ UpdateAccountAuditConfiguration](AuditCommands.md#dd-api-iot-UpdateAccountAuditConfiguration) command\. The SNS notification has the following payload:

### payload example<a name="device-defender-audit-notification-payload"></a>

```
{
    "accountId": "123456789012",
    "taskId": "4e2bcd1ccbc2a5dd15292a82ab80c380",
    "taskStatus": "FAILED|CANCELED|COMPLETED",
    "taskType": "ON_DEMAND_AUDIT_TASK|SCHEDULED_AUDIT_TASK",
    "scheduledAuditName": "myWeeklyAudit",
    "failedChecksCount": 0,
    "canceledChecksCount": 0,
    "nonCompliantChecksCount": 1,
    "compliantChecksCount": 0,
    "totalChecksCount": 1,
    "taskStartTime": 1524740766191,
    "auditDetails": [
        {
            "checkName": "DEVICE_CERT_APPROACHING_EXPIRATION_CHECK |
                          REVOKED_DEVICE_CERT_CHECK |
                          CA_CERT_APPROACHING_EXPIRATION_CHECK |
                          REVOKED_CA_CERT_CHECK |
                          DEVICE_CERTIFICATE_SHARED_CHECK |
                          IOT_POLICY_UNRESTRICTED_CHECK |
                          UNAUTHENTICATED_COGNITO_IDENTITY_UNRESTRICTED_ACCESS_CHECK |
                          AUTHENTICATED_COGNITO_IDENTITY_UNRESTRICTED_ACCESS_CHECK |
                          CONFLICTING_CLIENT_IDS_CHECK |
                          LOGGING_DISABLED_CHECK",
            
            "checkRunStatus": "FAILED |
                               CANCELED |
                               COMPLETED_COMPLIANT |
                               COMPLETED_NON_COMPLIANT",
            
            "nonCompliantResourcesCount": 1,
            "totalResourcesCount": 1,
            
            "message": "optional message if an error occurred",
            "errorCode": "INSUFFICIENT_PERMISSIONS|AUDIT_CHECK_DISABLED"
        }
    ]
}
```

### payload JSON schema<a name="device-defender-audit-notification-payload-schema"></a>

```
{
    "$schema": "http://json-schema.org/draft-07/schema#",
    "$id": "arn:aws:iot:::schema:auditnotification/1.0",
    "type": "object",
    "properties": {
        "accountId": {
            "type": "string"
        },
        "taskId": {
            "type": "string"
        },
        "taskStatus": {
            "type": "string",
            "enum": [
                "FAILED",
                "CANCELED",
                "COMPLETED"
            ]
        },
        "taskType": {
            "type": "string",
            "enum": [
                "SCHEDULED_AUDIT_TASK",
                "ON_DEMAND_AUDIT_TASK"
            ]
        },
        "scheduledAuditName": {
            "type": "string"
        },
        "failedChecksCount": {
            "type": "integer"
        },
        "canceledChecksCount": {
            "type": "integer"
        },
        "nonCompliantChecksCount": {
            "type": "integer"
        },
        "compliantChecksCount": {
            "type": "integer"
        },
        "totalChecksCount": {
            "type": "integer"
        },
        "taskStartTime": {
            "type": "integer"
        },
        "auditDetails": {
            "type": "array",
            "items": [
                {
                    "type": "object",
                    "properties": {
                        "checkName": {
                            "type": "string",
                            "enum": [
                                "DEVICE_CERT_APPROACHING_EXPIRATION_CHECK", 
                                "REVOKED_DEVICE_CERT_CHECK", 
                                "CA_CERT_APPROACHING_EXPIRATION_CHECK", 
                                "REVOKED_CA_CERT_CHECK", 
                                "LOGGING_DISABLED_CHECK"
                            ]
                        },
                        "checkRunStatus": {
                            "type": "string",
                            "enum": [
                                "FAILED",
                                "CANCELED",
                                "COMPLETED_COMPLIANT",
                                "COMPLETED_NON_COMPLIANT"
                            ]
                        },
                        "nonCompliantResourcesCount": {
                            "type": "integer"
                        },
                        "totalResourcesCount": {
                            "type": "integer"
                        },
                        "message": {
                            "type": "string",
                        },
                        "errorCode": {
                            "type": "string",
                            "enum": [
                                "INSUFFICIENT_PERMISSIONS",
                                "AUDIT_CHECK_DISABLED"
                            ]
                        }
                    },
                    "required": [
                        "checkName",
                        "checkRunStatus",
                        "nonCompliantResourcesCount",
                        "totalResourcesCount"
                    ]
                }
            ]
        }
    },
    "required": [
        "accountId",
        "taskId",
        "taskStatus",
        "taskType",
        "failedChecksCount",
        "canceledChecksCount",
        "nonCompliantChecksCount",
        "compliantChecksCount",
        "totalChecksCount",
        "taskStartTime",
        "auditDetails"
    ]
}
```

You can also view notifications in the AWS IoT console, along with information about the device, device statistics \(for example, last connection time, number of active connections, data transfer rate\), and historical alerts for the device\.

## Permissions<a name="device-defender-audit-permissions"></a>

This section contains information about how to set up the IAM roles and policies required to create, run, and manage AWS IoT Device Defender audits\. For more information, see the [AWS Identity and Access Management User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)\.

### Give AWS IoT Device Defender permission to collect your data to run an audit<a name="device-defender-audit-permissions-collect"></a>

When you call [ UpdateAccountAuditConfiguration](AuditCommands.md#dd-api-iot-UpdateAccountAuditConfiguration), you must specify an IAM role with two policies: a permissions policy and a trust policy\. The permissions policy grants AWS IoT Device Defender permission to access your account data, using AWS IoT APIs, when it runs an audit\. The trust policy grants AWS IoT Device Defender permission to assume the required role\. 

#### permissions policy<a name="audit-account-permissions-policy"></a>

```
{
    "Version":"2012-10-17",
    "Statement":[
        {
            "Effect":"Allow",
            "Action":[
                "iot:GetLoggingOptions",
                "iot:GetV2LoggingOptions",
                "iot:ListCACertificates",
                "iot:ListCertificates",
                "iot:DescribeCACertificate",
                "iot:DescribeCertificate",
                "iot:ListPolicies",
                "iot:GetPolicy",
                "iot:GetEffectivePolicies",
                "iot:ListRoleAliases",
                "iot:DescribeRoleAlias",
                "cognito-identity:GetIdentityPoolRoles",
                "iam:ListRolePolicies",
                "iam:ListAttachedRolePolicies",
                "iam:GetRole",
                "iam:GetPolicy",
                "iam:GetPolicyVersion",
                "iam:GetRolePolicy",
                "iam:GenerateServiceLastAccessedDetails",
                "iam:GetServiceLastAccessedDetails"
            ],
            "Resource":[
                "*"
            ]
        }
    ]
}
```

#### trust policy<a name="audit-account-trust-policy"></a>

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

### Give AWS IoT Device Defender permission to publish notifications to an SNS topic<a name="device-defender-audit-permissions-publish"></a>

If you use the `auditNotificationTargetConfigurations` parameter in [ UpdateAccountAuditConfiguration](AuditCommands.md#dd-api-iot-UpdateAccountAuditConfiguration), you must specify an IAM role with two policies: a permissions policy and a trust policy\. The permissions policy grants permission to AWS IoT Device Defender to publish notifications to your SNS topic\. The trust policy grants AWS IoT Device Defender permission to assume the required role\.

#### permissions policy<a name="audit-account-sns-permissions-policy"></a>

```
{
  "Version":"2012-10-17",
  "Statement":[
      {
        "Effect":"Allow",
        "Action":[
          "sns:Publish"
        ],
        "Resource":[
          "arn:aws:sns:region:account-id:your-topic-name"
        ]
    }
  ]
}
```

#### trust policy<a name="audit-account-sns-trust-policy"></a>

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

### Give IAM users or groups permission to run AWS IoT Device Defender audit commands<a name="device-defender-audit-permissions-manage"></a>

To allow IAM users or groups to manage, run, or view the results of AWS IoT Device Defender, you must create and assign roles with attached policies that grant permission to run the appropriate commands\. The content of each policy depends on which commands you want the user or group to run\. 
+ `UpdateAccountAuditConfiguration`

#### policy<a name="audit-manage1-policy"></a>

You must create the IAM role with the attached policy in same account from which this command is run\. Cross account access is not allowed\. The policy should have `iam:PassRole` permissions \(permissions to pass this role\)\. 

In the following policy template, `audit-permissions-role-arn` is the role ARN that you pass to AWS IoT Device Defender in the `UpdateAccountAuditConfiguration` request using the `roleArn` parameter\. `audit-notifications-permissions-role-arn` is the role ARN that you pass to AWS IoT Device Defender in the `UpdateAccountAuditConfiguration` request using the `auditNotificationTargetConfigurations` parameter\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:UpdateAccountAuditConfiguration"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:PassRole"
      ],
      "Resource": [
        "arn:aws:iam::account-id:role/audit-permissions-role-arn",
        "arn:aws:iam::account-id:role/audit-notifications-permissions-role-arn"
      ]
    }
  ]
}
```
+ `DescribeAccountAuditConfiguration`
+ `DeleteAccountAuditConfiguration`
+ `StartOnDemandAuditTask`
+ `CancelAuditTask`
+ `DescribeAuditTask`
+ `ListAuditTasks`
+ `ListScheduledAudits`
+ `ListAuditFindings`

#### policy<a name="audit-manage2-policy"></a>

All of these commands require \* in the `Resource` field of the policy\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:DescribeAccountAuditConfiguration",
        "iot:DeleteAccountAuditConfiguration",
        "iot:StartOnDemandAuditTask",
        "iot:CancelAuditTask",
        "iot:DescribeAuditTask",
        "iot:ListAuditTasks",
        "iot:ListScheduledAudits",
        "iot:ListAuditFindings"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```
+ `CreateScheduledAudit`
+ `UpdateScheduledAudit`
+ `DeleteScheduledAudit`
+ `DescribeScheduledAudit`

#### policy<a name="audit-manage3-policy"></a>

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:CreateScheduledAudit",
        "iot:UpdateScheduledAudit",
        "iot:DeleteScheduledAudit",
        "iot:DescribeScheduledAudit"
      ],
      "Resource": [
          "arn:aws:iot:region:account-id:scheduledaudit/scheduled-audit-name"
      ]
    }
  ]
}
```

The format for an AWS IoT Device Defender scheduled audit role ARN is:

```
arn:aws:iot:region:account-id:scheduledaudit/scheduled-audit-name
```