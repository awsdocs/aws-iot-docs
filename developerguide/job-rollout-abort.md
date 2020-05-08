# Job rollout and abort configuration<a name="job-rollout-abort"></a>

AWS IoT jobs can be deployed using variable rollout rates as various criteria and thresholds are met\. Job rollouts can also be aborted if the number of failed jobs matches a set of criteria\. These rollout configurations give you more granular control over a job's blast radius\. Job rollout rate criteria are set at job creation through the [https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionsRolloutConfig.html](https://docs.aws.amazon.com/iot/latest/apireference/API_JobExecutionsRolloutConfig.html) object\. Job abort criteria are set at job creation through the [https://docs.aws.amazon.com/iot/latest/apireference/API_AbortConfig.html](https://docs.aws.amazon.com/iot/latest/apireference/API_AbortConfig.html) object\.

## Using job rollout rates<a name="job-rollout-using"></a>

You set a job rollout rate by configuring the [https://docs.aws.amazon.com/iot/latest/apireference/API_ExponentialRolloutRate.html](https://docs.aws.amazon.com/iot/latest/apireference/API_ExponentialRolloutRate.html) property of the `JobExecutionsRolloutConfig` object when you run the [https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html) API\. The following example sets a variable rollout rate by using the `exponentialRate` parameter\.

```
{  
...
  "jobExecutionsRolloutConfig": { 
      "exponentialRate": { 
         "baseRatePerMinute": 50,
         "incrementFactor": 2,
         "rateIncreaseCriteria": { 
            "numberOfNotifiedThings": 1000, // Set one or the other
            "numberOfSucceededThings": 1000 // of these two values.
         },
         "maximumPerMinute": 1000
...
}
```

The `baseRatePerMinute` parameter specifies the rate at which the jobs are executed until the `numberOfNotifiedThings` or `numberOfSucceededThings` threshold has been met\.

The `incrementFactor` parameter specifies the exponential factor by which the rollout rate increases after the `numberOfNotifiedThings` or `numberOfSucceededThings` threshold has been met\.

The `rateIncreaseCriteria` parameter is an object that specifies either the `numberOfNotifiedThings` or `numberOfSucceededThings` threshold\.

The `maximumPerMinute` parameter specifies the upper limit of the rate at which job executions can occur\. Valid values range from 1 to 1000\. This parameter is required when you pass an `ExponentialRate` object\. In a variable rate rollout, this value establishes the upper limit of a job rollout rate\.

A job rollout with the configuration above would start at a rate of 50 job executions per minute\. It would continue at that rate until either 1000 things have received job execution notifications \(if a value for `numberOfNotifiedThings` has been specified\) or 1000 successful job executions have occurred \(if a value for `numberOfSucceededThings` has been specified\)\.

The following table illustrates how the rollout would proceed over the first four increments\.


|  |  |  |  |  | 
| --- |--- |--- |--- |--- |
|  Rollout rate per minute  |  50  |  100  |  200  |  400  | 
|  Number of notified devices or successful executions  |  1000  |  2000  |  3000  |  4000  | 

The following configuration sets a static rollout rate\.

```
{  
...
  "jobExecutionsRolloutConfig": { 
         "maximumPerMinute": 1000
...
}
```

The `maximumPerMinute` parameter specifies the upper limit of the rate at which job executions can occur\. Valid values range from 1 to 1000\. This parameter is optional\. If you don't specify a value, the default value of 1000 is used\.

## Using job rollout abort configurations<a name="job-abort-using"></a>

You set up a job abort condition by configuring the optional [https://docs.aws.amazon.com/iot/latest/apireference/API_AbortConfig.html](https://docs.aws.amazon.com/iot/latest/apireference/API_AbortConfig.html) object when you run the [https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html) API\. This section describes the effect that the following sample configuration would have on a job rollout that was experiencing multiple failed executions\.

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

The `action` parameter specifies the action to take when the abort criteria have been met\. This parameter is required, and `CANCEL` is the only valid value\.

The `failureType` parameter specifies which failure types should trigger a job abort\. Valid values are `FAILED`, `REJECTED`, `TIMED_OUT`, and `ALL`\.

The `minNumberOfExecutedThings` parameter specifies the number of completed job executions that must occur before the service checks to see if the job abort criteria have been met\. In this example, AWS IoT doesn't check to see if a job abort should occur until at least 100 devices have completed job executions\.

The `thresholdPercentage` parameter specifies the total number of executed things that initiate a job abort\. In this example, AWS IoT initiates a job abort and cancels the job rollout if at least 20% of all completed executions have failed in any way after 100 executions have completed\.

**Note**  
Deletion of job executions affects the computation value of the total completed execution\. When a job aborts, the service creates an automated `comment` and `reasonCode` to differentiate a user\-driven cancellation from a job abort cancellation\.