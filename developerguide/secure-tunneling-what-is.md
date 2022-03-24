# What is secure tunneling?<a name="secure-tunneling-what-is"></a>

Use secure tunneling to access devices that are deployed behind port\-restricted firewalls at remote sites\. You can connect to the destination device from your laptop or desktop computer as the source device by using the AWS Cloud\.The source and destination communicate by using an open source local proxy that runs on each device\. The local proxy communicates with the AWS Cloud by using an open port that is allowed by firewall, typically 443\. Data that is transmitted through the tunnel is encrypted using Transported Layer Security \(TLS\)\.

**Topics**
+ [Secure tunneling concepts](secure-tunneling-concepts.md)
+ [How secure tunneling works](how-secure-tunneling-works.md)
+ [Secure tunnel lifecycle](tunnel-lifecycle.md)