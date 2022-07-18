# Configure local proxy for devices that use web proxy<a name="configure-local-proxy-web-proxy"></a>

You can use local proxy on AWS IoT devices to communicate with AWS IoT secure tunneling APIs\. The local proxy transmits data sent by the device application using secure tunneling over a WebSocket secure connection\. The local proxy can work in `source` or `destination` mode\. In `source` mode, it runs on the same device or network that initiates the TCP connection\. In `destination` mode, the local proxy runs on the remote device, along with the destination application\. For more information, see [Local proxy](local-proxy.md)\.

The local proxy needs to connect directly to the internet to use AWS IoT secure tunneling\. For a long\-lived TCP connection with secure tunneling, the local proxy upgrades the HTTPS request to establish a WebSockets connection to one of the [secure tunneling device connection endpoints](https://docs.aws.amazon.com/general/latest/gr/iot_device_management.html)\.

If your devices are in a network that uses a web proxy, the web proxy can intercept the connections before forwarding them to the internet\. To establish a long\-lived connection to the secure tunneling device connection endpoints, configure your local proxy to use the web proxy as described in the [websocket specification](https://tools.ietf.org/html/rfc6455#section-4.1)\.

**Note**  
The [AWS IoT Device Client](iot-sdks.md#iot-sdk-device-client) doesn't support devices that use a web proxy\. To work with the web proxy, you'll need to use a local proxy and configure it to work with a web proxy as described below\.

The following steps show how the local proxy works with a web proxy\.

1. The local proxy sends an HTTP `CONNECT` request to the web proxy that contains the remote address of the secure tunneling service, along with the web proxy authentication information\.

1. The web proxy will then create a long\-lived connection to the remote secure tunneling endpoints\.

1. The TCP connection is established and the local proxy will now work in both source and destination modes for data transmission\.

**Topics**
+ [Build the local proxy](#build-local-proxy)
+ [Configure your web proxy](#configure-web-proxy)
+ [Configure and start the local proxy](#configure-start-local-proxy)

## Build the local proxy<a name="build-local-proxy"></a>

Open the [local proxy source code](https://github.com/aws-samples/aws-iot-securetunneling-localproxy) in the GitHub repository and follow the instructions for building and installing the local proxy\.

## Configure your web proxy<a name="configure-web-proxy"></a>

The local proxy relies on the HTTP tunneling mechanism described by the [HTTP/1\.1 specification](https://tools.ietf.org/html/rfc7231#section-4.3.6)\. To comply with the specifications, your web proxy must allow devices to use the `CONNECT` method\.

How you configure your web proxy depends on the web proxy you're using and the web proxy version\. To make sure you configure the web proxy correctly, check your web proxy's documentation\.

To configure your web proxy, first identify your web proxy URL and confirm whether your web proxy supports HTTP tunneling\. The web proxy URL will be used later when you configure and start the local proxy\.

1. 

**Identify your web proxy URL**  
Your web proxy URL will be in the following format\.

   ```
   protocol://web_proxy_host_domain:web_proxy_port
   ```

   AWS IoT secure tunneling supports only basic authentication for web proxy\. To use basic authentication, you must specify the **username** and **password** as part of the web proxy URL\. The web proxy URL will be in the following format\.

   ```
   protocol://username:password@web_proxy_host_domain:web_proxy_port
   ```
   + *protocol* can be `http` or `https`\. We recommend that you use `https`\.
   + *web\_proxy\_host\_domain* is the IP address of your web proxy or a DNS name that resolves to the IP address of your web proxy\.
   + *web\_proxy\_port* is the port on which the web proxy is listening\.
   + The web proxy uses this **username** and **password** to authenticate the request\.

1. 

**Test your web proxy URL**  
To confirm whether your web proxy supports TCP tunneling, use a `curl` command and make sure that you get a `2xx` or a `3xx` response\.

   For example, if your web proxy URL is `https://server.com:1235`, use a `proxy-insecure` flag with the `curl` command because the web proxy might rely on a self\-signed certificate\.

   ```
   export HTTPS_PROXY=https://server.com:1235
   curl -I https://aws.amazon.com --proxy-insecure
   ```

   If your web proxy URL has a `http` port \(for example, `http://server.com:1234`\), you don't have to use the `proxy-insecure` flag\.

   ```
   export HTTPS_PROXY=http://server.com:1234
   curl -I https://aws.amazon.com
   ```

## Configure and start the local proxy<a name="configure-start-local-proxy"></a>

To configure the local proxy to use a web proxy, you must configure the `HTTPS_PROXY` environment variable with either the DNS domain names or the IP addresses and port numbers that your web proxy uses\.

After you've configured the local proxy, you can use the local proxy as explained in this [README](https://github.com/aws-samples/aws-iot-securetunneling-localproxy#readme) document\.

**Note**  
Your environment variable declaration is case sensitive\. We recommend that you define each variable once using either all uppercase or all lowercase letters\. The following examples show the environment variable declared in uppercase letters\. If the same variable is specified using both uppercase and lowercase letters, the variable specified using lowercase letters takes precedence\. 

The following commands show how to configure the local proxy that is running on your destination to use the web proxy and start the local proxy\.
+ `AWSIOT_TUNNEL_ACCESS_TOKEN`: This variable holds the client access token \(CAT\) for the destination\.
+ `HTTPS_PROXY`: This variable holds the web proxy URL or the IP address for configuring the local proxy\.

The commands shown in the following examples depend on the operating system that you use and whether the web proxy is listening on an HTTP or an HTTPS port\.

### Web proxy listening on an HTTP port<a name="configure-start-local-proxy-http"></a>

If your web proxy is listening on an HTTP port, you can provide the web proxy URL or IP address for the `HTTPS_PROXY` variable\.

------
#### [ Linux/macOS ]

In Linux or macOS, run the following commands in the terminal to configure and start the local proxy on your destination to use a web proxy listening to an HTTP port\.

```
export AWSIOT_TUNNEL_ACCESS_TOKEN=${access_token}
export HTTPS_PROXY=http:proxy.example.com:1234
./localproxy -r us-east-1 -d 22
```

If you have to authenticate with the proxy, you must specify a **username** and **password** as part of the `HTTPS_PROXY` variable\.

```
export AWSIOT_TUNNEL_ACCESS_TOKEN=${access_token}
export HTTPS_PROXY=http://username:password@proxy.example.com:1234
./localproxy -r us-east-1 -d 22
```

------
#### [ Windows ]

In Windows, you configure the local proxy similar to how you do for Linux or macOS, but how you define the environment variables is different from the other platforms\. Run the following commands in the `cmd` window to configure and start the local proxy on your destination to use a web proxy listening to an HTTP port\.

```
set AWSIOT_TUNNEL_ACCESS_TOKEN=${access_token}
set HTTPS_PROXY=http://proxy.example.com:1234
.\localproxy -r us-east-1 -d 22
```

If you have to authenticate with the proxy, you must specify a **username** and **password** as part of the `HTTPS_PROXY` variable\.

```
set AWSIOT_TUNNEL_ACCESS_TOKEN=${access_token}
set HTTPS_PROXY=http://username:password@10.15.20.25:1234
.\localproxy -r us-east-1 -d 22
```

------

### Web proxy listening on an HTTPS port<a name="configure-start-local-proxy-https"></a>

Run the following commands if your web proxy is listening on an HTTPS port\. 

**Note**  
If you're using a self\-signed certificate for the web proxy or if you're running the local proxy on an OS that doesn't have native OpenSSL support and default configurations, you'll have to set up your web proxy certificates as described in the [Certificate setup](https://github.com/aws-samples/aws-iot-securetunneling-localproxy#certificate-setup) section in the GitHub repository\. 

The following commands will look similar to how you configured your web proxy for an HTTP proxy, with the exception that you'll also specify the path to the certificate files that you installed as described previously\.

------
#### [ Linux/macOS ]

In Linux or macOS, run the following commands in the terminal to configure the local proxy running on your destination to use a web proxy listening to an HTTPS port\.

```
export AWSIOT_TUNNEL_ACCESS_TOKEN=${access_token}
export HTTPS_PROXY=http:proxy.example.com:1234
./localproxy -r us-east-1 -d 22 -c /path/to/certs
```

If you have to authenticate with the proxy, you must specify a **username** and **password** as part of the `HTTPS_PROXY` variable\.

```
export AWSIOT_TUNNEL_ACCESS_TOKEN=${access_token}
export HTTPS_PROXY=http://username:password@proxy.example.com:1234
./localproxy -r us-east-1 -d 22 -c /path/to/certs
```

------
#### [ Windows ]

In Windows, run the following commands in the `cmd` window to configure and start the local proxy running on your destination to use a web proxy listening to an HTTP port\.

```
set AWSIOT_TUNNEL_ACCESS_TOKEN=${access_token}
set HTTPS_PROXY=http://proxy.example.com:1234
.\localproxy -r us-east-1 -d 22 -c \path\to\certs
```

If you have to authenticate with the proxy, you must specify a **username** and **password** as part of the `HTTPS_PROXY` variable\.

```
set AWSIOT_TUNNEL_ACCESS_TOKEN=${access_token}
set HTTPS_PROXY=http://username:password@10.15.20.25:1234
.\localproxy -r us-east-1 -d 22 -c \path\to\certs
```

------

### Example command and output<a name="example-cmd-output-localproxy-webproxy"></a>

The following shows an example of a command that you run on a Linux OS and the corresponding output\. The example shows a web proxy that's listening on an HTTP port and how the local proxy can be configured to use the web proxy in both `source` and `destination` modes\. Before you can run these commands, you must have already opened a tunnel and obtained the client access tokens for the source and destination\. You must have also built the local proxy and configured your web proxy as described previously\.

Here's an overview of the steps after you start the local proxy\. The local proxy:
+ Identifies the web proxy URL so that it can use the URL to connect to the proxy server\.
+ Establishes a TCP connection with the web proxy\.
+ Sends an HTTP `CONNECT` request to the web proxy and waits for the `HTTP/1.1 200` response, which indicates that connection has been established\.
+ Upgrades the HTTPS protocol to WebSockets to establish a long\-lived connection\.
+ Starts transmitting data through the connection to the secure tunneling device endpoints\. 

**Note**  
The following commands used in the examples use the `verbosity` flag to illustrate an overview of the different steps described previously after you run the local proxy\. We recommend that you use this flag only for testing purposes\.

**Running local proxy in source mode**  
The following commands show how to run the local proxy in source mode\.

```
export AWSIOT_TUNNEL_ACCESS_TOKEN=${access_token}
export HTTPS_PROXY=http:username:password@10.15.10.25:1234
./localproxy -s 5555 -v 5 -r us-west-2
```

The following shows a sample output of running the local proxy in `source` mode\.

```
...

Parsed basic auth credentials for the URL
Found Web proxy information in the environment variables, will use it to connect via the proxy.

...

Starting proxy in source mode
Attempting to establish web socket connection with endpoint wss://data.tunneling.iot.us-west-2.amazonaws.com:443
Resolved Web proxy IP: 10.10.0.11
Connected successfully with Web Proxy
Successfully sent HTTP CONNECT to the Web proxy
Full response from the Web proxy:
HTTP/1.1 200 Connection established
TCP tunnel established successfully
Connected successfully with proxy server
Successfully completed SSL handshake with proxy server
Web socket session ID: 0a109afffee745f5-00001341-000b8138-cc6c878d80e8adb0-f186064b
Web socket subprotocol selected: aws.iot.securetunneling-2.0
Successfully established websocket connection with proxy server: wss://data.tunneling.iot.us-west-2.amazonaws.com:443
Seting up web socket pings for every 5000 milliseconds
Scheduled next read:

...

Starting web socket read loop continue reading...
Resolved bind IP: 127.0.0.1
Listening for new connection on port 5555
```

**Running local proxy in destination mode**  
The following commands show how to run the local proxy in destination mode\.

```
export AWSIOT_TUNNEL_ACCESS_TOKEN=${access_token}
export HTTPS_PROXY=http:username:password@10.15.10.25:1234
./localproxy -d 22 -v 5 -r us-west-2
```

The following shows a sample output of running the local proxy in `destination` mode\.

```
...

Parsed basic auth credentials for the URL
Found Web proxy information in the environment variables, will use it to connect via the proxy.

...

Starting proxy in destination mode
Attempting to establish web socket connection with endpoint wss://data.tunneling.iot.us-west-2.amazonaws.com:443
Resolved Web proxy IP: 10.10.0.1
Connected successfully with Web Proxy
Successfully sent HTTP CONNECT to the Web proxy
Full response from the Web proxy:
HTTP/1.1 200 Connection established
TCP tunnel established successfully
Connected successfully with proxy server
Successfully completed SSL handshake with proxy server
Web socket session ID: 06717bfffed3fd05-00001355-000b8315-da3109a85da804dd-24c3d10d
Web socket subprotocol selected: aws.iot.securetunneling-2.0
Successfully established websocket connection with proxy server: wss://data.tunneling.iot.us-west-2.amazonaws.com:443
Seting up web socket pings for every 5000 milliseconds
Scheduled next read:

...

Starting web socket read loop continue reading...
```