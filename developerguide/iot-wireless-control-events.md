# Enable events for wireless resources<a name="iot-wireless-control-events"></a>

Before subscribers to the reserved topics can receive messages, you must enable event notifications\. To do this, you can use the AWS Management Console, or the AWS IoT Wireless API or AWS CLI\.

## Event configurations<a name="iot-wireless-control-events-config"></a>

You can configure events to send notifications to either all resources that belong to a particular type, or for individual wireless resources\. The resource type can be a wireless gateway, Sidewalk partner account, or a wireless device, which can be a LoRaWAN or Sidewalk device\. For information about the type of events that you can enable for your wireless devices, see [Event types for LoRaWAN resources](iot-lorawan-events.md#iot-lorawan-event-types) and [Event types for Sidewalk resources](iot-sidewalk-events.md#iot-sidewalk-event-types)\.

**All resources**  
You can enable events such that all resources in your AWS account that belong to a particular resource type receives notifications\. For example, you can enable an event that notifies you of changes to connection status for all LoRaWAN gateways that you've onboarded with AWS IoT Core for LoRaWAN\. Monitoring these events will help you get notified in cases such as when certain LoRaWAN gateways in your resource fleet get disconnected or if a beacon is lost for a number of Sidewalk devices in your AWS account\.

**Individual resources**  
You can also add individual LoRaWAN and Sidewalk resources to your event configuration and enable notifications for them\. This will help you monitor individual resources of a particular type\. For example, you can add select LoRaWAN and Sidewalk devices to your configuration and receive notifications for join or device registration state events for these resources\.

## Prerequisites<a name="iot-wireless-control-events-prereq"></a>

Your LoRaWAN or Sidewalk resource must have an appropriate policy that allows it to receive event notifications\. For more information, see [Policy for receiving wireless event notifications](iot-wireless-event-messages.md#iot-wireless-events-policy)\.

## Enable notifications using the AWS Management Console<a name="iot-wireless-control-events-console"></a>

To enable event messages from the console, go to the [Settings](console.aws.amazon.com/iot/home/settings/) tab of the AWS IoT console, and then go to the **LoRaWAN and Sidewalk event notification** section\. 

You can enable notifications for all resources in your AWS account that belong to a particular resource type and monitor them\.

**To enable notifications for all resources**

1. In the **LoRaWAN and Sidewalk event notification** section, go to the **All resources** tab, choose **Action**, and then choose **Manage events**\.

1. Enable the events that you want to monitor, and then choose **Update events**\. If you no longer want to monitor certain events, choose **Action** and choose **Manage events**, and then disable those events\.

You can also enable notifications for individual resources in your AWS account that belong to a particular resource type and monitor them\.

**To enable notifications for individual resources**

1. In the **LoRaWAN and Sidewalk event notification** section, choose **Action**, and then choose **Add resources**\.

1. Select the resources and events for which you want to receive notifications:

   1. Choose whether you want to monitor events for your **LoRaWAN resources** or **Sidewalk resources**\.

   1. Depending on the resource type, you can choose the events you want to enable for the resources\. You can then subscribe to these events and receive notifications\. If you choose:
      + **LoRaWAN resources**: You can enable **join** events for your LoRaWAN devices or **connection status** events for your LoRaWAN gateways\. 
      + Sidewalk resources: You can enable **device registration state** or **proximity** events or both for your Sidewalk partner accounts and Sidewalk devices\.

1. Depending on the resource type and events that you chose, select the wireless devices or gateways that you want to monitor\. You can select up to 250 resources for all resources combined\. 

1. Choose **Submit** to add your resources\.

The resources that you add will appear with their MQTT topics in the tab for your resource type in the **LoRaWAN and Sidewalk event notification** section of the console\.
+ **LoRaWAN join** events and events for your Sidewalk devices will appear in the **Wireless devices** section of the console\.
+ **Connection status** events for your LoRaWAN gateways will appear in the **Wireless gateways** section\.
+ **Device registration state** and **proximity** events for your Sidewalk accounts will appear in the **Sidewalk accounts** tab\.

**Subscribe to topics using MQTT client**  
Depending on whether you enabled events for all resources or for individual resource types, the events that you enabled will appear in the console with their MQTT topics on the **All resources** tab or the tab for the specified resource type\.
+ If you choose one of the MQTT topics, you can go to the MQTT client to subscribe to these topics and receive messages\.
+ If you've added multiple events, you can subscribe to multiple event topics and receive notifications for them\. To subscribe to multiple topics, choose your topics, and choose **Action** and then choose **Subscribe**\.

## Enable notifications using the AWS CLI<a name="iot-wireless-control-events-cli"></a>

You can configure events and add resources to your configuration by using the AWS IoT Wireless API or the AWS CLI\.

**Enable notifications for all resources**  
You can enable notifications for all resources in your AWS account that belong to a particular resource type and monitor them by using the [UpdateEventConfigurationByResourceTypes](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateResourceEventConfiguration.html) API or the [cli/latest/reference/iotwireless/update-event-configuration-by-resource-types.html](cli/latest/reference/iotwireless/update-event-configuration-by-resource-types.html) CLI command\. For example:

```
aws iotwireless update-event-configuration-by-resource-types \ 
   --cli-input-json input.json
```

**Contents of input\.json**

```
{   
   "DeviceRegistrationState": { 
      "Sidewalk": { 
         "AmazonIdEventTopic": "Enabled"
      }
   },
   "ConnectionStatus": { 
      "LoRaWAN": { 
         "WirelessGatewayEventTopic": "Enabled"
      }
   }
}
```

**Note**  
All quotation marks \("\) are escaped with a backslash \(\\\)\.

You can get the current event configuration by calling the [GetEventConfigurationByResourceTypes](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetResourceEventConfiguration.html) API or by using the [cli/latest/reference/iotwireless/get-event-configuration-by-resource-types.html](cli/latest/reference/iotwireless/get-event-configuration-by-resource-types.html) CLI command\. For example:

```
aws iotwireless get-event-configuration-by-resource-types
```

**Enable notifications for individual resources**  
To add individual resources to your event configuration and control which events are published by using the API or CLI, call the [UpdateResourceEventConfiguration](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateResourceEventConfiguration.html) API or use the [cli/latest/reference/iotwireless/update-resource-event-configuration.html](cli/latest/reference/iotwireless/update-resource-event-configuration.html) CLI command\. For example:

```
aws iotwireless update-resource-event-configuration \ 
   --identifer 1ffd32c8-8130-4194-96df-622f072a315f \ 
   --identifier-type WirelessDeviceId \ 
   --cli-input-json input.json
```

**Contents of input\.json**

```
{  
   "Join": { 
      "LoRaWAN": { 
         "DevEuiEventTopic": "Disabled"
      },
      "WirelessDeviceIdEventTopic": "Enabled"
   }
}
```

**Note**  
All quotation marks \("\) are escaped with a backslash \(\\\)\.

You can get the current event configuration by calling the [GetResourceEventConfiguration](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetResourceEventConfiguration.html) API or by using the [cli/latest/reference/iotwireless/get-resource-event-configuration.html](cli/latest/reference/iotwireless/get-resource-event-configuration.html) CLI command\. For example:

```
aws iotwireless get-resource-event-configuration \ 
    --identifier-type WirelessDeviceId \ 
    --identifier 1ffd32c8-8130-4194-96df-622f072a315f
```

**List event configurations**  
You can also use the AWS IoT Wireless API or the AWS CLI to list event configurations where at least one event topic has been enabled\. To list configurations, use the [ListEventConfigurations](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListEventConfigurations.html) API operation or by using the [cli/latest/reference/iotwireless/list-event-configurations.html](cli/latest/reference/iotwireless/list-event-configurations.html) CLI command\. For example:

```
aws iotwireless list-event-configurations --resource-type WirelessDevice
```