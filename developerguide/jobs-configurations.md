# Job configurations<a name="jobs-configurations"></a>

You can have the following additional configurations for each job that you deploy to the specified targets\.
+ **Rollout**: This configuration defines how many devices receive the job document every minute\.
+ **Abort**: Use this configuration to cancel a job in cases such as when some devices don't receive the job notification or your devices report failure for their job executions\.
+ **Timeout**: If there isn't a response from your job targets within a certain duration after their job executions have started, the job can fail\.
+ **Retry**: If your device reports failure when attempting to complete a job execution or if your job execution times out, you can use this configuration to retry the job execution for your device\.

By using these configurations, you can monitor the status of your job execution and avoid a bad update from being sent to an entire fleet\.

**Topics**
+ [How job configurations work](jobs-configurations-details.md)
+ [Specify additional configurations](jobs-configurations-specify.md)