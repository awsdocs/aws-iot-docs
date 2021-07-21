# Configure your gateway's subbands and filtering capabilities<a name="connect-iot-lorawan-subband-filter-configuration"></a>

LoRaWAN gateways run a [LoRa Basics Station](https://doc.sm.tc/station) software that enables the gateways to connect to AWS IoT Core for LoRaWAN\. To connect to AWS IoT Core for LoRaWAN, your LoRa gateway first queries the CUPS server for the LNS endpoint, and then establishes a WebSockets data connection with that endpoint\. After the connection is established, uplink and downlink frames can be exchanged through that connection\.

## Filtering of LoRa data frames received by gateway<a name="connect-iot-lorawan-frequency-channels-subbands"></a>

After your LoRaWAN gateway establishes a connection to the endpoint, AWS IoT Core for LoRaWAN responds with a `router_config` message that specifies a set of parameters for the LoRa gateway's configuration, including filtering parameters `NetID` and `JoinEui`\. For more information about `router_config` and how a connection is established with the LoRaWAN Network Server \(LNS\), see [LNS protocol](https://doc.sm.tc/station/tcproto.html)\.

```
{
"msgtype"    : "router_config"
"NetID"      : [ INT, .. ]
"JoinEui"    : [ [INT,INT], .. ] // ranges: beg,end inclusive
"region"     : STRING             // e.g. "EU863", "US902", ..
"hwspec"     : STRING
"freq_range" : [ INT, INT ]       // min, max (hz)
"DRs"        : [ [INT,INT,INT], .. ]   // sf,bw,dnonly
"sx1301_conf": [ SX1301CONF, .. ]
"nocca"      : BOOL
"nodc"       : BOOL
"nodwell"    : BOOL
}
```

The gateways carry LoRaWAN device data to and from LNS usually over high\-bandwidth networks like Wi\-Fi, Ethernet, or Cellular\. The gateways usually pick up all messages and pass through the traffic that comes to it to AWS IoT Core for LoRaWAN\. However, you can configure the gateways to filter some of the device data traffic, which helps conserve bandwidth usage and reduces the traffic flow between the gateway and LNS\.

To configure your LoRa gateway to filter the data frames, you can use the parameters `NetID` and `JoinEui` in the `router_config` message\. `NetID` is a list of NetID values that are accepted\. Any LoRa data frame carrying a data frame other than those listed will be dropped\. `JoinEui` is a list of pairs of integer values encoding ranges of JoinEUI values\. Join request frames will be dropped by the gateway unless the field `JoinEui` in the message is within the range \[BegEui,EndEui\]\.

## Frequency channels and subbands<a name="connect-iot-lorawan-frequency-channels-subbands"></a>

For US915 and AU915 RF regions, wireless devices have choices of 64 125KHz and 8 500KHz uplink channels to access the LoRaWAN networks using the LoRa gateways\. The uplink frequency channels are divided into 8 subbands, each with 8 125KHz channels and one 500KHz channel\. For each regular gateway in AU915 region, one or more subbands will be supported\.

Some wireless devices can't hop between subbands and use the frequency channels in only one subband when connected to AWS IoT Core for LoRaWAN\. For the uplink packets from those devices to be transmitted, configure the LoRa gateways to use that particular subband\. For gateways in other RF regions, such as EU868, this configuration is not required\.

## Configure your gateway to use filtering and subbands using the console<a name="connect-iot-lorawan-configure-gateway-channels-console"></a>

You can configure your gateway to use a particular subband and also enable the capability to filter the LoRa data frames\. To specify these parameters using the console:

1. Navigate to the [AWS IoT Core for LoRaWAN](https://console.aws.amazon.com/iot/home#/wireless/gateways) **Gateways** page of the AWS IoT console and choose **Add gateway**\.

1. Specify the gateway details such as the **Gateway's Eui**, **Frequency band \(RFRegion\)** and an optional **Name** and **Description**, and choose whether to associate an AWS IoT thing to your gateway\. For more information about how to add a gateway, see [Add a gateway using the console](connect-iot-lorawan-onboard-gateway-add.md#connect-iot-lorawan-onboard-gateway-console)\.

1. In the **LoRaWAN configuration** section, you can specify the subbands and filtering information\.
   + `SubBands`: To add a subband, choose **Add SubBand** and specify a list of integer values that indicate which subbands are supported by the gateway\. The `SubBands` parameter can only be configured in the `RfRegion` US915 and AU915 and must have values in the range `[1,8]` within one of these supported regions\.
   + `NetIdFilters`: To filter uplink frames, choose **Add NetId** and specify a list of string values that the gateway uses\. The NetID of the incoming uplink frame from the wireless device must match at least one of the listed values, otherwise the frame is dropped\.
   + `JoinEuiFilters`: Choose **Add JoinEui range** and specify a list of pairs of string values that a gateway uses to filter LoRa frames\. The JoinEUI value specified as part of the join request from the wireless device must be within the range of at least one of the JoinEuiRange values, each listed as a pair of \[BegEui, EndEui\], otherwise the frame is dropped\.

1. You can then continue to configure your gateway by following the instructions described in [Add a gateway using the console](connect-iot-lorawan-onboard-gateway-add.md#connect-iot-lorawan-onboard-gateway-console)\.

After you've added a gateway, in the [AWS IoT Core for LoRaWAN](https://console.aws.amazon.com/iot/home#/wireless/gateways) **Gateways** page of the AWS IoT console, if you select the gateway that you've added, you can see the `SubBands` and filters `NetIdFilters` and `JoinEuiFilters` in the **LoRaWAN specific details** section of the Gateway details page\.

## Configure your gateway to use filtering and subbands using the API<a name="connect-iot-lorawan-configure-gateway-channels-api"></a>

<<<<<<< HEAD
You can use the [CreateWirelessGateway](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_CreateWirelessGateway.html) API that you use to create a gateway to configure the subbands you want to use and enable the filtering capability\. Using the `CreateWirelessGateway` API, you can specify the subbands and filters as part of the gateway configuration information that you provide using the `LoRaWAN` field\. Following shows the request token that includes this information:
=======
You can use the [CreateWirelessGateway](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_CreateWirelessGateway.html) API that you use to create a gateway to configure the subbands you want to use and enable the filtering capability\. Using the `CreateWirelessGateway` API, you can specify the subbands and filters as part of the gateway configuration information that you provide using the `LoRaWAN` field\. The following shows the request token that includes this information\.
>>>>>>> gausekha-repo-refresh

```
POST /wireless-gateways HTTP/1.1
Content-type: application/json

{
"Arn": "arn:aws:iotwireless:us-east-1:400232685877aa:WirelessGateway/
       a11e3d21-e44c-471c-afca-6716c228336a",
"Description": "Using my first LoRaWAN gateway",
   "LoRaWAN": { 
      "GatewayEui": "a1b2c3d4567890ab",
      "JoinEuiFilters": [
        ["0000000000000001", "00000000000000ff"],
        ["000000000000ff00", "000000000000ffff"]
      ],
      "NetIdFilters": ["000000", "000001"],
      "RfRegion": "US915",
      "SubBands": [2]
   },
   "Name": "myFirstLoRaWANGateway"  
   "ThingArn": null,
   "ThingName": null
}
```

<<<<<<< HEAD
You can also use the [UpdateWirelessGateway](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateWirelessGateway.html) API to update the filters but not the subbands\. If the `JoinEuiFilters` and `NetIdfilters` values are null, it means there is no update for the fields\. If the values are not null and empty lists are included, then the update will be applied\. To get the values of the fields that you specified, use the [GetWirelessGateway](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetWirelessGateway.html) API\.
=======
You can also use the [UpdateWirelessGateway](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateWirelessGateway.html) API to update the filters but not the subbands\. If the `JoinEuiFilters` and `NetIdfilters` values are null, it means there is no update for the fields\. If the values aren't null and empty lists are included, then the update is applied\. To get the values of the fields that you specified, use the [GetWirelessGateway](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetWirelessGateway.html) API\.
>>>>>>> gausekha-repo-refresh
