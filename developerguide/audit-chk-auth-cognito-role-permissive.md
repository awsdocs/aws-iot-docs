# Authenticated Cognito role overly permissive<a name="audit-chk-auth-cognito-role-permissive"></a>

A policy attached to an authenticated Amazon Cognito identity pool role is considered too permissive because it grants permission to perform the following AWS IoT actions:
+ Manage or modify things\.
+ Manage non\-thing related data or resources\.

Or, because it grants permission to perform the following AWS IoT actions on a broad set of devices:
+ Read thing administrative data\.
+ Use MQTT to connect/publish/subscribe to reserved topics \(including shadow or job execution data\)\.
+ Use API commands to read or modify shadow or job execution data\.

In general, devices that connect using an authenticated Amazon Cognito identity pool role should have only limited permission to read thing\-specific administrative data, publish and subscribe to thing\-specific MQTT topics, or use the API commands to read and modify thing\-specific data related to shadow or job execution data\.

This check appears as `AUTHENTICATED_COGNITO_ROLE_OVERLY_PERMISSIVE_CHECK` in the CLI and API\.

Severity: **Critical**

## Details<a name="audit-chk-auth-cognito-role-permissive-details"></a>

For this check, AWS IoT Device Defender audits all Amazon Cognito identity pools that have been used to connect to the AWS IoT message broker during the 31 days before the audit execution\. All Amazon Cognito identity pools from which either an authenticated or unauthenticated Amazon Cognito identity connected are included in the audit\.

The following reason codes are returned when this check finds a noncompliant authenticated Amazon Cognito identity pool role:
+ ALLOWS\_BROAD\_ACCESS\_TO\_IOT\_THING\_ADMIN\_READ\_ACTIONS
+ ALLOWS\_ACCESS\_TO\_IOT\_NON\_THING\_ADMIN\_ACTIONS
+ ALLOWS\_ACCESS\_TO\_IOT\_THING\_ADMIN\_WRITE\_ACTIONS

## Why it matters<a name="audit-chk-auth-cognito-role-permissive-why-it-matters"></a>

If an authenticated identity is compromised, it can use administrative actions to modify account settings, delete resources, or gain access to sensitive data\.

## How to fix it<a name="audit-chk-auth-cognito-role-permissive-how-to-fix"></a>

A policy attached to an authenticated Amazon Cognito identity pool role should grant only those permissions required for a device to do its job\. We recommend the following steps:

1. Create a new compliant role\.

1. Create a Amazon Cognito identity pool and attach the compliant role to it\.

1. Verify that your identities can access AWS IoT using the new pool\.

1. After verification is complete, attach the role to the Amazon Cognito identity pool that was flagged as noncompliant\.

You can also use mitigation actions to:
+ Apply the `PUBLISH_FINDINGS_TO_SNS` mitigation action to implement a custom response in response to the Amazon SNS message\. 

For more information, see [Mitigation actions](dd-mitigation-actions.md)\. 

## Manage or modify things<a name="audit-chk-auth-cognito-role-permissive-manage-things"></a>

The following AWS IoT API actions are used to manage or modify things so permission to perform these should not be granted to devices connecting through an authenticated Amazon Cognito identity pool:
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

## Manage non\-things<a name="audit-chk-auth-cognito-role-permissive-manage-non-things"></a>

Devices that connect through an authenticated Amazon Cognito identity pool should not be given permission to perform AWS IoT API actions other than those discussed in these sections\. To manage your account with an application that connects through an authenticated Amazon Cognito identity pool, create a separate identity pool not used by devices\.

## Read thing administrative data<a name="audit-chk-auth-cognito-role-permissive-read-things-admin-data"></a>

The following AWS IoT API actions are used to read thing data, so devices that connect through an authenticated Amazon Cognito identity pool should be given permission to perform these on a limited set of things only:
+ `DescribeThing`
+ `ListJobExecutionsForThing`
+ `ListThingGroupsForThing`
+ `ListThingPrincipals`
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

  This allows the device to perform the specified actions on only one thing\.
+ compliant:

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
          "arn:aws:iot:region:account-id:/thing/MyThing*"
        ]
      }
    ]
  }
  ```

  This is compliant because, although the resource is specified with a wildcard \(\*\), it is preceded by a specific string, and that limits the set of things accessed to those with names that have the given prefix\.
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

  This allows the device to perform the specified actions on only one thing\.
+ compliant:

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
          "arn:aws:iot:region:account-id:/thing/MyThing*"
        ]
      }
    ]
  }
  ```

  This is compliant because, although the resource is specified with a wildcard \(\*\), it is preceded by a specific string, and that limits the set of things accessed to those with names that have the given prefix\.

## Subscribe/publish to MQTT topics<a name="audit-chk-auth-cognito-role-permissive-mqtt-topic"></a>

MQTT messages are sent through the AWS IoT message broker and are used by devices to perform many different actions, including accessing and modifying shadow state and job execution state\. A policy that grants permission to a device to connect, publish, or subscribe to MQTT messages should restrict these actions to specific resources as follows:

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

  The resource specification contains a variable that matches the device name used to connect, and the condition statement further restricts the permission by checking that the certificate used by the MQTT client matches that attached to the thing with the name used\.

**Publish**  
+ noncompliant:

  ```
  arn:aws:iot:region:account-id:topic/$aws/things/*/shadow/update
  ```

  This allows the device to update the shadow of any device \(\* = all devices\)\.

  ```
  arn:aws:iot:region:account-id:topic/$aws/things/*
  ```

  This allows the device to read/update/delete the shadow of any device\.
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
  arn:aws:iot:region:account-id:topicfilter/$aws/things/#
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

  This is compliant because the device can receive messages only from topics on which it has permission to subscribe\.

## Read or modify shadow or job data<a name="audit-chk-auth-cognito-role-permissive-shadow-job-data"></a>

A policy that grants permission to a device to perform an API action to access or modify device shadows or job execution data should restrict these actions to specific resources\. The following are the API actions:
+ `DeleteThingShadow`
+ `GetThingShadow`
+ `UpdateThingShadow`
+ `DescribeJobExecution`
+ `GetPendingJobExecutions`
+ `StartNextPendingJobExecution`
+ `UpdateJobExecution`

**Examples**
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

  This allows the device to perform the specified actions on only two things\.