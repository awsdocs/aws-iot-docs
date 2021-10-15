# Bulk registration<a name="bulk-provisioning"></a>

You can use the [ `start-thing-registration-task`](https://docs.aws.amazon.com/iot/latest/apireference/API_StartThingRegistrationTask.html) command to register things in bulk\. This command takes a registration template, an S3 bucket name, a key name, and a role ARN that allows access to the file in the S3 bucket\. The file in the S3 bucket contains the values used to replace the parameters in the template\. The file must be a newline\-delimited JSON file\. Each line contains all of the parameter values for registering a single device\. For example:

```
{"ThingName": "foo", "SerialNumber": "123", "CSR": "csr1"}
{"ThingName": "bar", "SerialNumber": "456", "CSR": "csr2"}
```

The following bulk registration\-related APIs might be useful:
+ [ListThingRegistrationTasks](https://docs.aws.amazon.com/iot/latest/apireference/API_ListThingRegistrationTasks.html): Lists the current bulk thing provisioning tasks\. 
+ [ DescribeThingRegistrationTask](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeThingRegistrationTask.html): Provides information about a specific bulk thing registration task\.
+ [StopThingRegistrationTask](https://docs.aws.amazon.com/iot/latest/apireference/API_StopThingRegistrationTask.html): Stops a bulk thing registration task\.
+ [ListThingRegistrationTaskReports](https://docs.aws.amazon.com/iot/latest/apireference/API_ListThingRegistrationTaskReports.html): Used to check the results and failures for a bulk thing registration task\.

**Note**  
Only one bulk registration operation task can run at a time \(per account\)\.
Bulk registration operations call other AWS IoT control plane APIs\. These calls might exceed the [ AWS IoT Throttling Quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#throttling-limits) in your account and cause throttle errors\. Contact [AWS Customer Support](https://console.aws.amazon.com/support/home) to raise your AWS IoT throttling quotas, if necessary\.