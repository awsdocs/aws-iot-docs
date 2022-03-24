# How to use the local proxy<a name="how-use-local-proxy"></a>

You can run the local proxy on the source and destination devices to transmit data to the secure tunneling endpoints\. If your devices are in a network that use a web proxy, the web proxy can intercept the connections before forwarding them to the internet\. In this case, you'll need to configure your local proxy to use the web proxy\. For more information, see [Configure local proxy for devices that use web proxy](configure-local-proxy-web-proxy.md)\. 

## Local proxy workflow<a name="local-proxy-workflow"></a>

The following steps show how the local proxy is run on the source and destination devices\.

1. 

**Connect local proxy to secure tunneling**  
First, local proxy must establish a connection to secure tunneling\. When you start the local proxy, use the following arguments:
   + The `-r` argument to specify the AWS Region in which the tunnel is opened\.
   + The `-t` argument to pass either the source or destination client access token returned from the `OpenTunnel`\.
**Note**  
Two local proxies using the same client access token value cannot be connected at the same time\.

1. 

**Perform source or destination actions**  
After the WebSocket connection is established, the local proxy performs either source mode or destination mode actions, depending on its configuration\.

   By default, the local proxy attempts to reconnect to secure tunneling if any input/output \(I/O\) errors occur or if the WebSocket connection is closed unexpectedly\. This causes the TCP connection to close\. If any TCP socket errors occur, the local proxy sends a message through the tunnel to notify the other side to close its TCP connection\. By default, the local proxy always uses SSL communication\.

1. 

**Stop the local proxy**  
After you use the tunnel, it is safe to stop the local proxy process\. We recommend that you explicitly close the tunnel by calling `CloseTunnel`\. Active tunnel clients might not be closed immediately after calling `CloseTunnel`\.

For more information about how to use the AWS Management Console to open a tunnel and start an SSH session, see [Open a tunnel and start SSH session to remote device](secure-tunneling-tutorial-open-tunnel.md)\.

## Local proxy best practices<a name="local-proxy-security"></a>

When running the local proxy, follow these best practices:
+ Avoid the use of the `-t` local proxy argument to pass in an access token\. We recommend that you use the `AWSIOT_TUNNEL_ACCESS_TOKEN` environment variable to set the access token for the local proxy\.
+ Run the local proxy executable with least privileges in the operating system or environment\.
  + Avoid running the local proxy as an administrator on Windows\.
  + Avoid running the local proxy as root on Linux and macOS\.
+ Consider running the local proxy on separate hosts, containers, sandboxes, chroot jail, or a virtualized environment\.
+ Build the local proxy with relevant security flags, depending on your toolchain\.
+ On devices with multiple network interfaces, use the `-b` argument to bind the TCP socket to the network interface used to communicate with the destination application\. 

## Example command and output<a name="example-cmd-output-localproxy"></a>

The following shows an example of a command that you run on a Linux OS and the corresponding output\. The example shows a web proxy that's listening on an HTTP port and how the local proxy can be configured in both `source` and `destination` modes\. Before you can run these commands, you must have already opened a tunnel and obtained the client access tokens for the source and destination\. You must have also built the local proxy as described previously\.

The local proxy upgrades the HTTPS protocol to WebSockets to establish a long\-lived connection and then starts transmitting data through the connection to the secure tunneling device endpoints\.

**Note**  
The following commands used in the examples use the `verbosity` flag to illustrate an overview of the different steps described previously after you run the local proxy\. We recommend that you use this flag only for testing purposes\.

**Running local proxy in source mode**  
The following commands show how to run the local proxy in source mode\.

------
#### [ Linux/macOS ]

In Linux or macOS, run the following commands in the terminal to configure and start the local proxy on your source\.

```
export AWSIOT_TUNNEL_ACCESS_TOKEN=${access_token}
./localproxy -s 5555 -v 5 -r us-west-2
```

Where:
+ `-s` is the source listen port, which starts the local proxy in source mode\.
+ `-v` is the verbosity of the output, which can be a value between zero and six\.
+ `-r` is the endpoint region where the tunnel is opened\.

For more information about the parameters, see [Options set using command line arguments](https://github.com/aws-samples/aws-iot-securetunneling-localproxy#options-set-via-command-line-arguments)\.

------
#### [ Windows ]

In Windows, you configure the local proxy similar to how you do for Linux or macOS, but how you define the environment variables is different from the other platforms\. Run the following commands in the `cmd` window to configure and start the local proxy on your source\.

```
set AWSIOT_TUNNEL_ACCESS_TOKEN=${access_token}
.\localproxy -s 5555 -v 5 -r us-west-2
```

Where:
+ `-s` is the source listen port, which starts the local proxy in source mode\.
+ `-v` is the verbosity of the output, which can be a value between zero and six\.
+ `-r` is the endpoint region where the tunnel is opened\.

For more information about the parameters, see [Options set using command line arguments](https://github.com/aws-samples/aws-iot-securetunneling-localproxy#options-set-via-command-line-arguments)\.

------

The following shows a sample output of running the local proxy in `source` mode\.

```
...
...

Starting proxy in source mode
Attempting to establish web socket connection with endpoint wss://data.tunneling.iot.us-west-2.amazonaws.com:443
Resolved proxy  server IP: 10.10.0.11
Connected successfully with proxy server
Performing SSL handshake with proxy server	
Successfully completed SSL handshake with proxy server
HTTP/1.1 101 Switching Protocols

...

Connection: upgrade
channel-id: 01234567890abc23-00001234-0005678a-b1234c5de677a001-2bc3d456
upgrade: websocket

...

Web socket session ID: 01234567890abc23-00001234-0005678a-b1234c5de677a001-2bc3d456
Web socket subprotocol selected: aws.iot.securetunneling-2.0
Successfully established websocket connection with proxy server: wss://data.tunneling.iot.us-west-2.amazonaws.com:443
Setting up web socket pings for every 5000 milliseconds
Scheduled next read:

...

Starting web socket read loop continue reading...
Resolved bind IP: 127.0.0.1
Listening for new connection on port 5555
```

**Running local proxy in destination mode**  
The following commands show how to run the local proxy in destination mode\.

------
#### [ Linux/macOS ]

In Linux or macOS, run the following commands in the terminal to configure and start the local proxy on your destination\.

```
export AWSIOT_TUNNEL_ACCESS_TOKEN=${access_token}
./localproxy -d 22 -v 5 -r us-west-2
```

Where:
+ `-d` is the destination application which starts the local proxy in destination mode\.
+ `-v` is the verbosity of the output, which can be a value between zero and six\.
+ `-r` is the endpoint region where the tunnel is opened\.

For more information about the parameters, see [Options set using command line arguments](https://github.com/aws-samples/aws-iot-securetunneling-localproxy#options-set-via-command-line-arguments)\.

------
#### [ Windows ]

In Windows, you configure the local proxy similar to how you do for Linux or macOS, but how you define the environment variables is different from the other platforms\. Run the following commands in the `cmd` window to configure and start the local proxy on your destination\.

```
set AWSIOT_TUNNEL_ACCESS_TOKEN=${access_token}
.\localproxy -d 22 -v 5 -r us-west-2
```

Where:
+ `-d` is the destination application which starts the local proxy in destination mode\.
+ `-v` is the verbosity of the output, which can be a value between zero and six\.
+ `-r` is the endpoint region where the tunnel is opened\.

For more information about the parameters, see [Options set using command line arguments](https://github.com/aws-samples/aws-iot-securetunneling-localproxy#options-set-via-command-line-arguments)\.

------

The following shows a sample output of running the local proxy in `destination` mode\.

```
...
...

Starting proxy in destination mode
Attempting to establish web socket connection with endpoint wss://data.tunneling.iot.us-west-2.amazonaws.com:443
Resolved proxy  server IP: 10.10.0.11
Connected successfully with proxy server
Performing SSL handshake with proxy server	
Successfully completed SSL handshake with proxy server
HTTP/1.1 101 Switching Protocols

...

Connection: upgrade
channel-id: 01234567890abc23-00001234-0005678a-b1234c5de677a001-2bc3d456
upgrade: websocket

...

Web socket session ID: 01234567890abc23-00001234-0005678a-b1234c5de677a001-2bc3d456
Web socket subprotocol selected: aws.iot.securetunneling-2.0
Successfully established websocket connection with proxy server: wss://data.tunneling.iot.us-west-2.amazonaws.com:443
Setting up web socket pings for every 5000 milliseconds
Scheduled next read:

...

Starting web socket read loop continue reading...
```