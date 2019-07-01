# AWS IoT Authentication<a name="iot-authentication"></a>

AWS IoT supports four types of identity principals for authentication:
+ X\.509 certificates
+ IAM users, groups, and roles
+ Amazon Cognito identities
+ Federated identities

These identities can be used with mobile applications, web applications, or desktop applications\. They can even be used by a user typing AWS IoT CLI commands\. Typically, AWS IoT devices use X\.509 certificates, while mobile applications use Amazon Cognito identities\. Web and desktop applications use IAM or federated identities\. CLI commands use IAM\.