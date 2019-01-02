# Bulk Provisioning<a name="bulk-provisioning"></a>

You can use the [ `start-thing-registration-task`](http://alpha-docs-aws.amazon.com/iot/latest/apireference/API_StartThingRegistrationTask.html) command to provision things in bulk\. This command takes a provisioning template, an Amazon S3 bucket name, a key name, and a role ARN that allows access to the file in the Amazon S3 bucket\. The file in the Amazon S3 bucket contains the values used to replace the parameters in the template\. The file must be a newline\-delimited JSON file\. Each line contains all of the parameter values for provisioning a single device\. For example:

```
{"ThingName": "foo", "SerialNumber": "123", "CSR": "csr1"}
{"ThingName": "bar", "SerialNumber": "456", "CSR": "csr2"}
```

The following bulk provisioning\-related APIs might be useful:

+ [ListThingRegistrationTasks](http://alpha-docs-aws.amazon.com/iot/latest/apireference/API_ListThingRegistrationTasks.html) – Lists the current bulk thing provisioning tasks\. 

+ [ DescribeThingRegistrationTask](http://alpha-docs-aws.amazon.com/iot/latest/apireference/API_DescribeThingRegistrationTask.html) – Provides information about a specific bulk thing provisioning task\.

+ [StopThingRegistrationTask](http://alpha-docs-aws.amazon.com/iot/latest/apireference/API_StopThingRegistrationTask.html) – Stops a bulk thing provisioning task\.

+ [ListThingRegistrationTaskReports](http://alpha-docs-aws.amazon.com/iot/latest/apireference/API_ListThingRegistrationTaskReports.html) – Used to check the results and/or failures for a bulk thing provisioning task\.

**Note**  
Only one bulk provisioning operation task can run at a time \(per account\)\.