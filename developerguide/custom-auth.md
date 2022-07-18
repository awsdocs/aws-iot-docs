# Connecting to AWS IoT Core by using custom authentication<a name="custom-auth"></a>

 Devices can connect to AWS IoT Core by using custom authentication with any protocol that AWS IoT Core supports for device messaging\. For more information about supported communication protocols, see [Device communication protocols](protocols.md)\.  The connection data that you pass to your authorizer Lambda function depends on the protocol you use\. For more information about creating your authorizer Lambda function, see [Defining your Lambda function](config-custom-auth.md#custom-auth-lambda)\. The following sections explain how to connect to authenticate by using each supported protocol\. 

## HTTP<a name="custom-auth-http"></a>

Devices sending data to AWS IoT Core by using the [HTTP Publish API](https://docs.aws.amazon.com/iot/latest/apireference/API_iotdata_Publish.html) can pass credentials either through request headers or query parameters in their HTTP POST requests\. Devices can specify an authorizer to invoke by using the `x-amz-customauthorizer-name` header or query parameter\. If you have token signing enabled in your authorizer, you must pass the `token-key-name` and `x-amz-customauthorizer-signature` in either request headers or query parameters\. Note that the `token-signature` value must be URL\-encoded when using JavaScript from within the browser\.

The following example requests show how you pass these parameters in both request headers and query parameters\. 

```
//Passing credentials via headers
POST /topics/topic?qos=qos HTTP/1.1
Host: your-endpoint 
x-amz-customauthorizer-signature: token-signature
token-key-name: token-value 
x-amz-customauthorizer-name: authorizer-name

//Passing credentials via query parameters
POST /topics/topic?qos=qos&x-amz-customauthorizer-signature=token-signature&token-key-name=token-value HTTP/1.1
```

## MQTT<a name="custom-auth-mqtt"></a>

 Devices connecting to AWS IoT Core by using an MQTT connection can pass credentials through the `username` and `password` fields of MQTT messages\. The `username` value can also optionally contain a query string that passes additional values \(including a token, signature, and authorizer name\) to your authorizer\. You can use this query string if you want to use a token\-based authentication scheme instead of `username` and `password` values\.  

**Note**  
 Data in the password field is base64\-encoded by AWS IoT Core\. Your Lambda function must decode it\. 

 The following example contains a `username` string that contains extra parameters that specify a token and signature\.  

```
username?x-amz-customauthorizer-name=authorizer-name&x-amz-customauthorizer-signature=token-signature&token-key-name=token-value
```

In order to invoke an authorizer, devices connecting to AWS IoT Core by using MQTT and custom authentication must connect on port 443\. They also must pass the Application Layer Protocol Negotiation \(ALPN\) TLS extension with a value of `mqtt` and the Server Name Indication \(SNI\) extension with the host name of their AWS IoT Core data endpoint\. To avoid potential errors, the value for `x-amz-customauthorizer-signature` should be URL encoded\. We also highly recommend that the values of `x-and-customauthorizer-name` and `token-key-name` be URL encoded\. For more information about these values, see [Device communication protocols](protocols.md)\. The V2 [AWS IoT Device SDKs, Mobile SDKs, and AWS IoT Device Client](iot-sdks.md) can configure both of these extensions\. 

## MQTT over WebSockets<a name="custom-auth-websockets"></a>

 Devices connecting to AWS IoT Core by using MQTT over WebSockets can pass credentials in one of the two following ways\. 
+ Through request headers or query parameters in the HTTP UPGRADE request to establish the WebSockets connection\.
+ Through the `username` and `password` fields in the MQTT CONNECT message\.

 If you pass credentials through the MQTT connect message, the ALPN and SNI TLS extensions are required\. For more information about these extensions, see [MQTT](#custom-auth-mqtt)\.   The following example demonstrates how to pass credentials through the HTTP Upgrade request\. 

```
GET /mqtt HTTP/1.1
Host: your-endpoint 
Upgrade: WebSocket 
Connection: Upgrade 
x-amz-customauthorizer-signature: token-signature
token-key-name: token-value 
sec-WebSocket-Key: any random base64 value 
sec-websocket-protocol: mqtt 
sec-WebSocket-Version: websocket version
```

## Signing the token<a name="custom-auth-token-signature"></a>

You must sign the token with the private key of the public\-private key pair that you used in the `create-authorizer` call\. The following examples show how to create the token signature by using a UNIX\-like command and JavaScript\. They use the SHA\-256 hash algorithm to encode the signature\.

------
#### [ Command line ]

```
echo -n TOKEN_VALUE | openssl dgst -sha256 -sign PEM encoded RSA private key | openssl base64
```

------
#### [ JavaScript ]

```
const crypto = require('crypto')

const key = "PEM encoded RSA private key"

const k = crypto.createPrivateKey(key)
let sign = crypto.createSign('SHA256')
sign.write(t)
sign.end()
const s = sign.sign(k, 'base64')
```

------