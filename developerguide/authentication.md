# Authentication<a name="authentication"></a>

Authentication is a mechanism where you verify the identity of a client or a server\. Server authentication is the process where devices or other clients ensure they are communicating with an actual AWS IoT endpoint\. Client authentication is the process where devices or other clients authenticate themselves with AWS IoT\. 

## X\.509 Certificate Overview<a name="x509-certificate-overview"></a>

X\.509 certificates are digital certificates that use the [X\.509 public key infrastructure standard](https://en.wikipedia.org/wiki/X.509) to associate a public key with an identity contained in a certificate\. X\.509 certificates are issued by a trusted entity called a certification authority \(CA\)\. The CA maintains one or more special certificates called CA certificates that it uses to issue X\.509 certificates\. Only the certification authority has access to CA certificates\. X\.509 certificate chains are used both for server authentication by clients and client authentication by the server\.