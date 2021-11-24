# Step 2: Create and run the job in AWS IoT<a name="iot-dc-runjobs-prepare-define"></a>

The procedures in this section create a job document and an AWS IoT job resource\. After you create the job resource, AWS IoT sends the job document to the specified job targets on which a jobs agent applies the job document to the device or client\.

**Topics**
+ [Create and store the job's job document](#iot-dc-runjobs-prepare-define-jobdoc)
+ [Run a job in AWS IoT for one IoT device](#iot-dc-runjobs-prepare-define-job)

## Create and store the job's job document<a name="iot-dc-runjobs-prepare-define-jobdoc"></a>

This procedure creates a simple job document to include in an AWS IoT job resource\. This job document displays "Hello world\!" on the job target\.

**To create and store a job document:**

1. Select the Amazon S3 bucket into which you'll save your job document\. If you don't have an existing Amazon S3 bucket to use for this, you'll need to create one\. For information about how to create Amazon S3 buckets, see the topics in [Getting started with Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/GetStartedWithS3.html)\.

1. Create and save the job document for this job

   1. On your local host computer, open a text editor\.

   1. Copy and paste this text into the editor\.

      ```
      {
          "operation": "echo",
          "args": ["Hello world!"]
      }
      ```

   1. On the local host computer, save the contents of the editor to a file named **hello\-world\-job\.json**\.

   1. Confirm the file was saved correctly\. Some text editors automatically append `.txt` to the file name when they save a text file\. If your editor appended `.txt` to the file name, correct the file name before proceeding\.

1. Replace the *path\_to\_file* with the path to **hello\-world\-job\.json**, if it's not in your current directory, replace *s3\_bucket\_name* with the Amazon S3 bucket path to the bucket you selected, and then run this command to put your job document into the Amazon S3 bucket\.

   ```
   aws s3api put-object \
   --key hello-world-job.json \
   --body path_to_file/hello-world-job.json --bucket s3_bucket_name
   ```

   The job document URL that identifies the job document that you stored in Amazon S3 is determined by replacing the *s3\_bucket\_name* and *AWS\_region* in the following URL\. Record the resulting URL to use later as the *job\_document\_path*

   ```
   https://s3_bucket_name.s3.AWS_Region.amazonaws.com/hello-world-job.json
   ```
**Note**  
AWS security prevents you from being able to open this URL outside of your AWS account, for example by using a browser\. The URL is used by the AWS IoT jobs engine, which has access to the file, by default\. In a production environment, you'll need to make sure that your AWS IoT services have permission to access to the job documents stored in Amazon S3\.

After you have saved the job document's URL, continue to [Run a job in AWS IoT for one IoT device](#iot-dc-runjobs-prepare-define-job)\.

## Run a job in AWS IoT for one IoT device<a name="iot-dc-runjobs-prepare-define-job"></a>

The procedures in this section start the AWS IoT Device Client on your Raspberry Pi to run the jobs agent on the device to wait for jobs to run\. It also creates a job resource in AWS IoT, which will send the job to and run on your IoT device\.

**Note**  
This procedure runs a job on only a single device\.

**To start the jobs agent on your Raspberry Pi:**

1. In the terminal window on your local host computer that's connected to your Raspberry Pi, run this command to start the AWS IoT Device Client\.

   ```
   cd ~/aws-iot-device-client/build
   ./aws-iot-device-client --config-file ~/dc-configs/dc-jobs-config.json
   ```

1. In the terminal window, confirm that the AWS IoT Device Client and displays these messages

   ```
   2021-11-15T18:45:56.708Z [INFO]  {Main.cpp}: Jobs is enabled
                         .
                         .
                         .
   2021-11-15T18:45:56.708Z [INFO]  {Main.cpp}: Client base has been notified that Jobs has started
   2021-11-15T18:45:56.708Z [INFO]  {JobsFeature.cpp}: Running Jobs!
   2021-11-15T18:45:56.708Z [DEBUG] {JobsFeature.cpp}: Attempting to subscribe to startNextPendingJobExecution accepted and rejected
   2021-11-15T18:45:56.708Z [DEBUG] {JobsFeature.cpp}: Attempting to subscribe to nextJobChanged events
   2021-11-15T18:45:56.708Z [DEBUG] {JobsFeature.cpp}: Attempting to subscribe to updateJobExecutionStatusAccepted for jobId +
   2021-11-15T18:45:56.738Z [DEBUG] {JobsFeature.cpp}: Ack received for SubscribeToUpdateJobExecutionAccepted with code {0}
   2021-11-15T18:45:56.739Z [DEBUG] {JobsFeature.cpp}: Attempting to subscribe to updateJobExecutionStatusRejected for jobId +
   2021-11-15T18:45:56.753Z [DEBUG] {JobsFeature.cpp}: Ack received for SubscribeToNextJobChanged with code {0}
   2021-11-15T18:45:56.760Z [DEBUG] {JobsFeature.cpp}: Ack received for SubscribeToStartNextJobRejected with code {0}
   2021-11-15T18:45:56.776Z [DEBUG] {JobsFeature.cpp}: Ack received for SubscribeToStartNextJobAccepted with code {0}
   2021-11-15T18:45:56.776Z [DEBUG] {JobsFeature.cpp}: Ack received for SubscribeToUpdateJobExecutionRejected with code {0}
   2021-11-15T18:45:56.777Z [DEBUG] {JobsFeature.cpp}: Publishing startNextPendingJobExecutionRequest
   2021-11-15T18:45:56.785Z [DEBUG] {JobsFeature.cpp}: Ack received for StartNextPendingJobPub with code {0}
   2021-11-15T18:45:56.785Z [INFO]  {JobsFeature.cpp}: No pending jobs are scheduled, waiting for the next incoming job
   ```

1. In the terminal window, after you see this message, continue to the next procedure and create the job resource\. Note that it might not be the last entry in the list\.

   ```
   2021-11-15T18:45:56.785Z [INFO]  {JobsFeature.cpp}: No pending jobs are scheduled, waiting for the next incoming job
   ```

**To create an AWS IoT job resource**

1. On your local host computer:

   1. Replace *job\_document\_url* with the job document URL from [Create and store the job's job document](#iot-dc-runjobs-prepare-define-jobdoc)\.

   1. Replace *thing\_arn* with the ARN of the thing resource you created for your device and then run this command\.

      ```
      aws iot create-job \
      --job-id hello-world-job-1 \
      --document-source "job_document_url" \
      --targets "thing_arn" \
      --target-selection SNAPSHOT
      ```

      If successful, the command returns a result like this one\.

      ```
      {
        "jobArn": "arn:aws:iot:us-west-2:57EXAMPLE833:job/hello-world-job-1",
        "jobId": "hello-world-job-1"
      }
      ```

1. In the terminal window, you should see output from the AWS IoT Device Client like this\.

   ```
   2021-11-15T18:02:26.688Z [INFO]  {JobsFeature.cpp}: No pending jobs are scheduled, waiting for the next incoming job
   2021-11-15T18:10:24.890Z [DEBUG] {JobsFeature.cpp}: Job ids differ
   2021-11-15T18:10:24.890Z [INFO]  {JobsFeature.cpp}: Executing job: hello-world-job-1
   2021-11-15T18:10:24.890Z [DEBUG] {JobsFeature.cpp}: Attempting to update job execution status!
   2021-11-15T18:10:24.890Z [DEBUG] {JobsFeature.cpp}: Not including stdout with the status details
   2021-11-15T18:10:24.890Z [DEBUG] {JobsFeature.cpp}: Not including stderr with the status details
   2021-11-15T18:10:24.890Z [DEBUG] {JobsFeature.cpp}: Assuming executable is in PATH
   2021-11-15T18:10:24.890Z [INFO]  {JobsFeature.cpp}: About to execute: echo Hello world!
   2021-11-15T18:10:24.890Z [DEBUG] {Retry.cpp}: Retryable function starting, it will retry until success
   2021-11-15T18:10:24.890Z [DEBUG] {JobsFeature.cpp}: Created EphermalPromise for ClientToken 3TEWba9Xj6 in the updateJobExecution promises map
   2021-11-15T18:10:24.890Z [DEBUG] {JobEngine.cpp}: Child process now running
   2021-11-15T18:10:24.890Z [DEBUG] {JobEngine.cpp}: Child process about to call execvp
   2021-11-15T18:10:24.890Z [DEBUG] {JobEngine.cpp}: Parent process now running, child PID is 16737
   2021-11-15T18:10:24.891Z [DEBUG] {16737}: Hello world!
   2021-11-15T18:10:24.891Z [DEBUG] {JobEngine.cpp}: JobEngine finished waiting for child process, returning 0
   2021-11-15T18:10:24.891Z [INFO]  {JobsFeature.cpp}: Job exited with status: 0
   2021-11-15T18:10:24.891Z [INFO]  {JobsFeature.cpp}: Job executed successfully!
   2021-11-15T18:10:24.891Z [DEBUG] {JobsFeature.cpp}: Attempting to update job execution status!
   2021-11-15T18:10:24.891Z [DEBUG] {JobsFeature.cpp}: Not including stdout with the status details
   2021-11-15T18:10:24.891Z [DEBUG] {JobsFeature.cpp}: Not including stderr with the status details
   2021-11-15T18:10:24.892Z [DEBUG] {Retry.cpp}: Retryable function starting, it will retry until success
   2021-11-15T18:10:24.892Z [DEBUG] {JobsFeature.cpp}: Created EphermalPromise for ClientToken GmQ0HTzWGg in the updateJobExecution promises map
   2021-11-15T18:10:24.905Z [DEBUG] {JobsFeature.cpp}: Ack received for PublishUpdateJobExecutionStatus with code {0}
   2021-11-15T18:10:24.905Z [DEBUG] {JobsFeature.cpp}: Removing ClientToken 3TEWba9Xj6 from the updateJobExecution promises map
   2021-11-15T18:10:24.905Z [DEBUG] {JobsFeature.cpp}: Success response after UpdateJobExecution for job hello-world-job-1
   2021-11-15T18:10:24.917Z [DEBUG] {JobsFeature.cpp}: Ack received for PublishUpdateJobExecutionStatus with code {0}
   2021-11-15T18:10:24.918Z [DEBUG] {JobsFeature.cpp}: Removing ClientToken GmQ0HTzWGg from the updateJobExecution promises map
   2021-11-15T18:10:24.918Z [DEBUG] {JobsFeature.cpp}: Success response after UpdateJobExecution for job hello-world-job-1
   2021-11-15T18:10:25.861Z [INFO]  {JobsFeature.cpp}: No pending jobs are scheduled, waiting for the next incoming job
   ```

1. While the AWS IoT Device Client is running and waiting for a job, you can submit another job by changing the `job-id` value and re\-running the create\-job from Step 1\.

When you’re done running jobs, in the terminal window, enter ^C \(control\-C\) to stop the AWS IoT Device Client\.