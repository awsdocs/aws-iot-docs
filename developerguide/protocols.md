# Protocols<a name="protocols"></a>

The message broker supports the use of the MQTT protocol to publish and subscribe and the HTTPS protocol to publish\. Both protocols are supported through IP version 4 and IP version 6\. The message broker also supports MQTT over the WebSocket protocol\.

## Protocol/Port Mappings<a name="protocol-port-mapping"></a>

The following table shows each protocol supported by AWS IoT, the authentication method, and port used for each protocol\.


**Protocol, Authentication, and Port Mappings**  

| Protocol | Authentication | Port | 
| --- | --- | --- | 
|  MQTT  |  Client Certificate  |  8883, 443†  | 
|  HTTP  |  Client Certificate  |  8443  | 
|  HTTP  |  SigV4  |  443  | 
|  MQTT \+ WebSocket  |  SigV4  |  443  | 

†Clients wishing to connect using MQTT with X\.509 Client Certificate authentication on port 443 must implement the [Application Layer Protocol Negotiation \(ALPN\)](https://tools.ietf.org/html/rfc7301) TLS extension and pass `x-amzn-mqtt-ca` as the [ProtocolName](https://tools.ietf.org/html/rfc7301#section-3.1) in the [ProtocolNameList](https://tools.ietf.org/html/rfc7301#section-3.1)\. Note that ALPN is not required to open connections using MQTT with X\.509 Client Certificate authentication on port 8883\.

## MQTT<a name="mqtt"></a>

MQTT is a widely adopted lightweight messaging protocol designed for constrained devices\. For more information, see [MQTT](http://www.mqtt.org)\. 

Although the AWS IoT message broker implementation is based on MQTT version 3\.1\.1, it deviates from the specification as follows:

+ In AWS IoT, subscribing to a topic with Quality of Service \(QoS\) 0 means a message will be delivered zero or more times\. A message might be delivered more than once\. Messages delivered more than once might be sent with a different packet ID\. In these cases, the DUP flag is not set\.

+ AWS IoT does not support publishing and subscribing with QoS 2\. The AWS IoT message broker does not send a PUBACK or SUBACK when QoS 2 is requested\.

+ The QoS levels for publishing and subscribing to a topic have no relation to each other\. One client can subscribe to a topic using QoS 1 while another client can publish to the same topic using QoS 0\.

+ When responding to a connection request, the message broker sends a CONNACK message\. This message contains a flag to indicate if the connection is resuming a previous session\. The value of this flag might be incorrect if two MQTT clients connect with the same client ID simultaneously\.

+ When a client subscribes to a topic, there might be a delay between the time the message broker sends a SUBACK and the time the client starts receiving new matching messages\.

+ The MQTT specification provides a provision for the publisher to request that the broker retain the last message sent to a topic and send it to all future topic subscribers\. AWS IoT does not support retained messages\. If a request is made to retain messages, the connection is disconnected\.

+ The message broker uses the client ID to identify each client\. The client ID is passed in from the client to the message broker as part of the MQTT payload\. Two clients with the same client ID are not allowed to be connected concurrently to the message broker\. When a client connects to the message broker using a client ID that another client is using, a CONNACK message will be sent to both clients and the currently connected client will be disconnected\.

+ The message broker does not support persistent sessions \(connections made with the `cleanSession` flag set to `false`\. The AWS IoT message broker assumes all sessions are clean sessions and messages are not stored across sessions\. If an MQTT client attempts to connect to the AWS IoT message broker with the `cleanSession` set to `false`, the client will be disconnected\.

+ On rare occasions, the message broker might resend the same logical PUBLISH message with a different packet ID\.

+ The message broker does not guarantee the order in which messages and ACK are received\.

## HTTP<a name="http"></a>

The message broker supports clients connecting with the HTTP protocol using a REST API\. Clients can publish by sending a POST message to `<AWS IoT Endpoint>/<url_encoded_topic_name>?qos=1"`\. 

For example, you can use [curl](https://curl.haxx.se) to emulate a button press\. If you followed the tutorial in Getting Started with AWS IoT, rather than using the AWS IoT MQTT client to publish a message as you did in AWS IoT MQTT Client, use something like the following command: 

```
curl --tlsv1.2 --cacert root-CA.crt --cert 4b7828d2e5-certificate.pem.crt --key 4b7828d2e5-private.pem.key -X POST -d "{ \"serialNumber\": \"G030JF053216F1BS\", \"clickType\": \"SINGLE\", \"batteryVoltage\": \"2000mV\" }" "https://a1pn10j0v8htvw.iot.us-east-1.amazonaws.com:8443/topics/iotbutton/virtualButton?qos=1"
```

\-\-tlsv1\.2  
Use TLSv1\.2 \(SSL\)\. curl must be installed with OpenSSL and you must use version 1\.2 of TLS\.

\-\-cacert <filename>  
The filename of the CA certificate to verify the peer\.

\-\-cert <filename>  
The client certificate filename\.

\-\-key <filename>  
The private key filename\.

\-X POST  
The type of request, in this case, POST\.

\-d <data>  
The HTTP POST data you want to publish\. In this case, we emulate the data sent by a single button press\.

"https://\.\.\."  
The URL\. In this case the REST API endpoint for the thing\. \(To find the endpoint for a thing, from the AWS IoT console choose **Registry** to expand your choices\. Choose **Things**, choose the thing, and then choose **Interact**\.\) After the endpoint add the port \(:8443\) followed by the topic and, finally, specify the quality of service in a query string \(?qos=1\)\.

## MQTT Over the WebSocket Protocol<a name="mqtt-ws"></a>

AWS IoT supports MQTT over the [WebSocket](https://en.wikipedia.org/wiki/WebSocket) protocol to enable browser\-based and remote applications to send and receive data from AWS IoT\-connected devices using AWS credentials\. AWS credentials are specified using [AWS Signature Version 4](http://alpha-docs-aws.amazon.com/general/latest/gr//sigv4_signing.html)\. WebSocket support is available on TCP port 443, which allows messages to pass through most firewalls and web proxies\.

A WebSocket connection is initiated on a client by sending an HTTP GET request\. The URL you use is of the following form:

```
wss://<endpoint>.iot.<region>.amazonaws.com/mqtt
```

wss  
Specifies the WebSocket protocol\.

endpoint  
Your AWS account\-specific AWS IoT endpoint\. You can use the AWS IoT CLI [describe\-endpoint](http://alpha-docs-aws.amazon.com/cli/latest/reference/iot/describe-endpoint.html) command to find this endpoint\.

region  
The AWS region of your AWS account\.

mqtt  
Specifies you will be sending MQTT messages over the WebSocket protocol\.

When the server responds, the client sends an upgrade request to indicate to the server it will communicate using the WebSocket protocol\. After the server acknowledges the upgrade request, all communication is performed using the WebSocket protocol\. The WebSocket implementation you use acts as a transport protocol\. The data you send over the WebSocket protocol are MQTT messages\.

### Using the WebSocket Protocol in a Web Application<a name="mqtt-ws-web-app"></a>

The WebSocket implementation provided by most web browsers does not allow the modification of HTTP headers, so you must add the Signature Version 4 information to the query string\. For more information, see [Adding Signing Information to the Query String](http://alpha-docs-aws.amazon.com/general/latest/gr/sigv4-add-signature-to-request.html#sigv4-add-signature-querystring)\. 

The following JavaScript defines some utility functions used in generating a Signature Version 4 request\.

```
  /**
   * utilities to do sigv4
   * @class SigV4Utils
   */
  function SigV4Utils() {}

SigV4Utils.getSignatureKey = function (key, date, region, service) {
    var kDate = AWS.util.crypto.hmac('AWS4' + key, date, 'buffer');
    var kRegion = AWS.util.crypto.hmac(kDate, region, 'buffer');
    var kService = AWS.util.crypto.hmac(kRegion, service, 'buffer');
    var kCredentials = AWS.util.crypto.hmac(kService, 'aws4_request', 'buffer');    
    return kCredentials;
};

SigV4Utils.getSignedUrl = function(host, region, credentials) {
    var datetime = AWS.util.date.iso8601(new Date()).replace(/[:\-]|\.\d{3}/g, '');
    var date = datetime.substr(0, 8);

    var method = 'GET';
    var protocol = 'wss';
    var uri = '/mqtt';
    var service = 'iotdevicegateway';
    var algorithm = 'AWS4-HMAC-SHA256';

    var credentialScope = date + '/' + region + '/' + service + '/' + 'aws4_request';
    var canonicalQuerystring = 'X-Amz-Algorithm=' + algorithm;
    canonicalQuerystring += '&X-Amz-Credential=' + encodeURIComponent(credentials.accessKeyId + '/' + credentialScope);
    canonicalQuerystring += '&X-Amz-Date=' + datetime;
    canonicalQuerystring += '&X-Amz-SignedHeaders=host';

    var canonicalHeaders = 'host:' + host + '\n';
    var payloadHash = AWS.util.crypto.sha256('', 'hex')
    var canonicalRequest = method + '\n' + uri + '\n' + canonicalQuerystring + '\n' + canonicalHeaders + '\nhost\n' + payloadHash;

    var stringToSign = algorithm + '\n' + datetime + '\n' + credentialScope + '\n' + AWS.util.crypto.sha256(canonicalRequest, 'hex');
    var signingKey = SigV4Utils.getSignatureKey(credentials.secretAccessKey, date, region, service);
    var signature = AWS.util.crypto.hmac(signingKey, stringToSign, 'hex');

    canonicalQuerystring += '&X-Amz-Signature=' + signature;
    if (credentials.sessionToken) {
        canonicalQuerystring += '&X-Amz-Security-Token=' + encodeURIComponent(credentials.sessionToken);
    }

    var requestUrl = protocol + '://' + host + uri + '?' + canonicalQuerystring;
    return requestUrl;
};
```

**To create a Signature Version 4 request**

1. Create a canonical request for Signature Version 4\.

   The following JavaScript code creates a canonical request:

   ```
   var datetime = AWS.util.date.iso8601(new Date()).replace(/[:\-]|\.\d{3}/g, '');
   var date = datetime.substr(0, 8);
   
   var method = 'GET';
   var protocol = 'wss';
   var uri = '/mqtt';
   var service = 'iotdevicegateway';
   var algorithm = 'AWS4-HMAC-SHA256';
   
   var credentialScope = date + '/' + region + '/' + service + '/' + 'aws4_request';
   var canonicalQuerystring = 'X-Amz-Algorithm=' + algorithm;
   canonicalQuerystring += '&X-Amz-Credential=' + encodeURIComponent(credentials.accessKeyId + '/' + credentialScope);
   canonicalQuerystring += '&X-Amz-Date=' + datetime;
   canonicalQuerystring += '&X-Amz-SignedHeaders=host';
   
   var canonicalHeaders = 'host:' + host + '\n';
   var payloadHash = AWS.util.crypto.sha256('', 'hex')
   var canonicalRequest = method + '\n' + uri + '\n' + canonicalQuerystring + '\n' + canonicalHeaders + '\nhost\n' + payloadHash;
   ```

1. Create a string to sign, generate a signing key, and sign the string\.

   Take the canonical URL you created in the previous step and assemble it into a string to sign\. You do this by creating a string composed of the hashing algorithm, the date, the credential scope, and the SHA of the canonical request\. Next, generate the signing key and sign the string, as shown in the following JavaScript code\.

   ```
   var stringToSign = algorithm + '\n' + datetime + '\n' + credentialScope + '\n' + AWS.util.crypto.sha256(canonicalRequest, 'hex');
   var signingKey = SigV4Utils.getSignatureKey(credentials.secretAccessKey, date, region, service);
   var signature = AWS.util.crypto.hmac(signingKey, stringToSign, 'hex');
   ```

1. Add the signing information to the request\.

   The following JavaScript code shows how to add the signing information to the query string\.

   ```
   canonicalQuerystring += '&X-Amz-Signature=' + signature;
   ```

1. If you have session credentials \(from an STS server, AssumeRole, or Amazon Cognito\), append the session token to the end of the URL string after signing:

   ```
   canonicalQuerystring += '&X-Amz-Security-Token=' + encodeURIComponent(credentials.sessionToken);
   ```

1. Prepend the protocol, host, and URI to the canonicalQuerystring:

   ```
   var requestUrl = protocol + '://' + host + uri + '?' + canonicalQuerystring;
   ```

1. Open the WebSocket\.

   The following JavaScript code shows how to create a Paho MQTT client and call CONNECT to AWS IoT\. The `endpoint` argument is your AWS account\-specific endpoint\. The `clientId` is a text identifier that is unique among all clients simultaneously connected in your AWS account\.

   ```
   var client = new Paho.MQTT.Client(requestUrl, clientId);
   var connectOptions = {
       onSuccess: function(){
           // connect succeeded
       },
       useSSL: true,
       timeout: 3,
       mqttVersion: 4,
       onFailure: function() {
           // connect failed
       }
   };
   client.connect(connectOptions);
   ```

### Using the WebSocket Protocol in a Mobile Application<a name="mqtt-ws-mobile-app"></a>

We recommend using one of the AWS IoT Device SDKs to connect your device to AWS IoT when making a WebSocket connection\. The following AWS IoT Device SDKs support WebSocket\-based MQTT connections to AWS IoT:

+ [Node\.js](https://github.com/aws/aws-iot-device-sdk-js)

+ [iOS](http://alpha-docs-aws.amazon.com/mobile/sdkforios/developerguide/)

+ [Android](http://alpha-docs-aws.amazon.com/mobile/sdkforandroid/developerguide/)

For a reference implementation for connecting a web application to AWS IoT using MQTT over the WebSocket protocol, see [AWS Labs WebSocket sample](https://github.com/awslabs/aws-iot-examples)\.

If you are using a programming or scripting language that is not currently supported, any existing WebSocket library can be used as long as the initial WebSocket upgrade request \(HTTP POST\) is signed using AWS Signature Version 4\. Some MQTT clients, such as [Eclipse Paho for JavaScript](http://www.eclipse.org/paho/), support the WebSocket protocol natively\.