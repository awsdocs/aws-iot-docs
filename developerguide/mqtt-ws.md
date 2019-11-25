# MQTT over the WebSocket Protocol<a name="mqtt-ws"></a>

AWS IoT supports MQTT over the [WebSocket](https://en.wikipedia.org/wiki/WebSocket) protocol to enable browser\-based and remote applications to send and receive data from AWS IoT\-connected devices using AWS credentials\. AWS credentials are specified using [AWS Signature Version 4](https://docs.aws.amazon.com/general/latest/gr//sigv4_signing.html)\. WebSocket support is available on TCP port 443, which allows messages to pass through most firewalls and web proxies\.

A WebSocket connection is initiated on a client by sending an HTTP GET request\. The URL you use is of the following form:

```
wss://<endpoint>.iot.<region>.amazonaws.com/mqtt
```

wss  
Specifies the WebSocket protocol\.

endpoint  
Your AWS account\-specific AWS IoT endpoint\. You can use the AWS IoT CLI [describe\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/iot/describe-endpoint.html) command to find this endpoint\.

region  
The AWS Region of your AWS account\.

mqtt  
Specifies you are sending MQTT messages over the WebSocket protocol\.

When the server responds, the client sends an upgrade request to indicate to the server it will communicate using the WebSocket protocol\. After the server acknowledges the upgrade request, all communication is performed using the WebSocket protocol\. The WebSocket implementation you use acts as a transport protocol\. The data you send over the WebSocket protocol are MQTT messages\.

## Using the WebSocket Protocol in a Web Application<a name="mqtt-ws-web-app"></a>

The WebSocket implementation provided by most web browsers does not allow the modification of HTTP headers, so you must add the Signature Version 4 information to the query string\. For more information, see [Adding Signing Information to the Query String](https://docs.aws.amazon.com/general/latest/gr/sigv4-add-signature-to-request.html#sigv4-add-signature-querystring)\. 

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

1. Prepend the protocol, host, and URI to the `canonicalQuerystring`:

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

## Using the WebSocket Protocol in a Mobile Application<a name="mqtt-ws-mobile-app"></a>

We recommend using one of the AWS IoT Device SDKs to connect your device to AWS IoT when making a WebSocket connection\. The following AWS IoT Device SDKs support WebSocket\-based MQTT connections to AWS IoT:
+ [Node\.js](https://github.com/aws/aws-iot-device-sdk-js)
+ [iOS](https://docs.aws.amazon.com/mobile/sdkforios/developerguide/)
+ [Android](https://docs.aws.amazon.com/mobile/sdkforandroid/developerguide/)

For a reference implementation for connecting a web application to AWS IoT using MQTT over the WebSocket protocol, see [AWS Labs WebSocket sample](https://github.com/awslabs/aws-iot-examples)\.

If you are using a programming or scripting language that is not currently supported, any existing WebSocket library can be used as long as the initial WebSocket upgrade request \(HTTP POST\) is signed using Signature Version 4\. Some MQTT clients, such as [Eclipse Paho for JavaScript](http://www.eclipse.org/paho/), support the WebSocket protocol natively\.