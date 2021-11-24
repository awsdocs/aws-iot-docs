# Job configurations<a name="jobs-configurations"></a>

You can have the following additional configurations for each job that you deploy to the specified targets\.
+ **Rollout**: This configuration defines how many devices receive the job document every minute\.
+ **Abort**: If a certain number of devices don't receive the job document, use this configuration to cancel the job and avoid sending a bad update to an entire fleet\.
+ **Timeout**: If a response is not received from your job targets within a certain duraton, the job can fail\. You can keep track of the job that's running on these devices\.

By using these configurations, you can track and monitor the status of your job execution and avoid a bad update from being sent to an entire fleet\. The rollout and abort configurations are related to your job whereas the timeout configuration is related to an execution of the job\.

## Specify additional configurations<a name="jobs-configurations-specify"></a>

You can specify these additional configurations when creating a job or a job template by using the console or the Jobs API\.

**Topics**
+ [Specify additional configurations](#jobs-configurations-specify)
+ [Job rollout and abort configuration](job-rollout-abort.md)
+ [Job executions timeout configuration](job-timeout-retry.md)