# Protocols<a name="protocols"></a>

The message broker supports the use of the MQTT protocol to publish and subscribe and the HTTPS protocol to publish\. Both protocols are supported through IP version 4 and IP version 6\. The message broker also supports MQTT over the WebSocket protocol\.

## Protocol/Port Mappings<a name="protocol-port-mapping"></a>

The following table shows each protocol supported by AWS IoT, the authentication method, and port used for each protocol\.


**Protocol, Authentication, and Port Mappings**  

| Protocol | Authentication | Port | ALPN ProtocolName | 
| --- | --- | --- | --- | 
|  MQTT  |  X\.509 client certificate  |  8883, 443†  |  `x-amzn-mqtt-ca`  | 
|  HTTPS  |  X\.509 client certificate  |  8443, 443†  |  `x-amzn-http-ca`  | 
|  HTTPS  |  SigV4  |  443  |  N/A  | 
|  MQTT over WebSocket  |  SigV4  |  443  |  N/A  | 

†Clients that connect on port 443 with X\.509 client certificate authentication must implement the [Application Layer Protocol Negotiation \(ALPN\)](https://tools.ietf.org/html/rfc7301) TLS extension and use the [ALPN ProtocolName](https://tools.ietf.org/html/rfc7301#section-3.1) listed above in the [ALPN ProtocolNameList](https://tools.ietf.org/html/rfc7301#section-3.1) sent by the client as part of the `ClientHello` message\.