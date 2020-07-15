# Creating and configuring AWS\-managed domains<a name="iot-custom-endpoints-configurable-aws"></a>

****  
This feature is currently in public beta and is available only in the US East \(N\. Virginia\) Region\.

You create a configurable endpoint on an AWS\-managed domain by using the [CreateDomainConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateDomainConfiguration.html) API\. A domain configuration for an AWS\-managed domain consists of the following:
+ `domainConfigurationName` – A user\-defined name that identifies the domain configuration\.
**Note**  
Domain configuration names that start with `IoT:` are reserved for default endpoints and can't be used\. Also, this value must be unique to your region\.
+ `defaultAuthorizerName` – The name of the custom authorizer to use on the endpoint\.
+ `allowAuthorizerOverride` – A Boolean value that specifies whether devices can override the default authorizer by specifying a different authorizer in the HTTP header of the request\. This value is required if a value for `defaultAuthorizerName` is specified\.
+ `serviceType` – Possible values are `DATA`, `CREDENTIAL_PROVIDER`, and `JOB`\. If you specify `DATA`, AWS IoT returns an endpoint with an endpoint type of `iot:Data-Beta`\. This is a special endpoint type for the configurable endpoints beta release\. The endpoint serves ATS\-signed server certificates\. You can't create a configurable `iot:Data` \(VeriSign\) endpoint\.
**Note**  
AWS IoT Core currently supports only the `DATA` service type\.

The following AWS CLI command creates domain configuration for a `Data` endpoint\.

```
aws iot create-domain-configuration --domain-configuration-name "myDomainConfigurationName" --service-type "DATA"
```