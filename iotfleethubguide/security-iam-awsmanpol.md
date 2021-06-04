# AWS managed policies for Fleet Hub for AWS IoT Device Management<a name="security-iam-awsmanpol"></a>







To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the **ReadOnlyAccess** AWS managed policy provides read\-only access to all AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.









## AWS managed policy: AWSIoTFleetHubFederationAccess<a name="security-iam-awsmanpol-AWSIoTFleetHubFederationAccess"></a>





You can attach the `AWSIoTFleetHubFederationAccess` policy to your IAM identities\.



This policy grants Fleet Hub for AWS IoT Device Management federated users the permissions they need to take actions in AWS IoT and other AWS services from Fleet Hub web applications\.

For more information about adding users to Fleet Hub web applications, see [How to add users to Fleet Hub for AWS IoT Device Management applications](aws-iot-monitor-admin-work-with-apps-add-users.md)\.

View this policy in the [AWS console](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/service-role/AWSIoTFleetHubFederationAccess$jsonEditor)\.





## Fleet Hub updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>



View details about updates to AWS managed policies for Fleet Hub since this service began tracking these changes\.




| Change | Description | Date | 
| --- | --- | --- | 
|   [AWSIoTFleetHubFederationAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/service-role/AWSIoTFleetHubFederationAccess$jsonEditor) – Update to an existing policy  |  Fleet Hub removed the following unsupported APIs\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/fleethubuserguide/security-iam-awsmanpol.html)  | April 12, 2021 | 
|   [AWSIoTFleetHubFederationAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/service-role/AWSIoTFleetHubFederationAccess$jsonEditor) – Update to an existing policy  |  Fleet Hub added the following APIs\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/fleethubuserguide/security-iam-awsmanpol.html)  | May 24, 2021 | 
|  Fleet Hub started tracking changes  |  Fleet Hub started tracking changes for its AWS managed policies\.  | April 12, 2021 | 