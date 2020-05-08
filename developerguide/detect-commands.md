# Detect commands<a name="detect-commands"></a>

You can use the AWS IoT Device Defender Detect commands in this section to identify unusual behavior that might indicate a compromised device\.


**AWS IoT Device Defender detect commands**  

|  Name  |  Description  | 
| --- | --- | 
|  [AttachSecurityProfile](dd-api-iot-AttachSecurityProfile.md)  |  Associates an AWS IoT Device Defender security profile with one of the following target types:  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/detect-commands.html)  | 
|  [CreateDimension](dd-api-iot-CreateDimension.md)  |  Creates a dimension that you can use to limit the scope for behaviors that you define in a AWS IoT Device Defender security profile\.  | 
|  [CreateSecurityProfile](dd-api-iot-CreateSecurityProfile.md)  |  Creates an AWS IoT Device Defender security profile\.  | 
|  [DeleteDimension](dd-api-iot-DeleteDimension.md)  |  Deletes a dimension if it is not referenced by a behavior in any AWS IoT Device Defender security profile\.  | 
|  [DeleteSecurityProfile](dd-api-iot-DeleteSecurityProfile.md)  |  Deletes an AWS IoT Device Defender security profile\.  | 
|  [DescribeDimension](dd-api-iot-DescribeDimension.md)  |  Gets information about a dimension that can be used to limit the scope for behaviors in an AWS IoT Device Defender security profile\.  | 
|  [DescribeSecurityProfile](dd-api-iot-DescribeSecurityProfile.md)  |  Gets information about an AWS IoT Device Defender security profile\.  | 
|  [DetachSecurityProfile](dd-api-iot-DetachSecurityProfile.md)  |  Disassociates an AWS IoT Device Defender security profile from a thing group or from this AWS account\.  | 
|  [ListActiveViolations](dd-api-iot-ListActiveViolations.md)  |  Lists the active violations for a given AWS IoT Device Defender security profile\.  | 
|  [ListDimensions](dd-api-iot-ListDimensions.md)  |  Lists the dimensions defined in your AWS account\.  | 
|  [ListSecurityProfiles](dd-api-iot-ListSecurityProfiles.md)  |  Lists the AWS IoT Device Defender security profiles for your AWS account, including those profiles that reference a dimension\.  | 
|  [ListSecurityProfilesForTarget](dd-api-iot-ListSecurityProfilesForTarget.md)  |  Lists the AWS IoT Device Defender security profiles attached to a target \(thing group\)\.  | 
|  [ListTargetsForSecurityProfile](dd-api-iot-ListTargetsForSecurityProfile.md)  |  Lists the targets \(thing groups\) associated with a given AWS IoT Device Defender security profile\.  | 
|  [ListViolationEvents](dd-api-iot-ListViolationEvents.md)  |  Lists the AWS IoT Device Defender security profile violations discovered during the given time period\.  | 
|  [UpdateDimension](dd-api-iot-UpdateDimension.md)  |  Updates the value of a defined dimension for use in the behaviors of your security profiles\.  | 
|  [UpdateSecurityProfile](dd-api-iot-UpdateSecurityProfile.md)  |  Updates an AWS IoT Device Defender security profile\.  | 
|  [ValidateSecurityProfileBehaviors](dd-api-iot-ValidateSecurityProfileBehaviors.md)  |  Validates an AWS IoT Device Defender security profile behaviors specification\.  | 