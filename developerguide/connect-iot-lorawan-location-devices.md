# Configuring position of LoRaWAN devices<a name="connect-iot-lorawan-location-devices"></a>

When you add your device to AWS IoT Core for LoRaWAN, you can specify the static position information, activate optional positioning solvers, and add a destination\. The destination describes the IoT rule that processes the device's position information and routes the updated position to Amazon Location Service\. After you configure your device position, the position data is displayed on an Amazon Location map with the accuracy information, your solver configuration, and the destination that you specified\.

You can configure the position of your device using the AWS Management Console, the AWS IoT Wireless API, or the AWS CLI\.

## Frequency ports and format of uplink messages<a name="connect-iot-lorawan-location-devices-uplink"></a>

If you activate the positioning solver, you must specify the frequency ports for communicating the position computed by the solver to AWS IoT Core for LoRaWAN\. The LoRaWAN specification provides a data delivery field \(FRMPayload\) and a Port field \(FPort\) to distinguish between different types of messages\.

To communicate the position information, you can specify a value anywhere between 1 and 223 for the frequency ports\. FPort 0 is reserved for MAC messages, FPort 224 is reserved for MAC compliance testing, and ports 225\-255 are reserved for future standardized application extensions\.

**Note**  
The LoRa Edge chipset uses a streaming design and doesn't differentiate between WiFi and GNSS payloads\. Both payloads are delivered using a shared FPort\. For more information and to learn about clock synchronization, see [ Reliable data streams](https://www.loracloud.com/documentation/modem_services?url=overview.html#reliable-data-stream) in the *Semtech documentation*\.

If you've activated a positioning solver, when your LoRaWAN device sends an uplink message, the solver first computes the position information\. This position information is communicated to AWS IoT Core for LoRaWAN using the frequency ports\. When you add a destination, it creates an AWS IoT rule to route the data to Amazon Location Service using the rules engine\. The updated position information is then displayed on an Amazon Location map\. If you haven't activated any solvers, the destination routes the position data when you update the static position coordinates of your device\.

The following code shows the format of the uplink message sent from AWS IoT Core for LoRaWAN with the position information, accuracy, solver configuration, and the wireless metadata\. The fields highlighted below are optional\. If no solvers are activated, or when there's no vertical accuracy information, the value is `null`\.

**Note**  
In this preview release, only the horizontal accuracy information is obtained from the Amazon Location map\. The vertical accuracy will be displayed as `null`\.

```
{
    // Position configuration parameters for given wireless device
    "WirelessDeviceId": "5b58245e-146c-4c30-9703-0ca942e3ff35", 
    "PayloadData": "Cc48AAAAAAAAAAA=",  

    // Position information for a device. Altitude is optional.
    // If no vertical accuracy information is available or no
    // solvers are specified, the value is set to null.
    "PositionInfo": {
        "Position": [-22.219999313354492, 33.33000183105469, 99.0],
        "Accuracy": { 
          "Horizontal": number
          "Vertical": number
          },
        "SolverProvider": "Semtech",
        "SolverType": "GNSS",
        "SolverVersion": "1.0.2",
        "Timestamp": "2022-06-02T06:13:37.208Z"

    },

    //Parameters controlled by IoT Core for LoRaWAN
    "WirelessMetadata": 
    {
        "LoRaWAN":
        {
            "ADR": false,
            "Bandwidth": 125,
            "ClassB": false,
            "CodeRate": "4/5",
            "DataRate": "0",
            "DevAddr": "00b96cd4",
            "DevEui": "58a0cb000202c99",            
            "FOptLen": 2,
            "FCnt": 1,
            "Fport": 136,   
            "Frequency": "868100000",     
            "Gateways": [
             {
                    "GatewayEui": "80029cfffe5cf1cc",      
                    "Snr": -29,
                    "Rssi": 9.75
             }
             ],  
            "MIC": "7255cb07",  
            "MType": "UnconfirmedDataUp",
            "Major": "LoRaWANR1",
            "Modulation": "LORA", 
            "PolarizationInversion": false,    
            "SpreadingFactor": 12,                         
            "Timestamp": "2021-05-03T03:24:29Z"
            
        }
    }
}
```

## Configuring position of your devices using the console<a name="connect-iot-lorawan-location-devices-console"></a>

To configure and manage the position of your device resources by using the AWS Management Console, first sign in to the console and then go to the [https://console.aws.amazon.com/iot/home#/wireless/devices](https://console.aws.amazon.com/iot/home#/wireless/devices) hub page of the AWS IoT console\.

**Add a position configuration**  
To add a position configuration for your device:

1. In the ** Devices** hub page, choose **Add wireless device**\.

1. Enter the wireless device specification, device and service profiles, and the destination that defines the IoT rule for routing the data to other AWS services\. For more information, see [Onboard your devices to AWS IoT Core for LoRaWAN](connect-iot-lorawan-onboard-end-devices.md)\.

1. Enter the position information, any optional geolocation solvers that you want to use, and a position data destination for your device\.

   1. 

**Position information**  
Specify the position data for your device using the latitude and longitude coordinates, and an optional altitude coordinate\. The position information is based on the WGS84 coordinate system\.

   1. 

**Geolocation solvers**  
Specify any geolocation solvers that you want AWS IoT Core for LoRaWAN to use for computing the device position\. These solvers can be used to identify the position of your device in real time\. To enter the solver information, choose **Activate positioning solvers**\.

      1. Check whether the **Solver type** is specified as `Semtech GNSS`\. To activate this solver, use a LoRaWAN device that has the Semtech LoRa Edge chip\. For more information, see [Positioning solvers: Semtech GNSS](connect-iot-lorawan-configure-location.md#connect-iot-lorawan-location-solver)\.

      1. Enter the positioning frequency ports, which include the FPort values for the Clock sync, Stream, and GNSS\. You'll see default FPorts populated for your reference\. However, you can choose a different value anywhere between 1 and 223\.

      1. Activate optional forward error correction \(FEC\) to the position data to avoid packet loss and correct errors without having to re\-transmit the data\. Using error correction may incur additional charges to your AWS account\. For more information about FEC and GNSS solver, see [ Modem and geolocation services](https://www.loracloud.com/documentation/modem_services?url=overview.html) in the *Semtech documentation*\.

   1. 

**Position data destination**  
Choose a destination to describe the AWS IoT rule that processes the device's position data and forwards it to AWS IoT Core for LoRaWAN\. Use this destination only to route position data\. It must be different from the destination that you use for routing device data to other AWS services

**View device's position configuration**  
After you've configured your device's position, AWS IoT Core for LoRaWAN creates an Amazon Location map called `iotwireless.map`\. You can see this map in the details page of your device on the **Position** tab\. Based on the position coordinates that you specified or the position computed by the positioning solver, the position of your device will be displayed as a marker on the map\. You can zoom in or zoom out to clearly view the position of your device on the map\.

**Note**  
If you haven't activated Amazon Location Service maps, you'll see a message indicating that you'll have to use Amazon Location Service to access the map and view the position\. Using Amazon Location Service maps may incur additional charges to your AWS account\. For more information, see [AWS IoT Core pricing](http://aws.amazon.com/iot-core/pricing/)\.

The map, `iotwireless.map`, acts as a source of map data which is accessed using `Get` API operations, such as [https://docs.aws.amazon.com/location-maps/latest/APIReference/API_GetMapTile.html](https://docs.aws.amazon.com/location-maps/latest/APIReference/API_GetMapTile.html)\. For information about `Get` APIs used with maps, see [Amazon Location Service API reference](https://docs.aws.amazon.com/location-maps/latest/APIReference/Welcome.html)\.

To get additional details about this map, go to the Amazon Location Service console, choose **maps**, and then choose [iotwireless\.map](https://console.aws.amazon.com/location/maps/home#/describe/iotwireless.map)\. For more information, see [Maps](https://docs.aws.amazon.com/location/latest/developerguide/map-concepts.html) in the *Amazon Location Service developer guide*\.

In the device's details page, on the **Position** tab, you'll also see the accuracy information, the timestamp at which your device's position was determined, and the position data destination that you specified\.

**Update device's position configuration**  
To change the device's position configuration, in the device details page, choose **Edit** and then update the position information, any solver configuration settings, and the destination\.

**Note**  
In this preview release, historical position information isn't available\. When you update the device's position coordinates, it overwrites the previously reported position data\. After you've updated the position, in the **Position** tab of the device details, you'll see the new position information\. The change in timestamp indicates that it corresponds to the last known position of the device\.

## Configure device position using the API<a name="connect-iot-lorawan-location-devices-api"></a>

You can specify the position information, configure the device position, and add any optional geolocation solver configurations using the AWS IoT Wireless API or the AWS CLI\. 

### Add position information and configuration<a name="connect-iot-lorawan-location-devices-api-add"></a>

Use the following AWS IoT Wireless API operations or AWS CLI commands to update the position information and the position configuration for a given wireless device\.
+ 

**Add device position**  
To add the static position information for a given wireless device, specify the coordinates using the [ UpdatePosition](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdatePosition.html) API operation or the [update\-position](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/update-position.html) CLI command\. Specify `WirelessDevice` as the `ResourceType`, the ID of the wireless device to be updated as the `ResourceIdentifier`, and the position coordinates as an array with the latitude, longitude, and altitude information\.

  ```
  aws iotwireless update-position \ 
      --resource-type WirelessDevice \ 
      --resource-id "1ffd32c8-8130-4194-96df-622f072a315f" \ 
      --position [33.33, -33.33, 10.0]
  ```

  Running this command doesn't produce any output\. To see the position information that you specified, use the `GetPosition` API\.
+ 

**Configure solvers and position destination**  
To configure the position destination and the solver for a given wireless device, use the [PutPositionConfiguration](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_PutPositionConfiguration.html) API operation or the [put\-position\-configuration](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/put-position-configuration.html) CLI command\. Specify `WirelessDevice` as the `ResourceType`, the ID of the wireless device to be updated as the `ResourceIdentifier`, the configuration information for any optional sovers that you want to use, and the destination for routing the position data\.

  ```
  aws iotwireless put-position-configuration \ 
      --resource-type WirelessDevice \ 
      --resource-id "1ffd32c8-8130-4194-96df-622f072a315f" \ 
      --cli-input-json file:\\input.json
  ```

  The following code shows the contents of the `input.json` file:

  **Contents of input\.json**

  ```
  {
      "Destination": "Device_position_destination"
      "Solvers": {
          "SemtechGnss": {
              "Status": "Enabled",
              "Fec": "ROSE"
          }
      }
  }
  ```

  Running this command doesn't produce any output\. To see the position information that you specified, use the `GetPositionConfiguration` or `ListPositionConfigurations` API operations\.

### Get position information and configuration<a name="connect-iot-lorawan-location-devices-api-get"></a>

Use the following AWS IoT Wireless API operations or AWS CLI commands to get the position information and the position configuration for a given wireless device\.
+ 

**Get position information of a device**  
To get the position information for a given wireless device, use the [ GetPosition](https://docs.aws.amazon.com/https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetPosition.html) API or the [get\-position](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/get-position.html) CLI command\. Specify `WirelessDevice` as the `resourceType` and provide the ID of the wireless device as the `resourceIdentifier`\.

  ```
  aws iotwireless get-position \ 
      --resource-type WirelessDevice \ 
      --resource-id "1ffd32c8-8130-4194-96df-622f072a315f"
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

**Get position configuration of a device**  
To get the position configuration for a given wireless device, use the [ GetPositionConfiguration](https://docs.aws.amazon.com/https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetPositionConfiguration.html) API or the [get\-position\-configuration](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/get-position-configuration.html) CLI command\. Specify `WirelessDevice` as the `resourceType` and provide the ID of the wireless device as the `resourceIdentifier`\.

  ```
  aws iotwireless get-position-configuration \ 
      --resource-type WirelessDevice \ 
      --resource-id "1ffd32c8-8130-4194-96df-622f072a315f"
  ```

  Running this command displays the position configuration of your wireless device\. You'll see information about the solver configuration and the destination that you specified for routing the position data\.

  ```
  {
      "Destination": "Device_position_destination",
      "Solvers": {
          "SemtechGnss": {
              "Fec": "ROSE",
              "Provider": "Semtech",
              "Status": "Enabled",
              "Type": "GNSS"
          }
      }
  }
  ```

### List position configurations<a name="connect-iot-lorawan-location-devices-api-list"></a>

You can create position configurations for multiple resources in your account\. The resources can be wireless devices or gateways or both\. After you create these configurations, you can use the [ListPositionConfigurations](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/                     API_ListPositionConfigurations.html) API operation or the [list\-position\-configurations](https://docs.aws.amazon.com/cli/latest/reference/iotwireless/list-position-configurations.html) CLI command to get a list of these configurations\. To filter the list to display position configurations for only wireless devices, set `resourceType` to `WirelessDevice`\.

```
aws iotwireless list-position-configurations \ 
    --resource-type WirelessDevice
```

Running this command displays position configurations for all LoRaWAN devices in your AWS account\. You can also use the `max-results` parameter to specify how many configurations you want to display\. The following code shows the output of running this command\.

```
{    
    "PositionConfigurationList": [
        {
            "Destination": "Device_position_destination1",
            "ResourceIdentifier": "12345678-a1b2-3c45-67d8-e90fa1b2c34d",
            "ResourceType": "WirelessDevice",
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
            "Destination": "Device_position_destination2",
            "ResourceIdentifier": "12345678-a1b2-c3d4-e5f6-e90fa1b2c34d",
            "ResourceType": "WirelessDevice",
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