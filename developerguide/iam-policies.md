# IAM Policies<a name="iam-policies"></a>

AWS IoT works with AWS IoT and IAM policies\. This topic discusses IAM policies only\. For more information, see [AWS IoT Core Policies](iot-policies.md)\. AWS Identity and Access Management defines a policy action for each operation defined by AWS IoT, including control plane and data plane APIs\.

## IAM Managed Policies<a name="iam-managed-policies"></a>

AWS IoT provides a set of IAM managed policies you can either use as\-is or as a starting point for creating custom IAM policies\. These policies allow access to configuration and data operations\. Configuration operations allow you to create things, certificates, policies, and rules\. Data operations send data over MQTT or HTTP protocols\. The following table describes these templates\.


****  

| Policy Template | Description | 
| --- | --- | 
| AWSIoTConfigAccess | Allows the associated identity access to all AWS IoT configuration operations\. This policy can affect data processing and storage\. | 
| AWSIoTConfigReadOnlyAccess | Allows the associated identity to access read\-only configuration operations\. | 
| AWSIoTDataAccess | Allows the associated identity full access to all AWS IoT data operations\. Data operations send data over MQTT or HTTP protocols\. | 
| AWSIoTEventsFullAccess | Allows the associated identity full access to AWS IoT events\. | 
| AWSIoTEventsReadOnlyAccess | Allows the associated identity read only access to AWS IoT events\. | 
| AWSIoTFullAccess | Allows the associated identity full access to all AWS IoT configuration and messaging operations\. | 
| AWSIotLogging |  Allows the associated identity to create Amazon CloudWatch Logs groups and stream logs to the groups\. This policy is attached to your CloudWatch logging role\.  | 
|  AWSIoTOTAUpdate   |  Allows the associated identity to create AWS IoT jobs, AWS IoT code signing jobs, and to describe AWS code signer jobs\.  | 
| AWSIoTRuleActions | Allows the associated identity access to all AWS services supported in AWS IoT rule actions\.  | 
|  AWSIoTThingsRegistration  | Allows the associated identity to register things in bulk using the [StartThingRegistrationTask](https://docs.aws.amazon.com/iot/latest/apireference/API_StartThingRegistrationTask.html) API\. This policy can affect data processing and storage\. | 