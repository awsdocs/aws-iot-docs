# How secure tunneling works<a name="how-secure-tunneling-works"></a>

The following shows how secure tunneling establishes a connection between your source and destination device\. For information about the different terms such as client access token \(CAT\), see [Secure tunneling concepts](secure-tunneling-concepts.md)\.

1. 

**Open a tunnel**  
To open a tunnel for initiating a session with your remote destination device, you can use the AWS Management Console, the [AWS CLI open\-tunnel](https://docs.aws.amazon.com/cli/latest/reference/iotsecuretunneling/open-tunnel.html) command, or the [OpenTunnel API](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-secure-tunneling_OpenTunnel)\.

1. 

**Download the client access token pair**  
After you've opened a tunnel, you can download the client access token \(CAT\) for your source and destination and save it on your source device\. You must retrieve the CAT and save it now before starting the local proxy\.

1. 

**Start local proxy in destination mode**  
The IoT agent that has been installed and is running on your destination device will be subscribed to the reserved MQTT topic `$aws/things/thing-name/tunnels/notify` and will receive the CAT\. Here, *thing\-name* is the name of the AWS IoT thing you create for your destination\. For more information, see [Secure tunneling topics](reserved-topics.md#reserved-topics-secure)\.

   The IoT agent then uses the CAT to start the local proxy in destination mode and set up a connection on the destination side of the tunnel\. For more information, see [IoT agent snippet](agent-snippet.md)\.

1. 

**Start local proxy in source mode**  
After the tunnel has been opened, AWS IoT Device Management provides the CAT for the source that you can download on the source device\. You can use the CAT to start the local proxy in source mode, which then connects the source side of the tunnel\. For more information about local proxy, see [Local proxy](local-proxy.md)\.

1. 

**Open an SSH session**  
As both sides of the tunnel are connected, you can start an SSH session by using the local proxy on the source side\.

For more information about how to use the AWS Management Console to open a tunnel and start an SSH session, see [Open a tunnel and start SSH session to remote device](secure-tunneling-tutorial-open-tunnel.md)\.

 The following video describes how secure tunneling works and walks you through the process of setting up an SSH session to a Raspberry Pi device\.