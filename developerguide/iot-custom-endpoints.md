# Connecting Devices<a name="iot-custom-endpoints"></a>

Devices connect to AWS IoT using a fully qualified domain name \(FQDN\) that is specific to your account\. All connections to AWS IoT must be secured with TLS version 1\.2\. For more information, see [Transport Security in AWS IoT](transport-security.html)\. 

AWS IoT provides three types of endpoints, which are distinguished by the following three service types that they expose:
+ `Data` – Used to send and receive data to and from the [Message Broker](iot-message-broker.html), [Device Shadow](iot-device-shadows.html), and [Rules Engine](iot-rules.html) components of AWS IoT\.
+ `Credential_Provider` – Used to exchange a device's built\-in X\.509 certificate for temporary credentials to connect directly with other AWS services\. For more information about connecting to other AWS services, see [Authorizing Direct Calls to AWS Services](authorizing-direct-aws.html)\.
+ `Jobs` – Used to enable devices to interact with the AWS IoT Jobs service using the [Jobs Device MQTT and HTTPS APIs](jobs-api.html#jobs-mqtt-api)\. 

Every AWS IoT customer has default endpoints for each service type\. You use the [DescribeEndpoint](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeEndpoint.html) API to get your default endpoints\. The following list contains the valid endpoint types:
+ `iot:Data-ATS` – Endpoint for the `Data` service type\.
+ `iot:CredentialProvider` – Endpoint for the `Credential_Provider` service type\.
+ `iot:Jobs` – Endpoint for the `Jobs` service type\.

**Important**  
Every customer has another endpoint type: `iot:Data`\. Use this endpoint to retrieve old data endpoints that use VeriSign certificates for backward compatibility\. We strongly recommend that customers use the newer `iot:Data-ATS` endpoint type to avoid issues related to the widespread distrust of Symantec certificate authorities\. For more information, see [Server Authentication](server-authentication.html)\.

Specify the endpoint type to get from the `DescribeEndpoint` API by using the `endpointType` parameter\. The following AWS CLI example gets your default `iot:Data-ATS` endpoint\.

```
aws iot describe-endpoint --endpoint-type iot:Data-ATS
```

This command returns an endpoint in the following format: `account-specific-prefix-ats.iot.region.amazonaws.com`

You can also use your own FQDN \(for example, *example\.com*\) and the associated server certificate to connect devices to AWS IoT by using the configurable endpoints feature, which is currently in public beta\. The following section explains how to use configurable endpoints for your custom domains and AWS\-managed domains\.

**Topics**
+ [Configurable Endpoints \(Beta\)](iot-custom-endpoints-configurable.md)