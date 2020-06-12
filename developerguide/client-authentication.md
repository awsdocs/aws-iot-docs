# Client authentication<a name="client-authentication"></a>

AWS IoT supports three types of identity principals for device or client authentication:
+ [X\.509 client certificates](x509-client-certs.md)
+ [IAM users, groups, and roles](iam-users-groups-roles.md)
+ [Amazon Cognito identities](cognito-identities.md)

These identities can be used with devices, mobile, web, or desktop applications\. They can even be used by a user typing AWS IoT command line interface \(CLI\) commands\. Typically, AWS IoT devices use X\.509 certificates, while mobile applications use Amazon Cognito identities\. Web and desktop applications use IAM or federated identities\. AWS CLI commands use IAM\. For more information about IAM identities, see [Identity and access management for AWS IoT](security-iam.md)\.