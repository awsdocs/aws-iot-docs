# Custom Authentication<a name="custom-authentication"></a>

AWS IoT lets you define custom authorizers so that you can manage your own client authentication and authorization\. To do so, you use AWS Lambda functions to send client credentials to your authentication service\.

When an HTTP connection is established \(and, optionally, upgraded to a WebSocket connection\) and *Signature Version 4* headers aren't present, the AWS IoT device gateway checks if a custom authorizer is configured for the endpoint\. If so, AWS IoT device gateway uses the custom authorizer to authenticate the connection and authorize the device\. Custom authorizers can implement various authentication strategies \(for example, JWT verification, OAuth provider callout, and so on\) and must return policy documents that are used by the device gateway to authorize MQTT operations\.

There are two ways to implement custom authentication:
+ **Custom authentication** – Uses HTTP Publish operations or MQTT over WSS connections to receive client credentials in a bearer token \(such as a JSON Web Token \(JWT\) or OAuth token\) inside an HTTP header\.
+ **Enhanced custom authentication** – Extends custom authentication to enable passing client credentials through an MQTT CONNECT message \(in the `username` and `password` fields\) or as query parameters in an HTTP Publish or Upgrade request \(in the case of MQTT over WSS\)\. This feature also enables you to remove the token signing requirements for your custom authorizers\. This features is beta\.

**Note**  
You can define up to 10 custom authorizers\.

**Topics**
+ [Custom Authorizers](custom-authorizer.md)
+ [Configure a Custom Authorizer](config-custom-auth.md)
+ [Custom Authorizer Workflow](custom-auth.md)
+ [Enhanced Custom Authentication \(Beta\)](enhanced-custom-authentication.md)