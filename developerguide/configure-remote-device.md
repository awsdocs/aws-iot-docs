# Configuring a Remote Device<a name="configure-remote-device"></a>

If you want to deliver the destination client access token to the remote device through methods other than subscribing to the reserved IoT MQTT topic, you might need two components on the remote device:
+ A destination client access token listener\.
+ A local proxy\.

The destination client access token listener should work with the client access token delivery mechanism of your choice\. It must be able to start a local proxy in destination mode\.