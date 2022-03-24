# Troubleshooting "Stream limit exceeded for your AWS account"<a name="ota-troubleshooting-stream-limit"></a>

**Help us improve this topic**  
 [Let us know what would help make it better](https://docs.aws.amazon.com/forms/aws-doc-feedback?hidden_service_name=IoT%20Docs&topic_url=http://docs.aws.amazon.com/en_us/iot/latest/developerguide/ota-troubleshooting-stream-limit.html) 

If you see `"Error: You have exceeded the limit for the number of streams in your AWS account."`, you can clean up the unused streams in your account instead of requesting a limit increase\.

To clean up an unused stream that you created using the AWS CLI or SDK:

```
aws iot delete-stream –stream-id value
```

For more details, see [delete\-stream](https://docs.aws.amazon.com/cli/latest/reference/iot/delete-stream.html)\.

**Note**  
You can use the `list-streams` command to find the stream IDs\.