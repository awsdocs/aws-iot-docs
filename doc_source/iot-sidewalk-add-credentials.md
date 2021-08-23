# Add your Sidewalk account credentials<a name="iot-sidewalk-add-credentials"></a>

You can connect Sidewalk devices to AWS IoT by using the AWS Management Console or the AWS IoT Wireless API\. To onboard your device, we'll create a wireless connectivity profile for your Sidewalk device and then add a destination and AWS IoT rule for the profile and Sidewalk endpoints\.

Before adding your device, you must add your Sidewalk account credentials\. You can add your credentials by using the AWS Management Console or the AWS IoT Wireless API\.

## Add your Sidewalk account credentials by using the console<a name="iot-sidewalk-credentials-console"></a>

To add your Sidewalk account credentials from the console:

1. Navigate to the [ Profiles](https://console.aws.amazon.com/iot/home#/wireless/profiles) page of the AWS IoT console and choose the **Sidewalk** tab\.
**Note**  
Make sure that you're using the `us-east-1` Region\. This tab doesn't appear in the console if you're using a different Region\.

1. Enter the Sidewalk Amazon ID\. You get this ID from the [Sidewalk Developer Service \(SDS\) console](http://developer.amazon.com/acs-devices/console/Sidewalk) when designing your Sidewalk product\. For more information, see [Design your Sidewalk product](https://developer.amazon.com/acs-devices/console/sidewalk/docs/group__qsg__step2.html)\.

1. Upload the **AppServerPrivateKey**, which is the server key that your vendor provided\. The **AppServerPrivateKey** is the ED25519 private key \(the `app-server-ed25519-private.txt` file\), which is a 64\-digit hexadecimal value that you generate by using the Sidewalk certificate generation tool when designing your Sidewalk product\. For more information, see [Design your Sidewalk product](https://developer.amazon.com/acs-devices/console/sidewalk/docs/group__qsg__step2.html)\.

1. To add your Sidewalk credentials, choose **Add credential**\.

## Add your Sidewalk account credentials by using the API<a name="iot-sidewalk-credentials-api"></a>

You can use the AWS IoT Wireless API to add your Sidewalk account credentials\. The following list describes the API actions\.

**AWS IoT Wireless API actions for Sidewalk account**
+ [AssociateAwsAccountWithPartnerAccount](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_AssociateAwsAccountWithPartnerAccount.html)
+ [DisasociateAwsAccountFromPartnerAccount](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_DisassociateAwsAccountFromPartnerAccount.html)
+ [GetPartnerAccount](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_GetPartnerAccount.html)
+ [ListPartnerAccounts](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_ListPartnerAccounts.html)
+ [UpdatePartnerAccount](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/API_UpdatePartnerAccount.html)

For the complete list of the actions and data types available to create and manage AWS IoT Core for LoRaWAN resources, see the [AWS IoT Wireless API reference](https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/welcome.html)\.

**How to use the AWS CLI to add an account**  
You can use the AWS CLI to associate a Sidewalk account to your AWS account by using the [associate\-aws\-account\-with\-partner\-account](cli/latest/reference/iotwireless/associate-aws-account-with-partner-account.html) command, as illustrated by the following example\.

**Note**  
You can also perform this procedure with the API by using the methods in the AWS API that correspond to the CLI commands shown here\. 

```
aws iotwireless associate-aws-account-with-partner-account \
    --sidewalk AmazonId="12345678901234",AppServerPrivateKey="a123b45c6d78e9f012a34cd5e6a7890b12c3d45e6f78a1b234c56d7e890a1234"
```

## Next steps<a name="iot-sidewalk-credentials-next-steps"></a>

Now that you've added the credentials and set up a wireless connectivity profile, you can add a destination for your Sidewalk device\. You'll see the credentials that you added in the [ Profiles](https://console.aws.amazon.com/iot/home#/wireless/profiles) page of the AWS IoT console, on the **Sidewalk** tab\. You'll define a role name and a rule name for the destination that can route messages sent from your devices\. For more information, see [Add a destination for your Sidewalk device](iot-sidewalk-add-destination.md)\.