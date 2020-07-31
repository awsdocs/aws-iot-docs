# Protocols<a name="protocols"></a>

The message broker supports clients that use the MQTT protocol to publish and subscribe to messages and the HTTPS protocol to publish messages\. Both protocols are supported through IP version 4 and IP version 6\. The message broker also supports the MQTT protocol over the WebSocket protocol\. The way in which a client can connect to the message broker depends on the protocol used\.

The AWS IoT message broker and Device Shadow service use [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) [version 1\.2](https://en.wikipedia.org/wiki/Transport_Layer_Security#TLS_1.2) to encrypt all communication\. For more information, see [Transport security in AWS IoT](transport-security.md)\.

## Protocols, port mappings, and authentication<a name="protocol-port-mapping"></a>

The following table lists the protocols supported by AWS IoT and the authentication methods and ports used by each\.


**Protocols, authentication, and port mappings**  

| Protocol | Authentication | Port | ALPN protocol name | 
| --- | --- | --- | --- | 
|  MQTT over WebSocket  | Signature Version 4 | 443 |  N/A  | 
|  MQTT  |  X\.509 client certificate  |  443†  |  `x-amzn-mqtt-ca`  | 
| MQTT | X\.509 client certificate | 8883 | N/A | 
|  HTTPS  |  Signature Version 4  |  443  |  N/A  | 
|  HTTPS  |  X\.509 client certificate  |  443†  |  `x-amzn-http-ca`  | 
| HTTPS | X\.509 client certificate | 8443 | N/A | 

†Clients that connect on port 443 with X\.509 client certificate authentication must implement the [Application Layer Protocol Negotiation \(ALPN\)](https://tools.ietf.org/html/rfc7301) TLS extension and use the [ALPN protocol name](https://tools.ietf.org/html/rfc7301#section-3.1) listed in the ALPN ProtocolNameList sent by the client as part of the `ClientHello` message\. Clients must also send the [Server Name Indication \(SNI\) TLS extension](https://tools.ietf.org/html/rfc3546#section-3.1) to prevent their connections from being refused\. For more information, see [Transport Security in AWS IoT](transport-security.html)\. 

**Note**  
Unlike MQTT, HTTPS does not support a `clientId` value\. So, while a `clientId` is available when using MQTT, it is not available when using HTTPS\.