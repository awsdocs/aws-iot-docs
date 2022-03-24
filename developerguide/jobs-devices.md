# Devices and jobs<a name="jobs-devices"></a>

Devices can communicate with AWS IoT Jobs using MQTT, HTTP Signature Version 4, or HTTP TLS\. To determine the endpoint to use when your device communicates with AWS IoT Jobs, run the DescribeEndpoint command For example, if you run this command:

```
aws iot describe-endpoint --endpoint-type iot:Data-ATS
```

you get a result similar to the following:

```
{
    "endpointAddress": "a1b2c3d4e5f6g7-ats.iot.us-west-2.amazonaws.com"
}
```

## Using the MQTT protocol<a name="jobs-using-mqtt"></a>

Devices can communicate with AWS IoT Jobs using MQTT protocol\. Devices subscribe to MQTT topics to be notified of new jobs and to receive responses from the AWS IoT Jobs service\. Devices publish on MQTT topics to query or update the state of a job execution\. Each device has its own general MQTT topic\. For more information about publishing and subscribing to MQTT topics, see [Device communication protocols](protocols.md)\.

With this method of communication, your device uses its device\-specific certificate and private key to authenticate with AWS IoT Jobs\.

Your devices can subscribe to the following topics\. `thing-name` is the name of the thing associated with the device\.
+ 

**`$aws/things/thing-name/jobs/notify`**  
Subscribe to this topic to notify you when a job execution is added or removed from the list of pending job executions\.
+ 

**`$aws/things/thing-name/jobs/notify-next`**  
Subscribe to this topic to notify you when when the next pending job execution has changed\.
+ 

**`$aws/things/thing-name/request-name/accepted`**  
The AWS IoT Jobs service publishes success and failure messages on an MQTT topic\. The topic is formed by appending `accepted` or `rejected` to the topic used to make the request\. Here, `request-name` is the name of a request such as `Get` and the topic can be: `$aws/things/myThing/jobs/get`\. AWS IoT Jobs then publishes success messages on the `$aws/things/myThing/jobs/get/accepted` topic\.
+ 

**`$aws/things/thing-name/request-name/rejected`**  
Here, `request-name` is the name of a request such as `Get`\. If the request failed, 

You can also use the following APIs:
+ Update the status of a job execution by calling the [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html) API\.
+ Query the status of a job execution by calling the [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_DescribeJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_DescribeJobExecution.html) API\.
+ Retrieve a list of pending job executions by calling the [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_GetPendingJobExecutions.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_GetPendingJobExecutions.html) API\.
+ Retrieve the next pending job execution by calling the [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_DescribeJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_DescribeJobExecution.html) API with `jobId` as `$next`\.
+ Get and start the next pending job execution by calling the [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_StartNextPendingJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_StartNextPendingJobExecution.html) API\.

## Using HTTP Signature Version 4<a name="jobs-using-http-v4"></a>

Devices can communicate with AWS IoT Jobs using HTTP Signature Version 4 on port 443\. This is the method used by the AWS SDKs and CLI\. For more information about those tools, see [AWS CLI Command Reference: iot\-jobs\-data](https://docs.aws.amazon.com/cli/latest/reference/iot-jobs-data/index.html) or [AWS SDKs and Tools](http://aws.amazon.com/tools/#sdk) and refer to the IotJobsDataPlane section for your preferred language\.

With this method of communication, your device uses IAM credentials to authenticate with AWS IoT Jobs\.

The following commands are available using this method: 
+ DescribeJobExecution

  `aws iot-jobs-data describe-job-execution ...` 
+ GetPendingJobExecutions

  `aws iot-jobs-data get-pending-job-executions ...` 
+ StartNextPendingJobExecution

  `aws iot-jobs-data start-next-pending-job-execution ...` 
+ UpdateJobExecution

  `aws iot-jobs-data update-job-execution ...` 

## Using HTTP TLS<a name="jobs-using-http-tls"></a>

Devices can communicate with AWS IoT Jobs using HTTP TLS on port 8443 using a third\-party software client that supports this protocol\.

With this method, your device uses X\.509 certificate\-based authentication \(for example, its device\-specific certificate and private key\)\.

The following commands are available using this method: 
+ DescribeJobExecution
+ GetPendingJobExecutions
+ StartNextPendingJobExecution
+ UpdateJobExecution

**Topics**
+ [Programming devices to work with jobs](programming-devices.md)