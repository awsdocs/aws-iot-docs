# Custom authentication<a name="custom-authentication"></a>

 AWS IoT Core lets you define custom authorizers so that you can manage your own client authentication and authorization\. This is useful when you need to use authentication mechanisms other than the ones that AWS IoT Core natively supports\. \(For more information about the natively supported mechanisms, see [Client authentication](client-authentication.md)\)\.  

 For example, if you are migrating existing devices in the field to AWS IoT Core and these devices use a custom bearer token or MQTT user name and password to authenticate, you can migrate them to AWS IoT Core without having to provision new identities for them\. You can use custom authentication with any of the communication protocols that AWS IoT Core supports\. For more information about the protocols that AWS IoT Core supports, see [Device communication protocols](protocols.md)\. 

**Topics**
+ [Understanding the custom authentication workflow](custom-authorizer.md)
+ [Creating and managing custom authorizers](config-custom-auth.md)
+ [Connecting to AWS IoT Core by using custom authentication](custom-auth.md)
+ [Troubleshooting your authorizers](custom-auth-troubleshooting.md)