# Authorizing your devices to securely use AWS IoT Jobs on the data plane<a name="iot-data-plane-jobs"></a>

To authorize your devices to interact securely with AWS IoT Jobs on the data plane, you must use AWS IoT Core policies\. AWS IoT Core policies for jobs are JSON documents containing policy statements\. These policies also use *Effect*, *Action*, and *Resource* elements, and follow a similar convention to IAM policies\. For more information about the elements, see [IAM JSON Policy Elements Reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_elements.html) in the *IAM User Guide*\.

The policies can be used with both MQTT and HTTPS protocols and must use TCP or TLS mutual authentication to authenticate the devices\. The following shows how to use these policies across the different communication protocols\.

**Warning**  
We recommend that you don't use wildcard permissions, such as `"Action": ["iot:*"]` in your IAM policies or AWS IoT Core policies\. Using wildcard permissions is not a recommended security best practice\. For more information, see [AWS IoT policies overly permissive](audit-chk-iot-policy-permissive.md)\. 

## AWS IoT Core policies for MQTT protocol<a name="iot-jobs-data-mqtt"></a>

AWS IoT Core policies for MQTT protocol grant you permissions to use the jobs device MQTT API actions\. The MQTT API operations are used to work with MQTT topics that are reserved for jobs commands\. For more information about these API operations, see [Jobs device MQTT API](jobs-mqtt-api.md)\.

MQTT policies use policy actions such as `iot:Connect`, `iot:Publish`, `iot:Subscribe`, and `iot:Receieve` to work with the jobs topics\. These policies allow you to connect to the message broker, subscribe to the jobs MQTT topics, and send and receive MQTT messages between your devices and the cloud\. For more information about these actions, see [AWS IoT Core policy actions](iot-policy-actions.md)\.

For information about topics for AWS IoT Jobs, see [Job topics](reserved-topics.md#reserved-topics-job)\.

### Basic MQTT policy example<a name="iot-jobs-mqtt-example"></a>

The following example shows how you can use `iot:Publish` and `iot:Subscribe` to publish and subscribe to jobs and job executions\.

In the example, replace:
+ *region* with your AWS Region, such as `us-east-1`\.
+ *account\-id* with your AWS account number, such as `57EXAMPLE833`\.
+ *thing\-name* with the name of your IoT thing for which you're targeting jobs, such as `MyIoTThing`\.

```
{
    "Statement": [
        {
           "Effect": "Allow",
           "Action": [
                "iot:Publish",
                "iot:Subscribe"
            ],
            "Resource": [
                "arn:aws:iot:region:account-id:topic/$aws/events/job/*",
                "arn:aws:iot:region:account-id:topic/$aws/events/jobExecution/*",
                "arn:aws:iot:region:account-id:topic/$aws/things/thing-name/jobs/*"
            ]
        }
    ],
    "Version": "2012-10-17"
}
```

## AWS IoT Core policies for HTTPS protocol<a name="iot-jobs-data-http"></a>

AWS IoT Core policies on the data plane can also use the HTTPS protocol with the TLS authentication mechanism to authorize your devices\. On the data plane, policies use the `iotjobsdata:` prefix to authorize jobs API operations that your devices can perform\. For example, the `iotjobsdata:DescribeJobExecution` policy action grants the user permission to use the [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_DescribeJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_DescribeJobExecution.html) API\.

**Note**  
The data plane policy actions must use the `iotjobsdata:` prefix\. On the control plane, the actions must use the `iot:` prefix\. For an example IAM policy when both control plane and data plane policy actions are used, see [IAM policy example for both control plane and data plane](iam-policy-users-jobs.md#iam-data-plane-example2)\. 

### Policy actions<a name="iot-data-plane-actions"></a>

The following table shows a list of AWS IoT Core policy actions and permissions for authorizing devices to use the API actions\. For a list of API operations that you can perform in the data plane, see [Jobs device HTTP API](jobs-http-device-api.md)\.

**Note**  
These job execution policy actions apply only to the HTTP TLS endpoint\. If you use the MQTT endpoint, you must use the MQTT policy actions defined previously\.


**AWS IoT Core policy actions on data plane**  

| Policy action | API operation | Resource types | Description | 
| --- | --- | --- | --- | 
| iotjobsdata:DescribeJobExecution | [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_DescribeJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_DescribeJobExecution.html) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iot-data-plane-jobs.html)  | Represents the permission to retrieve a job execution\. The iotjobsdata:DescribeJobExecution permission is checked every time a request is made to retrieve a job execution\. | 
| iotjobsdata:GetPendingJobExecutions | [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_GetPendingJobExecutions.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_GetPendingJobExecutions.html) | thing | Represents the permission to retrieve the list of jobs that are not in a terminal status for a thing\. The iotjobsdata:GetPendingJobExecutions permission is checked every time a request is made to retrieve the list\. | 
| iotjobsdata:StartNextPendingJobExecution | [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_GetPendingJobExecutions.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_GetPendingJobExecutions.html) | thing | Represents the permission to get and start the next pending job execution for a thing\. That is, to update a job execution with status QUEUED to IN\_PROGRESS\. The iot:StartNextPendingJobExecution permission is checked every time a request is made to start the next pending job execution\. | 
| iotjobsdata:UpdateJobExecution | [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html) | thing | Represents the permission to update a job execution\. The iot:UpdateJobExecution permission is checked every time a request is made to update the state of a job execution\. | 

### Basic policy example<a name="iot-data-plane-example"></a>

The following shows an example of an AWS IoT Core policy that allows the user permission to perform the actions on the data plane API operations for any resource\. You can scope your policy to a specific resource, such as an IoT thing\. In your example, replace:
+ *region* with your AWS Region such as `us-east-1`\.
+ *account\-id* with your AWS account number, such as `57EXAMPLE833`\.
+ *thing\-name* with the name of the IoT thing, such as `MyIoTthing`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "iotjobsdata:GetPendingJobExecutions",
                "iotjobsdata:StartNextPendingJobExecution",
                "iotjobsdata:DescribeJobExecution",
                "iotjobsdata:UpdateJobExecution"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:iot:region:account-id:thing/thing-name"
        }
    ]
}
```

An example of when you must use these policies can be when your IoT devices use an AWS IoT Core policy to access one of these API operations, such as the following example of the `DescribeJobExecution` API:

```
GET /things/thingName/jobs/jobId?executionNumber=executionNumber&includeJobDocument=includeJobDocument&namespaceId=namespaceId HTTP/1.1
```