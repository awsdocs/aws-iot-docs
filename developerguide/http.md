# HTTP<a name="http"></a>

Clients can publish messages by making requests to the REST API using the HTTP 1\.0 or 1\.1 protocols\. For the authentication and port mappings used by HTTP requests, see [Protocols, port mappings, and authentication](protocols.md#protocol-port-mapping)\.

**Note**  
Unlike MQTT, HTTPS does not support a `clientId` value\. So, while a `clientId` is available when using MQTT, it is not available when using HTTPS\.

## HTTP URL<a name="httpurl"></a>

Publishing clients make POST requests to a client\-specific endpoint and a topic\-specific URL: `https://AWS IoT endpoint/topics/url_encoded_topic_name?qos=1"`\.
+  `AWS IoT endpoint` is the REST API endpoint\. You can find the endpoint in the AWS IoT console on the thing's details page or on the client by using the AWS IoT CLI command: 

  aws iot describe\-endpoint \-\-endpoint\-type iot:Data\-ATS

   The endpoint should look something like this: `a3qj468xinsffp-ats.iot.us-west-2.amazonaws.com` 
+ *url\_encoded\_topic\_name* is the full [topic name](topics.md#topicnames) of the message being sent\.

## Curl example<a name="curlexample"></a>

You can use [curl](https://curl.haxx.se) to emulate a client sending a message to AWS IoT Core\.

**To use curl to send a message from an AWS IoT Core client device**

1. Check the curl version\.

   1. On your client, run this command at a command prompt\.

      curl \-\-help

      In the help text, look for the TLS options\. You should see the `--tlsv1.2` option\.

   1. If you see the `--tlsv1.2` option, continue\.

   1. If you don't see the `--tlsv1.2` option or you get a `command not found` error, you might need to update or install curl on your client or install `openssl` before you continue\.

1. Install the certificates on your client\.

   Copy the certificate files that you created when you registered your client \(thing\) in the AWS IoT console\. Make sure you have these three certificate files on your client before you continue\.
   + The client's private key file \(`private.key` in this example\)\.
   + The client's certificate file \(`certificate.pem.crt` in this example\)\.
   + The CA certificate file \(`AmazonRootCA1.pem` in this example\)\.

1. Create the curl command line\.

   curl \-\-tlsv1\.2 \-\-cacert *AmazonRootCA1\.pem* \-\-cert *certificate\.pem\.crt* \-\-key *private\.key* \-X POST \-d "\{ \\"message\\": \\"Hello, world\\" \}" "https://*AWS IoT Endpoint*:8443/topics/*topic*?qos=1"  
\-\-tlsv1\.2  
Use TLS 1\.2 \(SSL\)\.  
\-\-cacert <filename>  
The file name and path, if necessary, of the CA certificate to verify the peer\.  
\-\-cert <filename>  
The client's certificate file name and path, if necessary\.  
\-\-key <filename>  
The client's private key file name and path, if necessary\.  
\-X POST  
The type of HTTP request \(in this case, POST\)\.  
\-d <data>  
The HTTP POST data you want to publish\. In this case, it's a JSON string, with the internal quotation marks escaped with the backslash character \(\\\)\.  
"https://*AWS IoT endpoint*:8443/topics/*topic*?qos=1"  
The URL of your client's REST API endpoint, followed by the HTTPS port, `:8443`, which is then followed by the keyword, `/topics/` and the topic name, `topic`, in this case\. Specify the quality of service as the query parameter, `?qos=1`\.

1. Open the MQTT test client in the console\.

   Follow the instructions in [View MQTT messages with the AWS IoT MQTT client](view-mqtt-messages.md) and configure the console to subscribe to messages with the topic name of `topic`, or use the wildcard topic filter of `#`\.

1. Test the command\.

   While monitoring the topic in the test client of the AWS IoT console, go to your client and issue the curl command line that you created in step 3\. You should see your client's messages in the console\.