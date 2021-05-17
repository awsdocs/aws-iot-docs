# Security use cases<a name="dd-detect-security-use-cases"></a>

This section describes the different types of attacks that threaten your device fleet and the recommended metrics you can use to monitor for these attacks\. We recommend using metric anomalies as a starting point to investigate security issues, but you should not base your determination of any security threats solely on a metric anomaly\. 

To investigate an anomaly alarm, correlate the alarm details with other contextual information such as device attributes, device metric historical trends, Security Profile metric historical trends, custom metrics, and logs to determine if a security threat is present\.

## Cloud\-side use cases<a name="Cloud-side-threats"></a>

Device Defender can monitor the following use cases on the AWS IoT cloud side\.

**Intellectual property theft:**  
Intellectual property theft involves stealing a person's or companies' intellectual properties, including trade secrets, hardware, or software\. It often occurs during the manufacturing stage of devices\. Intellectual property theft can come in the form of piracy, device theft, or device certificate theft\. Cloud\-based intellectual property theft can occur due to the presence of policies that permit unintended access to IoT resources\. You should review your [IoT policies](https://docs.aws.amazon.com/iot/latest/developerguide/iot-policies.html) and turn on [Audit overly permissive checks](https://docs.aws.amazon.com/iot/latest/developerguide/device-defender-audit-checks.html) to identify overly permissive policies\.    
****Related metrics:****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/dd-detect-security-use-cases.html)

**MQTT\-based data exfiltration: **  
Data exfiltration occurs when a malicious actor carries out an unauthorized data transfer from an IoT deployment or from a device\. The attacker launches this type of attacks through MQTT against cloud\-side data sources\.    
****Related metrics:****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/dd-detect-security-use-cases.html)

**Impersonation:**  
An impersonation attack is where attackers pose as known or trusted entities in an effort to access AWS IoT cloud\-side services, applications, data, or engage in command and control of IoT devices\.     
****Related metrics:****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/dd-detect-security-use-cases.html)

**Cloud Infrastructure abuse:**  
Abuse to AWS IoT cloud services occurs when publishing or subscribing to topics with a high message volume or with messages in large sizes\. Overly permissive policies or device vulnerability exploit for command and control can also cause cloud infrastructure abuse\. One of the main objectives of this attack is to increase your AWS bill\. You should review your [IoT policies](https://docs.aws.amazon.com/iot/latest/developerguide/iot-policies.html) and turn on [Audit overly permissive checks](https://docs.aws.amazon.com/iot/latest/developerguide/device-defender-audit-checks.html) to identify overly permissive policies\.    
****Related metrics:****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/dd-detect-security-use-cases.html)

## Device\-side use cases<a name="Device-side-threats"></a>

Device Defender can monitor the following use cases on your device side\.

**Denial\-of\-service attack:**  
A denial\-of\-service \(DoS\) attack is aimed at shutting down a device or network, making the device or network inaccessible to their intended users\. DoS attacks block access by flooding the target with traffic, or sending it requests that start a system slow\-down or cause the system to fail\. Your IoT devices can be used in DoS attacks\.    
****Related metrics:****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/dd-detect-security-use-cases.html)

**Lateral threat escalation:**  
Lateral threat escalation usually begins with an attacker gaining access to one point of a network, for example a connected device\. The attacker then tries to increase their level of privileges, or their access to other devices through methods such as stolen credentials or vulnerability exploits\.    
****Related metrics:****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/dd-detect-security-use-cases.html)

**Data exfiltration or surveillance:**  
Data exfiltration occurs when malware or a malicious actor carries out an unauthorized data transfer from a device or a network endpoint\. Data exfiltration normally serves two purposes for the attacker, obtaining data or intellectual property, or conducting reconnaissance of a network\. Surveillance means that malicious code is used to monitor user activities for the purpose of stealing credentials and gathering information\. The metrics below can provide a starting point of investigating either type of attacks\.    
****Related metrics:****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/dd-detect-security-use-cases.html)

**Cryptocurrency mining**  
Attackers leverage processing power from devices to mine cryptocurrency\. Crypto\-mining is a computationally intensive process, typically requiring network communication with other mining peers and pools\.    
****Related metrics:****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/dd-detect-security-use-cases.html)

**Command and control, malware and ransomware**  
Malware or ransomware restricts your control over your devices, and limits your device functionality\. In the case of a ransomware attack, data access would be lost due to encryption the ransomware uses\.    
****Related metrics:****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/dd-detect-security-use-cases.html)