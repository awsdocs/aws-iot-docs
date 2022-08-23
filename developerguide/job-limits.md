# Job limits<a name="job-limits"></a>

AWS IoT Jobs has Service quotas, or limits, that correspond to the maximum number of service resources or operations for your AWS account\.

## Active and concurrent job limits<a name="job-limits-active-concurrent"></a>

This section will help you learn more about active and concurrent jobs and the limits that apply to them\.

**Active jobs and active job limit**  
When you create a job by using the AWS IoT console or the `CreateJob` API, the job status changes to `IN_PROGRESS`\. All in\-progress jobs are *active jobs* and count towards the active jobs limit\. This includes jobs that are either rolling out new job executions, or jobs that are waiting for devices to complete their job executions\. This limit applies to both continuous and snapshot jobs\.

**Concurrent jobs and job concurrency limit**  
In\-progress jobs that are either rolling out new job executions, or jobs that are canceling previously created job executions are *concurrent jobs* and count towards the job concurrency limit\. As AWS IoT Jobs can roll out and cancel job executions swiftly at a rate of 1000 devices per minute, each job is `concurrent` and counts towards the job concurrency limit only for a short time\. After the job executions have been rolled out or canceled, the job is no longer concurrent and does not count towards the job concurrency limit\. You can use the job concurrency to create a large number of jobs while waiting for devices to complete the job execution\.

To determine whether a job is concurrent, you can use the `IsConcurrent` property of a job from the AWS IoT console, or by using the `DescribeJob` or `ListJob` API\. This limit applies to both continuous and snapshot jobs\.

To view the active jobs and job concurrency limits and other AWS IoT Jobs quotas for your AWS account and to request a limit increase, see [AWS IoT Device Management endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/iot_device_management.html#job-limits) in the AWS General Reference\.

The following diagram shows how the job concurrency applies to in\-progress jobs and jobs that are being canceled\.

![\[Image showing the different states of an AWS IoT job.\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/job-states-concurrency.png)

The following table shows the limits that apply to active and concurrent jobs and the concurrent and non\-concurrent phases of the job states\.


**Active and concurrent job limits**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/job-limits.html)