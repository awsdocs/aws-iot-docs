# Unauthenticated Cognito role overly permissive<a name="audit-chk-unauth-cognito-role-permissive"></a>

A policy attached to an unauthenticated Amazon Cognito identity pool role is considered too permissive because it grants permission to perform any of the following AWS IoT actions:
+ Manage or modify things\.
+ Read thing administrative data\.
+ Manage non\-thing related data or resources\.

Or, because it grants permission to perform the following AWS IoT actions on a broad set of devices:
+ Use MQTT to connect, publish, or subscribe to reserved topics \(including shadow or job execution data\)\.
+ Use API commands to read or modify shadow or job execution data\.

In general, devices that connect using an unauthenticated Amazon Cognito identity pool role should have only limited permission to publish and subscribe to thing\-specific MQTT topics or use the API commands to read and modify thing\-specific data related to shadow or job execution data\.

This check appears as `UNAUTHENTICATED_COGNITO_ROLE_OVERLY_PERMISSIVE_CHECK` in the CLI and API\.

Severity: **Critical**

## Details<a name="audit-chk-unauth-cognito-role-permissive-details"></a>

For this check, AWS IoT Device Defender audits all Amazon Cognito identity pools that have been used to connect to the AWS IoT message broker during the 31 days before the audit execution\. All Amazon Cognito identity pools from which either an authenticated or unauthenticated Amazon Cognito identity connected are included in the audit\.

The following reason codes are returned when this check finds a noncompliant unauthenticated Amazon Cognito identity pool role:
+ ALLOWS\_ACCESS\_TO\_IOT\_ADMIN\_ACTIONS
+ ALLOWS\_BROAD\_ACCESS\_TO\_IOT\_DATA\_PLANE\_ACTIONS

## Why it matters<a name="audit-chk-unauth-cognito-role-permissive-why-it-matters"></a>

Because unauthenticated identities are never authenticated by the user, they pose a much greater risk than authenticated Amazon Cognito identities\. If an unauthenticated identity is compromised, it can use administrative actions to modify account settings, delete resources, or gain access to sensitive data\. Or, with broad access to device settings, it can access or modify shadows and jobs for all devices in your account\. A guest user might use the permissions to compromise your entire fleet or launch a DDOS attack with messages\.

## How to fix it<a name="audit-chk-unauth-cognito-role-permissive-how-to-fix"></a>

A policy attached to an unauthenticated Amazon Cognito identity pool role should grant only those permissions required for a device to do its job\. We recommend the following steps:

1. Create a new compliant role\.

1. Create a Amazon Cognito identity pool and attach the compliant role to it\.

1. Verify that your identities can access AWS IoT using the new pool\.

1. After verification is complete, attach the compliant role to the Amazon Cognito identity pool that was flagged as noncompliant\.

You can also use mitigation actions to:
+ Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action to implement a custom response in response to the Amazon SNS message\. 

For more information, see [Mitigation actions](device-defender-mitigation-actions.md)\. 

## Manage or modify things<a name="manage-modify-things-check"></a>

The following AWS IoT API actions are used to manage or modify things\. Permission to perform these actions should not be granted to devices that connect through an unauthenticated Amazon Cognito identity pool\.
+ `AddThingToThingGroup` 
+ `AttachThingPrincipal` 
+ `CreateThing` 
+ `DeleteThing` 
+ `DetachThingPrincipal` 
+ `ListThings` 
+ `ListThingsInThingGroup` 
+ `RegisterThing` 
+ `RemoveThingFromThingGroup` 
+ `UpdateThing` 
+ `UpdateThingGroupsForThing` 

Any role that grants permission to perform these actions on even a single resource is considered noncompliant\.

## Read thing administrative data<a name="read-thing-admin-data-check"></a>

The following AWS IoT API actions are used to read or modify thing data\. Devices that connect through an unauthenticated Amazon Cognito identity pool should not be given permission to perform these actions\.
+ `DescribeThing`
+ `ListJobExecutionsForThing`
+ `ListThingGroupsForThing`
+ `ListThingPrincipals`

**Example**  
+ noncompliant:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [ 
            "iot:DescribeThing",
            "iot:ListJobExecutionsForThing",
            "iot:ListThingGroupsForThing",
            "iot:ListThingPrincipals"
        ],
        "Resource": [
          "arn:aws:iot:region:account-id:/thing/MyThing"
        ]
      }
    ]
  }
  ```

  This allows the device to perform the specified actions even though it is granted for one thing only\.

## Manage non\-things<a name="manage-non-things-check"></a>

Devices that connect through an unauthenticated Amazon Cognito identity pool should not be given permission to perform AWS IoT API actions other than those discussed in these sections\. You can manage your account with an application that connects through an unauthenticated Amazon Cognito identity pool by creating a separate identity pool not used by devices\.

## Subscribe/publish to MQTT topics<a name="audit-chk-unauth-cognito-role-permissive-mqtt-topics"></a>

MQTT messages are sent through the AWS IoT message broker and are used by devices to perform many actions, including accessing and modifying shadow state and job execution state\. A policy that grants permission to a device to connect, publish, or subscribe to MQTT messages should restrict these actions to specific resources as follows:

**Connect**  
+ noncompliant:

  ```
  arn:aws:iot:region:account-id:client/*
  ```

  The wildcard \* allows any device to connect to AWS IoT\.

  ```
  arn:aws:iot:region:account-id:client/${iot:ClientId}
  ```

  Unless `iot:Connection.Thing.IsAttached` is set to true in the condition keys, this is equivalent to the wildcard \* in the previous example\.
+ compliant:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [ "iot:Connect" ],
        "Resource": [
          "arn:aws:iot:region:account-id:client/${iot:Connection.Thing.ThingName}"
        ],
        "Condition": {
          "Bool": { "iot:Connection.Thing.IsAttached": "true" }
        }
      }
    ]
  }
  ```

  The resource specification contains a variable that matches the device name used to connect\. The condition statement further restricts the permission by checking that the certificate used by the MQTT client matches that attached to the thing with the name used\.

**Publish**  
+ noncompliant:

  ```
  arn:aws:iot:region:account-id:topic/$aws/things/*/shadow/update
  ```

  This allows the device to update the shadow of any device \(\* = all devices\)\.

  ```
  arn:aws:iot:region:account-id:topic/$aws/things/*
  ```

  This allows the device to read, update, or delete the shadow of any device\.
+ compliant:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [ "iot:Publish" ],
        "Resource": [
          "arn:aws:iot:region:account-id:topic/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
        ],
      }
    ]
  }
  ```

  The resource specification contains a wildcard, but it only matches any shadow\-related topic for the device whose thing name is used to connect\.

**Subscribe**  
+ noncompliant:

  ```
  arn:aws:iot:region:account-id:topicfilter/$aws/things/*
  ```

  This allows the device to subscribe to reserved shadow or job topics for all devices\.

  ```
  arn:aws:iot:region:account-id:topicfilter/$aws/things/*
  ```

  The same as the previous example, but using the \# wildcard\.

  ```
  arn:aws:iot:region:account-id:topic/$aws/things/+/shadow/update
  ```

  This allows the device to see shadow updates on any device \(\+ = all devices\)\.
+ compliant:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [ "iot:Subscribe" ],
        "Resource": [
          "arn:aws:iot:region:account-id:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
          "arn:aws:iot:region:account-id:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/jobs/*"
        ],
      }
    ]
  }
  ```

  The resource specifications contain wildcards, but they only match any shadow\-related topic and any job\-related topic for the device whose thing name is used to connect\.

**Receive**  
+ compliant:

  ```
  arn:aws:iot:region:account-id:topicfilter/$aws/things/*
  ```

  This is allowed because the device can receive messages only from topics on which it has permission to subscribe\.

## Read/modify shadow or job data<a name="read-modify-shadow-job-data-check"></a>

A policy that grants permission to a device to perform an API action to access or modify device shadows or job execution data should restrict these actions to specific resources\. The following are the API actions:
+ `DeleteThingShadow`
+ `GetThingShadow`
+ `UpdateThingShadow`
+ `DescribeJobExecution`
+ `GetPendingJobExecutions`
+ `StartNextPendingJobExecutio`n
+ `UpdateJobExecution`

**Example**  
+ noncompliant:

  ```
  arn:aws:iot:region:account-id:thing/*
  ```

  This allows the device to perform the specified action on any thing\.
+ compliant:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [ 
            "iot:DeleteThingShadow",
            "iot:GetThingShadow",
            "iot:UpdateThingShadow",
            "iot:DescribeJobExecution",
            "iot:GetPendingJobExecutions",
            "iot:StartNextPendingJobExecution",
            "iot:UpdateJobExecution"
        ],
        "Resource": [
          "arn:aws:iot:region:account-id:/thing/MyThing1",
          "arn:aws:iot:region:account-id:/thing/MyThing2"
        ]
      }
    ]
  }
  ```

  This allows the device to perform the specified actions on two things only\.