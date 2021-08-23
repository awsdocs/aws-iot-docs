# Mitigation action commands<a name="mitigation-action-commands"></a>

You can use these mitigation action commands to define a set of actions for your AWS account that you can later apply to one or more sets of audit findings\. There are three command categories: 
+ Those used to define and manage actions\.
+ Those used to start and manage the application of those actions to Audit findings\.
+ Those used to start and manage the application of those actions to Detect alarms\.


**Mitigation action commands**  

|  Define and manage actions  |  Start and manage Audit execution  |  Start and manage Detect execution  | 
| --- | --- | --- | 
|  [CreateMitigationAction](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateMitigationAction.html)  |  [CancelAuditMitigationActionsTask](https://docs.aws.amazon.com/iot/latest/apireference/API_CancelAuditMitigationActionsTask.html)  |  [CancelDetectMitigationActionsTask](https://docs.aws.amazon.com/iot/latest/apireference/API_CancelDetectMitigationActionsTask4e.html)   | 
|  [DeleteMitigationAction](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteMitigationAction.html)  |  [DescribeAuditMitigationActionsTask](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeAuditMitigationActionsTask.html)  |  [DescribeDetectMitigationActionsTask](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeDetectMitigationActionsTask.html)  | 
|  [DescribeMitigationAction](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeMitigationAction.html)  |  [ListAuditMitigationActionsTasks](https://docs.aws.amazon.com/iot/latest/apireference/API_ListAuditMitigationActionsTasks.html)  |  [ListDetectMitigationActionsTasks](https://docs.aws.amazon.com/iot/latest/apireference/API_ListDetectMitigationActionsTasks.html)  | 
|  [ListMitigationActions](https://docs.aws.amazon.com/iot/latest/apireference/API_ListMitigationActions.html)  |  [StartAuditMitigationActionsTask](https://docs.aws.amazon.com/iot/latest/apireference/API_StartAuditMitigationActionsTask.html)  | [StartDetectMitigationActionsTask](https://docs.aws.amazon.com/iot/latest/apireference/API_StartDetectMitigationActionsTask.html) | 
|  [UpdateMitigationAction](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateMitigationAction.html)  |  [ListAuditMitigationActionsExecutions](https://docs.aws.amazon.com/iot/latest/apireference/API_ListAuditMitigationActionsExecutions.html)  |  [ListDetectMitigationActionsExecutions](https://docs.aws.amazon.com/iot/latest/apireference/API_ListDetectMitigationActionsExecutions.html)  | 