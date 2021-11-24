# Job templates<a name="job-templates"></a>

Use job templates to preconfigure jobs that you can deploy to multiple sets of target devices\. For frequently performed remote actions that you want to deploy to your devices, such as rebooting or installing an application, you can use templates to define standard configurations\. When you want to perform operations such as deploying security patches and bug fixes, you can also create templates from existing jobs\.

When creating a job template, you can specify the following additional configurations and resources\.
+ Job properties
+ Job documents and targets
+ Rollout and cancel criteria
+ Timeout criteria

## Custom and AWS managed templates<a name="job-templates-types"></a>

Depending on the remote action that you want to perform, you can either create a custom job template or use an AWS managed template\. Use custom job templates to provide your own custom job document and create reusable jobs to deploy to your devices\. AWS managed templates are job templates provided by AWS IoT Jobs for commonly performed actions\. These templates have a predefined job document for some remote actions so you don't have to create your own job document\. Managed templates help you to create reusable jobs for faster execution to your devices\.

**Topics**
+ [Custom and AWS managed templates](#job-templates-types)
+ [Deploy common remote operations by using AWS managed templates](job-templates-managed.md)
+ [Create custom job templates](job-templates-create.md)