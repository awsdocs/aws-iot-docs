# Security Best Practices for Device Agents<a name="device-defender-DetectMetricsMessagesBestPract"></a>

Least Privilege  
The agent process should granted only the minimum permissions necessary to perform its duties\.  

**Basic Mechanisms**

+ Agent should be run as non\-root user\.

+ Agent should run as a dedicated user, in it's own group\.

+ User/groups should be granted Read\-Only permissions on the resources required to gather and transmit metrics\.

+ Example: read\-only on /proc /sys for the sample agent\.

+ For an example of how to setup a process to run with reduced permissions, see the Setup Instructions which are included with the Python Sample Agent\.
There are a number of well\-known Linux mechanisms that can help you further restrict/isolate your agent process:  

**Advanced Mechanisms**

+ [CGroups](https://en.wikipedia.org/wiki/Cgroups)

+ [SELinux](https://en.wikipedia.org/wiki/Security-Enhanced_Linux)

+ [Chroot](https://en.wikipedia.org/wiki/Chroot)

+ [Linux Namespaces](https://en.wikipedia.org/wiki/Linux_namespaces)

Operational Resiliency  
An agent process must be resilient to unexpected operational errors and exceptions and must not crash or exit permanently\. The code needs to gracefully handle exceptions and, as a precaution, it must be configured to automatically restart in case of unexpected termination \(for example, due to system restarts or uncaught exceptions\)\.

Least Dependencies  
An agent must use the least possible number of dependencies \(i\.e\. third\-party libraries\) in its implementation\. If use of a library is justified due to the complexity of a task \(for example, transport layer security\) only use well\-maintained dependencies and establish a mechanism to keep dependencies up to date\. If the added dependencies contain functionality not used by the agent and active by default \(for example, opening ports, domain sockets\), disable them in your code or by means of the library's configuration files\.

Process Isolation  
An agent process must only contain functionality required for performing device metric gathering and transmission\. It must not piggyback on other system processes as a container or implement functionality for other out of scope use\-cases\. Additionally, the agent process must refrain from creating inbound communication channels such as domain socket and network service ports which would allow local or remote processes to interfere with its operation and impact its integrity and isolation\.

Stealthiness  
An agent process must not be named with keywords such as security, monitoring, or audit indicating its purpose and security value\. Generic code names or random and unique\-per\-device process names must be preferred\. The same principle must be followed in naming the directory in which the agent's binaries reside and any names and values of process arguments\.

Least Information Shared  
Any agent artifacts deployed to devices must not contain sensitive information such as privileged credentials, debugging and dead code, or inline comments or documentation files which reveal details about server\-side processing of agent\-gathered metrics or other details about backend systems\.

Transport Layer Security  
To establish TLS secure channels for data transmission, an agent process must enforce all client\-side validations, such as certificate chain and domain name validation, at the application level, if not enabled by default\. Furthermore, an agent must use a root certificate store which contains trusted authorities and does not contain certificates belonging to compromised certificate issuers\. 

Secure Deployment  
Any agent deployment mechanism, such as code push or sync and repositories containing its binaries, source code and any configuration files \(including trusted root certificates\), must be access controlled to prevent unauthorized code injection or tampering\. If the deployment mechanism relies on network communication, then cryptographic methods must be utilized to protect the integrity of deployment artifacts in transit\.

Further Reading  

+ [ Security and Identity for AWS IoT](https://docs.aws.amazon.com/iot/latest/developerguide/iot-security-identity.html)

+ [ Understanding the AWS IoT Security Model](https://aws.amazon.com/blogs/iot/understanding-the-aws-iot-security-model/)

+ [ Redhat: A Bite of Python](https://access.redhat.com/blogs/766093/posts/2592591)

+ [ 10 common security gotchas in Python and how to avoid them](https://hackernoon.com/10-common-security-gotchas-in-python-and-how-to-avoid-them-e19fbe265e03)

+ [ What Is Least Privilege & Why Do You Need It?](https://www.beyondtrust.com/blog/what-is-least-privilege/)

+ [ OWASP Embedded Security Top 10](https://www.owasp.org/index.php/OWASP_Embedded_Application_Security#tab=Embedded_Top_10_Best_Practices)

+ [ OWASP IoT Project](https://www.owasp.org/index.php/OWASP_Internet_of_Things_Project#tab=Main)