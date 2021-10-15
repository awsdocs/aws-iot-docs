# Job templates<a name="job-templates"></a>

**Note**  
The job templates feature is in preview and subject to change\.

Job templates enable you to preconfigure jobs so that you can deploy them to multiple sets of target devices\. They provide an efficient way to create standard configurations for remote actions that you need to deploy repeatedly to your devices\. When you create job templates, you can specify the following configurations and resources\.
+ Job documents
+ Rollout and cancel criteria
+ Timeout criteria

Optionally, you can create templates from existing jobs\. This can be helpful when you need to perform operations, such as security patches and bug fixes, in a standardized manner to customers\.

You can create jobs from job templates by using the AWS CLI and the AWS IoT console\. Operators can also create jobs from job templates by using Fleet Hub for AWS IoT Device Management web applications\. For more information about working with job templates in Fleet Hub applications, see [Working with job templates in Fleet Hub for AWS IoT Device Management](https://docs.aws.amazon.com/iot/latest/fleethubuserguide/aws-iot-monitor-technician-job-templates.html)\.

**Topics**
+ [Creating and managing job templates \(console\)](job-templates-console.md)
+ [Creating and managing job templates \(CLI\)](job-templates-cli.md)