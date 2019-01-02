# IAM IoT Policies<a name="iam-policies"></a>

AWS Identity and Access Management defines a policy action for each operation defined by AWS IoT, including control plane and data plane APIs\.

## AWS IoT API Permissions<a name="iam-policy-permissions"></a>

The following table lists the AWS IoT API, the IAM permissions required, and the resource the API manipulates\.


****  

| API | Required permission \(policy actions\) | Resources | 
| --- | --- | --- | 
| AcceptCertificateTransfer | iot:AcceptCertificateTransfer |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*   The AWS account specified in the ARN must be the account to which the certificate is being transferred\.   | 
| AddLoggingRole | iot:AddLoggingRole |  none | 
| AddThingToThingGroup | iot:AddThingToThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*   | 
| AssociateTargetsWithJob | iot:AssociateTargetsWithJob | none  | 
| AttachPolicy | iot:AttachPolicy |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*  or arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*   | 
| AttachPrincipalPolicy | iot:AttachPrincipalPolicy |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*   | 
| AttachThingPrincipal | iot:AttachThingPrincipal |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*   | 
| CancelCertificateTransfer | iot:CancelCertificateTransfer |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*   The AWS account specified in the ARN must be the account to which the certificate is being transferred\.   | 
| CancelJob | iot:CancelJob |  arn:aws:iot:*region*:*account\-id*:job/*job\-id*   | 
| CancelJobExecution | iot:CancelJobExecution |  arn:aws:iot:*region*:*account\-id*:job/*job\-id*  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*   | 
| ClearDefaultAuthorizer | iot:ClearDefaultAuthorizer | none  | 
| CreateAuthorizer | iot:CreateAuthorizer |  arn:aws:iot:*region*:*account\-id*:authorizer/*authorizer\-function\-name*   | 
| CreateCertificateFromCsr | iot:CreateCertificateFromCsr | \* | 
| CreateJob | iot:CreateJob |  arn:aws:iot:*region*:*account\-id*:job/*job\-id*   | 
| CreateKeysAndCertificate | iot:CreateKeysAndCertificate | \* | 
| CreatePolicy | iot:CreatePolicy | \* | 
| CreatePolicyVersion | iot:CreatePolicyVersion |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*   This must be an AWS IoT policy, not an IAM policy\.   | 
| CreateRoleAlias | iot:CreateRoleAlias |  \(parameter: roleAlias\) arn:aws:iot:*region*:*account\-id*:rolealias/*role\-alias\-name*   | 
| CreateThing | iot:CreateThing |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*   | 
| CreateThingGroup | iot:CreateThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*  for group being created and for parent group, if used  | 
| CreateThingType | iot:CreateThingType |  arn:aws:iot:*region*:*account\-id*:thingtype/*thing\-type\-name*   | 
| CreateTopicRule | iot:CreateTopicRule |  arn:aws:iot:*region*:*account\-id*:rule/*rule\-name*   | 
| DeleteAuthorizer | iot:DeleteAuthorizer |  arn:aws:iot:*region*:*account\-id*:authorizer/*authorizer\-name*   | 
| DeleteCACertificate | iot:DeleteCACertificate |  arn:aws:iot:*region*:*account\-id*:cacert/*cert\-id*   | 
| DeleteCertificate | iot:DeleteCertificate |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*   | 
| DeleteJob | iot:DeleteJob |   `arn:aws:iot:region:account-id:job/job-id`  | 
| DeleteJobExecution | iot:DeleteJobExecution |   `arn:aws:iot:region:account-id:job/job-id`  `arn:aws:iot:region:account-id:thing/thing-name`  | 
| DeleteLoggingLevel | iot:DeleteLoggingLevel |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*   | 
| DeleteLoggingRole | iot:DeleteLoggingRole | none  | 
| DeletePolicy | iot:DeletePolicy |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*   | 
| DeletePolicyVersion | iot:DeletePolicyVersion |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*   | 
| DeleteRegistrationCode | iot:DeleteRegistrationCode | \* | 
| DeleteRoleAlias | iot:DeleteRoleAlias |  arn:aws:iot:*region*:*account\-id*:rolealias/*role\-alias\-name*   | 
| DeleteThing | iot:DeleteThing |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*   | 
| DeleteThingGroup | iot:DeleteThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*   | 
| DeleteThingType | iot:DeleteThingType |  arn:aws:iot:*region*:*account\-id*:thingtype/*thing\-type\-name*   | 
| DeleteTopicRule | iot:DeleteTopicRule |  arn:aws:iot:*region*:*account\-id*:rule/*rule\-name*   | 
| DeleteV2LoggingLevel | iot:DeleteV2LoggingLevel |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*   | 
| DeprecateThingType | iot:DeprecateThingType |  arn:aws:iot:*region*:*account\-id*:thingtype/*thing\-type\-name*   | 
| DescribeAuthorizer | iot:DescribeAuthorizer |  arn:aws:iot:*region*:*account\-id*:authorizer/*authorizer\-function\-name*  \(parameter: authorizerName\) none  | 
| DescribeCACertificate | iot:DescribeCACertificate |  arn:aws:iot:*region*:*account\-id*:cacert/*cert\-id*   | 
| DescribeCertificate | iot:DescribeCertificate |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*   | 
| DescribeDefaultAuthorizer | iot:DescribeDefaultAuthorizer | none  | 
| DescribeEndpoint | iot:DescribeEndpoint | \* | 
| DescribeEventConfigurations | iot:DescribeEventConfigurations | none  | 
| DescribeIndex | iot:DescribeIndex |  arn:aws:iot:*region*:*account\-id*:index/*index\-name*   | 
| DescribeJob | iot:DescribeJob |  arn:aws:iot:*region*:*account\-id*:job/*job\-id*   | 
| DescribeJobExecution | iot:DescribeJobExecution | none | 
| DescribeRoleAlias | iot:DescribeRoleAlias |  arn:aws:iot:*region*:*account\-id*:rolealias/*role\-alias\-name*   | 
| DescribeThing | iot:DescribeThing |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*   | 
| DescribeThingGroup | iot:DescribeThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*   | 
| DescribeThingRegistrationTask | iot:DescribeThingRegistrationTask | none | 
| DescribeThingType | iot:DescribeThingType |  arn:aws:iot:*region*:*account\-id*:thingtype/*thing\-type\-name*   | 
| DetachPolicy | iot:DetachPolicy |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  or arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*   | 
| DetachPrincipalPolicy | iot:DetachPrincipalPolicy |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*   | 
| DetachThingPrincipal | iot:DetachThingPrincipal |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*   | 
| DisableTopicRule | iot:DisableTopicRule |  arn:aws:iot:*region*:*account\-id*:rule/*rule\-name*   | 
| EnableTopicRule | iot:EnableTopicRule |  arn:aws:iot:*region*:*account\-id*:rule/*rule\-name*   | 
| GetEffectivePolicies | iot:GetEffectivePolicies |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*   | 
| GetIndexingConfiguration | iot:GetIndexingConfiguration | none | 
| GetJobDocument | iot:GetJobDocument |  arn:aws:iot:*region*:*account\-id*:job/*job\-id*   | 
| GetLoggingOptions | iot:GetLoggingOptions | \* | 
| GetLoggingRole | iot:GetLoggingRole | none | 
| GetPolicy | iot:GetPolicy |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*   | 
| GetPolicyVersion | iot:GetPolicyVersion |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*   | 
| GetRegistrationCode | iot:GetRegistrationCode | \* | 
| GetTopicRule | iot:GetTopicRule |  arn:aws:iot:*region*:*account\-id*:rule/*rule\-name*   | 
| GetV2LoggingOptions | iot:GetV2LoggingOptions | none | 
| ListAttachedPolicies | iot:ListAttachedPolicies |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*  or arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*   | 
| ListAuthorizers | iot:ListAuthorizers | none | 
| ListCACertificates | iot:ListCACertificates | \* | 
| ListCertificates | iot:ListCertificates | \* | 
| ListCertificatesByCA | iot:ListCertificatesByCA | \* | 
| ListIndices | iot:ListIndices | none | 
| ListJobExecutionsForJob | iot:ListJobExecutionsForJob | none | 
| ListJobExecutionsForThing | iot:ListJobExecutionsForThing | none | 
| ListJobs | iot:ListJobs |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*  if thingGroupName parameter used  | 
| ListLoggingLevels | iot:ListLoggingLevels | none | 
| ListOutgoingCertificates | iot:ListOutgoingCertificates | \* | 
| ListPolicies | iot:ListPolicies | \* | 
| ListPolicyPrincipals | iot:ListPolicyPrincipals |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*   | 
| ListPolicyVersions | iot:ListPolicyVersions |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*   | 
| ListPrincipalPolicies | iot:ListPrincipalPolicies |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*   | 
| ListPrincipalThings | iot:ListPrincipalThings |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*   | 
| ListRoleAliases | iot:ListRoleAliases | none | 
| ListTargetsForPolicy | iot:ListTargetsForPolicy |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*   | 
| ListThingGroups | iot:ListThingGroups | none | 
| ListThingGroupsForThing | iot:ListThingGroupsForThing |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*   | 
| ListThingPrincipals | iot:ListThingPrincipals |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*   | 
| ListThingRegistrationTaskReports | iot:ListThingRegistrationTaskReports | none | 
| ListThingRegistrationTasks | iot:ListThingRegistrationTasks | none | 
| ListThingTypes | iot:ListThingTypes | \* | 
| ListThings | iot:ListThings | \* | 
| ListThingsInThingGroup | iot:ListThingsInThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*   | 
| ListTopicRules | iot:ListTopicRules | \* | 
| ListV2LoggingLevels | iot:ListV2LoggingLevels | none | 
| RegisterCACertificate | iot:RegisterCACertificate | \* | 
| RegisterCertificate | iot:RegisterCertificate | \* | 
| RegisterThing | iot:RegisterThing | none | 
| RejectCertificateTransfer | iot:RejectCertificateTransfer |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*   | 
| RemoveThingFromThingGroup | iot:RemoveThingFromThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*   | 
| ReplaceTopicRule | iot:ReplaceTopicRule |  arn:aws:iot:*region*:*account\-id*:rule/*rule\-name*   | 
| SearchIndex | iot:SearchIndex |  arn:aws:iot:*region*:*account\-id*:index/*index\-id*   | 
| SetDefaultAuthorizer | iot:SetDefaultAuthorizer |  arn:aws:iot:*region*:*account\-id*:authorizer/*authorizer\-function\-name*   | 
| SetDefaultPolicyVersion | iot:SetDefaultPolicyVersion |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*   | 
| SetLoggingLevel | iot:SetLoggingLevel | none | 
| SetLoggingOptions | iot:SetLoggingOptions |  arn:aws:iot:*region*:*account\-id*:role/*role\-name*   | 
| SetV2LoggingLevel | iot:SetV2LoggingLevel |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*   | 
| SetV2LoggingOptions | iot:SetV2LoggingOptions |  arn:aws:iot:*region*:*account\-id*:role/*role\-name*   | 
| StartThingRegistrationTask | iot:StartThingRegistrationTask | none | 
| StopThingRegistrationTask | iot:StopThingRegistrationTask | none | 
| TestAuthorization | iot:TestAuthorization |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*   | 
| TestInvokeAuthorizer | iot:TestInvokeAuthorizer | none | 
| TransferCertificate | iot:TransferCertificate |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*   | 
| UpdateAuthorizer | iot:UpdateAuthorizer |  arn:aws:iot:*region*:*account\-id*:authorizerfunction/*authorizer\-function\-name*   | 
| UpdateCACertificate | iot:UpdateCACertificate |  arn:aws:iot:*region*:*account\-id*:cacert/*cert\-id*   | 
| UpdateCertificate | iot:UpdateCertificate |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*   | 
| UpdateEventConfigurations | iot:UpdateEventConfigurations | none | 
| UpdateIndexingConfiguration | iot:UpdateIndexingConfiguration | none | 
| UpdateRoleAlias | iot:UpdateRoleAlias |  arn:aws:iot:*region*:*account\-id*:rolealias/*role\-alias\-name*   | 
| UpdateThing | iot:UpdateThing |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*   | 
| UpdateThingGroup | iot:UpdateThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*   | 
| UpdateThingGroupsForThing | iot:UpdateThingGroupsForThing |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*   | 

## IAM Policy Templates<a name="iam-policy-templates"></a>

AWS IoT provides a set of IAM policy templates you can either use as\-is or as a starting point for creating custom IAM policies\. These templates allow access to configuration and data operations\. Configuration operations allow you to create things, certificates, policies, and rules\. Data operations send data over MQTT or HTTP protocols\. The following table describes these templates\.


****  

| Policy Template | Description | 
| --- | --- | 
| AWSIotLogging |  Allows the associated identity to configure CloudWatch logging\. This policy is attached to your CloudWatch logging role\.  | 
| AWSIoTConfigAccess | Allows the associated identity access to all AWS IoT configuration operations\. This policy can affect data processing and storage\. | 
| AWSIoTConfigReadOnlyAccess | Allows the associated identity to call read\-only configuration operations\. | 
| AWSIoTDataAccess | Allows the associated identity full access to all AWS IoT data operations\. Data operations send data over MQTT or HTTP protocols\. | 
| AWSIoTFullAccess | Allows the associated identity full access to all AWS IoT configuration and data operations\. | 
|  AWSIoTOTAUpdate   |  Allows the associated identity access to create AWS IoT jobs and AWS IoT code signing jobs\.  | 
| AWSIoTRuleActions | Allows the associated identity access to all AWS services supported in AWS IoT rule actions\.  | 
|  AWSIoTThingsRegistration  | Allows the associated identity to register things in bulk using [StartThingRegistrationTask](http://alpha-docs-aws.amazon.com/iot/latest/apireference/API_StartThingRegistrationTask.html)\. This policy can affect data processing and storage\. | 