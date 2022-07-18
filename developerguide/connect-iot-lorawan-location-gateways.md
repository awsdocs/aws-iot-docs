# Configuring the position of LoRaWAN gateways<a name="connect-iot-lorawan-location-gateways"></a>

When you add your gateway to AWS IoT Core for LoRaWAN, you can specify the static position data and add a destination\. The destination describes the IoT rule that processes the gateway's position information and routes the updated position to Amazon Location Service\. After you configure the gateway position, the position data is displayed on an Amazon Location map with the accuracy information and the destination that you specified\. 

**Note**  
In this preview release, you can use the GNSS solver for computing the position information\. This solver can only be used with LoRaWAN devices that have the LoRa Edge chip\. It can't be used with LoRaWAN gateways\. For gateways, you can still specify the static position coordinates and add a destination\. When solvers aren't used to compute the position, such as in the case of gateways, the accuracy information will be reported as `0.0`\.

You can configure the gateway position using the AWS Management Console, the AWS IoT Wireless API, or the AWS CLI\. 

## Configuring position of your gateway using the console<a name="connect-iot-lorawan-location-gateways-console"></a>

To configure the position of your gateway resources by using the AWS Management Console, first sign in to the console and then go to the [https://console.aws.amazon.com/iot/home#/wireless/gateways](https://console.aws.amazon.com/iot/home#/wireless/gateways) hub page of the AWS IoT console\.

**Add a position configuration**  
To add a position configuration for your gateway

1. In the **Gateways** hub page, choose **Add gateway**\.

1. Enter the gateway's EUI, frequency band \(RFRegion\), and any additional gateway details and LoRaWAN configuration information\. For more information, see [Add a gateway using the console](connect-iot-lorawan-onboard-gateway-add.md#connect-iot-lorawan-onboard-gateway-console)\.

1. Enter the position information and a position data destination for your gateway\.

   1. 

**Position information**  
Specify the position data for your gateway using the latitude and longitude coordinates, and an optional altitude coordinate\. The position information is based on the WGS84 coordinate system\.

   1. 

**Position data destination**  
Choose a destination to describe the AWS IoT rule that processes the gateway's position data and forwards the updated position to AWS IoT Core for LoRaWAN\. Use this destination only to route position data\. It must be different from the destination that you use for routing device data to other AWS services

**View gateway's position configuration**  
After you've configured your gateway's position, AWS IoT Core for LoRaWAN creates an Amazon Location map called `iotwireless.map`\. You can see this map in the details page of your gateway on the **Position** tab\. Based on the position coordinates that you specified, the position of your gateway will be displayed as a marker on the map\. You can zoom in or zoom out to clearly view the position of your gateway on the map\.

**Note**  
If you don't have Amazon Location Service maps installed, you'll see a message indicating that you must use Amazon Location Service to access the map and view the gateway position\. Using Amazon Location Service maps may incur additional charges to your AWS account\. For more information, see [AWS IoT Core pricing](http://aws.amazon.com/iot-core/pricing/)\.

The map, `iotwireless.map`, acts as a source of map data which is accessed using `Get` API operations, such as [https://docs.aws.amazon.com/location-maps/latest/APIReference/API_GetMapTile.html](https://docs.aws.amazon.com/location-maps/latest/APIReference/API_GetMapTile.html)\. For information about `Get` APIs used with maps, see [Amazon Location Service API reference](https://docs.aws.amazon.com/location-maps/latest/APIReference/Welcome.html)\.

To get additional details about this map, go to the Amazon Location Service console, choose **maps**, and then choose [iotwireless\.map](https://console.aws.amazon.com/location/maps/home#/describe/iotwireless.map)\. For more information, see [Maps](https://docs.aws.amazon.com/location/latest/developerguide/map-concepts.html) in the *Amazon Location Service developer guide*\.

In the gateway's details page, on the **Position** tab, you'll also see the accuracy information, the timestamp at which your gateway's position was determined, and the position data destination that you specified\.

**Update gateway's position configuration**  
To change the gateway's position configuration, in the gateway details page, choose **Edit** and then update the position information and the destination\.

**Note**  
In this preview release, historical position information isn't available\. When you update the gateway's position coordinates, it overwrites the previously reported position data\. After you've updated the position, in the **Position** tab of the gateway details, you'll see the new position information\. The change in timestamp indicates that it corresponds to the last known position of the gateway\.

## Configure position of your gateway using the API<a name="connect-iot-lorawan-location-gateways-api"></a>

You can specify the position information and configure the gateway position using the AWS IoT Wireless API or the AWS CLI\.

### Add position information and configuration<a name="connect-iot-lorawan-location-gateways-api-add"></a>

Use the following AWS IoT Wireless API operations or AWS CLI commands to update the position information and the position configuration for a given wireless gateway\.
+ 

**Add gateway position**  
To add the static position information for a given wireless gateway, specify the coordinates using the [ UpdatePosition](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdatePosition.html) API operation or the [update\-position](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/update-position.html) CLI command\. Specify `WirelessGateway` as the `ResourceType`, the ID of the wireless gateway to be updated as the `ResourceIdentifier`, and the position coordinates as an array with the latitude, longitude, and altitude information\.

  ```
  aws iotwireless update-position \ 
      --resource-type WirelessGateway \ 
      --resource-id "12345678-a1b2-3c45-67d8-e90fa1b2c34d" \ 
      --position [37.33, 108.10, 20.0]
  ```

  Running this command doesn't produce any output\. To see the position information that you specified, use the `GetPosition` API\.
+ 

**Specify position destination**  
To configure the position of a given wireless gateway, use the [PutPositionConfiguration](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_PutPositionConfiguration.html) API operation or the [put\-position\-configuration](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/put-position-configuration.html) CLI command\. Specify `WirelessGateway` as the `ResourceType`, the ID of the wireless gateway to be updated as the `ResourceIdentifier`, and the configuration information\. While you can't use geolocation solvers, you can still specify a destination for routing the position data\.

  ```
  aws iotwireless put-position-configuration \ 
      --resource-type WirelessGateway \ 
      --resource-id "12345678-a1b2-3c45-67d8-e90fa1b2c34d" \ 
      --destination "Gateway_position_destination"
  ```

  Running this command doesn't produce any output\. To see the position information that you specified, use the `GetPositionConfiguration` or `ListPositionConfigurations` API operations\.

### Get position information and configuration<a name="connect-iot-lorawan-location-devices-api-get"></a>

Use the following AWS IoT Wireless API operations or AWS CLI commands to get the position information and the position configuration for a given wireless device\.
+ 

**Get position information**  
To get the position information for a given wireless gateway, use the [ GetPosition](https://docs.aws.amazon.com/https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetPosition.html) API or the [get\-position](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/get-position.html) CLI command\. Specify `WirelessGateway` as the `resourceType` and provide the ID of the wireless gateway as the `resourceIdentifier`\.

  ```
  aws iotwireless get-position \ 
      --resource-type WirelessGateway \ 
      --resource-id "12345678-a1b2-3c45-67d8-e90fa1b2c34d"
  ```

  Running this command displays the position information of your wireless device\. You'll see information about the position coordinates, the timestamp which corresponds to the last known position of the device, and the accuracy information\.

  ```
  {
      "Accuracy": {
          "HorizontalAccuracy": 0.0,
          "VerticalAccuracy": 0.0
      },
      "Position": [
          33.33000183105469,
          -33.33000183105469,
          10.0
      ],
      "SolverProvider": null,
      "SolverType": null,
      "SolverVersion": null,
      "Timestamp": "Fri Jul 01 19:43:37 UTC 2022"
  }
  ```
+ 

**Get position configuration of a gateway**  
To get the position configuration for a given wireless gateway, use the [ GetPositionConfiguration](https://docs.aws.amazon.com/https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetPositionConfiguration.html) API or the [get\-position\-configuration](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/get-position-configuration.html) CLI command\. Specify `WirelessGateway` as the `resourceType` and provide the ID of the wireless gateway as the `resourceIdentifier`\.

  ```
  aws iotwireless get-position-configuration \ 
      --resource-type WirelessGateway \ 
      --resource-id "12345678-a1b2-3c45-67d8-e90fa1b2c34d"
      --destination "Gateway_position_destination"
  ```

  Running this command displays the position configuration of your wireless gateway\. You'll see information about the destination that you specified for routing the position data\.

  ```
  {
      "Destination": "Gateway_position_destination",
  }
  ```

### List position configurations<a name="connect-iot-lorawan-location-devices-api-list"></a>

You can configure the position information and destination for multiple resources in your account\. The resources can be wireless devices or gateways or both\. After you create these configurations, you can use the [ListPositionConfigurations](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/                         API_ListPositionConfigurations.html) API operation or the [list\-position\-configurations](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/list-position-configurations.html) CLI command to get a list of these configurations\. To filter the list to display position configurations for only wireless gateways, set `resourceType` to `WirelessGateway`\.

```
aws iotwireless list-position-configurations \ 
    --resource-type WirelessGateway
```

Running this command displays position configurations for all LoRaWAN gateways in your AWS account\. You can also use the `max-results` parameter to specify how many configurations you want to display\. The following code shows the output of running this command\.

```
{    
    "PositionConfigurationList": [
        {
            "Destination": "Gateway_position_destination1",
            "ResourceIdentifier": "12345678-a1b2-3c45-67d8-e90fa1b2c34d",
            "ResourceType": "WirelessGateway",
            "Solvers": {
                "SemtechGnss": {
                    "Fec": "ROSE",
                    "Provider": "Semtech",
                    "Status": "Enabled",
                    "Type": "GNSS"
                }
            }
        },
        {
            "Destination": "Gateway_position_destination2",
            "ResourceIdentifier": "12345678-a1b2-c3d4-e5f6-e90fa1b2c34d",
            "ResourceType": "WirelessGateway",
            "Solvers": {
                "SemtechGnss": {
                    "Fec": "ROSE",
                    "Provider": "Semtech",
                    "Status": "Enabled",
                    "Type": "GNSS"
                }
            }
        }
    ]
}
```