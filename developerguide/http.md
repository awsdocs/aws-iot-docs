# HTTP<a name="http"></a>

The message broker supports clients connecting with the HTTP 1\.0 and 1\.1 protocols using a REST API\. For more information about authentication and port mappings for HTTP requests, see [Protocols, Port Mappings, and Authentication](protocols.md#protocol-port-mapping)\. Clients can publish by sending a POST message to `<AWS IoT Endpoint>/topics/<url_encoded_topic_name>?qos=1"`\. 

For example, you can use [curl](https://curl.haxx.se) to emulate sending a message\. For example: 

```
curl --tlsv1.2 --cacert root-CA.crt --cert 4b7828d2e5-certificate.pem.crt --key 4b7828d2e5-private.pem.key -X POST -d "{ \"message\": \"Hello, world\" }" "https://a1pn10j0v8htvw.iot.us-east-1.amazonaws.com:8443/topics/my/topic"
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
The type of request \(in this case, POST\)\.

\-d <data>  
The HTTP POST data you want to publish\.

"https://\.\.\."  
The URL\. In this case, the REST API endpoint for the thing\.   
To find the endpoint for a thing, in the AWS IoT console, choose **Registry** to expand your choices\. Choose **Things**, choose the thing, and then choose **Interact**\.\) After the endpoint add the port \(:8443\) followed by the keyword "topics", the topic and, finally, specify the quality of service in a query string \(?qos=1\)\.