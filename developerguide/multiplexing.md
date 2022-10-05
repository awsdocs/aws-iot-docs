# Multiplex data streams and using simultaneous TCP connections in a secure tunnel<a name="multiplexing"></a>

You can use multiple data streams per tunnel by using the secure tunneling multiplexing feature\. With multiplexing, you can troubleshoot devices using multiple data streams\. You can also reduce your operational load by eliminating the need to build, deploy, and start multiple local proxies or open multiple tunnels to the same device\. For example, multiplexing can be used in case of a web browser that requires sending multiple HTTP and SSH data streams\.

For each data stream, AWS IoT secure tunneling supports simultaneous TCP connections\. Using simultaneous connections reduces the potential for a time\-out in case of multiple requests from the client\. For example, it can reduce the loading time when remotely accessing a web server that's local to the destination device\.

The following sections explain more about multiplexing and using simultaneous TCP connections, and their different use cases\.

**Topics**
+ [Multiplexing multiple data streams in a secure tunnel](multiplexing-multiple-streams.md)
+ [Using simultaneous TCP connections in a secure tunnel](multiplexing-simultaenous-tcp.md)