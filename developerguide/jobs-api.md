# Using the AWS IoT jobs APIs<a name="jobs-api"></a>

There are two categories of API used in the AWS IoT Jobs service: 
+ Those used for management and control of jobs\.
+ Those used by the devices executing those jobs\.

In general, job management and control uses an HTTPS protocol API\. Devices can use either an MQTT or an HTTPS protocol API\. \(The HTTPS API is designed for a low volume of calls typical when creating and tracking jobs\. It usually opens a connection for a single request, and then closes the connection after the response is received\. The MQTT API allows long polling\. It is designed for large amounts of traffic that can scale to millions of devices\.\)

**Note**  
Each AWS IoT Jobs HTTPS API has a corresponding command that allows you to call the API from the AWS CLI\. The commands are lowercase, with hyphens between the words that make up the name of the API\. For example, you can invoke the `CreateJob` API on the CLI by typing:  

```
aws iot create-job ...
```