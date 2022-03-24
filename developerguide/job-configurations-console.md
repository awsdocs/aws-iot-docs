# Specify job configurations by using the AWS Management Console<a name="job-configurations-console"></a>

You can add the different configurations for your job by using the AWS IoT console\. After you've created a job, you can see the status details of your job configurations on the job details page\. For more information about the different configurations and how they work, see [What are the different configurations?](jobs-configurations.md#jobs-configurations-details)\.

Add the job configurations when you create a job or a job template\.

**When creating a custom job template**  
To specify the rollout configuration when creating a custom job template

1. Go to the [Job templates hub of the AWS IoT console](https://console.aws.amazon.com/iot/home#/jobtemplatehub) and choose **Create job template**\.

1. Specify the job template properties, provide the job document, expand the configuration that you want to add, and then specify the configuration parameters\.

**When creating a custom job**  
To specify the rollout configuration when creating a custom job

1. Go to the [Job hub of the AWS IoT console](https://console.aws.amazon.com/iot/home#/jobhub) and choose **Create job**\.

1. Choose **Create a custom job** and specify the job properties, targets, and whether to use a job file or a template for the job document\. You can use a custom template or an AWS managed template\.

1. Choose the job configuration and then expand **Rollout configuration** to specify whether to use a **Constant rate** or **Exponential rate**\. Then, specify the configuration parameters\.

The next section shows the parameters you can specify for each configuration\.

## Rollout configuration<a name="job-rollout-console"></a>

You can specify whether to use a constant rollout rate or an exponential rate\.
+ 

**Set a constant rollout rate**  
To set a constant rate for job executions, choose **Constant rate** and then specify the **Maximum per minute** for the upper limit of the rate\. This value is optional and ranges from 1 to 1000\. If you don't set it, it uses 1000 as the default value\. 
+ 

**Set an exponential rollout rate**  
To set an exponential rate, choose **Exponential rate** and then specify these parameters:
  + 

**Base rate per minute**  
Rate at which the jobs are executed until the **Number of notified devices** or **Number of succeeded devices** threshold is met for **Rate increase criteria**\.
  + 

**Increment factor**  
The exponential factor by which the rollout rate increases after the **Number of notified devices** or **Number of succeeded devices** threshold is met for **Rate increase criteria**\.
  + 

**Rate increase criteria**  
The threshold for either **Number of notified devices** or **Number of succeeded devices**\.

## Abort configuration<a name="job-abort-console"></a>

Choose **Add new configuration** and specify the following parameters for each configuration:
+ 

**Failure type**  
Specifies the failure types that initiate a job abort\. These include **FAILED**, **REJECTED**, **TIMED\_OUT**, or **ALL**\.
+ 

**Increment factor**  
Specifies the number of completed job executions that must occur before the job abort criteria has been met\.
+ 

**Threshold percentage**  
Specifies the total number of executed things that initiate a job abort\.

## Timeout configuration<a name="job-timeout-console"></a>

By default, there's no timeout and your job runs canceled or deleted\. To use timeouts, choose **Enable timeout** and then specify a timeout value between 1 minute and 7 days\.

## Retry configuration<a name="job-retry-console"></a>

**Note**  
After a job has been created, the number of retries can't be updated\. You can only remove the retry configuration for all failure types\. When you're creating a job, consider the appropriate number of retries to use for your configuration\. To avoid incurring excess costs because of potential retry failures, add an abort configuration\.

Choose **Add new configuration** and specify the following parameters for each configuration:
+ 

**Failure type**  
Specifies the failure types that should trigger a job execution retry\. These include **Failed**, **Timeout**, and **All**\. 
+ 

**Number of retries**  
Specifies the number of retries for the chosen **Failure type**\. For both failure types combined, up to 10 retries can be attempted\.