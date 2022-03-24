# Create a job from AWS managed templates by using the AWS CLI<a name="job-template-manage-cli-create"></a>

Use the AWS CLI to get information about AWS managed templates and create a job by using these templates\. You can further save the job as a template and then create your own custom template\.

## Get details about managed templates<a name="job-template-manage-cli-get"></a>

The following AWS CLI command gets details about a specified job template\. Specify the job template name and an optional template version\. If the template version is not specified, the predefined, default version is returned\.

```
aws iot describe-managed-job-template \
      --template-name template-name
```

The command displays the following output\.

```
{
   "description": "string",
   "document": "string",
   "documentParameters": [ 
      { 
         "description": "string",
         "example": "string",
         "key": "string",
         "optional": boolean,
         "regex": "string"
      }
   ],
   "environments": [ "string" ],
   "templateArn": "string",
   "templateName": "string",
   "templateVersion": "string"
}
```

For more information, see [DescribeManagedJobTemplate](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeManagedJobTemplate.html)\. 

## List managed templates<a name="job-template-manage-cli-list"></a>

The following AWS CLI command lists all of the job templates in your AWS account\.

```
 aws iot list-managed-job-templates
```

The command displays the following output\.

```
{
   "managedJobTemplates": [ 
      { 
         "description": "string",
         "environments": [ "string" ],
         "templateArn": "string",
         "templateName": "string",
         "templateVersion": "string"
      }
   ],
   "nextToken": "string"
}
```

To retrieve an additional set of results, use the value of the `nextToken` field\. For more information, see [ListManagedJobTemplates](https://docs.aws.amazon.com/iot/latest/apireference/API_ListManagedJobTemplates.html)\.

## Create a job by using managed templates<a name="job-template-manage-cli-create-job"></a>

The following AWS CLI command creates a job from a job template\. It targets a device named `thingOne` and specifies the Amazon Resource Name \(ARN\) of the managed template to use as the basis for the job\. You can override advanced configurations, such as timeout and cancel configurations, by passing the associated parameters of the `create-job` command\.

```
aws iot create-job \ 
      --targets arn:aws:iot:region:123456789012:thing/thingOne  \
      --job-template-arn arn:aws:iot:region::jobtemplate/managed-template-name
```

where:
+ *region* is the AWS Region and,
+ *managed\-template\-name* is the name of the managed template, such as `AWS-Restart-Application:1.0`\.

## Create a custom job template from managed templates<a name="job-template-manage-cli-create-jobtemplate"></a>

1. Create a job using a managed template as described in the previous section\.

1. Create a custom job template by using the ARN of the job that you created\. For more information, see [Create a job template from an existing job](job-templates-cli.md#job-templates-cli-create-from-job)\.