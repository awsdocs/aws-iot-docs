# IAM Users, Groups, and Roles<a name="iam-users-groups-roles"></a>

IAM users, groups, and roles are the standard mechanisms for managing identity and authentication in AWS\. You can use them to connect to AWS IoT HTTP interfaces using the AWS SDK and AWS CLI\.

IAM roles also allow AWS IoT to access other AWS resources in your account on your behalf\. For example, if you want to have a device publish its state to a DynamoDB table, IAM roles allow AWS IoT to interact with Amazon DynamoDB\. For more information, see [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use.html)\.

For message broker connections over HTTP, AWS IoT authenticates IAM users, groups, and roles using the Signature Version 4 signing process\. For information, see [Signing AWS API Requests](https://docs.aws.amazon.com/general/latest/gr/signing_aws_api_requests.html)\.

When using AWS Signature Version 4 with AWS IoT, clients must support the following in their TLS implementation:
+ TLS 1\.2, TLS 1\.1, TLS 1\.0
+ SHA\-256 RSA certificate signature validation
+ One of the cipher suites from the TLS cipher suite support section

For information, see [Identity and Access Management for AWS IoT](security-iam.md)\.