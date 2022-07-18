# Securing users and devices with AWS IoT Jobs<a name="iot-jobs-security"></a>

To authorize users to use AWS IoT Jobs with their devices, you must grant them permissions by using IAM policies\. The devices must then be authorized by using AWS IoT Core policies to connect securely to AWS IoT, receive job executions, and update the execution status\.

## Required policy type for AWS IoT Jobs<a name="jobs-required-policy"></a>

The following table shows the different types of policies that you must use for authorization\. For more information about the required policy to use, see [Authorization](iot-authorization.md)\.


**Required policy type**  

| Use case | Protocol | Authentication | Control plane/data plane | Identity type | Required policy type | 
| --- | --- | --- | --- | --- | --- | 
| Authorize an administrator, operator, or Cloud Service to work securely with Jobs | HTTPS | AWS Signature Version 4 authentication \(port 443\) | Both control plane and data plane | Amazon Cognito Identity, IAM, or federated user | IAM policy | 
| Authorize your IoT device to work securely with Jobs | MQTT/HTTPS | TCP or TLS mutual authentication \(port 8883 or 443\) | Data plane | X\.509 certificates | AWS IoT Core policy | 

To authorize AWS IoT Jobs operations that can be performed both on the control plane and data plane, you must use IAM policies\. The identities must have been authenticated with AWS IoT to perform these operations, which must be [Amazon Cognito identities](cognito-identities.md) or [IAM users, groups, and roles](iam-users-groups-roles.md)\. For more information about authentication, see [Authentication](authentication.md)\.

The devices must now be authorized on the data plane by using AWS IoT Core policies to connect securely to the device gateway\. The device gateway enables devices to securely communicate with AWS IoT, receive job executions, and update the job execution status\. Device communication is secured by using secure [MQTT](mqtt.md) or [HTTPS](http.md) communication protocols\. These protocols use [X\.509 client certificates](x509-client-certs.md) that are provided by AWS IoT to authenticate the device connections\.

The following shows how you authorize your users, cloud services, and devices to use AWS IoT Jobs\. For information about control plane and data plane API operations, see [AWS IoT jobs APIs](jobs-api.md)\.

**Topics**
+ [Required policy type for AWS IoT Jobs](#jobs-required-policy)
+ [Authorizing users and cloud services to use AWS IoT Jobs](iam-policy-users-jobs.md)
+ [Authorizing your devices to securely use AWS IoT Jobs on the data plane](iot-data-plane-jobs.md)