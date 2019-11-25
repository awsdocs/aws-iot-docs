# AWS IoT Limits<a name="limits-iot"></a>

This section describes current limits within AWS IoT\. Each limit applies on a per\-region basis unless otherwise specified\. 

## Thing Limits<a name="thing-limits"></a>


****  

| Resource | Limit | 
| --- | --- | 
| Thing name size | 128 bytes of UTF\-8 encoded characters\. This limit applies for both the thing registry and Thing Shadow services\. | 
| Maximum number of thing attributes for a thing with a thingtype | 50 | 
| Maximum number of thing attribute for a thing without a thing type | 3 | 
| Number of thing types that can be associated with a thing | 1 | 
| Maximum number of thing types in an AWS account | Unlimited | 

## Thing Group Limits<a name="thing-group-limits"></a>


| Resource | Description | Limit | Adjustable | 
| --- | --- | --- | --- | 
| Maximum direct child groups | The maximum number of direct child groups\. | 100 | No | 
| Maximum dynamic groups | The maximum number of dynamic groups\. | 100 | No | 
| Thing Group Hierarchy | The maximum depth of a thing group hierarchy\. | 7 | No | 
| Thing Group Attributes | The maximum number of attributes associated with a thing group\. | 50 | No | 
| Thing Group Attribute Name | The maximum size of a thing group attribute name \(in chars\)\. | 128 | No | 
| Thing Group Attribute Value | The maximum size of a thing group attribute value \(in chars\)\. | 800 | No | 

## Message Broker Limits<a name="message-broker-limits"></a>


| Resource | Description | Limit | Adjustable | 
| --- | --- | --- | --- | 
| Maximum concurrent client connections per account | The maximum number of concurrent connections allowed per account\. | 500,000 |  [Yes](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-iot) | 
| Connect requests per second per account | AWS IoT limits an account to a maximum number of MQTT CONNECT requests per second\. | 500 | [Yes](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-iot) | 
|  Connect requests per second per client ID  |  AWS IoT limits MQTT `CONNECT` requests from the same `accountId` and `clientId` to 1 MQTT `CONNECT` operation per second\.  | 1 | No | 
| Subscriptions per account | AWS IoT limits an account to a maximum number of subscriptions across all active connections\. | 500,000 | [Yes](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-iot) | 
| Subscriptions per second per account | AWS IoT limits an account to a maximum number of subscriptions per second\. For example, if there are two MQTT SUBSCRIBE requests within a second with 3 subscriptions \(topic filters\) each, AWS IoT counts those as 6 subscriptions towards this limit\. | 500 | [Yes](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-iot) | 
| Subscriptions per connection | AWS IoT supports 50 subscriptions per connection\. Subscription requests on the same connection in excess of this amount may be rejected by AWS IoT and the connection will be closed\. Clients should validate the SUBACK message to ensure that their subscription requests have been successfully processed\. | 50 | No | 
| Publish requests per second per connection | AWS IoT limits each client connection to a maximum number of inbound and outbound publish requests per second\. Publish requests exceeding that limit will be discarded\. | 100 | No | 
| Inbound publish requests per second per account | Inbound publish requests count for all the messages that AWS IoT processes before routing the messages to the subscribed clients or the rules engine\. For example, a single message published on $aws/things/device/shadow/update topic can result in publishing three additional messages to $aws/things/device/shadow/update/accepted, $aws/things/device/shadow/update/documents, and $aws/things/device/shadow/delta topics\. In this case, AWS IoT counts those as 4 inbound publish requests towards this limit\. However, a single message to an unreserved topic like a/b is counted only as a single inbound publish request\. | 20,000 | [Yes](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-iot) | 
| Outbound publish requests per second per account | Outbound publish requests count for every message that resulted in matching a client's subscription or matching a rules engine subscription\. For example, two clients are subscribed to topic filter a/b and a rule is subscribed to topic filter a/\#\. An inbound publish request on topic a/b results in a total of 3 outbound publish requests\. | 20,000 | [Yes](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-iot) | 
| Throughput per second per connection | Data received or sent over a client connection is processed at a maximum throughput rate\. Data exceeding the maximum throughput will be delayed in processing\. | 512 KiB | No | 
| Maximum inbound unacknowledged QoS 1 publish requests | AWS IoT limits the number of unacknowledged inbound publish requests per client\. When this limit is reached, no new publish requests are accepted from this client until a PUBACK message is returned by the server\. | 100 | No | 
| Maximum outbound unacknowledged QoS 1publish requests  | AWS IoT limits the number of unacknowledged outbound publish requests per client\. When this limit is reached, no new publish requests are sent to the client until the client acknowledges the publish requests\. | 100 | No | 
| Maximum retry interval for delivering QoS 1 messages | AWS IoT will retry delivery of unacknowledged quality\-of\-service 1 \(QoS 1\) publish requests to a client for up to one hour\. If AWS IoT does not receive a PUBACK message from the client after one hour, it will drop the publish requests\. | 1 hour | No | 
| Persistent Session expiry period | The duration of time for which the Message Broker will store an MQTT persistent session\. The expiry period begins when the Message Broker detects the session has become disconnected\. Once the expiry period has elapsed, the Message Broker terminates the session and discards any associated queued messages\. | 1 hour | Yes | 

## Protocol Limits<a name="iot-protocol-limits"></a>


| Resource | Description | 
| --- | --- | 
| Connection inactivity \(keep\-alive interval\) | For MQTT \(or MQTT over WebSockets\) connections, a client can request a keep\-alive interval between 30 \- 1200 seconds as part of the MQTT CONNECT message\. AWS IoT starts the keep\-alive timer for a client when sending CONNACK in response to the CONNECT message\. This timer is reset whenever AWS IoT receives a PUBLISH, SUBSCRIBE, PING, or PUBACK message from the client\. AWS IoT will disconnect a client whose keep\-alive timer has reached 1\.5x the specified keep\-alive interval \(i\.e\., by a factor of 1\.5\)\.The default keep\-alive interval is 1200 seconds\. If a client requests a keep\-alive interval of zero, the default keep\-alive interval will be used\. If a client requests a keep\-alive interval greater than 1200 seconds, the default keep\-alive interval will be used\. If a client requests a keep\-alive interval shorter than 30 seconds but greater than zero, the server treats the client as though it requested a keep\-alive interval of 30 seconds\. | 
| WebSocket connection duration | WebSocket connections are limited to 24 hours\. If the limit is exceeded, the WebSocket connection is automatically closed when an attempt is made to send a message by the client or server\.  | 
| Maximum subscriptions per subscribe request | A single SUBSCRIBE request is limited a maximum of eight subscriptions\. | 
| Message size | The payload for every publish request is limited to 128 KB\. The AWS IoT service rejects publish and connect requests larger than this size\. | 
| Client ID size | 128 bytes of UTF\-8 encoded characters\. | 
| Restricted client ID prefix | $ is reserved for AWS IoT generated client IDs\. | 
| Topic size | The topic passed to the AWS IoT when sending a publish request is limited to 256 bytes of UTF\-8 encoded characters\. This excludes the first three mandatory segments for Basic Ingest topics \($AWS/rules/rule\-name/\)\. | 
| Restricted topic prefix | Topics beginning with $ are reserved by AWS IoT and are not supported for publishing and subscribing except for using the specific topic names defined by AWS IoT services \(i\.e\., Thing Shadow\)\. | 
| Maximum number of slashes in topic and topic filter | A topic in a publish or subscribe request is limited to 7 forward slashes \(/\)\. This excludes the first three slashes in the mandatory segments for Basic Ingest topics \($AWS/rules/rule\-name/\)\. | 

## Device Shadow Limits<a name="device-shadow-limits"></a>


****  

|  |  | 
| --- |--- |
| Maximum depth of JSON device state documents | The maximum number of levels in the desired or reported section of the JSON device state document is 5\. For example: <pre>"desired": {<br />    "one": {<br />        "two": {<br />            "three": {<br />                "four": {<br />                    "five":{<br />                    }<br />                 }<br />             }<br />        }<br />    }<br />}</pre> | 
| Maximum number of in\-flight, unacknowledged messages per thing\. |  The Thing Shadows service supports up to 10 in\-flight unacknowledged messages per thing\. When this limit is reached, all new shadow requests are rejected with a 429 error code\.  | 
| Maximum number of JSON objects per AWS account\. | There is no limit on the number of JSON objects per AWS account\. | 
| Maximum size of a JSON state document\. | 8 KB\. Note that metadata do not contribute to the document size for service limits or pricing\. | 
| Maximum size of a thing name\. | 128 bytes of UTF\-8 encoded characters\. | 
| Maximum number of shadows in an AWS account\. | Unlimited\. | 
| Requests per second per thing\. | The Thing Shadows service supports up to 20 requests per second per thing\. Note that this limit is per thing and not per API\. | 

**Note**  
A thing shadow is deleted by AWS IoT after the creating account is deleted or per customer request\. For operational purposes, AWS IoT service backups are kept for 6 months\.

## Security and Identity Limits<a name="security-limits"></a>


****  

|  |  | 
| --- |--- |
| Maximum number of CA certificates with the same subject field allowed per AWS account per Region | 10 | 
| Maximum number of policies that can be attached to a certificate or Amazon Cognito identity | 10 | 
| Maximum number of named policy versions | 5 | 
| Maximum policy document size | 2048 characters \(excluding white space\) | 
| Maximum number of device certificates that can be registered per second | 15 | 
| Maximum number of AWS IoT role aliases | 100 | 
| Maximum number of custom authenticators | 10 | 

## AWS IoT Throttling Limits<a name="throttling-limits"></a>


| API | Transactions per Second | 
| --- | --- | 
| AcceptCertificateTransfer | 10 | 
| AddThingToBillingGroup | 60 | 
| AddThingToThingGroup | 60 | 
| AssociateTargetsWithJob | 10 | 
| AttachPrincipalPolicy | 15 | 
| AttachPolicy | 15 | 
| AttachThingPrincipal | 15 | 
| CancelCertificateTransfer | 10 | 
| CancelJob | 10 | 
| CancelJobExecution | 10 | 
| ClearDefaultAuthorizer | 10 | 
| CloseTunnel | 1 | 
| CreateAuthorizer | 10 | 
| CreateBillingGroup | 25 | 
| CreateCertificateFromCsr | 15 | 
| CreateDynamicThingGroup | 5 | 
| CreateJob | 10 | 
| CreatePolicy | 10 | 
| CreatePolicyVersion | 10 | 
| CreateRoleAlias | 10 | 
| CreateThing | 15 | 
| CreateThingGroup | 25 | 
| CreateThingType | 15 | 
| DeleteAuthorizer | 10 | 
| DeleteBillingGroup | 15 | 
| DeleteCertificate | 10 | 
| DeleteCACertificate | 10 | 
| DeleteDynamicThingGroup | 5 | 
| DeleteJob | 10 | 
| DeleteJobExecution | 10 | 
| DeletePolicy | 10 | 
| DeletePolicyVersion | 10 | 
| DeleteRegistrationCode | 10 | 
| DeleteRoleAlias | 10 | 
| DeleteThing | 15 | 
| DeleteThingGroup | 15 | 
| DeleteThingType | 15 | 
| DeprecateThingType | 15 | 
| DescribeAuthorizer | 10 | 
| DescribeBillingGroup | 100 | 
| DescribeCertificate | 10 | 
| DescribeCACertificate | 10 | 
| DescribeDefaultAuthorizer | 10 | 
| DescribeJob | 10 | 
| [DescribeJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeJobExecution.html) | 10 | 
| DescribeRoleAlias | 10 | 
| DescribeThing | 350 | 
| DescribeThingGroup | 100 | 
| DescribeThingType | 10 | 
| DetachThingPrincipal | 15 | 
| DescribeTunnel | 10 | 
| DetachPrincipalPolicy | 15 | 
| DetachPolicy | 15 | 
| GetEffectivePolicies | 50 | 
| GetJobDocument | 10 | 
| GetPolicy | 10 | 
| GetPolicyVersion | 15 | 
| GetRegistrationCode | 10 | 
| ListAttachedPolicies | 15 | 
| ListAuthorizers | 10 | 
| ListBillingGroups | 10 | 
| ListCACertificates | 10 | 
| ListCertificates | 10 | 
| ListChildThingGroups | 15 | 
| ListCertificatesByCA | 10 | 
| ListJobExecutionsForJob | 10 | 
| ListJobExecutionsForThing | 10 | 
| ListJobs | 10 | 
| ListOutgoingCertificates | 10 | 
| ListPolicies | 10 | 
| ListPolicyPrincipals | 10 | 
| ListPolicyVersions | 10 | 
| ListPrincipalPolicies | 15 | 
| ListPrincipalThings | 10 | 
| ListRoleAliases | 10 | 
| ListTagsForResource | 10 | 
| ListTargetsForPolicy | 10 | 
| ListThingGroups | 10 | 
| ListThingGroupsForThing | 10 | 
| ListThingPrincipals | 10 | 
| ListThings | 10 | 
| ListThingsInBillingGroup | 25 | 
| ListThingsInThingGroup | 25 | 
| ListThingTypes | 10 | 
| ListTunnels | 10 | 
| OpenTunnel | 1 | 
| RegisterCertificate | 10 | 
| RegisterCACertificate | 10 | 
| RegisterThing | 10 | 
| RejectCertificateTransfer | 10 | 
| RemoveThingFromBillingGroup | 15 | 
| RemoveThingFromThingGroup | 15 | 
| SetDefaultAuthorizer | 10 | 
| SetDefaultPolicyVersion | 10 | 
| TagResource | 10 | 
| TestAuthorization | 10 | 
| TestInvokeAuthorizer | 10 | 
| TransferCertificate | 10 | 
| UntagResource | 10 | 
| UpdateAuthorizer | 10 | 
| UpdateBillingGroup | 15 | 
| UpdateCertificate | 10 | 
| UpdateCACertificate | 10 | 
| UpdateDynamicThingGroup | 5 | 
| UpdateJob | 10 | 
| UpdateRoleAlias | 10 | 
| UpdateThing | 10 | 
| UpdateThingGroup | 15 | 

## AWS IoT Rules Engine Limits<a name="rules-limits"></a>


****  

|  |  | 
| --- |--- |
| Maximum number of rules per AWS account | 1000 | 
| Actions per rule | A maximum of 10 actions can be defined per rule\. | 
| Rule size | 256 KB | 
| Basic ingest inbound publish requests per second per account | Up to 256 KB of UTF\-8 encoded characters \(including white space\)\. | 
| Inbound publish requests per second per account | 20,000 | 
| Basic ingest inbound publish requests count for all of the messages published on the basic ingest topics that start with $aws/rules/<rule\-name> | 20,000 | 

## AWS IoT Rules Engine HTTP Action Limits<a name="rules-http-action-limits"></a>


****  

|  |  | 
| --- |--- |
| The HTTPS protocol requires a valid certificate issued by a public certificate authority \(CA\) | N/A | 
| Ports allowed for HTTP action | 443 and 8443 | 
| Maximum topic rule destinations per account | 1,000 | 
| Request timeout | 3,000 ms | 
| Maximum number of headers per action | 100 | 
| Maximum size of a header key | 256 bytes | 
| Maximum length of an endpoint URL | 2 KiB | 
| HTTP method supported | POST | 

## AWS IoT Job Limits<a name="job-limits"></a>


****  

| Resource | Min | Max | Note | 
| --- | --- | --- | --- | 
| JobId | 1 character | 64 characters | The JobId length must not exceed 64 characters\. | 
| Document | N/A | 32768 bytes | The maximum size of a document that can be sent to an AWS IoT device is 32 KB\. | 
| DocumentSource | N/A | 1350 characters |  The maximum job document source size is 1350 characters\.  | 
| Description | N/A | 2028 characters | The maximum job description size is 2028 characters\. | 
| Targets | 1 | 100 | The number of targets a job can have\. | 
| ExpiresInSec | 60 seconds | 3600 seconds | The lifetime of pre\-signed URLs must be configured greater than 60 seconds and less than 1 hour\. | 
| Comment  | N/A | 2028 characters | The maximum comment size is 2028 characters\. | 
| MaxResults  | 1 | 250 | The maximum list result per page is 250\. | 
| MaximumJobExecutionsPerMinute | 1 | 1000 | Configures the rollout speed for a job\. | 
| Active snapshot jobs | 0 | 100 | The maximum number of active snapshot jobs is 100 \(irrespective of the number of active continuous jobs\)\. | 
| Active continuous jobs | 0 | 100 | The maximum number of active continuous jobs is 100 \(irrespective of the number of active snapshot jobs\)\. | 
| Job document variable substitution | 0 | 10 |  Up to 10 variables substitutions, including the presign URL, are allowed in a job document\.  | 
| Data retention | N/A | 730 days |  Job data and job execution data for inactive jobs \(jobs that aren't IN\_PROGRESS\) will be purged after 730 days\.  | 
| StatusDetail map key:value pairs  | 1 key:value pair | 10 key:value pairs |  | 
| StatusDetail map key size  | 1 character | 128 characters |  | 
| StatusDetail map value size  | 1 character | 128 characters |  | 
| [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_DescribeJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_DescribeJobExecution.html) and [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_GetPendingJobExecutions.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_GetPendingJobExecutions.html) | N/A | 200 TPS per account | If invoking one or more of these "read" APIs in the data plane† causes the associated AWS account to exceed 200 read transactions per second \(TPS\) in total, then the offending API invocation\(s\) will be throttled to maintain the maximum allowed 200 read TPS per AWS account\. Be aware that in the control plane†, [DescribeJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeJobExecution.html) is limited to 10 TPS per invocation\. | 
| [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_StartNextPendingJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_StartNextPendingJobExecution.html) and [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html) | N/A | 200 TPS per account | If invoking one or more of these "write" APIs in the data plane† causes the associated AWS account to exceed 200 write transactions per second \(TPS\) in total, then the offending API invocation\(s\) will be throttled to maintain the maximum allowed 200 write TPS per AWS account\. | 
| inProgressTimeoutInMinutes property of [https://docs.aws.amazon.com/iot/latest/apireference/API_TimeoutConfig.html](https://docs.aws.amazon.com/iot/latest/apireference/API_TimeoutConfig.html) | 1 | 10080 | Values are in minutes \(1 minute to 7 days\)\. | 
| stepTimeoutInMinutes value passed with [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html) and [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_StartNextPendingJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_StartNextPendingJobExecution.html) | 1 | 10080 | Values are in minutes \(1 minute to 7 days\)\. A value of \-1 is also valid when using the UpdateJobExecution API and discards a previously set timer\. | 

† For definitions of data plane and control plane, see [What are the ways for accessing AWS IoT Core?](https://aws.amazon.com/iot-core/faqs/)

## AWS IoT Secure Tunnneling Limits<a name="secure-tunneling-limits"></a>


****  

| Description | Limit | 
| --- | --- | 
| Maximum bandwidth per tunnel | 800 Kbps | 
| Maximum connection rate | 10 TPS | 
| Maximum tunnel lifetime timeout | 12 hours | 
| Tagging limit | For more information, see [Tagging Limits](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html#tag-restrictions) | 

## AWS IoT Fleet Indexing Limits<a name="fleet-indexing-limits"></a>


****  

| Resource | Limit | Note | 
| --- | --- | --- | 
| Maximum number of query terms per query | 5 | You can have up to 5 terms per query\. | 
| Maximum query length | 1000 | Your queries can be up to 1000 bytes of UTF\-8 encoded characters long\. | 
| Maximum number of query results | 500 | Fleet indexing service will return up to 500 results per query\. | 
| Maximum number of \* wild card operators per query term | 2 | Each query term can have up to 2 multi\-character wildcards \(\*\)\. | 
| Maximum number of ? wild card operators per query term | 5 | Each query term can have up to 5 single\-character wildcards \(?\)\. | 
| Maximum number of queries per second | 15 | You can execute up to 15 search queries per second\. | 
| Maximum number of things in the fleet index | Unlimited | There is no limit to the number of things that can be indexed\. | 
| Maximum number of dynamic groups in the fleet index | 100 | A maximum of 100 dynamic groups can be indexed\. | 
| Maximum number of custom fields\. | 5 | You can configure a maximum of 5 custom fields\. | 
| Maximum length of a custom field name | 1024 | A custom field name can have a length of up to 1024 characters\. | 

### AWS IoT Throttling Limits<a name="fleet-indexing-throttling-limits"></a>


****  

| API | Max Calls Per Second | 
| --- | --- | 
| UpdateIndexingConfiguration | 1 | 
| GetIndexingConfiguration | 20 | 
| DescribeIndex | 10 | 
| ListIndices | 5 | 
| SearchIndex | 15 | 
| GetStatistics | 15 | 
| GetCardinality | 15 | 
| GetPercentiles | 15 | 

## AWS IoT Fleet Provisioning Limits<a name="fleet-provision-limits"></a>


****  

| Description | Limit | 
| --- | --- | 
| Maximum size of a fleet provisioning template | 10 KiB | 

## AWS IoT Bulk Thing Registration Limits<a name="bulk-thing-reg-limits"></a>


****  

| Resource | Limit | Note | 
| --- | --- | --- | 
| Registration task termination | 30 days | Any pending/uncompleted bulk registration tasks are terminated after 30 days\. | 
| Data retention policy | 30 days | Once the associated bulk registration task has completed \(which can be long lived\), bulk Thing registration related data is permanently deleted after 30 days\. | 
| Allowed registration tasks | 1 | For any given AWS account, only one bulk registration task can run at a time\. | 
| Maximum line length | 256K | Each line in an [Amazon S3 input JSON file](https://docs.aws.amazon.com/iot/latest/developerguide/bulk-provisioning.html) cannot exceed 256K in length\. | 

## AWS IoT Device Defender Limits<a name="limits_iot_device_defender"></a>


**Audit Limits**  

| Resource | Limit | Description | 
| --- | --- | --- | 
| scheduled audits | 5 max\. | You can create up to 5 scheduled audits before a LimitExceeded Exception occurs\. | 
| simultaneous in progress "on\-demand" audits | 10 max\. | You can create up to 10 "on\-demand" audits before a LimitExceeded Exception occurs\. | 

**Detect Limits**
+ The maximum number of security profiles per target \(thing group or user account\) is 5\.
+ The maximum number of behaviors per security profile is 100\.
+ The maximum number of `value` elements \(counts, IP addresses, ports\) per security profile is 1000\. 
+ Device metric reporting is throttled to one metric per 5 minutes per device \(a device may not report more than one metric every 5 minutes\)\.
+ Device Defender Detect violations are stored for 30 days after they have been generated\.