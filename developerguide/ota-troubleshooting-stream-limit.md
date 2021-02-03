# Troubleshooting "Stream limit exceeded for your AWS account"<a name="ota-troubleshooting-stream-limit"></a>

If you see `"Error: You have exceeded the limit for the number of streams in your AWS account."`, you can clean up the unused streams in your account instead of requesting a limit increase\.

To clean up an unused stream that you created using the AWS CLI or SDK:

```
aws iot delete-stream –stream-id value
```

For more details, see [delete\-stream](https://docs.aws.amazon.com/cli/latest/reference/iot/delete-stream.html)\.

**Note**  
You can use the `list-streams` command to find the stream IDs\.