# Configure beaconing and filtering capabilities of your LoRaWAN gateways<a name="connect-iot-lorawan-gateway-configure"></a>

When working with LoRaWAN devices, you can configure certain optional parameters for your LoRaWAN gateways\. The parameters include:
+ 

**Beaconing**  
You can configure beaconing parameters for your LoRaWAN gateways that are acting as a bridge for your class B LoRaWAN devices\. These devices receive a downlink message at scheduled time slots, so you must configure the beaconing parameters for your gateways to transmit these time\-synchronized beacons\.
+ 

**Filtering**  
You can configure the `NetID` and `JoinEUI` parameters for your LoRaWAN gateways to filter the device data traffic\. Filtering the traffic helps conserve bandwidth usage and reduces the traffic flow between the gateways and LNS\.
+ 

**Sub\-bands**  
You can configure the sub\-bands for your gateway to specify the particular sub\-band that you want to use\. For wireless devices that can't hop between the various sub\-bands, you can use this capability to communicate with the devices using only the frequency channels in that particular sub\-band\.

The following topics contain more information about these parameters and how to configure them\. The beaconing parameters aren't available in the AWS Management Console and can only be specified using the AWS IoT Wireless API or the AWS CLI\.

**Topics**
+ [Configuring your gateways to send beacons to class B devices](connect-iot-lorawan-gateway-beaconing.md)
+ [Configuring your gateway's subbands and filtering capabilities](connect-iot-lorawan-subband-filter-configuration.md)