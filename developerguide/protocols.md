# Protocols<a name="protocols"></a>

The message broker supports clients that use the MQTT protocol to publish and subscribe to messages and the HTTPS protocol to publish messages\. Both protocols are supported through IP version 4 and IP version 6\. The message broker also supports the MQTT protocol over the WebSocket protocol\. The way in which a client can connect to the message broker depends on the protocol used\.

## Protocols, Port Mappings, and Authentication<a name="protocol-port-mapping"></a>

The following table lists the protocols supported by AWS IoT and the authentication methods and ports used by each\.


**Protocols, Authentication, and Port Mappings**  

| Protocol | Authentication | Port | ALPN Protocol Name | 
| --- | --- | --- | --- | 
|  MQTT over WebSocket  | Signature Version 4 | 443 |  N/A  | 
|  MQTT  |  X\.509 client certificate  |  443†  |  `x-amzn-mqtt-ca`  | 
| MQTT | X\.509 client certificate | 8883 | N/A | 
|  HTTPS  |  Signature Version 4  |  443  |  N/A  | 
|  HTTPS  |  X\.509 client certificate  |  443†  |  `x-amzn-http-ca`  | 
| HTTPS | X\.509 client certificate | 8443 | N/A | 

†Clients that connect on port 443 with X\.509 client certificate authentication must implement the [Application Layer Protocol Negotiation \(ALPN\)](https://tools.ietf.org/html/rfc7301) TLS extension and use the [ALPN protocol name](https://tools.ietf.org/html/rfc7301#section-3.1) listed in the ALPN ProtocolNameList sent by the client as part of the `ClientHello` message\.