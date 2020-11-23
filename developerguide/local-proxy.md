# Local proxy<a name="local-proxy"></a>

The local proxy is a process that acts as the recipient or sender of incoming TCP connections\. It transmits data sent by the device application through the Secure Tunneling service over a WebSocket secure connection\. You can download the local proxy source from [GitHub](https://github.com/aws-samples/aws-iot-securetunneling-localproxy)\. The local proxy can run in two modes: `source` or `destination`\. In source mode, the local proxy runs on the same device or network as the client application that initiates the TCP connection\. In destination mode, the local proxy runs on the remote device, along with the destination application\. Currently, a single tunnel can support only one TCP connection at a time\.

The local proxy first establishes a connection to the Secure Tunneling service\. When you start the local proxy, use the `-r` argument to specify the AWS Region in which the tunnel is opened\. Use the `-t` argument to pass either the source or destination client access token returned from the `OpenTunnel`\. Two local proxies using the same client access token value cannot be connected at the same time\.

After the WebSocket connection is established, the local proxy performs either source mode or destination mode behaviors, depending on its configuration\.

By default, the local proxy attempts to reconnect to the Secure Tunneling service if any I/O errors occur or if the WebSocket connection is closed unexpectedly\. This causes the TCP connection to close\. If any TCP socket errors occur, the local proxy sends a message through the tunnel to notify the other side to close its TCP connection\. By default, the local proxy always uses SSL communication\.

After you use the tunnel, it is safe to stop the local proxy process\. We recommend that you explicitly close the tunnel by calling `CloseTunnel`\. Active tunnel clients might not be closed immediately after calling `CloseTunnel`\.

## Local proxy security best practices<a name="local-proxy-security"></a>

When running the local proxy, follow these security best practices:
+ Avoid the use of the `-t` local proxy argument to pass in an access token\. We recommend that you use the `AWSIOT_TUNNEL_ACCESS_TOKEN` environment variable to set the access token for the local proxy\.
+ Run the local proxy executable with least privileges in the operating system or environment\.
  + Avoid running the local proxy as an administrator on Windows\.
  + Avoid running the local proxy as root on Linux and macOS\.
+ Consider running the local proxy on separate hosts, containers, sandboxes, chroot jail, or a virtualized environment\.
+ Build the local proxy with relevant security flags, depending on your toolchain\.
+ On devices with multiple network interfaces, use the `-b` argument to bind the TCP socket to the network interface used to communicate with the destination application\. 