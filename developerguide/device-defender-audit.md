# Audit<a name="device-defender-audit"></a>

An audit looks at account and device\-related settings and policies to ensure security measures are in place\. An audit can help you detect any drifts from security best practices or proper access policies, such as multiple devices using the same identity, or overly\-permissive policies that allow one device to read and update data for many other devices\. You can run audits as needed \("on\-demand audits"\) or schedule them to be run periodically \("scheduled audits"\)\. 

An audit runs a set of pre\-defined checks \([Audit checks](#device-defender-audit-checks)\) for common IoT security best practices and device vulnerabilities\. Examples of pre\-defined checks include policies that grant permission to read or update data on multiple device's, devices that share an identity \(X\.509 certificate\), or certificates that are expiring or have been revoked but are still active\.

## Audit checks<a name="device-defender-audit-checks"></a>

**Note**  
When a check is enabled, data collection begins immediately\. If you have a large amount of data in your account to be collected, then results of the check may not be available for some time after you have enabled it\.

The following audit checks are supported:

------
#### [ REVOKED\_CA\_CERT\_CHECK ]

A CA certificate was revoked but is still active in AWS IoT\.

Severity: **Critical**

------
#### [ more info \(1\) ]

A CA certificate is marked as revoked in the Certificate Revocation List maintained by the issuing authority, but is still marked as "ACTIVE" or "PENDING\_TRANSFER" in the AWS IoT system\.

The following reason codes are returned when this check finds a non\-compliant CA certificate:

+ CERTIFICATE\_REVOKED\_BY\_ISSUER

------
#### [ why it matters \(1\) ]

A revoked CA certificate should no longer be used to sign device certificates\. It may have been revoked because it was compromised, and newly added devices with certificates signed using this CA certificate might pose a security threat\. 

------
#### [ how to fix it \(1\) ]

1. Mark the CA certificate as "INACTIVE" in the AWS IoT system using [ UpdateCACertificate](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-UpdateCACertificate.html)\.

1. Review the device certificate registration activity for the time after the CA certificate was revoked and consider revoking any device certificates that may have been issued with it during this time\. \(Use [ ListCertificatesByCA](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-ListCertificatesByCA.html) to list the device certificates signed by the CA certificate and [ UpdateCertificate](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-UpdateCertificate.html) to revoke a device certificate\.\)

------

 

------
#### [ DEVICE\_CERTIFICATE\_SHARED\_CHECK ]

Multiple, concurrent connections use the same X\.509 certificate to authenticate with the AWS IoT service\.

Severity: **Critical**

------
#### [ more info \(2\) ]

When this check is enabled, data collection begins immediately, but results of the check are not available for at least two hours\.

When performed as part of an on\-demand audit, this check looks at the certificates and clientIDs that were used by devices to connect during the 31 days prior to the start of the audit\. For scheduled audits, this check looks at data from the last time the audit was run to the time this instance of the audit started\. If you have taken steps to mitigate this condition during the time checked, note when the concurrent connections were made to determine if the problem persists\.

The following reason codes are returned when this check finds a non\-compliant certificate:

+ CERTIFICATE\_SHARED\_BY\_MULTIPLE\_DEVICES

In addition, the findings returned by this check include the ID of the shared certificate, the IDs of the clients using the certificate to connect, and the connect/disconnect times\. Results are listed in order of most recent first\.

------
#### [ why it matters \(2\) ]

Each device should have a unique certificate to authenticate with AWS IoT\. When multiple devices use the same certificate this may indicate that a device has been compromised and its identity cloned in order to further compromise the system\. 

------
#### [ how to fix it \(2\) ]

Verify that the device certificate has not been compromised\. If it has, follow your security best practices to mitigate the situation\. 

If you are using the same certificate on multiple devices, you may want to:

1. Provision new, unique certificates and attach them to each device\. 

1. Verify that the new certificates are valid and the devices are able to use them to connect\.

1. Mark the old certificate as "REVOKED" in the AWS IoT system using [ UpdateCertificate](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-UpdateCertificate.html)

1. Detach the old certificate from each of the devices\.

------

 

------
#### [ UNAUTHENTICATED\_COGNITO\_ROLE\_OVERLY\_PERMISSIVE\_CHECK ]

A policy attached to an unauthenticated Cognito identity pool role is considered overly\-permissive because it grants permission to perform any of the following AWS IoT actions:

+ manage or modify things

+ read thing administrative data

+ manage non\-thing related data or resources

Or, because it grants permission to perform the following AWS IoT actions on a broad set of devices:

+ use MQTT to connect/publish/subscribe to reserved topics \(including shadow or job execution data\)

+ use API commands to read or modify shadow or job execution data

In general, devices which connect using an unauthenticated Cognito identity pool role should have only limited permission to publish/subscribe to thing\-specific MQTT topics or use the API commands to read/modify thing\-specific data related to shadow or job execution data\.

Severity: **Critical**

------
#### [ manage or modify things \(3\) ]

The following AWS IoT API actions are used to manage or modify things so permission to perform these should not be granted to devices connecting via an unauthenticated Cognito identity pool:

+ AddThingToThingGroup 

+ AttachThingPrincipal 

+ CreateThing 

+ DeleteThing 

+ DetachThingPrincipal 

+ ListThings 

+ ListThingsInThingGroup 

+ RegisterThing 

+ RemoveThingFromThingGroup 

+ UpdateThing 

+ UpdateThingGroupsForThing 

Any role that grants permission to perform these actions on even a single resource is considered non\-compliant\.

------
#### [ read thing administrative data \(3\) ]

The following AWS IoT API actions are used to read or modify thing data so devices connecting via an unauthenticated Cognito identity pool should not be given permission to perform these:

+ DescribeThing

+ ListJobExecutionsForThing

+ ListThingGroupsForThing

+ ListThingPrincipals

**Example:**

+ non\-compliant:

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
          "arn:aws:iot:<region>:<account-id>:/thing/MyThing"
        ]
      }
    ]
  }
  ```

  This allows the device to perform the specified actions even though it is granted for only one specific thing\.

------
#### [ manage non\-things \(3\) ]

Devices connecting via an unauthenticated Cognito identity pool should not be given permission to perform any other AWS IoT API actions other than those discussed in these sections\. In order to manage your account with an application that connects via an unauthenticated Cognito identity pool, create a separate identity pool not used by devices\.

------
#### [ subscribe/publish to MQTT topics \(3\) ]

MQTT messages are sent through the AWS IoT message broker and are used by devices to perform many different actions, including accessing and modifying shadow state and job execution state\. A policy that grants permission to a device to connect, publish or subscribe to MQTT messages should restrict these actions to specific resources as follows:

Connect  

+ non\-compliant:

  ```
  arn:aws:iot:<region>:<account-id>:client/*
  ```

  The wildcard \* allows any device to connect to AWS IoT\.

  ```
  arn:aws:iot:<region>:<account-id>:client/${iot:ClientId}
  ```

  Unless "iot:Connection\.Thing\.IsAttached" is set to true in the condition keys, this is equivalent to the wildcard \* as in the previous example\.

+ compliant:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [ "iot:Connect" ],
        "Resource": [
          "arn:aws:iot:<region>:<account-id>:client/${iot:Connection.Thing.ThingName}"
        ],
        "Condition": {
          "Bool": { "iot:Connection.Thing.IsAttached": "true" }
        }
      }
    ]
  }
  ```

  The resource specification contains a variable that matches the device name used to connect, and the condition statement further restricts the permission by checking that the certificate used by the MQTT client matches that attached to the thing with the name used\.

Publish  

+ non\-compliant:

  ```
  arn:aws:iot:<region>:<account-id>:topic/$aws/things/*/shadow/update
  ```

  This allows the device to update the shadow of any device \(\* = all devices\)\.

  ```
  arn:aws:iot:<region>:<account-id>:topic/$aws/things/*
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
          "arn:aws:iot:<region>:<account-id>:topic/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
        ],
      }
    ]
  }
  ```

  The resource specification contains a wildcard, but it only matches any shadow\-related topic for the device whose thing name is used to connect\.

Subscribe  

+ non\-compliant:

  ```
  arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/*
  ```

  This allows the device to subscribe to reserved shadow or job topics for all devices\.

  ```
  arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/*
  ```

  The same as the previous example, but using the \# wildcard\.

  ```
  arn:aws:iot:<region>:<account-id>:topic/$aws/things/+/shadow/update
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
          "arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
          "arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/jobs/*"
        ],
      }
    ]
  }
  ```

  The resource specifications contain wildcards but they only match any shadow\-related topic and any job\-related topic for the device whose thing name is used to connect\.

Receive  

+ compliant:

  ```
  arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/*
  ```

  This is okay because the device can only receive messages from topics on which it has permission to subscribe\.

------
#### [ read/modify shadow or job data \(3\) ]

A policy that grants permission to a device to perform an API action to access or modify thing shadows or job execution data should restrict these actions to specific resources\. The API actions are:

+ DeleteThingShadow

+ GetThingShadow

+ UpdateThingShadow

+ DescribeJobExecutions

+ GetPendingJobExecutions

+ StartNextPendingJobExecution

+ UpdateJobExecution

**Examples:**

+ non\-compliant:

  ```
  arn:aws:iot:<region>:<account-id>:thing/*
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
          "arn:aws:iot:<region>:<account-id>:/thing/MyThing1",
          "arn:aws:iot:<region>:<account-id>:/thing/MyThing2"
        ]
      }
    ]
  }
  ```

  This allows the device to perform the specified actions on only two specific things\.

------
#### [ more info \(3\) ]

For this check, audits all Cognito identity pools that have been used to connect to the AWS IoT message broker during the past 31 days prior to the audit execution\. All Cognito identity pools from which either an authenticated or unauthenticated Cognito identity connected are included in the audit\.

The following reason codes are returned when this check finds a non\-compliant unauthenticated Cognito identity pool role:

+ ALLOWS\_ACCESS\_TO\_IOT\_ADMIN\_ACTIONS

+ ALLOWS\_BROAD\_ACCESS\_TO\_IOT\_DATA\_PLANE\_ACTIONS

------
#### [ why it matters \(3\) ]

Because unauthenticated identities are never authenticated by the user, they pose a much greater risk than authenticated Cognito identities\. If an unauthenticated identity is compromised it could use administrative actions to modify account settings, delete resources or gain access to sensitive data\. Or, with broad access to device settings, it could access or modify shadows and jobs for all devices in your account\. A guest user might use the permissions to compromise your entire fleet or launch a DDOS attack with messages\.

------
#### [ how to fix it \(3\) ]

A policy attached to an unauthenticated Cognito identity pool role should grant only those permissions required for a device to do its job\. We recommend the following steps:

1. Create a new compliant role\.

1. Create a new Cognito identity pool and attach the compliant role to it\.

1. Verify that your identities can access AWS IoT using the new pool\.

1. Once verification is complete, attach the new, compliant role to the Cognito identity pool that was flagged as non\-compliant\.

------

 

------
#### [ AUTHENTICATED\_COGNITO\_ROLE\_OVERLY\_PERMISSIVE\_CHECK ]

A policy attached to an authenticated Cognito identity pool role is considered overly\-permissive because it grants permission to perform the following AWS IoT actions:

+ manage or modify things

+ manage non\-thing related data or resources

Or, because it grants permission to perform the following AWS IoT actions on a broad set of devices:

+ read thing administrative data

+ use MQTT to connect/publish/subscribe to reserved topics \(including shadow or job execution data\)

+ use API commands to read or modify shadow or job execution data

In general, devices that connect using an authenticated Cognito identity pool role should have only limited permission to read thing\-specific administrative data, publish/subscribe to thing\-specific MQTT topics or use the API commands to read/modify thing\-specific data related to shadow or job execution data\.

Severity: **Critical**

------
#### [ manage or modify things \(4\) ]

The following AWS IoT API actions are used to manage or modify things so permission to perform these should not be granted to devices connecting via an authenticated Cognito identity pool:

+ AddThingToThingGroup 

+ AttachThingPrincipal 

+ CreateThing 

+ DeleteThing 

+ DetachThingPrincipal 

+ ListThings

+ ListThingsInThingGroup 

+ RegisterThing 

+ RemoveThingFromThingGroup 

+ UpdateThing 

+ UpdateThingGroupsForThing 

Any role that grants permission to perform these actions on even a single resource is considered non\-compliant\.

------
#### [ manage non\-things \(4\) ]

Devices connecting via an authenticated Cognito identity pool should not be given permission to perform any other AWS IoT API actions other than those discussed in these sections\. In order to manage your account with an application that connects via an authenticated Cognito identity pool, create a separate identity pool not used by devices\.

------
#### [ read thing administrative data \(4\) ]

The following AWS IoT API actions are used to read thing data so devices connecting via an authenticated Cognito identity pool should be given permission to perform these on only a limited set of things:

+ DescribeThing

+ ListJobExecutionsForThing

+ ListThingGroupsForThing

+ ListThingPrincipals

**Examples:**

+ non\-compliant:

  ```
  arn:aws:iot:<region>:<account-id>:thing/*
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
          "arn:aws:iot:<region>:<account-id>:/thing/MyThing"
        ]
      }
    ]
  }
  ```

  This allows the device to perform the specified actions on only one specific thing\.

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
          "arn:aws:iot:<region>:<account-id>:/thing/MyThing*"
        ]
      }
    ]
  }
  ```

  This is compliant because, although the resource is specified with a wildcard \(\*\), this is preceded by a specific string, and that limits the set of things accessed to those with names that have the given prefix\.

------
#### [ subscribe/publish to MQTT topics \(4\) ]

MQTT messages are sent through the AWS IoT message broker and are used by devices to perform many different actions, including accessing and modifying shadow state and job execution state\. A policy that grants permission to a device to connect, publish or subscribe to MQTT messages should restrict these actions to specific resources as follows:

Connect  

+ non\-compliant:

  ```
  arn:aws:iot:<region>:<account-id>:client/*
  ```

  The wildcard \* allows any device to connect to AWS IoT\.

  ```
  arn:aws:iot:<region>:<account-id>:client/${iot:ClientId}
  ```

  Unless "iot:Connection\.Thing\.IsAttached" is set to true in the condition keys, this is equivalent to the wildcard \* as in the previous example\.

+ compliant:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [ "iot:Connect" ],
        "Resource": [
          "arn:aws:iot:<region>:<account-id>:client/${iot:Connection.Thing.ThingName}"
        ],
        "Condition": {
          "Bool": { "iot:Connection.Thing.IsAttached": "true" }
        }
      }
    ]
  }
  ```

  The resource specification contains a variable that matches the device name used to connect, and the condition statement further restricts the permission by checking that the certificate used by the MQTT client matches that attached to the thing with the name used\.

Publish  

+ non\-compliant:

  ```
  arn:aws:iot:<region>:<account-id>:topic/$aws/things/*/shadow/update
  ```

  This allows the device to update the shadow of any device \(\* = all devices\)\.

  ```
  arn:aws:iot:<region>:<account-id>:topic/$aws/things/*
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
          "arn:aws:iot:<region>:<account-id>:topic/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
        ],
      }
    ]
  }
  ```

  The resource specification contains a wildcard, but it only matches any shadow\-related topic for the device whose thing name is used to connect\.

Subscribe  

+ non\-compliant:

  ```
  arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/*
  ```

  This allows the device to subscribe to reserved shadow or job topics for all devices\.

  ```
  arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/#
  ```

  The same as the previous example, but using the \# wildcard\.

  ```
  arn:aws:iot:<region>:<account-id>:topic/$aws/things/+/shadow/update
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
          "arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
          "arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/jobs/*"
        ],
      }
    ]
  }
  ```

  The resource specifications contain wildcards but they only match any shadow\-related topic and any job\-related topic for the device whose thing name is used to connect\.

Receive  

+ compliant:

  ```
  arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/*
  ```

  This is okay because the device can only receive messages from topics on which it has permission to subscribe\.

------
#### [ read/modify shadow or job data \(4\) ]

A policy that grants permission to a device to perform an API action to access or modify thing shadows or job execution data should restrict these actions to specific resources\. The API actions are:

+ DeleteThingShadow

+ GetThingShadow

+ UpdateThingShadow

+ DescribeJobExecutions

+ GetPendingJobExecutions

+ StartNextPendingJobExecution

+ UpdateJobExecution

**Examples:**

+ non\-compliant:

  ```
  arn:aws:iot:<region>:<account-id>:thing/*
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
          "arn:aws:iot:<region>:<account-id>:/thing/MyThing1",
          "arn:aws:iot:<region>:<account-id>:/thing/MyThing2"
        ]
      }
    ]
  }
  ```

  This allows the device to perform the specified actions on only two specific things\.

------
#### [ more info \(4\) ]

For this check, audits all Cognito identity pools that have been used to connect to the AWS IoT message broker during the past 31 days prior to the audit execution\. All Cognito identity pools from which either an authenticated or unauthenticated Cognito identity connected are included in the audit\.

The following reason codes are returned when this check finds a non\-compliant authenticated Cognito identity pool role:

+ ALLOWS\_BROAD\_ACCESS\_TO\_IOT\_THING\_ADMIN\_READ\_ACTIONS

+ ALLOWS\_ACCESS\_TO\_IOT\_NON\_THING\_ADMIN\_ACTIONS

+ ALLOWS\_ACCESS\_TO\_IOT\_THING\_ADMIN\_WRITE\_ACTIONS

------
#### [ why it matters \(4\) ]

If an authenticated identity is compromised it could use administrative actions to modify account settings, delete resources or gain access to sensitive data\.

------
#### [ how to fix it \(4\) ]

A policy attached to an authenticated Cognito identity pool role should grant only those permissions required for a device to do its job\. We recommend the following steps:

1. Create a new compliant role\.

1. Create a new Cognito identity pool and attach the compliant role to it\.

1. Verify that your identities can access AWS IoT using the new pool\.

1. Once verification is complete, attach the new, compliant role to the Cognito identity pool that was flagged as non\-compliant\.

------

 

------
#### [ IOT\_POLICY\_OVERLY\_PERMISSIVE\_CHECK ]

An AWS IoT policy gives permissions that are too broad/unrestricted\. It grants permission to send or receive MQTT messages for a broad set of devices, or grants permission to access or modify shadow and job execution data for a broad set of devices\.  

In general, a policy for a device should grant access to very specific resources associated with just that device and no, or very few, other devices\. With certain exceptions, using a wildcard \(for example, "\*"\) to specify resources in such a policy is considered too broad/unrestricted\.

Severity: **Critical**

------
#### [ MQTT permissions \(5\) ]

MQTT messages are sent through the AWS IoT message broker and are used by devices to perform many different actions, including accessing and modifying shadow state and job execution state\. A policy that grants permission to a device to connect, publish or subscribe to MQTT messages should restrict these actions to specific resources as follows:

Connect  

+ non\-compliant:

  ```
  arn:aws:iot:<region>:<account-id>:client/*
  ```

  The wildcard \* allows any device to connect to AWS IoT\.

  ```
  arn:aws:iot:<region>:<account-id>:client/${iot:ClientId}
  ```

  Unless "iot:Connection\.Thing\.IsAttached" is set to true in the condition keys, this is equivalent to the wildcard \* as in the previous example\.

+ compliant:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [ "iot:Connect" ],
        "Resource": [
          "arn:aws:iot:<region>:<account-id>:client/${iot:Connection.Thing.ThingName}"
        ],
        "Condition": {
          "Bool": { "iot:Connection.Thing.IsAttached": "true" }
        }
      }
    ]
  }
  ```

  The resource specification contains a variable that matches the device name used to connect, and the condition statement further restricts the permission by checking that the certificate used by the MQTT client matches that attached to the thing with the name used\.

Publish  

+ non\-compliant:

  ```
  arn:aws:iot:<region>:<account-id>:topic/$aws/things/*/shadow/update
  ```

  This allows the device to update the shadow of any device \(\* = all devices\)\.

  ```
  arn:aws:iot:<region>:<account-id>:topic/$aws/things/*
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
          "arn:aws:iot:<region>:<account-id>:topic/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
        ],
      }
    ]
  }
  ```

  The resource specification contains a wildcard, but it only matches any shadow\-related topic for the device whose thing name is used to connect\.

Subscribe  

+ non\-compliant:

  ```
  arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/*
  ```

  This allows the device to subscribe to reserved shadow or job topics for all devices\.

  ```
  arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/*
  ```

  The same as the previous example, but using the \# wildcard\.

  ```
  arn:aws:iot:<region>:<account-id>:topic/$aws/things/+/shadow/update
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
          "arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/shadow/*"
          "arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/${iot:Connection.Thing.ThingName}/jobs/*"
        ],
      }
    ]
  }
  ```

  The resource specifications contain wildcards but they only match any shadow\-related topic and any job\-related topic for the device whose thing name is used to connect\.

Receive  

+ compliant:

  ```
  arn:aws:iot:<region>:<account-id>:topicfilter/$aws/things/*
  ```

  This is okay because the device can only receive messages from topics on which it has permission to subscribe\.

------
#### [ shadow and job permissions \(5\) ]

A policy that grants permission to a device to perform an API action to access or modify thing shadows or job execution data should restrict these actions to specific resources\. The API actions are:

+ DeleteThingShadow

+ GetThingShadow

+ UpdateThingShadow

+ DescribeJobExecutions

+ GetPendingJobExecutions

+ StartNextPendingJobExecution

+ UpdateJobExecution

**Examples:**

+ non\-compliant:

  ```
  arn:aws:iot:<region>:<account-id>:thing/*
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
          "arn:aws:iot:<region>:<account-id>:/thing/MyThing1",
          "arn:aws:iot:<region>:<account-id>:/thing/MyThing2"
        ]
      }
    ]
  }
  ```

  This allows the device to perform the specified actions on only two specific things\.

------
#### [ more info \(5\) ]

The following reason code is returned when this check finds a non\-compliant IoT policy:

+ ALLOWS\_BROAD\_ACCESS\_TO\_IOT\_DATA\_PLANE\_ACTIONS

------
#### [ why it matters \(5\) ]

A certificate, Cognito identity or thing group with an overly\-permissive policy can, if compromised, impact the security of your entire account\. An attacker could use such broad access to read or modify shadows, jobs or job executions for all your devices\. Or an attacker could use a compromised certificate to connect malicious devices or launch a DDOS attack on your network\.

------
#### [ how to fix it \(5\) ]

Follow these steps to fix any non\-compliant policies attached to things, thing groups, or other entities:

1. Create a new, compliant version of the policy using [ CreatePolicyVersion](https://docs.aws.amazon.com/iot/latest/apireference/API_CreatePolicyVersion.html) with the "setAsDefault" flag set to true \(making this new version operative for all entities that use the policy\)\.

1. Get a list of targets \(certificates, thing groups\) that the policy is attached to using [ ListTargetsForPolicy](https://docs.aws.amazon.com/iot/latest/apireference/API_ListTargetsForPolicy.html) and determine which devices are included in the groups or which use the certificates to connect\.

1. Verify that all associated devices are able to connect to AWS IoT\. If a device is unable to connect, roll back the default policy to the previous version using [ SetPolicyVersion](https://docs.aws.amazon.com/iot/latest/apireference/API_SetPolicyVersion.html), revise the policy and try again\. 

Use [ AWS IoT Policy Variables](https://docs.aws.amazon.com/iot/latest/developerguide/policy-variables.html) to dynamically reference specific AWS IoT resources in your policies\.

------

 

------
#### [ CA\_CERT\_APPROACHING\_EXPIRATION\_CHECK ]

A CA certificate is expiring within 30 days or has expired\.

Severity: **Medium**

------
#### [ more info \(6\) ]

This check applies to CA certificates that are "ACTIVE" or "PENDING\_TRANSFER"\.

The following reason codes are returned when this check finds a non\-compliant CA certificate:

+ CERTIFICATE\_APPROACHING\_EXPIRATION

+ CERTIFICATE\_PAST\_EXPIRATION

------
#### [ why it matters \(6\) ]

An expired CA certificate should not be used to sign new device certificates\.

------
#### [ how to fix it \(6\) ]

Consult your security best practices for how to proceed\. You may want to:

1. Register a new CA certificate with AWS IoT\.

1. Verify that you are able to sign device certificates using the new CA certificate\.

1. Mark the old CA certificate as "INACTIVE" in the AWS IoT system using [ UpdateCACertificate](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-UpdateCACertificate.html)\.

------

 

------
#### [ CONFLICTING\_CLIENT\_IDS\_CHECK ]

Multiple devices connect using the same client ID\.

Severity: **High**

------
#### [ more info \(7\) ]

Multiple connections were made using the same client ID, causing an already connected device to be disconnected\. The MQTT specification allows only one active connection per client ID, so when another device connects using the same client ID, it knocks the previous one off the connection\.

When performed as part of an on\-demand audit, this check looks at how clientIDs were used to connect during the 31 days prior to the start of the audit\. For scheduled audits, this check looks at data from the last time the audit was run to the time this instance of the audit started\. If you have taken steps to mitigate this condition during the time checked, note when the connections/disconnections were made to determine if the problem persists\.

The following reason codes are returned when this check finds non\-compliance:

+ DUPLICATE\_CLIENT\_ID\_ACROSS\_CONNECTIONS

In addition, the findings returned by this check include the clientID used to connect, principal IDs, and disconnect times\. Results are listed in order of most recent first\.

------
#### [ why it matters \(7\) ]

Devices with conflicting IDs are forced to constantly reconnect, which may result in lost messages or leave a device unable to connect\.

This may indicate that a device or a device's credentials have been compromised, and might be part of a DDoS attack\. It is also possible that devices are misconfigured in the account or a device has a bad connection and is forced to reconnect several times per minute\.

------
#### [ how to fix it \(7\) ]

Register each device as a unique thing in AWS IoT, and use the thing name as the client ID to connect\. Or use a UUID as the client ID when connecting the device over MQTT\.

------

 

------
#### [ DEVICE\_CERT\_APPROACHING\_EXPIRATION\_CHECK ]

A device certificate is expiring within 30 days or has expired\.

Severity: **Medium**

------
#### [ more info \(8\) ]

This check applies to device certificates that are "ACTIVE" or "PENDING\_TRANSFER"\.

The following reason codes are returned when this check finds a non\-compliant device certificate:

+ CERTIFICATE\_APPROACHING\_EXPIRATION

+ CERTIFICATE\_PAST\_EXPIRATION

------
#### [ why it matters \(8\) ]

A device certificate should not be used after it expires\.

------
#### [ how to fix it \(8\) ]

Consult your security best practices for how to proceed\. You may want to:

1. Provision a new certificate and attach it to the device\. 

1. Verify that the new certificate is valid and the device is able to use it to connect\.

1. Mark the old certificate as "INACTIVE" in the AWS IoT system using [ UpdateCertificate](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-UpdateCertificate.html)\.

1. Detach the old certificate from the device\. \(See [ DetachThingPrincipal](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-DetachThingPrincipal.html)\.\)

------

 

------
#### [ REVOKED\_DEVICE\_CERT\_CHECK ]

A revoked device certificate is still active\.

Severity: **Medium**

------
#### [ more info \(9\) ]

A device certificate is in its CA's [ Certificate revocation list](https://en.wikipedia.org/wiki/Certificate_revocation_list) but it is still active in AWS IoT\.

This check applies to device certificates that are "ACTIVE" or "PENDING\_TRANSFER"\.

The following reason codes are returned when this check finds non\-compliance:

+ CERTIFICATE\_REVOKED\_BY\_ISSUER

------
#### [ why it matters \(9\) ]

A device certificate is usually revoked because it has been compromised\. It is possible that it has not yet been revoked in AWS IoT due to an error or an oversight\.

------
#### [ how to fix it \(9\) ]

Verify that the device certificate has not been compromised\. If it has, follow your security best practices to mitigate the situation\. You may want to:

1. Provision a new certificate for the device\.

1. Verify that the new certificate is valid and the device is able to use it to connect\.

1. Mark the old certificate as "REVOKED" in the AWS IoT system using [ UpdateCertificate](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-UpdateCertificate.html)\.

1. Detach the old certificate from the device\. \(See [ DetachThingPrincipal](https://docs.aws.amazon.com/iot/latest/developerguide/api-iot-DetachThingPrincipal.html)\.\)

------

 

------
#### [ LOGGING\_DISABLED\_CHECK ]

Amazon IoT CloudWatch logs are not enabled\.

Severity: **Low**

------
#### [ more info \(10\) ]

The following reason codes are returned when this check finds non\-compliance:

+ LOGGING\_DISABLED

------
#### [ why it matters \(10\) ]

Amazon IoT CloudWatch logs provide visibility into behaviors within AWS IoT, including authentication failures and unexpected connects and disconnects, that may indicate that a device has been compromised\.

------
#### [ how to fix it \(10\) ]

Enable Amazon IoT CloudWatch logs\. See [ Monitoring Tools](https://docs.aws.amazon.com/iot/latest/developerguide/monitoring_automated_manual.html)\.

------

## How To Perform Audits<a name="device-defender-HowToProceed"></a>

1. Configure audit settings for your account\. Use [ UpdateAccountAuditConfiguration](AuditCommands.md#dd-api-iot-UpdateAccountAuditConfiguration-doc) to enable those checks you want to be available for audits, set up optional notifications, and configure permissions\. 

   For certain checks, AWS IoT begins collecting data as soon as the check is enabled\.

1. Create one or more audit schedules\. Use [ CreateScheduledAudit](AuditCommands.md#dd-api-iot-CreateScheduledAudit-doc) to specify the checks you want to perform during an audit and how often these audits should be run\. 

   Or, you can run an on\-demand audit when necessary\. Use [ StartOnDemandAuditTask](AuditCommands.md#dd-api-iot-StartOnDemandAuditTask-doc) to specify the checks you want to perform and start an audit running right away\. \(Results may not be ready for some time if you have recently enabled a check that is included in the on\-demand audit\.\)

1. You can use the [ AWS IoT console](https://console.aws.amazon.com/iot/home) to view the results of your audits\.

   Or, you can see the results of your audits with [ ListAuditFindings](AuditCommands.md#dd-api-iot-ListAuditFindings-doc)\. With this command, you can filter the results by the type of check, a specific resource, or the time of the audit\. Armed with this information, you can take the appropriate steps to mitigate any problems found\.

## Notifications<a name="device-defender-audit-notifications"></a>

When an audit is completed, an SNS notification can be sent with a summary of the results of each audit check that was performed, including details about the number of non\-compliant resources that were found\. Use the `auditNotificationTargetConfigurations` field in the input to the [ UpdateAccountAuditConfiguration](AuditCommands.md#dd-api-iot-UpdateAccountAuditConfiguration-doc) command\. The SNS notification has the following payload:

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
            
            "message": "optional message if an error occured",
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

Notifications can also be viewed in the AWS IoT console, along with additional information about the device, device statistics \(for example, last connection time, number of active connections, data transfer rate\), and historical alerts for the device\.

## Permissions<a name="device-defender-audit-permissions"></a>

This section contains information on how to set up the AWS IAM roles and policies required to create, run and manage audits\. For more information on AWS IAM, see the [AWS Identity and Access Management User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)\.

### Give permission to collect your data in order to run an audit\.<a name="device-defender-audit-permissions-collect"></a>

When you call [ UpdateAccountAuditConfiguration](AuditCommands.md#dd-api-iot-UpdateAccountAuditConfiguration-doc) you must specify an IAM Role with two policies, a permissions policy and a trust policy\. The permissions policy grants permission to access your account data, using AWS IoT APIs, when it runs an audit\. The trust policy grants permission to assume the required role\. 

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
          "cognito-identity:GetIdentityPoolRoles",
          "iam:ListRolePolicies",
          "iam:ListAttachedRolePolicies",
          "iam:GetPolicy",
          "iam:GetPolicyVersion",
          "iam:GetRolePolicy"
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

### Give permission to publish notifications to an SNS topic\.<a name="device-defender-audit-permissions-publish"></a>

If you use the `auditNotificationTargetConfigurations` parameter in [ UpdateAccountAuditConfiguration](AuditCommands.md#dd-api-iot-UpdateAccountAuditConfiguration-doc), you must specify an IAM Role with two policies, a permissions policy and a trust policy\. The permissions policy grants permission to to publish notifications to your SNS topic\. The trust policy grants permission to assume the required role\.

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

### Give IAM users or groups permission to run audit commands<a name="device-defender-audit-permissions-manage"></a>

To allow IAM users or groups to manage, run, or view the results of , you must create and assign roles with attached policies that grant permission to run the appropriate commands\. What each specific policy contains depends on what commands you want the user or group to run\. 

+ **UpdateAccountAuditConfiguration**

#### policy<a name="audit-manage1-policy"></a>

NOTE: You must create the IAM Role with the attached policy in same account from which this command is run\. Cross account access is not allowed\. The policy should have "iam:PassRole" permissions \(permissions to pass this role\)\. 

In the following policy template `audit-permissions-role-arn` is the roleArn that you pass to in the `UpdateAccountAuditConfiguration` request using the `roleArn` parameter\. `audit-notifications-permissions-role-arn` is the roleArn that you pass to in the `UpdateAccountAuditConfiguration` request using the `auditNotificationTargetConfigurations` parameter\. 

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

+ **DescribeAccountAuditConfiguration**

+ **DeleteAccountAuditConfiguration**

+ **StartOnDemandAuditTask**

+ **CancelAuditTask**

+ **DescribeAuditTask**

+ **ListAuditTasks**

+ **ListScheduledAudits**

+ **ListAuditFindings**

#### policy<a name="audit-manage2-policy"></a>

NOTE: all these commands require \* in the Resource field of the policy\.

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

+ **CreateScheduledAudit**

+ **UpdateScheduledAudit**

+ **DeleteScheduledAudit**

+ **DescribeScheduledAudit**

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

The format for an scheduled audit roleArn is:

```
arn:aws:iot:region:account-id:scheduledaudit/scheduled-audit-name
```

## Service Limits<a name="device-defender-audit-limits"></a>


****  

| Resource | Limit | Description | 
| --- | --- | --- | 
| scheduled audits | 5 max\. | You can create up to 5 scheduled audits before a LimitExceeded Exception occurs\. | 
| simultaneous in progress "on\-demand" audits | 10 max\. | You can create up to 10 "on\-demand" audits before a LimitExceeded Exception occurs\. | 