# Authorizing users and cloud services to use AWS IoT Jobs<a name="iam-policy-users-jobs"></a>

To authorize your users and cloud services, you must use IAM policies on both the control plane and data plane\. The policies must be used with HTTPS protocol and must use AWS Signature Version 4 authentication \(port 443\) to authenticate users\.

**Note**  
AWS IoT Core policies must not be used on the control plane\. Only IAM policies are used for authorizing users or Cloud Services\. For more information about using the required policy type, see [Required policy type for AWS IoT Jobs](iot-jobs-security.md#jobs-required-policy)\.

IAM policies are JSON documents that contain policy statements\. Policy statements use *Effect*, *Action*, and *Resource* elements to specify resources, allowed or denied actions, and conditions under which actions are allowed or denied\. For more information, see [IAM JSON Policy Elements Reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_elements.html) in the *IAM User Guide*\.

**Warning**  
We recommend that you don't use wildcard permissions, such as `"Action": ["iot:*"]` in your IAM policies or AWS IoT Core policies\. Using wildcard permissions is not a recommended security best practice\. For more information, see [AWS IoT policies overly permissive](audit-chk-iot-policy-permissive.md)\. 

## IAM policies on the control plane<a name="iam-jobs-control-plane"></a>

On the control plane, IAM policies use the `iot:` prefix with the action to authorize the corresponding jobs API operation\. For example the `iot:CreateJob` policy action grants the user permission to use the [https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html) API\.

### Policy actions<a name="iam-control-plane-actions"></a>

The following table shows a list of IAM policy actions and permissions to use the API actions\. For information about resource types, see [Resource types defined by AWS IoT](https://docs.aws.amazon.com/service-authorization/latest/reference/list_awsiot.html#awsiot-job)\. For more information about AWS IoT actions, see [Actions defined by AWS IoT](https://docs.aws.amazon.com/service-authorization/latest/reference/list_awsiot.html)\.


**IAM policy actions on control plane**  

| Policy action | API operation | Resource types | Description | 
| --- | --- | --- | --- | 
| iot:AssociateTargetsWithJob | [https://docs.aws.amazon.com/iot/latest/apireference/API_AssociateTargetsWithJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_AssociateTargetsWithJob.html) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iam-policy-users-jobs.html)  | Represents the permission to associate a group with a continuous job\. The iot:AssociateTargetsWithJob permission is checked every time a request is made to associate targets\. | 
| iot:CancelJob | [https://docs.aws.amazon.com/iot/latest/apireference/API_CancelJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_CancelJob.html) | job | Represents the permission to cancel a job\. The iot:CancelJob permission is checked every time a request is made to cancel a job\. | 
| iot:CancelJobExecution | [https://docs.aws.amazon.com/iot/latest/apireference/API_CancelJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_CancelJobExecution.html) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iam-policy-users-jobs.html)  | Represents the permission to cancel a job execution\. The iot: CancelJobExecution permission is checked every time a request is made to cancel a job execution\. | 
| iot:CreateJob | [https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJob.html) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iam-policy-users-jobs.html)  | Represents the permission to create a job\. The iot: CreateJob permission is checked every time a request is made to create a job\. | 
| iot:CreateJobTemplate | [https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJobTemplate.html](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateJobTemplate.html) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iam-policy-users-jobs.html)  | Represents the permission to create a job template\. The iot: CreateJobTemplate permission is checked every time a request is made to create a job template\. | 
| iot:DeleteJob | [https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJob.html) | job | Represents the permission to delete a job\. The iot: DeleteJob permission is checked every time a request is made to delete a job\. | 
| iot:DeleteJobTemplate | [https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJobTemplate.html](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJobTemplate.html) | jobtemplate | Represents the permission to delete a job template\. The iot: CreateJobTemplate permission is checked every time a request is made to delete a job template\. | 
| iot:DeleteJobExecution | [https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteJobExecution.html) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iam-policy-users-jobs.html)  | Represents the permission to delete a job execution\. The iot: DeleteJobExecution permission is checked every time a request is made to delete a job execution\. | 
| iot:DescribeJob | [https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeJob.html) | job | Represents the permission to describe a job\. The iot: DescribeJob permission is checked every time a request is made to describe a job\. | 
| iot:DescribeJobExecution | [https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeJobExecution.html) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iam-policy-users-jobs.html)  | Represents the permission to describe a job execution\. The iot: DescribeJobExecution permission is checked every time a request is made to describe a job execution\. | 
| iot:DescribeJobTemplate | [https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeJobTemplate.html](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeJobTemplate.html) | jobtemplate | Represents the permission to describe a job template\. The iot: DescribeJobTemplate permission is checked every time a request is made to describe a job template\. | 
| iot:DescribeManagedJobTemplate | [https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeManagedJobTemplate.html](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeManagedJobTemplate.html) | jobtemplate | Represents the permission to describe a managed job template\. The iot: DescribeManagedJobTemplate permission is checked every time a request is made to describe a managed job template\. | 
| iot:GetJobDocument | [https://docs.aws.amazon.com/iot/latest/apireference/API_GetJobDocument.html](https://docs.aws.amazon.com/iot/latest/apireference/API_GetJobDocument.html) | job | Represents the permission to get the job document for a job\. The iot:GetJobDocument permission is checked every time a request is made to get a job document\. | 
| iot:ListJobExecutionsForJob | [https://docs.aws.amazon.com/iot/latest/apireference/API_ListJobExecutionsForJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_ListJobExecutionsForJob.html) | job | Represents the permission to list the job executions for a job\. The iot:ListJobExecutionsForJob permission is checked every time a request is made to list the job executions for a job\. | 
| iot:ListJobExecutionsForThing | [https://docs.aws.amazon.com/iot/latest/apireference/API_ListJobExecutionsForThing.html](https://docs.aws.amazon.com/iot/latest/apireference/API_ListJobExecutionsForThing.html) | thing | Represents the permission to list the job executions for a job\. The iot:ListJobExecutionsForThing permission is checked every time a request is made to list the job executions for a thing\. | 
| iot:ListJobs | [https://docs.aws.amazon.com/iot/latest/apireference/API_ListJobs.html](https://docs.aws.amazon.com/iot/latest/apireference/API_ListJobs.html) | none | Represents the permission to list the jobs\. The iot:ListJobs permission is checked every time a request is made to list the jobs\. | 
| iot:ListJobTemplates | [https://docs.aws.amazon.com/iot/latest/apireference/API_ListJobTemplates.html](https://docs.aws.amazon.com/iot/latest/apireference/API_ListJobTemplates.html) | none | Represents the permission to list the job templates\. The iot:ListJobTemplates permission is checked every time a request is made to list the job templates\. | 
| iot:ListManagedJobTemplates | [https://docs.aws.amazon.com/iot/latest/apireference/API_ListManagedJobTemplates.html](https://docs.aws.amazon.com/iot/latest/apireference/API_ListManagedJobTemplates.html) | none | Represents the permission to list the managed job templates\. The iot:ListManagedJobTemplates permission is checked every time a request is made to list the managed job templates\. | 
| iot:UpdateJob | [https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateJob.html](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateJob.html) | job | Represents the permission to update a job\. The iot:UpdateJob permission is checked every time a request is made to update a job\. | 
| iot:TagResource | [https://docs.aws.amazon.com/iot/latest/apireference/API_TagResource.html](https://docs.aws.amazon.com/iot/latest/apireference/API_TagResource.html) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iam-policy-users-jobs.html)  | Grants permission to tag a specific resource\. | 
| iot:UntagResource | [https://docs.aws.amazon.com/iot/latest/apireference/API_UntagResource.html](https://docs.aws.amazon.com/iot/latest/apireference/API_UntagResource.html) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot/latest/developerguide/iam-policy-users-jobs.html)  | Grants permission to untag a specific resource\. | 

### Basic IAM policy example<a name="iam-control-plane-example"></a>

The following shows an example of an IAM policy that allows the user permission to perform the following actions for your IoT thing and thing group\. 

In the example, replace:
+ *region* with your AWS Region, such as `us-east-1`\.
+ *account\-id* with your AWS account number, such as `57EXAMPLE833`\.
+ *thing\-group\-name* with the name of your IoT thing group for which you're targeting jobs, such as `FirmwareUpdateGroup`\.
+ *thing\-name* with the name of your IoT thing for which you're targeting jobs, such as `MyIoTThing`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "iot:CreateJobTemplate",
                "iot:CreateJob",
                ],
            "Effect": "Allow",
            "Resource": "arn:aws:iot:region:account-id:thinggroup/thing-group-name"
        },
        {
            "Action": [
                "iot:DescribeJob",
                "iot:CancelJob",
                "iot:DeleteJob",
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:iot:region:account-id:job/*"
        },
        {
            "Action": [
                "iot:DescribeJobExecution",
                "iot:CancelJobExecution",
                "iot:DeleteJobExecution",
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:iot:region:account-id:thing/thing-name"
                "arn:aws:iot:region:account-id:job/*"
            ]
        }
    ]
}
```

## IAM policies on the data plane<a name="iam-jobs-data-plane"></a>

IAM policies on the data plane use the `iotjobsdata:` prefix to authorize jobs API operations that users can perform\. On the data plane, you can grant a user permission to use the [https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_DescribeJobExecution.html](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_DescribeJobExecution.html) API by using the `iotjobsdata:DescribeJobExecution` policy action\.

**Warning**  
Using IAM policies on the data plane is not recommended when targeting AWS IoT Jobs for your devices\. We recommend that you use IAM policies on the control plane for users to create and manage jobs\. On the data plane, for authorizing devices to retrieve job executions and update the execution status, use [AWS IoT Core policies for HTTPS protocol](iot-data-plane-jobs.md#iot-jobs-data-http)\.

### Basic IAM policy example<a name="iam-data-plane-example"></a>

The API operations that must be authorized are usually performed by a user typing CLI commands\. The following shows an example of a user performing a `DescribeJobExecution` operation\.

In the example, replace:
+ *region* with your AWS Region, such as `us-east-1`\.
+ *account\-id* with your AWS account number, such as `57EXAMPLE833`\.
+ *thing\-name* with the name of your IoT thing for which you're targeting jobs, such as `myRegisteredThing`\.
+ `job-id` is the unique identifier for the job that's targeted using the API\.

```
aws iot-jobs-data describe-job-execution \ 
    --endpoint-url "https://account-id.jobs.iot.region.amazonaws.com" \ 
    --job-id jobID --thing-name thing-name
```

The following shows a sample IAM policy that authorizes this action:

```
{
    "Version": "2012-10-17",
    "Statement": 
    {
        "Action": ["iotjobsdata:DescribeJobExecution"],
        "Effect": "Allow",
        "Resource": "arn:aws:iot:region:account-id:thing/thing-name",
    }
   }
```

### IAM policy example for both control plane and data plane<a name="iam-data-plane-example2"></a>

If a user performs an API operation on both the control plane and data plane, your control plane policy action must use the `iot:` prefix, and your data plane policy action must use the `iotjobsdata:` prefix\. 

For example, the `DescribeJobExecution` API can be used in both the control plane and data plane\. On the control plane, the [DescribeJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeJobExecution.html) API is used to describe a job execution\. On the data plane, the [ DescribeJobExecution](https://docs.aws.amazon.com/iot/latest/apireference/API_iot-jobs-data_DescribeJobExecution.html) API is used to get details of a job execution\.

The following IAM policy authorizes a user permission to use the `DescribeJobExecution` API on both the control plane and data plane\.

In the example, replace:
+ *region* with your AWS Region, such as `us-east-1`\.
+ *account\-id* with your AWS account number, such as `57EXAMPLE833`\.
+ *thing\-name* with the name of your IoT thing for which you're targeting jobs, such as `MyIoTThing`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": ["iotjobsdata:DescribeJobExecution"],
            "Effect": "Allow",
            "Resource": "arn:aws:iot:region:account-id:thing/thing-name"
        },
        {
            "Action": [
                "iot:DescribeJobExecution",
                "iot:CancelJobExecution",
                "iot:DeleteJobExecution",
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:iot:region:account-id:thing/thing-name"
                "arn:aws:iot:region:account-id:job/*"
            ]
        }
    ]            
}
```

## Authorize tagging of IoT resources<a name="tagging-iot-jobs"></a>

For better control over jobs and job templates that a user can create, modify, or use, you can attach tags to the jobs or job templates\. Tags also help you discern ownership and assign and allocate costs by placing them in billing groups and attaching tags to them\.

When a user wants to tag their jobs or job templates that they created by using the AWS Management Console or the AWS CLI, your IAM policy must grant the user permissions to tag them\. To grant permissions, your IAM policy must use the `iot:TagResource` action\.

For general information about tagging your resources, see [Tagging your AWS IoT resources](tagging-iot.md)\.

### IAM policy example<a name="iam-jobs-tags-example"></a>

For an example that shows how to grant tagging permissions, consider a user that runs the following command to create a job and tag it to a specific environment\.

In the example, replace:
+ *region* with your AWS Region, such as `us-east-1`\.
+ *account\-id* with your AWS account number, such as `57EXAMPLE833`\.
+ *thing\-name* with the name of your IoT thing for which you're targeting jobs, such as `MyIoTThing`\.

```
aws iot create-job 
    --job-id test_job 
    --targets "arn:aws:iot:region:account-id:thing/thingOne"
    --document-source "https://s3.amazonaws.com/my-s3-bucket/job-document.json"
    --description "test job description" 
    --tags Key=environment,Value=beta
```

For this example, you must use the following IAM policy:

```
{
    "Version": "2012-10-17",
    "Statement": 
    {
        "Action": [ "iot:CreateJob", "iot:CreateJobTemplate", "iot:TagResource" ],
        "Effect": "Allow",
        "Resource": [
            "arn:aws:iot:aws-region:account-id:job/*",
            "arn:aws:iot:aws-region:account-id:jobtemplate/*"
        ]
    }
}
```