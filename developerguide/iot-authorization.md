# Authorization<a name="iot-authorization"></a>

Authorization is the process of granting permissions to an authenticated identity\. You grant permissions in AWS IoT Core using AWS IoT Core and IAM policies\. This topic covers AWS IoT Core policies\. For more information about IAM policies, see [Identity and access management for AWS IoT](security-iam.md) and [How AWS IoT works with IAM](security_iam_service-with-iam.md)\.

AWS IoT Core policies determine what an authenticated identity can do\. An authenticated identity is used by devices, mobile applications, web applications, and desktop applications\. An authenticated identity can even be a user typing AWS IoT Core CLI commands\. An identity can execute AWS IoT Core operations only if it has a policy that grants it permission for those operations\.

Both AWS IoT Core policies and IAM policies are used with AWS IoT Core to control the operations an identity \(also called a *principal*\) can perform\. The policy type you use depends on the type of identity you are using to authenticate with AWS IoT Core\. 

AWS IoT Core operations are divided into two groups: 
+ Control plane API allows you to perform administrative tasks like creating or updating certificates, things, rules, and so on\.
+ Data plane API allows you send data to and receive data from AWS IoT Core\. 

The type of policy you use depends on whether you are using control plane or data plane API\.

The following table shows the identity types, the protocols they use, and the policy types that can be used for authorization\.


**AWS IoT Core data plane API and policy types**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iot-authorization.html)


**AWS IoT Core control plane API and policy types**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iot-authorization.html)

AWS IoT Core policies are attached to X\.509 certificates or Amazon Cognito identities\. IAM policies are attached to an IAM user, group, or role\. If you use the AWS IoT console or the AWS IoT Core CLI to attach the policy \(to a certificate or Amazon Cognito Identity\), you use an AWS IoT Core policy\. Otherwise, you use an IAM policy\.

Policy\-based authorization is a powerful tool\. It gives you complete control over what a device, user, or application can do in AWS IoT Core\. For example, consider a device connecting to AWS IoT Core with a certificate\. You can allow the device to access all MQTT topics, or you can restrict its access to a single topic\. In another example, consider a user typing CLI commands at the command line\. By using a policy, you can allow or deny access to any command or AWS IoT Core resource for the user\. You can also control an application's access to AWS IoT Core resources\.

Changes made to a policy can take a few minutes to become effective because of how AWS IoT caches the policy documents\. That is, it may take a few minutes to access a resource that has recently been granted access, and a resource may be accessible for several minutes after its access has been revoked\.

## AWS training and certification<a name="iot-authorization-training"></a>

For information about authorization in AWS IoT Core, take the [Deep Dive into AWS IoT Core Authentication and Authorization](https://www.aws.training/Details/Curriculum?id=42335) course on the AWS Training and Certification website\.