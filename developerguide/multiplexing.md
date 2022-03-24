# Multiplex data streams in a secure tunnel<a name="multiplexing"></a>

You can use multiple data streams per tunnel by using the secure tunneling multiplexing feature\. With multiplexing, you can troubleshoot devices using multiple connections or ports \(for example, a web browser that requires sending multiple HTTP and SSH data streams\)\. You can also reduce your operational load by eliminating the need to build, deploy, and start multiple local proxies or open multiple tunnels to the same device\.

## Example use case<a name="multiplexing-multiple-streams"></a>

You can use the multiplexing feature in the event that a device in the field requires more than one connection to the device in order to properly troubleshoot it\. For example, you might need to connect to an on\-device web application to change some networking parameters, while simultaneously issuing shell commands through the terminal to verify that the device is working properly with the new networking parameters\. In this scenario, you may need to connect to the device through both HTTP and SSH and transfer two parallel data streams in order to concurrently access the web application and terminal\. With the multiplexing feature, these two independent streams can be transferred over the same tunnel at the same time\.

## How to set up a multiplexed tunnel<a name="multiplexing-tutorial"></a>

The following procedure will walk you through how to set up a multiplexed tunnel for troubleshooting devices using applications that require connections to multiple ports\. You will set up one tunnel with two multiplexed streams: one HTTP stream and one SSH stream\.

1. First, configure the destination device with configuration files\. Configuration files can be provided on the device if the port mappings are unlikely to change\. On the destination device, create a configuration directory called `config` in the same folder where the local proxy is running\. Then, create a file called `SSHSource.ini` in this directory\. The content of this file is:

   ```
   HTTP1 = 5555
   SSH1 = 3333
   ```
**Note**  
You can skip this step if you prefer to specify the port mapping through the CLI or don't need to start local proxy on designated listening ports\.

1. Next, configure the source device with configuration files\. In the same folder as where the local proxy is running, create a configuration directory called `config` and give local proxy the read permission to this directory\. Then, create a file called `SSHDestination.ini` in this directory\. The content of this file is:

   ```
   HTTP1 = 80
   SSH1 = 22
   ```
**Note**  
You can skip this step if you prefer to specify the port mapping through the CLI\. If so, you will need to update the [tunnel agent](agent-snippet.md) to use the new parameters\.

1. Open a tunnel with the service identifier `HTTP1` and `SSH1`\. `thingName` is optional if your device isn't registered with AWS IoT\.

   ```
   aws iotsecuretunneling open-tunnel \
   --destination-config thingName=foo,services=HTTP1,SSH1
   ```

   A destination and source client access token will be given after this call\. Note the `destination_client_access_token` and `source_client_access_token` for next steps\. The output should look similar to the following\.

   ```
   {
   	"tunnelId": "b2de92a3-b8ff-46c0-b0f2-afa28b00cecd",
   	"tunnelArn": "arn:aws:iot:us-west-2:431600097591:tunnel/b2de92a3-b8ff-46c0-b0f2-afa28b00cecd",
   	"sourceAccessToken": source_client_access_token,
   	"destinationAccessToken": destination_client_access_token
   }
   ```

1. Next, start the destination local proxy\. You will be connected to the secure tunneling service upon token delivery\. A local proxy running on destination devices starts in destination mode\. You have two options to achieve this:

   1. Start destination local proxy with configuration files from Step 1\.

      ```
      ./localproxy -r us-east-1 -m dst -t destination_client_access_token
      ```

   1. Start destination local proxy with the mapping specified through the CLI\.

      ```
      ./localproxy -r us-east-1 -d HTTP1=80,SSH1=22 -t destination_client_access_token
      ```

1. Now, start the source local proxy\. A local proxy running on source devices starts in source mode\. You have three options to achieve this:

   1. Start source local proxy with configuration files from Step 2\.

      ```
      ./localproxy -r us-east-1 -m src -t source_client_access_token
      ```

   1. Start source local proxy with the mapping specified through the CLI\.

      ```
      ./localproxy -r us-east-1 -s HTTP1=5555,SSH1=3333 -t source_client_access_token
      ```

   1. Start source local proxy with no configuration files and no mapping specified from the CLI\. Local proxy will pick up available ports to use and manage the mappings for you\.

      ```
      ./localproxy -r us-east-1 -m src -t source_client_access_token
      ```

1. Application data from SSH and HTTP connection can now be transferred concurrently over the multiplexed tunnel\. As can be seen from the map below, the service identifier acts as a readable format to translate the port mapping between the source and destination device\. With this configuration, the secure tunneling will:

   1. Forward any incoming HTTP traffic from port 5555 on the source device to port 80 on the destination device\.

   1. Forward any incoming SSH traffic from port 3333 on the source device to port 22 on the destination device\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/multiplexing-post-mapping-translation.png)