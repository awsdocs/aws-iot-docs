# Authorization<a name="iot-authorization"></a>

Authorization is the process of granting permissions to an authenticated identity\. You grant permissions in using and IAM policies\. This topic covers policies\. For more information about IAM policies, see [Identity and Access Management for AWS IoT](security-iam.md) and [IAM Policies](iam-policies.md)\. 

 policies determine what an authenticated identity can do\. An authenticated identity is used by devices, mobile applications, web applications, and desktop applications\. An authenticated identity can even be a user typing CLI commands\. An identity can execute operations only if it has a policy that grants it permission for those operations\.

Both policies and IAM policies are used with to control the operations an identity \(also called a *principal*\) can perform\. The policy type you use depends on the type of identity you are using to authenticate with \. 

 operations are divided into two groups: 
+ Control plane API allows you to perform administrative tasks like creating or updating certificates, things, rules, and so on\.
+ Data plane API allows you send data to and receive data from \. 

The type of policy you use depends on whether you are using control plane or data plane API\.

The following table shows the identity types, the protocols they use, and the policy types that can be used for authorization\.


**Data Plane API and Policy Types**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iot-authorization.html)


**Control Plane API and Policy Types**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iot-authorization.html)

 policies are attached to X\.509 certificates or Amazon Cognito identities\. IAM policies are attached to an IAM user, group, or role\. If you use the AWS IoT console or the CLI to attach the policy \(to a certificate or Amazon Cognito Identity\), you use an policy\. Otherwise, you use an IAM policy\.

Policy\-based authorization is a powerful tool\. It gives you complete control over what a device, user, or application can do in \. For example, consider a device connecting to with a certificate\. You can allow the device to access all MQTT topics, or you can restrict its access to a single topic\. In another example, consider a user typing CLI commands at the command line\. By using a policy, you can allow or deny access to any command or resource for the user\. You can also control an application's access to resources\. 

## AWS Training and Certification<a name="iot-authorization-training"></a>

For information about authorization in , take the [Deep Dive into Authentication and Authorization](https://www.aws.training/Details/Curriculum?id=42335) course on the AWS Training and Certification website\.