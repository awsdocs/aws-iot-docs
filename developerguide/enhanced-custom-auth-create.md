# Creating a custom authorizer for enhanced custom authentication<a name="enhanced-custom-auth-create"></a>


****  

|  | 
| --- |
| This feature is currently in public beta and is available only in the US East \(N\. Virginia\) Region\. | 

You create a custom authorizer for enhanced custom authentication using the same [CreateAuthorizer](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateAuthorizer.html) API that you use to create a custom authorizer for custom authentication\. For procedures to create a custom authorizer, see [Configure a Custom Authorizer](config-custom-auth.html)\.

You can use the `signingDisabled` parameter of the `CreateAuthorizer` API\. This lets you opt out of signature validation by AWS IoT against the token provided in your HTTP requests\. We strongly recommend that you use this feature only in test environments\. You can't update the `signingDisabled` value of a custom authorizer\. To change this behavior, you must create a custom authorizer with a different value for `signingDisabled`\. Values for `tokenKeyName` and `tokenSigningPublicKeys` are optional, but `tokenKeyName` is required when you specify a value for `tokenSigningPublicKeys`\. 

**Adding a Custom Authorizer to a Configurable Endpoint**  
After you create your custom authorizer, you can attach it to an existing domain configuration as the default authorizer by using the [UpdateDomainConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateDomainConfiguration.html) API\. You can also create a configuration by using the [CreateDomainConfiguration](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateDomainConfiguration) API and specifying the name of your custom authorizer in the `defaultAuthorizerName` parameter\.