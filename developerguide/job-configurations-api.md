# Specify job configurations by using the AWS IoT Jobs API<a name="job-configurations-api"></a>

You can use the [CreateJob](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html) or the [CreateJobTemplate](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJobTemplate.html) API to specify the different job configurations\. The following shows how you add these configurations\. After you've added the configurations, you can use [JobExecutionSummary](https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionSummary.html) and [JobExecutionSummaryForJob](https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionSummaryForJob.html) to view their status\.

For more information about the different configurations and how it works, see [What are the different configurations?](jobs-configurations.md#jobs-configurations-details)\.

## Rollout configuration<a name="job-rollout-api"></a>

You can specify a constant rollout rate or an exponential rollout rate for your rollout configuration\.
+ 

**Set a constant rollout rate**  
To set a constant rollout rate, use the [https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionsRolloutConfig.html](https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionsRolloutConfig.html) object to add the `maximumPerMinute` parameter to the `CreateJob` request\. This parameter specifies the upper limit of the rate at which job executions can occur\. This value is optional and ranges from 1 to 1000\. If you don't set the value, it uses 1000 as the default value\.

  ```
      "jobExecutionsRolloutConfig": {
          "maximumPerMinute": 1000
      }
  ```
+ 

**Set an exponential rollout rate**  
To set a variable job rollout rate, use the [https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionsRolloutConfig.html](https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionsRolloutConfig.html) object\. You can configure the `ExponentialRolloutRate`property when you run the `CreateJob` API operation\. The following example sets an exponential rollout rate by using the `exponentialRate` parameter\. For more information about the parameters, see [https://docs.aws.amazon.com/iot/latest/apireference/API_ExponentialRolloutRate.html](https://docs.aws.amazon.com/iot/latest/apireference/API_ExponentialRolloutRate.html)

  ```
  {
  ...
    "jobExecutionsRolloutConfig": {
      "exponentialRate": {
        "baseRatePerMinute": 50,
        "incrementFactor": 2,
        "rateIncreaseCriteria": {
          "numberOfNotifiedThings": 1000,
          "numberOfSucceededThings": 1000
        },
        "maximumPerMinute": 1000
      }
    }
  ...
  }
  ```

Where the parameter:

**baseRatePerMinute**  
Specifies the rate at which the jobs are executed until the `numberOfNotifiedThings` or `numberOfSucceededThings` threshold has been met\.

**incrementFactor**  
Specifies the exponential factor by which the rollout rate increases after the `numberOfNotifiedThings` or `numberOfSucceededThings` threshold has been met\.

**rateIncreaseCriteria**  
Specifies either the `numberOfNotifiedThings` or `numberOfSucceededThings` threshold\.

## Abort configuration<a name="job-abort-api"></a>

To add this configuration by using the API, specify the [https://docs.aws.amazon.com/iot/latest/apireference/API_AbortConfig.html](https://docs.aws.amazon.com/iot/latest/apireference/API_AbortConfig.html) parameter when you run the [https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html) or the [https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJobTemplate.html](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJobTemplate.html) API operation\. The following example shows an abort configuration for a job rollout that was experiencing multiple failed executions, as specified with the `CreateJob` API operation\.

**Note**  
Deleting a job execution affects the computation value of the total completed execution\. When a job aborts, the service creates an automated `comment` and `reasonCode` to differentiate a user\-driven cancellation from a job abort cancellation\.

```
   "abortConfig": { 
      "criteriaList": [ 
         { 
            "action": "CANCEL",
            "failureType": "FAILED",
            "minNumberOfExecutedThings": 100,
            "thresholdPercentage": 20
         },
         { 
            "action": "CANCEL",
            "failureType": "TIMED_OUT",
            "minNumberOfExecutedThings": 200,
            "thresholdPercentage": 50
         }
      ]
    }
```

Where the parameter:

**action**  
Specifies the action to take when the abort criteria have been met\. This parameter is required, and `CANCEL` is the only valid value\.

**failureType**  
Specifies which failure types should initiate a job abort\. Valid values are `FAILED`, `REJECTED`, `TIMED_OUT`, and `ALL`\.

**minNumberOfExecutedThings**  
Specifies the number of completed job executions that must occur before the job abort criteria has been met\. In this example, AWS IoT doesn't check to see if a job abort should occur until at least 100 devices have completed job executions\.

**thresholdPercentage**  
Specifies the total number of things for which jobs are executed that can initiate a job abort\. In this example, AWS IoT checks sequentially and initiates a job abort if the threshold percentage is met\. If at least 20% of the complete executions failed after 100 executions are complete, it cancels the job rollout\. If this criteria isn't met, AWS IoT then checks if at least 50% of completed executions timed out after 200 executions are complete\. If this is the case, it cancels the job rollout\.

## Timeout configuration<a name="job-timeout-api"></a>

To add this configuration by using the API, specify the [https://docs.aws.amazon.com/iot/latest/apireference/API_TimeoutConfig.html](https://docs.aws.amazon.com/iot/latest/apireference/API_TimeoutConfig.html) parameter when you run the [https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html) or the [https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJobTemplate.html](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJobTemplate.html) API operation\.

To use the timeout configuration

1. To set the in\-progress timer when you're creating a job or job template, set a value for the `inProgressTimeoutInMinutes` property of the optional [TimeoutConfig](https://docs.aws.amazon.com/iot/latest/apireference/API_TimeoutConfig.html) object\.

   ```
       "timeoutConfig": { 
         "inProgressTimeoutInMinutes": number
      }
   ```

1. To specify a step timer for a job execution, set a value for `stepTimeoutInMinutes` when you call [UpdateJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_UpdateJobExecution.html)\. The step timer applies only to the job execution that you update\. You can set a new value for this timer each time you update a job execution\.
**Note**  
`UpdateJobExecution` can also discard a step timer that has already been created by creating a new step timer with a value of \-1\.

   ```
   {
      ... 
       "statusDetails": { 
         "string" : "string" 
      },
      "stepTimeoutInMinutes": number
   }
   ```

1. To create a new step timer, you can also call the [StartNextPendingJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_StartNextPendingJobExecution.html) API operation\.

## Retry configuration<a name="job-retry-api"></a>

**Note**  
When you're creating a job, consider the appropriate number of retries to use for your configuration\. To avoid incurring excess costs because of potential retry failures, add an adbort configuration\. After a job has been created, the number of retries can't be updated\. You can only set the number of retries to 0 by using the [UpdateJob](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateJob.html) API operation\.

To add this configuration by using the API, specify the [https://docs.aws.amazon.com/iot/latest/apireference/API_jobExecutionsRetryConfig.html](https://docs.aws.amazon.com/iot/latest/apireference/API_jobExecutionsRetryConfig.html) parameter when you run the [https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html) or the [https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJobTemplate.html](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJobTemplate.html) API operation\.

```
{
...
  "jobExecutionsRetryConfig": { 
      "criteriaList": [ 
         { 
            "failureType": "string",
            "numberOfRetries": number
         }
      ]
  }
...
}
```

Where **criteriaList** is an array specifying the list of criteria that determines the number of retries allowed for each failure type for a job\.