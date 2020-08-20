# How AWS IoT works with IAM<a name="security_iam_service-with-iam"></a>

Before you use IAM to manage access to AWS IoT, you should understand which IAM features are available to use with AWS IoT\. To get a high\-level view of how AWS IoT and other AWS services work with IAM, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

**Topics**
+ [IAM policies](iam-policies.md)
+ [AWS IoT identity\-based policies](#security_iam_service-with-iam-id-based-policies)
+ [AWS IoT resource\-based policies](#security_iam_service-with-iam-resource-based-policies)
+ [Authorization based on AWS IoT tags](#security_iam_service-with-iam-tags)
+ [AWS IoT IAM roles](#security_iam_service-with-iam-roles)

## AWS IoT identity\-based policies<a name="security_iam_service-with-iam-id-based-policies"></a>

With IAM identity\-based policies, you can specify allowed or denied actions and resources as well as the conditions under which actions are allowed or denied\. AWS IoT supports specific actions, resources, and condition keys\. To learn about all of the elements that you use in a JSON policy, see [IAM JSON Policy Elements Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\.

### Actions<a name="security_iam_service-with-iam-id-based-policies-actions"></a>

The `Action` element of an IAM identity\-based policy describes the specific action or actions that will be allowed or denied by the policy\. Policy actions usually have the same name as the associated AWS API operation\. The action is used in a policy to grant permissions to perform the associated operation\. 

The following table lists the IAM IoT actions, the associated AWS IoT API, and the resource the action manipulates\.


****  

| Policy actions | AWS IoT API | Resources | 
| --- | --- | --- | 
| iot:AcceptCertificateTransfer | AcceptCertificateTransfer |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  The AWS account specified in the ARN must be the account to which the certificate is being transferred\.   | 
| iot:AddThingToThingGroup | AddThingToThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name* arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 
| iot:AssociateTargetsWithJob | AssociateTargetsWithJob | none  | 
| iot:AttachPolicy | AttachPolicy |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name* or arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:AttachPrincipalPolicy | AttachPrincipalPolicy |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:AttachSecurityProfile | AttachSecurityProfile |  arn:aws:iot:*region*:*account\-id*:securityprofile/*security\-profile\-name* arn:aws:iot:*region*:*account\-id*:dimension/*dimension\-name*  | 
| iot:AttachThingPrincipal | AttachThingPrincipal |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:CancelCertificateTransfer | CancelCertificateTransfer |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  The AWS account specified in the ARN must be the account to which the certificate is being transferred\.   | 
| iot:CancelJob | CancelJob |  arn:aws:iot:*region*:*account\-id*:job/*job\-id*  | 
| iot:CancelJobExecution | CancelJobExecution |  arn:aws:iot:*region*:*account\-id*:job/*job\-id* arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 
| iot:ClearDefaultAuthorizer | ClearDefaultAuthorizer | None | 
| iot:CreateAuthorizer | CreateAuthorizer |  arn:aws:iot:*region*:*account\-id*:authorizer/*authorizer\-function\-name*  | 
| iot:CreateCertificateFromCsr | CreateCertificateFromCsr | \* | 
| iot:CreateDimension | CreateDimension | arn:aws:iot:region:account\-id:dimension/dimension\-name | 
| iot:CreateJob | CreateJob |  arn:aws:iot:*region*:*account\-id*:job/*job\-id*  | 
| iot:CreateKeysAndCertificate | CreateKeysAndCertificate | \* | 
| iot:CreatePolicy | CreatePolicy | \* | 
| iot:CreatePolicyVersion | CreatePolicyVersion |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*  This must be an AWS IoT policy, not an IAM policy\.   | 
| iot:CreateRoleAlias | CreateRoleAlias |  \(parameter: roleAlias\) arn:aws:iot:*region*:*account\-id*:rolealias/*role\-alias\-name*  | 
| iot:CreateSecurityProfile | CreateSecurityProfile |  arn:aws:iot:*region*:*account\-id*:securityprofile/*security\-profile\-name* arn:aws:iot:*region*:*account\-id*:dimension/*dimension\-name*  | 
| iot:CreateThing | CreateThing |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 
| iot:CreateThingGroup | CreateThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name* for group being created and for parent group, if used  | 
| iot:CreateThingType | CreateThingType |  arn:aws:iot:*region*:*account\-id*:thingtype/*thing\-type\-name*  | 
| iot:CreateTopicRule | CreateTopicRule |  arn:aws:iot:*region*:*account\-id*:rule/*rule\-name*  | 
| iot:DeleteAuthorizer | DeleteAuthorizer |  arn:aws:iot:*region*:*account\-id*:authorizer/*authorizer\-name*  | 
| iot:DeleteCACertificate | DeleteCACertificate |  arn:aws:iot:*region*:*account\-id*:cacert/*cert\-id*  | 
| iot:DeleteCertificate | DeleteCertificate |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:DeleteDimension | DeleteDimension |  arn:aws:iot:*region*:*account\-id*:dimension/*dimension\-name*  | 
| iot:DeleteJob | DeleteJob |  arn:aws:iot:*region*:*account\-id*:job/*job\-id*  | 
| iot:DeleteJobExecution | DeleteJobExecution |  arn:aws:iot:*region*:*account\-id*:job/*job\-id* arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 
| iot:DeletePolicy | DeletePolicy |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*  | 
| iot:DeletePolicyVersion | DeletePolicyVersion |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*  | 
| iot:DeleteRegistrationCode | DeleteRegistrationCode | \* | 
| iot:DeleteRoleAlias | DeleteRoleAlias |  arn:aws:iot:*region*:*account\-id*:rolealias/*role\-alias\-name*  | 
| iot:DeleteSecurityProfile | DeleteSecurityProfile |  arn:aws:iot:*region*:*account\-id*:securityprofile/*security\-profile\-name* arn:aws:iot:*region*:*account\-id*:dimension/*dimension\-name*  | 
| iot:DeleteThing | DeleteThing |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 
| iot:DeleteThingGroup | DeleteThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*  | 
| iot:DeleteThingType | DeleteThingType |  arn:aws:iot:*region*:*account\-id*:thingtype/*thing\-type\-name*  | 
| iot:DeleteTopicRule | DeleteTopicRule |  arn:aws:iot:*region*:*account\-id*:rule/*rule\-name*  | 
| iot:DeleteV2LoggingLevel | DeleteV2LoggingLevel |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*  | 
| iot:DeprecateThingType | DeprecateThingType |  arn:aws:iot:*region*:*account\-id*:thingtype/*thing\-type\-name*  | 
| iot:DescribeAuthorizer | DescribeAuthorizer |  arn:aws:iot:*region*:*account\-id*:authorizer/*authorizer\-function\-name* \(parameter: authorizerName\) none  | 
| iot:DescribeCACertificate | DescribeCACertificate |  arn:aws:iot:*region*:*account\-id*:cacert/*cert\-id*  | 
| iot:DescribeCertificate | DescribeCertificate |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:DescribeDefaultAuthorizer | DescribeDefaultAuthorizer | None  | 
| iot:DescribeEndpoint | DescribeEndpoint | \* | 
| iot:DescribeEventConfigurations | DescribeEventConfigurations | none  | 
| iot:DescribeIndex | DescribeIndex |  arn:aws:iot:*region*:*account\-id*:index/*index\-name*  | 
| iot:DescribeJob | DescribeJob |  arn:aws:iot:*region*:*account\-id*:job/*job\-id*  | 
| iot:DescribeJobExecution | DescribeJobExecution | None | 
| iot:DescribeRoleAlias | DescribeRoleAlias |  arn:aws:iot:*region*:*account\-id*:rolealias/*role\-alias\-name*  | 
| iot:DescribeThing | DescribeThing |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 
| iot:DescribeThingGroup | DescribeThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*  | 
| iot:DescribeThingRegistrationTask | DescribeThingRegistrationTask | None | 
| iot:DescribeThingType | DescribeThingType |  arn:aws:iot:*region*:*account\-id*:thingtype/*thing\-type\-name*  | 
| iot:DetachPolicy | DetachPolicy |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id* or arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*  | 
| iot:DetachPrincipalPolicy | DetachPrincipalPolicy |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:DetachSecurityProfile | DetachSecurityProfile |  arn:aws:iot:*region*:*account\-id*:securityprofile/*security\-profile\-name* arn:aws:iot:*region*:*account\-id*:dimension/*dimension\-name*  | 
| iot:DetachThingPrincipal | DetachThingPrincipal |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:DisableTopicRule | DisableTopicRule |  arn:aws:iot:*region*:*account\-id*:rule/*rule\-name*  | 
| iot:EnableTopicRule | EnableTopicRule |  arn:aws:iot:*region*:*account\-id*:rule/*rule\-name*  | 
| iot:GetEffectivePolicies | GetEffectivePolicies |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:GetIndexingConfiguration | GetIndexingConfiguration | None | 
| iot:GetJobDocument | GetJobDocument |  arn:aws:iot:*region*:*account\-id*:job/*job\-id*  | 
| iot:GetLoggingOptions | GetLoggingOptions | \* | 
| iot:GetPolicy | GetPolicy |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*  | 
| iot:GetPolicyVersion | GetPolicyVersion |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*  | 
| iot:GetRegistrationCode | GetRegistrationCode | \* | 
| iot:GetTopicRule | GetTopicRule |  arn:aws:iot:*region*:*account\-id*:rule/*rule\-name*  | 
| iot:ListAttachedPolicies | ListAttachedPolicies |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name* or arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:ListAuthorizers | ListAuthorizers | None | 
| iot:ListCACertificates | ListCACertificates | \* | 
| iot:ListCertificates | ListCertificates | \* | 
| iot:ListCertificatesByCA | ListCertificatesByCA | \* | 
| iot:ListIndices | ListIndices | None | 
| iot:ListJobExecutionsForJob | ListJobExecutionsForJob | None | 
| iot:ListJobExecutionsForThing | ListJobExecutionsForThing | None | 
| iot:ListJobs | ListJobs |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name* if thingGroupName parameter used  | 
| iot:ListOutgoingCertificates | ListOutgoingCertificates | \* | 
| iot:ListPolicies | ListPolicies | \* | 
| iot:ListPolicyPrincipals | ListPolicyPrincipals |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*  | 
| iot:ListPolicyVersions | ListPolicyVersions |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*  | 
| iot:ListPrincipalPolicies | ListPrincipalPolicies |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:ListPrincipalThings | ListPrincipalThings |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:ListRoleAliases | ListRoleAliases | None | 
| iot:ListTargetsForPolicy | ListTargetsForPolicy |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*  | 
| iot:ListThingGroups | ListThingGroups | None | 
| iot:ListThingGroupsForThing | ListThingGroupsForThing |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 
| iot:ListThingPrincipals | ListThingPrincipals |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 
| iot:ListThingRegistrationTaskReports | ListThingRegistrationTaskReports | None | 
| iot:ListThingRegistrationTasks | ListThingRegistrationTasks | None | 
| iot:ListThingTypes | ListThingTypes | \* | 
| iot:ListThings | ListThings | \* | 
| iot:ListThingsInThingGroup | ListThingsInThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*  | 
| iot:ListTopicRules | ListTopicRules | \* | 
| iot:ListV2LoggingLevels | ListV2LoggingLevels | None | 
| iot:RegisterCACertificate | RegisterCACertificate | \* | 
| iot:RegisterCertificate | RegisterCertificate | \* | 
| iot:RegisterThing | RegisterThing | None | 
| iot:RejectCertificateTransfer | RejectCertificateTransfer |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:RemoveThingFromThingGroup | RemoveThingFromThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name* arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 
| iot:ReplaceTopicRule | ReplaceTopicRule |  arn:aws:iot:*region*:*account\-id*:rule/*rule\-name*  | 
| iot:SearchIndex | SearchIndex |  arn:aws:iot:*region*:*account\-id*:index/*index\-id*  | 
| iot:SetDefaultAuthorizer | SetDefaultAuthorizer |  arn:aws:iot:*region*:*account\-id*:authorizer/*authorizer\-function\-name*  | 
| iot:SetDefaultPolicyVersion | SetDefaultPolicyVersion |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*  | 
| iot:SetLoggingOptions | SetLoggingOptions |  arn:aws:iot:*region*:*account\-id*:role/*role\-name*  | 
| iot:SetV2LoggingLevel | SetV2LoggingLevel |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*  | 
| iot:SetV2LoggingOptions | SetV2LoggingOptions |  arn:aws:iot:*region*:*account\-id*:role/*role\-name*  | 
| iot:StartThingRegistrationTask | StartThingRegistrationTask | None | 
| iot:StopThingRegistrationTask | StopThingRegistrationTask | None | 
| iot:TestAuthorization | TestAuthorization |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:TestInvokeAuthorizer | TestInvokeAuthorizer | None | 
| iot:TransferCertificate | TransferCertificate |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:UpdateAuthorizer | UpdateAuthorizer |  arn:aws:iot:*region*:*account\-id*:authorizerfunction/*authorizer\-function\-name*  | 
| iot:UpdateCACertificate | UpdateCACertificate |  arn:aws:iot:*region*:*account\-id*:cacert/*cert\-id*  | 
| iot:UpdateCertificate | UpdateCertificate |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:UpdateDimension | UpdateDimension |  arn:aws:iot:*region*:*account\-id*:dimension/*dimension\-name*  | 
| iot:UpdateEventConfigurations | UpdateEventConfigurations | None | 
| iot:UpdateIndexingConfiguration | UpdateIndexingConfiguration | None | 
| iot:UpdateRoleAlias | UpdateRoleAlias |  arn:aws:iot:*region*:*account\-id*:rolealias/*role\-alias\-name*  | 
| iot:UpdateSecurityProfile | UpdateSecurityProfile |  arn:aws:iot:*region*:*account\-id*:securityprofile/*security\-profile\-name* arn:aws:iot:*region*:*account\-id*:dimension/*dimension\-name*  | 
| iot:UpdateThing | UpdateThing |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 
| iot:UpdateThingGroup | UpdateThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*  | 
| iot:UpdateThingGroupsForThing | UpdateThingGroupsForThing |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 

Policy actions in AWS IoT use the following prefix before the action: `iot:`\. For example, to grant someone permission to list all IoT things registered in their AWS account with the `ListThings` API, you include the `iot:ListThings` action in their policy\. Policy statements must include either an `Action` or `NotAction` element\. AWS IoT defines its own set of actions that describe tasks that you can perform with this service\.

To specify multiple actions in a single statement, separate them with commas as follows:

```
"Action": [
      "ec2:action1",
      "ec2:action2"
```

You can specify multiple actions using wildcards \(\*\)\. For example, to specify all actions that begin with the word `Describe`, include the following action:

```
"Action": "iot:Describe*"
```

To see a list of AWS IoT actions, see [Actions Defined by AWS IoT](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsiot.html#awsiot-actions-as-permissions) in the *IAM User Guide*\.

### Resources<a name="security_iam_service-with-iam-id-based-policies-resources"></a>

The `Resource` element specifies the object or objects to which the action applies\. Statements must include either a `Resource` or a `NotResource` element\. You specify a resource using an ARN or using the wildcard \(\*\) to indicate that the statement applies to all resources\.


**AWS IoT resources**  

| Policy actions | AWS IoT API | Resources | 
| --- | --- | --- | 
| iot:AcceptCertificateTransfer | AcceptCertificateTransfer |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  The AWS account specified in the ARN must be the account to which the certificate is being transferred\.   | 
| iot:AddThingToThingGroup | AddThingToThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name* arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 
| iot:AssociateTargetsWithJob | AssociateTargetsWithJob | none  | 
| iot:AttachPolicy | AttachPolicy | arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name* or arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:AttachPrincipalPolicy | AttachPrincipalPolicy |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:AttachThingPrincipal | AttachThingPrincipal |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:CancelCertificateTransfer | CancelCertificateTransfer |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  The AWS account specified in the ARN must be the account to which the certificate is being transferred\.   | 
| iot:CancelJob | CancelJob |  arn:aws:iot:*region*:*account\-id*:job/*job\-id*  | 
| iot:CancelJobExecution | CancelJobExecution |  arn:aws:iot:*region*:*account\-id*:job/*job\-id* arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 
| iot:ClearDefaultAuthorizer | ClearDefaultAuthorizer | None | 
| iot:CreateAuthorizer | CreateAuthorizer |  arn:aws:iot:*region*:*account\-id*:authorizer/*authorizer\-function\-name*  | 
| iot:CreateCertificateFromCsr | CreateCertificateFromCsr | \* | 
| iot:CreateJob | CreateJob |  arn:aws:iot:*region*:*account\-id*:job/*job\-id*  | 
| iot:CreateKeysAndCertificate | CreateKeysAndCertificate | \* | 
| iot:CreatePolicy | CreatePolicy | \* | 
| CreatePolicyVersion | iot:CreatePolicyVersion |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*  This must be an AWS IoT policy, not an IAM policy\.   | 
| iot:CreateRoleAlias | CreateRoleAlias |  \(parameter: roleAlias\) arn:aws:iot:*region*:*account\-id*:rolealias/*role\-alias\-name*  | 
| iot:CreateThing | CreateThing |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 
| iot:CreateThingGroup | CreateThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name* for group being created and for parent group, if used  | 
| iot:CreateThingType | CreateThingType |  arn:aws:iot:*region*:*account\-id*:thingtype/*thing\-type\-name*  | 
| iot:CreateTopicRule | CreateTopicRule |  arn:aws:iot:*region*:*account\-id*:rule/*rule\-name*  | 
| iot:DeleteAuthorizer | DeleteAuthorizer |  arn:aws:iot:*region*:*account\-id*:authorizer/*authorizer\-name*  | 
| iot:DeleteCACertificate | DeleteCACertificate |  arn:aws:iot:*region*:*account\-id*:cacert/*cert\-id*  | 
| iot:DeleteCertificate | DeleteCertificate |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:DeleteJob | DeleteJob |  arn:aws:iot:*region*:*account\-id*:job/*job\-id*  | 
| iot:DeleteJobExecution | DeleteJobExecution |  arn:aws:iot:*region*:*account\-id*:job/*job\-id* arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 
| iot:DeletePolicy | DeletePolicy |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*  | 
| iot:DeletePolicyVersion | DeletePolicyVersion |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*  | 
| iot:DeleteRegistrationCode | DeleteRegistrationCode | \* | 
| iot:DeleteRoleAlias | DeleteRoleAlias |  arn:aws:iot:*region*:*account\-id*:rolealias/*role\-alias\-name*  | 
| iot:DeleteThing | DeleteThing |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 
| iot:DeleteThingGroup | DeleteThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*  | 
| iot:DeleteThingType | DeleteThingType |  arn:aws:iot:*region*:*account\-id*:thingtype/*thing\-type\-name*  | 
| iot:DeleteTopicRule | DeleteTopicRule |  arn:aws:iot:*region*:*account\-id*:rule/*rule\-name*  | 
| iot:DeleteV2LoggingLevel | DeleteV2LoggingLevel |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*  | 
| iot:DeprecateThingType | DeprecateThingType |  arn:aws:iot:*region*:*account\-id*:thingtype/*thing\-type\-name*  | 
| iot:DescribeAuthorizer | DescribeAuthorizer |  arn:aws:iot:*region*:*account\-id*:authorizer/*authorizer\-function\-name* \(parameter: authorizerName\) none  | 
| iot:DescribeCACertificate | DescribeCACertificate |  arn:aws:iot:*region*:*account\-id*:cacert/*cert\-id*  | 
| iot:DescribeCertificate | DescribeCertificate |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:DescribeDefaultAuthorizer | DescribeDefaultAuthorizer | None  | 
| iot:DescribeEndpoint | DescribeEndpoint | \* | 
| iot:DescribeEventConfigurations | DescribeEventConfigurations | none  | 
| iot:DescribeIndex | DescribeIndex |  arn:aws:iot:*region*:*account\-id*:index/*index\-name*  | 
| iot:DescribeJob | DescribeJob |  arn:aws:iot:*region*:*account\-id*:job/*job\-id*  | 
| iot:DescribeJobExecution | DescribeJobExecution | None | 
| iot:DescribeRoleAlias | DescribeRoleAlias |  arn:aws:iot:*region*:*account\-id*:rolealias/*role\-alias\-name*  | 
| iot:DescribeThing | DescribeThing |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 
| iot:DescribeThingGroup | DescribeThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*  | 
| iot:DescribeThingRegistrationTask | DescribeThingRegistrationTask | None | 
| iot:DescribeThingType | DescribeThingType |  arn:aws:iot:*region*:*account\-id*:thingtype/*thing\-type\-name*  | 
| iot:DetachPolicy | DetachPolicy |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id* or arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*  | 
| iot:DetachPrincipalPolicy | DetachPrincipalPolicy |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:DetachThingPrincipal | DetachThingPrincipal |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:DisableTopicRule | DisableTopicRule |  arn:aws:iot:*region*:*account\-id*:rule/*rule\-name*  | 
| iot:EnableTopicRule | EnableTopicRule |  arn:aws:iot:*region*:*account\-id*:rule/*rule\-name*  | 
| iot:GetEffectivePolicies | GetEffectivePolicies |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:GetIndexingConfiguration | GetIndexingConfiguration | None | 
| iot:GetJobDocument | GetJobDocument |  arn:aws:iot:*region*:*account\-id*:job/*job\-id*  | 
| iot:GetLoggingOptions | GetLoggingOptions | \* | 
| iot:GetPolicy | GetPolicy |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*  | 
| iot:GetPolicyVersion | GetPolicyVersion |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*  | 
| iot:GetRegistrationCode | GetRegistrationCode | \* | 
| iot:GetTopicRule | GetTopicRule |  arn:aws:iot:*region*:*account\-id*:rule/*rule\-name*  | 
| iot:ListAttachedPolicies | ListAttachedPolicies |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name* or arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:ListAuthorizers | ListAuthorizers | None | 
| iot:ListCACertificates | ListCACertificates | \* | 
| iot:ListCertificates | ListCertificates | \* | 
| iot:ListCertificatesByCA | ListCertificatesByCA | \* | 
| iot:ListIndices | ListIndices | None | 
| iot:ListJobExecutionsForJob | ListJobExecutionsForJob | None | 
| iot:ListJobExecutionsForThing | ListJobExecutionsForThing | None | 
| iot:ListJobs | ListJobs |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name* if thingGroupName parameter used  | 
| iot:ListOutgoingCertificates | ListOutgoingCertificates | \* | 
| iot:ListPolicies | ListPolicies | \* | 
| iot:ListPolicyPrincipals | ListPolicyPrincipals |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*  | 
| iot:ListPolicyVersions | ListPolicyVersions |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*  | 
| iot:ListPrincipalPolicies | ListPrincipalPolicies |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:ListPrincipalThings | ListPrincipalThings |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:ListRoleAliases | ListRoleAliases | None | 
| iot:ListTargetsForPolicy | ListTargetsForPolicy |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*  | 
| iot:ListThingGroups | ListThingGroups | None | 
| iot:ListThingGroupsForThing | ListThingGroupsForThing |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 
| iot:ListThingPrincipals | ListThingPrincipals |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 
| iot:ListThingRegistrationTaskReports | ListThingRegistrationTaskReports | None | 
| iot:ListThingRegistrationTasks | ListThingRegistrationTasks | None | 
| iot:ListThingTypes | ListThingTypes | \* | 
| iot:ListThings | ListThings | \* | 
| iot:ListThingsInThingGroup | ListThingsInThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*  | 
| iot:ListTopicRules | ListTopicRules | \* | 
| iot:ListV2LoggingLevels | ListV2LoggingLevels | None | 
| iot:RegisterCACertificate | RegisterCACertificate | \* | 
| iot:RegisterCertificate | RegisterCertificate | \* | 
| iot:RegisterThing | RegisterThing | None | 
| iot:RejectCertificateTransfer | RejectCertificateTransfer |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:RemoveThingFromThingGroup | RemoveThingFromThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name* arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 
| iot:ReplaceTopicRule | ReplaceTopicRule |  arn:aws:iot:*region*:*account\-id*:rule/*rule\-name*  | 
| iot:SearchIndex | SearchIndex |  arn:aws:iot:*region*:*account\-id*:index/*index\-id*  | 
| iot:SetDefaultAuthorizer | SetDefaultAuthorizer |  arn:aws:iot:*region*:*account\-id*:authorizer/*authorizer\-function\-name*  | 
| iot:SetDefaultPolicyVersion | SetDefaultPolicyVersion |  arn:aws:iot:*region*:*account\-id*:policy/*policy\-name*  | 
| iot:SetLoggingOptions | SetLoggingOptions |  arn:aws:iot:*region*:*account\-id*:role/*role\-name*  | 
| iot:SetV2LoggingLevel | SetV2LoggingLevel |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*  | 
| iot:SetV2LoggingOptions | SetV2LoggingOptions |  arn:aws:iot:*region*:*account\-id*:role/*role\-name*  | 
| iot:StartThingRegistrationTask | StartThingRegistrationTask | None | 
| iot:StopThingRegistrationTask | StopThingRegistrationTask | None | 
| iot:TestAuthorization | TestAuthorization |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:TestInvokeAuthorizer | TestInvokeAuthorizer | None | 
| iot:TransferCertificate | TransferCertificate |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:UpdateAuthorizer | UpdateAuthorizer |  arn:aws:iot:*region*:*account\-id*:authorizerfunction/*authorizer\-function\-name*  | 
| iot:UpdateCACertificate | UpdateCACertificate |  arn:aws:iot:*region*:*account\-id*:cacert/*cert\-id*  | 
| iot:UpdateCertificate | UpdateCertificate |  arn:aws:iot:*region*:*account\-id*:cert/*cert\-id*  | 
| iot:UpdateEventConfigurations | UpdateEventConfigurations | None | 
| iot:UpdateIndexingConfiguration | UpdateIndexingConfiguration | None | 
| iot:UpdateRoleAlias | UpdateRoleAlias |  arn:aws:iot:*region*:*account\-id*:rolealias/*role\-alias\-name*  | 
| iot:UpdateThing | UpdateThing |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 
| iot:UpdateThingGroup | UpdateThingGroup |  arn:aws:iot:*region*:*account\-id*:thinggroup/*thing\-group\-name*  | 
| iot:UpdateThingGroupsForThing | UpdateThingGroupsForThing |  arn:aws:iot:*region*:*account\-id*:thing/*thing\-name*  | 

For more information about the format of ARNs, see [Amazon Resource Names \(ARNs\) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\.

Some AWS IoT actions, such as those for creating resources, cannot be performed on a specific resource\. In those cases, you must use the wildcard \(\*\)\.

```
"Resource": "*"
```

To see a list of AWS IoT resource types and their ARNs, see [Resources Defined by AWS IoT](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsiot.html#awsiot-resources-for-iam-policies) in the *IAM User Guide*\. To learn with which actions you can specify the ARN of each resource, see [Actions Defined by AWS IoT](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsiot.html#awsiot-actions-as-permissions)\.

### Condition keys<a name="security_iam_service-with-iam-id-based-policies-conditionkeys"></a>

The `Condition` element \(or `Condition` *block*\) lets you specify conditions in which a statement is in effect\. The `Condition` element is optional\. You can create conditional expressions that use [condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html), such as equals or less than, to match the condition in the policy with values in the request\. 

If you specify multiple `Condition` elements in a statement, or multiple keys in a single `Condition` element, AWS evaluates them using a logical `AND` operation\. If you specify multiple values for a single condition key, AWS evaluates the condition using a logical `OR` operation\. All of the conditions must be met before the statement's permissions are granted\.

 You can also use placeholder variables when you specify conditions\. For example, you can grant an IAM user permission to access a resource only if it is tagged with their IAM user name\. For more information, see [IAM Policy Elements: Variables and Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html) in the *IAM User Guide*\. 

AWS IoT defines its own set of condition keys and also supports using some global condition keys\. To see all AWS global condition keys, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\. 


**AWS IoT condition keys**  

| AWS IoT condition keys | Description | Type | 
| --- | --- | --- | 
| aws:RequestTag/$\{<tag\-key>\} | A tag key that is present in the request that the user makes to AWS IoT\. | String | 
| aws:ResourceTag/$\{<tag\-key>\} | The tag key component of a tag attached to an AWS IoT resource\. | String | 
| aws:TagKeys | The list of all the tag key names associated with the resource in the request\. | String | 

To see a list of AWS IoT condition keys, see [Condition Keys for AWS IoT](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsiot.html#awsiot-policy-keys) in the *IAM User Guide*\. To learn with which actions and resources you can use a condition key, see [Actions Defined by AWS IoT](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsiot.html#awsiot-actions-as-permissions)\.

### Examples<a name="security_iam_service-with-iam-id-based-policies-examples"></a>



To view examples of AWS IoT identity\-based policies, see [AWS IoT identity\-based policy examples](security_iam_id-based-policy-examples.md)\.

## AWS IoT resource\-based policies<a name="security_iam_service-with-iam-resource-based-policies"></a>

Resource\-based policies are JSON policy documents that specify what actions a specified principal can perform on the AWS IoT resource and under what conditions\.

AWS IoT does not support IAM resource\-based policies\. It does, however, support AWS IoT resource\-based policies\. 

## Authorization based on AWS IoT tags<a name="security_iam_service-with-iam-tags"></a>

You can attach tags to AWS IoT resources or pass tags in a request to AWS IoT\. To control access based on tags, you provide tag information in the [condition element](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) of a policy using the `iot:ResourceTag/key-name`, `aws:RequestTag/key-name`, or `aws:TagKeys` condition keys\. For more information, see [Using tags with IAM policies](tagging-iot-iam.md)\. For more information about tagging AWS IoT resources, see [Tagging your AWS IoT resources](tagging-iot.md)\.

To view an example identity\-based policy for limiting access to a resource based on the tags on that resource, see [Viewing AWS IoT resources based on tags](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-view-thing-tags)\.

## AWS IoT IAM roles<a name="security_iam_service-with-iam-roles"></a>

An [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is an entity within your AWS account that has specific permissions\.

### Using temporary credentials with AWS IoT<a name="security_iam_service-with-iam-roles-tempcreds"></a>

You can use temporary credentials to sign in with federation, assume an IAM role, or to assume a cross\-account role\. You obtain temporary security credentials by calling AWS STS API operations such as [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) or [GetFederationToken](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html)\. 

AWS IoT supports using temporary credentials\. 

### Service\-linked roles<a name="security_iam_service-with-iam-roles-service-linked"></a>

[Service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) allow AWS services to access resources in other services to complete an action on your behalf\. Service\-linked roles appear in your IAM account and are owned by the service\. An IAM administrator can view but not edit the permissions for service\-linked roles\.

AWS IoT does not supports service\-linked roles\.

### Service roles<a name="security_iam_service-with-iam-roles-service"></a>

This feature allows a service to assume a [service role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-role) on your behalf\. This role allows the service to access resources in other services to complete an action on your behalf\. Service roles appear in your IAM account and are owned by the account\. This means that an IAM administrator can change the permissions for this role\. However, doing so might break the functionality of the service\.

AWS IoT does not support service roles\.