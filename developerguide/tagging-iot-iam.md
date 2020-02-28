# Using Tags with IAM Policies<a name="tagging-iot-iam"></a>

You can apply tag\-based resource\-level permissions in the IAM policies you use for AWS IoT API actions\. This gives you better control over what resources a user can create, modify, or use\. You use the `Condition` element \(also called the `Condition` block\) with the following condition context keys and values in an IAM policy to control user access \(permissions\) based on a resource's tags: 
+ Use `aws:ResourceTag/tag-key: tag-value` to allow or deny user actions on resources with specific tags\.
+ Use `aws:RequestTag/tag-key: tag-value` to require that a specific tag be used \(or not used\) when making an API request to create or modify a resource that allows tags\.
+ Use `aws:TagKeys: [tag-key, ...]` to require that a specific set of tag keys be used \(or not used\) when making an API request to create or modify a resource that allows tags\.

**Note**  
The condition context keys and values in an IAM policy apply only to those AWS IoT actions where an identifier for a resource capable of being tagged is a required parameter\. For example, the use of [DescribeEndpoint](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeEndpoint) is not allowed or denied on the basis of condition context keys and values because no taggable resource \(thing groups, thing types, topic rules, jobs, or security profile\) is referenced in this request\.

For more information about using tags, see [Controlling Access Using Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html) in the *AWS Identity and Access Management User Guide*\. The [IAM JSON Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) section of that guide has detailed syntax, descriptions, and examples of the elements, variables, and evaluation logic of JSON policies in IAM\.

The following example policy applies two tag\-based restrictions\. An IAM user restricted by this policy:
+ Cannot give a resource the tag "env=prod" \(in the example, see the line `"aws:RequestTag/env" : "prod"`
+ Cannot modify or access a resource that has an existing tag "env=prod" \(in the example, see the line `"aws:ResourceTag/env" : "prod"`\)\.

```
{
    "Version" : "2012-10-17",
    "Statement" : [
        {
          "Effect" : "Deny",
          "Action" : "iot:*",
          "Resource" : "*",
          "Condition" : {
            "StringEquals" : {
              "aws:RequestTag/env" : "prod"
            }
          }
        },
        {
          "Effect" : "Deny",
          "Action" : "iot:*",
          "Resource" : "*",
          "Condition" : {
            "StringEquals" : {
              "aws:ResourceTag/env" : "prod"
            }
          }
        },
        {
          "Effect": "Allow",
          "Action": [
            "iot:*"
          ],
          "Resource": "*"
        }
    ]
}
```

You can also specify multiple tag values for a given tag key by enclosing them in a list, like this: 

```
            "StringEquals" : {
              "aws:ResourceTag/env" : ["dev", "test"]
            }
```

**Note**  
If you allow or deny users access to resources based on tags, you must consider explicitly denying users the ability to add those tags to or remove them from the same resources\. Otherwise, it's possible for a user to circumvent your restrictions and gain access to a resource by modifying its tags\.