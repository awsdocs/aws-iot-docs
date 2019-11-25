# Authorization<a name="iot-authorization"></a>

Authorization is the process of granting permissions to an authenticated identity\. You grant permissions in AWS IoT using AWS IoT and IAM policies\. This topic covers AWS IoT policies\. For more information about IAM policies, see [Identity and Access Management for AWS IoT](security-iam.md) and [IAM Policies](iam-policies.md)\. 

AWS IoT policies determine what an authenticated identity can do\. An authenticated identity is used by devices, mobile applications, web applications, and desktop applications\. An authenticated identity can even be a user typing AWS IoT CLI commands\. An identity can execute AWS IoT operations only if it has a policy that grants it permission for those operations\.

Both AWS IoT policies and IAM policies are used with AWS IoT to control the operations an identity \(also called a *principal*\) can perform\. The policy type you use depends on the type of identity you are using to authenticate with AWS IoT\. 

AWS IoT operations are divided into two groups: 
+ Control plane API allows you to perform administrative tasks like creating or updating certificates, things, rules, and so on\.
+ Data plane API allows you send data to and receive data from AWS IoT\. 

The type of policy you use depends on whether you are using control plane or data plane API\.

The following table shows the identity types, the protocols they use, and the policy types that can be used for authorization\.


**AWS IoT Data Plane API and Policy Types**  

| Protocol and Authentication Mechanism  | SDK  | Identity Type | Policy Type | 
| --- | --- | --- | --- | 
| MQTT over mutual authentication \(port 8883 or 443[†](protocols.md#protocol-port)\) | AWS IoT Device SDK | X\.509 certificates | AWS IoT policy | 
| MQTT over WebSocket \(port 443\) | AWS Mobile SDK | Amazon Cognito, IAM, or federated identity |  AWS IoT policy for authenticated Amazon Cognito identities IAM policy for other identities  | 
| HTTP over server authentication \(port 443\) | AWS CLI | Amazon Cognito, IAM, or federated identity |  AWS IoT policy for authenticated Amazon Cognito identities IAM policy for other identities  | 
| HTTP over mutual authentication \(port 8443\) | No SDK support | X\.509 certificates | AWS IoT policy | 


**AWS IoT Control Plane API and Policy Types**  

| Protocol and Authentication Mechanism  | SDK  | Identity Type | Policy Type | 
| --- | --- | --- | --- | 
| HTTP over server authentication \(port 443\) | AWS CLI | Amazon Cognito, IAM, or federated identity |  AWS IoT policy for authenticated Amazon Cognito identities IAM policy for other identities  | 

AWS IoT policies are attached to X\.509 certificates or Amazon Cognito identities\. IAM policies are attached to an IAM user, group, or role\. If you use the AWS IoT console or the AWS IoT CLI to attach the policy \(to a certificate or Amazon Cognito Identity\), you use an AWS IoT policy\. Otherwise, you use an IAM policy\.

Policy\-based authorization is a powerful tool\. It gives you complete control over what a device, user, or application can do in AWS IoT\. For example, consider a device connecting to AWS IoT with a certificate\. You can allow the device to access all MQTT topics, or you can restrict its access to a single topic\. In another example, consider a user typing CLI commands at the command line\. By using a policy, you can allow or deny access to any command or AWS IoT resource for the user\. You can also control an application's access to AWS IoT resources\. 