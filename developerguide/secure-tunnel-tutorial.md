# Secure tunneling tutorial<a name="secure-tunnel-tutorial"></a>

In this tutorial, you open a tunnel and use it to start an SSH session to a remote device\. The remote device is behind firewalls that block all inbound traffic, making direct SSH into the device impossible\. Before you begin, make sure that you understand how to [register a device in the AWS IoT registry](register-device.md) and [connect a device to the AWS IoT device gateway](sdk-tutorials.md)\.

## Prerequisites<a name="tunnel-prereqs"></a>
+ The firewalls the remote device is behind must allow outbound traffic on port 443\.
+ You have created an IoT thing named `RemoteDeviceA` in the AWS IoT registry\.
+ You have an IoT device agent running on the remote device that connects to the AWS IoT device gateway and is configured with an MQTT topic subscription\. This tutorial includes a snippet that shows you how to implement an agent\. For more information, see [IoT agent snippet](agent-snippet.md)\.
+ You must have an SSH daemon running on the remote device\.
+ You have downloaded the local proxy source code from [GitHub](https://github.com/aws-samples/aws-iot-securetunneling-localproxy) and built it for the platform of your choice\. We'll refer to the built local proxy executable file as `localproxy` in this tutorial\.

## Open a tunnel<a name="open-tunnel"></a>

****

1. Open the [AWS IoT console](https://console.aws.amazon.com/iot/)\. In the navigation pane, choose **Manage**, and then choose **Tunnels**\.

1. Choose **Open a tunnel**\.

1. On the **Open a new tunnel** page, enter an optional description for the tunnel\.

1. Under **Choose a thing to open a tunnel for**, choose **RemoteDeviceA**\. 

1. Under **Service**, enter **SSH**, and then choose **Open New**\. For more information, see [IoT agent snippet](agent-snippet.md)\.

1. In the **New tunnel opened** page, download your source and destination access tokens and save them to your computer\. This is the only time you can retrieve the tokens\.

1. Choose **Done**\.

If you configure the destination when calling `OpenTunnel`, the Secure Tunneling service delivers the destination client access token to the remote device over MQTT and the reserved MQTT topic \(`$aws/things/RemoteDeviceA/tunnels/notify`\)\. For more information, see [Reserved topics](reserved-topics.md)\. Upon receipt of the MQTT message, the IoT agent on the remote device starts the local proxy in destination mode\. You can omit the destination configuration if you want to deliver the destination client access token to the remote device through another method\. For more information, see [Configuring a remote device](configure-remote-device.md)\.

## Start the local proxy<a name="start-local-proxy"></a>

Open a terminal on your laptop, copy the source client access token, and use it to start the local proxy in source mode\. In the following command, the local proxy is configured to listen for new connections on port 5555\.

\./localproxy \-r us\-east\-1 \-s 5555 \-t *<source\-client\-access\-token>*

**Note**  
The AWS Region in this command must be the same AWS Region where the tunnel was created\.

\-r  
Specifies the AWS Region where your tunnel is created\.

\-s  
Specifies the port to which the proxy should connect\.

\-t  
Specifies the client token text\.

**Note**  
If you receive the following error, set up the CA path\. For information, see [GitHub](https://github.com/aws-samples/aws-iot-securetunneling-localproxy)\.  
`Could not perform SSL handshake with proxy server: certificate verify failed`

## Start an SSH session<a name="start-ssh-session"></a>

Open another terminal and use the following command to start a new SSH session by connecting to the local proxy on port 5555\.

ssh *<username>*@localhost \-p 5555

You might be prompted for a password for the SSH session\. When you are done with the SSH session, type **exit** to close the session\.

## Close the tunnel<a name="close-tunnel"></a>

1. Open the [AWS IoT console](https://console.aws.amazon.com/iot/)\.

1.  Choose the tunnel and from **Actions**, choose **Close**\. Closing the tunnel causes both local proxy instances to close\.