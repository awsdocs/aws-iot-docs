# Deleting a rule<a name="iot-delete-rule"></a>

When you are finished with a rule, you can delete it\.

**To delete a rule \(AWS CLI\)**  
Use the [delete\-topic\-rule](https://docs.aws.amazon.com/cli/latest/reference/iot/delete-topic-rule.html) command to delete a rule:

```
aws iot delete-topic-rule --rule-name myrule
```