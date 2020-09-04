# HTTPS<a name="http"></a>

Clients can publish messages by making requests to the REST API using the HTTP 1\.0 or 1\.1 protocols\. For the authentication and port mappings used by HTTP requests, see [Protocols, port mappings, and authentication](protocols.md#protocol-port-mapping)\.

**Note**  
Unlike MQTT, HTTPS does not support a `clientId` value\. So, while a `clientId` is available when using MQTT, it's not available when using HTTPS\.

## HTTPS message URL<a name="httpurl"></a>

Devices and clients publish their messages by making POST requests to a client\-specific endpoint and a topic\-specific URL:

```
https://IoT_data_endpoint/topics/url_encoded_topic_name?qos=1"
```
+  *IoT\_data\_endpoint* is the [AWS IoT device data endpoint](iot-connect-devices.md#iot-connect-device-endpoints)\. You can find the endpoint in the AWS IoT console on the thing's details page or on the client by using the AWS CLI command: 

  aws iot describe\-endpoint \-\-endpoint\-type iot:Data\-ATS

   The endpoint should look something like this: `a3qjEXAMPLEffp-ats.iot.us-west-2.amazonaws.com` 
+ *url\_encoded\_topic\_name* is the full [topic name](topics.md#topicnames) of the message being sent\.

## HTTPS message code examples<a name="codeexample"></a>

These are some examples of how to send an HTTPS message to AWS IoT\.

------
#### [ Python ]

```
import requests
import argparse

# define command-line parameters
parser = argparse.ArgumentParser(description="Send messages through an HTTPS connection.")
parser.add_argument('--endpoint', required=True, help="Your AWS IoT data custom endpoint, not including a port. " +
                                                      "Ex: \"abcdEXAMPLExyz-ats.iot.us-east-1.amazonaws.com\"")
parser.add_argument('--cert', required=True, help="File path to your client certificate, in PEM format.")
parser.add_argument('--key', required=True, help="File path to your private key, in PEM format.")
parser.add_argument('--topic', required=True, default="test/topic", help="Topic to publish messages to.")
parser.add_argument('--message', default="Hello World!", help="Message to publish. " +
                                                      "Specify empty string to publish nothing.")

# parse and load command-line parameter values
args = parser.parse_args()

# create and format values for HTTPS request
publish_url = 'https://' + args.endpoint + ':8443/topics/' + args.topic + '?qos=1'
publish_msg = args.message.encode('utf-8')

# make request
publish = requests.request('POST',
            publish_url,
            data=publish_msg,
            cert=[args.cert, args.key])

# print results
print("Response status: ", str(publish.status_code))
if publish.status_code == 200:
        print("Response body:", publish.text)
```

------
#### [ CURL ]

You can use [curl](https://curl.haxx.se) from a client or device to send a message to AWS IoT\.

**To use curl to send a message from an AWS IoT client device**

1. Check the curl version\.

   1. On your client, run this command at a command prompt\.

      curl \-\-help

      In the help text, look for the TLS options\. You should see the `--tlsv1.2` option\.

   1. If you see the `--tlsv1.2` option, continue\.

   1. If you don't see the `--tlsv1.2` option or you get a `command not found` error, you might need to update or install curl on your client or install `openssl` before you continue\.

1. Install the certificates on your client\.

   Copy the certificate files that you created when you registered your client \(thing\) in the AWS IoT console\. Make sure you have these three certificate files on your client before you continue\.
   + The CA certificate file \(*Amazon\-root\-CA\-1\.pem* in this example\)\.
   + The client's certificate file \(*device\.pem\.crt* in this example\)\.
   + The client's private key file \(*private\.pem\.key* in this example\)\.

1. Create the curl command line, replacing the replaceable values for those of your account and system\.

   ```
   curl --tlsv1.2 \
       --cacert Amazon-root-CA-1.pem \
       --cert device.pem.crt \
       --key private.pem.key \
       --request POST \ 
       --data "{ \"message\": \"Hello, world\" }" \
       "https://IoT_data_endpoint:8443/topics/topic?qos=1"
   ```  
\-\-tlsv1\.2  
Use TLS 1\.2 \(SSL\)\.  
\-\-cacert *Amazon\-root\-CA\-1\.pem*  
The file name and path, if necessary, of the CA certificate to verify the peer\.  
\-\-cert *device\.pem\.crt*  
The client's certificate file name and path, if necessary\.  
\-\-key *private\.pem\.key*  
The client's private key file name and path, if necessary\.  
\-\-request POST  
The type of HTTP request \(in this case, POST\)\.  
\-\-data "*\{ \\"message\\": \\"Hello, world\\" \}*"  
The HTTP POST data you want to publish\. In this case, it's a JSON string, with the internal quotation marks escaped with the backslash character \(\\\)\.  
"https://*IoT\_data\_endpoint*:8443/topics/*topic*?qos=1"  
The URL of your client's AWS IoT device data endpoint, followed by the HTTPS port, `:8443`, which is then followed by the keyword, `/topics/` and the topic name, `topic`, in this case\. Specify the Quality of Service as the query parameter, `?qos=1`\.

1. Open the MQTT test client in the AWS IoT console\.

   Follow the instructions in [View MQTT messages with the AWS IoT MQTT client](view-mqtt-messages.md) and configure the console to subscribe to messages with the topic name of *topic* used in your curl command, or use the wildcard topic filter of `#`\.

1. Test the command\.

   While monitoring the topic in the test client of the AWS IoT console, go to your client and issue the curl command line that you created in step 3\. You should see your client's messages in the console\.

------