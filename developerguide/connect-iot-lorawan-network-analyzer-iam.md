# Add necessary IAM role for network analyzer<a name="connect-iot-lorawan-network-analyzer-iam"></a>

When you use network analyzer, you must grant a user permission to use the API operations [UpdateNetworkAnalyzerConfiguration](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdateNetworkAnalyzerConfiguration.html) and [GetNetworkAnalyzerConfiguration](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetNetworkAnalyzerConfiguration.html) to access network analyzer resources\. The following shows the IAM policies that you use to grant permissions\.

## IAM policies for network analyzer<a name="connect-iot-lorawan-network-analyzer-policies"></a>

Use either of the following:
+ 

**Full access wireless policy**  
Grant AWS IoT Core for LoRaWAN the full access policy by attaching the policy **AWSIoTWirelessFullAccess** to your role\. For more information, see [`AWSIoTWirelessFullAccess` policy summary](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIoTWirelessFullAccess$serviceLevelSummary)\.
+ 

**Scoped IAM policy for Get and Update API**  
Create the following IAM policy by going to the [Create policy](https://console.aws.amazon.com/iam/home#/policies$new?step=edit) page of the IAM console, and on the **Visual editor** tab:

  1. Choose **IoTWireless** for **Service**\.

  1. Under **Access level**, expand **Read** and choose **GetNetworkAnalyzerConfiguration**, and then expand **Write** and choose **UpdateNetworkAnalyzerConfiguration**\.

  1. Choose **Next:Tags**, and enter a **Name** for the policy, such as **IoTWirelessNetworkAnalyzerPolicy**\. Choose **Create policy**\.

  The following shows the policy **IoTWirelessNetworkAnalyzerPolicy** that you created\. For more information about creating a policy, see [Create IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create)\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Sid": "VisualEditor0",
              "Effect": "Allow",
              "Action": [
                  "iotwireless:GetNetworkAnalyzerConfiguration",
                  "iotwireless:UpdateNetworkAnalyzerConfiguration"
              ],
              "Resource": "*"
          }
      ]
  }
  ```

**Scoped policy to access specific resources**  
To configure more fine\-grained access control, you must add the wireless gateways and devices to the **Resource** field\. The following policy uses the wildcard ARN to grant access to all gateways and devices\. You can control access to specific gateways and devices by using the `WirelessGatewayId` and `WirelessDeviceId`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "iotwireless:GetNetworkAnalyzerConfiguration",
                "iotwireless:UpdateNetworkAnalyzerConfiguration"
            ],
            "Resource": [
                "arn:aws:iotwireless:*:{accountId}:WirelessDevice/*", 
                "arn:aws:iotwireless:*:{accountId}:WirelessGateway/*", 
                "arn:aws:iotwireless:*:{accountId}:NetworkAnalyzerConfiguration/*"
            ]
        }
    ]
}
```

To grant a user permission to use network analyzer but not to use any wireless gateways or devices, use the following policy\. Unless specified, permissions to use the resources are implicitly denied\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "iotwireless:GetNetworkAnalyzerConfiguration",
                "iotwireless:UpdateNetworkAnalyzerConfiguration"
            ],
            "Resource": [                
                "arn:aws:iotwireless:*:{accountId}:NetworkAnalyzerConfiguration/*"
            ]
        }
    ]
}
```

## Next steps<a name="connect-iot-lorawan-network-analyzer-iam-next"></a>

Now that you've created the policy, you can add resources to your network analyzer configuration and receive trace messaging information for those resources\. For more information, see [Create a network analyzer configuration and add resources](connect-iot-lorawan-network-analyzer-create-resources.md)\.