# ML Detect guide<a name="dd-detect-ml-getting-started"></a>


|  | 
| --- |
| ML Detect is in preview release for AWS IoT Device Defender and is subject to change\. | 

In this Getting Started guide, you create an ML Detect Security Profile that uses machine learning \(ML\) to create models of expected behavior based on historical metric data from your devices\. While ML Detect is creating the ML model, you can monitor its progress\. Once the ML model is built, you can view and investigate alarms on an ongoing basis and mitigate identified issues\.

For more information about ML Detect and its API and CLI commands, see [ML Detect reference](dd-detect-ml.md)\.

**Topics**
+ [Prerequisites](#ml-detect-prereqs)
+ [How to use ML Detect in the console](#dd-detect-ml-console)
+ [How to use ML Detect on the CLI](#dd-detect-ml-cli)

## Prerequisites<a name="ml-detect-prereqs"></a>
+ An AWS account\. If you don't have this, see [Setting up](https://docs.aws.amazon.com/iot/latest/developerguide/dd-setting-up.html)\.

## How to use ML Detect in the console<a name="dd-detect-ml-console"></a>

**Topics**
+ [Enable ML Detect](#enable-ml-detect-console)
+ [Monitor your ML model status](#monitor-ml-models-console)
+ [Review your ML Detect alarms](#review-ml-alarms-console)
+ [Fine\-tune your ML alarms](#fine-tune-ml-models-console)
+ [Mitigate identified device issues](#mitigate-ml-issues-console)

### Enable ML Detect<a name="enable-ml-detect-console"></a>

The following procedures detail how to set up ML Detect in the console\.

1. First, make sure your devices will create the minimum datapoints required as defined in [ML Detect minimum requirements](dd-detect-ml.md#dd-detect-ml-requirements) for ongoing training and refreshing of the model\. For data collection to progress, ensure your Security Profile is attached to a target \(for example, a thing group\)\.

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend**\. Choose **Detect**, **Security profiles**, **Create security profile**, and then **Create ML anomaly Detect profile**\.

1. On the **Set basic configurations** page, do the following\.
   + Under **Target**, choose your target device groups\.
   + Under **Security profile name**, enter a name for your Security Profile\.
   + Under **Device behavior metrics**, choose the metrics you'd like to monitor\.
   + \(Optional\) If you don't want to customize your metric configurations, select **Skip edit metric behaviors** and then select **Next**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dd-ml-set-basic.png)

   When you're done, choose **Next**\.

1. On the **Update configurations** page, you can optionally customize your ML behavior settings\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dd-ml-update-config.png)

1. When you're done, choose **Next**\.

1. On the **Review metric behaviors** screen, verify the behaviors you'd like machine learning to monitor\.

   \(Optional\) If you don't want to enable SNS, select **Skip next step and finish**\.

   When you're done, choose **Next**\.

1. \(Optional\) On the **Set SNS \(optional\)** page, specify an SNS topic for alarms when a device violates a behavior in your profile\. Select an IAM role you will use to publish on the selected SNS topic\.

   If you don't have an SNS role yet, use the following steps to create a role with the proper permissions and trust relationships required\. 
   + Navigate to the [IAM console](https://console.aws.amazon.com/iam/)\. In the navigation pane, choose **Roles** and then choose **Create role**\.
   + Under **Select type of trusted entity**, select **AWS Service**\. Then, under **Choose a use case**, choose **IoT** and under **Select your use case**, choose **IoT \- Device Defender Mitigation Actions**\. Choose **Next: Permissions** when done\.
   + Under **Attached permissions policies**, ensure that **AWSIoTDeviceDefenderPublishFindingsToSNSMitigationAction** is selected then choose **Next: Tags**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dd-ml-sns-findings.png)
   + Under **Add tags \(optional\)**, you can add any tags you'd like to associate with your role\. Choose **Next: Review** when done\.
   + Under **Review**, give your role a name and ensure that **AWSIoTDeviceDefenderPublishFindingsToSNSMitigationAction** is listed under **Permissions** and **AWS service: iot\.amazonaws\.com** is listed under **Trust relationships**\. Choose **Create role** when done\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dd-ml-detect-permissions.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dd-ml-detect-trust-relationships.png)

1. After you've created your Security Profile, you're redirected to the **Security Profiles** page, where newly created Security Profile is displayed\.
**Note**  
The initial ML model training and creation takes 14 days to complete\. You can expect to see alarms after it's complete if there is any anomalous activity on your devices\.

### Monitor your ML model status<a name="monitor-ml-models-console"></a>

While your ML models are in the initial training period, you can monitor their progress at any time by taking the following steps\.

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend**, and then choose **Detect**, **Security profiles**\.

1. On the **Security Profiles** page, select the check box next to the Security Profile you'd like to review\. Then, choose **Other actions** and **View ML model training report**\.

1. On the **ML model training report** page, check the training progress of your ML models\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dd-ml-model-training.png)

   Once your model status is **Active**, it'll start making Detect decisions based on your usage and update the profile every day\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dd-ml-active-state.png)

**Note**  
If your model doesn't progress as expected, make sure your devices are meeting the [Minimum requirements](dd-detect-ml.md#dd-detect-ml-requirements)\.

### Review your ML Detect alarms<a name="review-ml-alarms-console"></a>

After your ML models are built and ready for data inference, you can regularly view and investigate alarms that are identified by the models\.

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend**, and then choose **Detect**, **Alarms**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dd-ml-alarms.png)

1. If you navigate to the **History** tab and scroll down, you can view details about your alarms and their state\.

   Here, you can see thing `ddml12` received too many messages and cleared subsequently, which cleared the alarm state\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dd-ml-history-alarm.png)

   To dive deeper, choose the **Thing name** you'd like to see more details for\. Here, we select `ddml12` and then navigate to **Defender metrics**\. You can access the **Defender metrics graph** and perform your investigation on any thing in alarm from the **Active** tab\. In this case, the graph shows a spike in messages received, which triggered the alarm\. However, the alarm cleared subsequently\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dd-ml-defender-metrics.png)

### Fine\-tune your ML alarms<a name="fine-tune-ml-models-console"></a>

After your ML models are built, you can update your Security Profile's ML behavior settings by using the following steps\.

1. In the [AWS IoT console](https://console.aws.amazon.com/iot), in the navigation pane, expand **Defend**, and then choose **Detect**, **Security profiles**\.

1. On the **Security Profiles** page, select the check box next to the Security Profile you'd like to review\. Then, choose **Actions**, **Edit**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dd-ml-fine-tune.png)

1. Under **Set basic configurations**, you can change what metrics you want to monitor as well as attach them to other things or thing groups\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dd-ml-set-basic.png)

1. You can update any of the following by navigating to **Edit metric behaviors \(optional\)**\.
   + Your ML model datapoints required to trigger alarm
   + Your ML model datapoints required to clear alarm
   + Your ML Detect confidence level
   + Your ML Detect notifications \(for example, **On**, **Suppressed**\)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dd-ml-update-config-2.png)

### Mitigate identified device issues<a name="mitigate-ml-issues-console"></a>

1. *\(Optional\)* Before setting up quarantine mitigation actions, let's set up a quarantine group where we'll move the device in violation to\. You can use any existing group if you have it\.

1. Navigate to **Manage**, **Thing groups**, and then **Create Thing Group**\. Name your thing group\. For this tutorial, we'll name our thing group `Quarantine_group`\. Under **Thing group**, **Security**, apply the following policy to the thing group\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Deny",
         "Action": "iot:*",
         "Resource": "*",
       }
     ]
   }
   ```  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dd-create-thing-group.png)

   Choose **Create thing group** when done\.

1. Now that we've created a thing group, let's create a mitigation action that move devices in alarm into `Quarantine_group`\.

   Under **Defend**, **Mitigation actions**, choose **Create**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dd-miti-create.png)

1. On the **Create a new mitigation action** screen, fill out the following\.
   + **Action name**: Give your mitigation action a name, such as **Quarantine\_action**\.
   + **Action type**: Select the type of action\. We'll select **Add things to thing group \(Audit or Detect mitigation\)**\.
   + **Action execution role**: Create a role or select an existing role if you created one earlier\.
   + **Parameters**: Select a thing group\. We can use `Quarantine_group`, which we created earlier\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dd-miti-create-form.png)

   Once you're done, choose **Save**\. You'll now have a mitigation action that moves devices in alarm to a quarantine thing group and a mitigation action to isolate the device while you investigate\.

1. Navigate to **Defender**, **Detect**, **Alarms**\. You'll be able to see which devices are in alarm state under **Active**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dd-published-alarms.png)

   Select the device you want to move to the quarantine group and choose **Start Mitigation Actions**\.

1. Under **Start mitigation actions**, **Start Actions** select the mitigation action you created earlier\. For example, we'll choose **Quarantine\_action**\. Then choose **Start**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dd-ml-start-action.png)

1. You'll be redirected to **Action tasks**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/dd-ml-action-tasks.png)

   The device is now isolated in **Quarantine\_group** and you can investigate the root cause of the issue that set off the alarm\. Once the investigation is complete, you can always move the device out of the thing group or take further actions\.

## How to use ML Detect on the CLI<a name="dd-detect-ml-cli"></a>

The following shows you how to set up ML Detect using the CLI\.

**Topics**
+ [Enable ML Detect](#enable-ml-detect-cli)
+ [Monitor your ML model status](#monitor-ml-models-cli)
+ [Review your ML Detect alarms](#review-ml-alarms-cli)
+ [Fine\-tune your ML alarms](#fine-tune-ml-models-cli)
+ [Mitigate identified device issues](#mitigate-issues-cli)

### Enable ML Detect<a name="enable-ml-detect-cli"></a>

The following procedure shows you how to enable ML Detect in the AWS CLI\. 

1. First, make sure your devices will create the minimum datapoints required as defined in [ML Detect minimum requirements](dd-detect-ml.md#dd-detect-ml-requirements) for ongoing training and refreshing of the model\. For data collection to progress, ensure your things are in a thing group attached to a Security Profile\.

1. First, create an ML Detect Security Profile by using the `[create\-security\-profile](https://docs.aws.amazon.com/cli/latest/reference/iot/create-security-profile.html)` command\. The following example creates a Security Profile named *security\-profile\-for\-smart\-lights* that checks for number of messages sent, number of authorization failures, number of connection attempts, and number of disconnects\. The example uses `mlDetectionConfig` to establish that the metric will use the ML Detect model\.

   ```
   aws iot create-security-profile \
       --security-profile-name security-profile-for-smart-lights \
       --behaviors \
        '[{
         "name": "num-messages-sent-ml-behavior",
         "metric": "aws:num-messages-sent",
         "criteria": {
             "mlDetectionConfig": {
                 "confidenceLevel" : "MEDIUM"
             }
         },
         "suppressAlerts": true    
     },
     {
         "name": "num-authorization-failures-ml-behavior",
         "metric": "aws:num-authorization-failures",
         "criteria": {
             "mlDetectionConfig": {
                 "confidenceLevel" : "MEDIUM"
             }
         },
         "suppressAlerts": true                
     },
     {
         "name": "num-connection-attempts-ml-behavior",
         "metric": "aws:num-connection-attempts",
         "criteria": {
             "mlDetectionConfig": {
                 "confidenceLevel" : "MEDIUM"
             }
         },
         "suppressAlerts": true
     },
     {
         "name": "num-disconnects-ml-behavior",
         "metric": "aws:num-disconnects",
         "criteria": {
             "mlDetectionConfig": {
                 "confidenceLevel" : "MEDIUM"
             }
         },
         "suppressAlerts": true
   
     }]'
   ```

   Output:

   ```
   {
       "securityProfileName": "security-profile-for-smart-lights",
       "securityProfileArn": "arn:aws:iot:eu-west-1:123456789012:securityprofile/security-profile-for-smart-lights"
     }
   ```

1. Next, associate your Security Profile with one or multiple thing groups\. Use the `[attach\-security\-profile](https://docs.aws.amazon.com/cli/latest/reference/iot/attach-security-profile.html)` command to attach a thing group to your Security Profile\. The following example associates a thing group named *ML\_Detect\_beta\_static\_group* with the *security\-profile\-for\-smart\-lights* Security Profile\.

   ```
   aws iot attach-security-profile \
   --security-profile-name security-profile-for-smart-lights \
   --security-profile-target-arn arn:aws:iot:eu-west-1:123456789012:thinggroup/ML_Detect_beta_static_group
   ```

   Output:

   None\.

1. After you've created your complete Security Profile, the ML model will begin training\. The initial ML model training and building takes 14 days to complete\. After 14 days, if there's anomalous activity on your device, you can expect to see alarms\.

### Monitor your ML model status<a name="monitor-ml-models-cli"></a>

The following procedure shows you how to monitor your ML models in\-progress training\.
+ Use the `[get\-behavior\-model\-training\-summaries](https://docs.aws.amazon.com/cli/latest/reference/iot/get-behavior-model-training-summaries.html)` command to view your ML model's progress\. The following example gets the ML model training progress summary for the *security\-profile\-for\-smart\-lights* Security Profile\. `modelStatus` will show you if a model has completed training or still pending build for a particular behavior\.

  ```
  aws iot get-behavior-model-training-summaries \
     --security-profile-name security-profile-for-smart-lights
  ```

  Output:

  ```
  {
      "summaries": [
          {
              "securityProfileName": "security-profile-for-smart-lights",
              "behaviorName": "Messages_sent_ML_behavior",
              "trainingDataCollectionStartDate": "2020-11-30T14:00:00-08:00",
              "modelStatus": "ACTIVE",
              "datapointsCollectionPercentage": 29.408,
              "lastModelRefreshDate": "2020-12-07T14:35:19.237000-08:00"
          },
          {
              "securityProfileName": "security-profile-for-smart-lights",
              "behaviorName": "Messages_received_ML_behavior",
              "modelStatus": "PENDING_BUILD",
              "datapointsCollectionPercentage": 0.0
          },
          {
              "securityProfileName": "security-profile-for-smart-lights",
              "behaviorName": "Authorization_failures_ML_behavior",
              "trainingDataCollectionStartDate": "2020-11-30T14:00:00-08:00",
              "modelStatus": "ACTIVE",
              "datapointsCollectionPercentage": 35.464,
              "lastModelRefreshDate": "2020-12-07T14:29:44.396000-08:00"
          },
          {
              "securityProfileName": "security-profile-for-smart-lights",
              "behaviorName": "Message_size_ML_behavior",
              "trainingDataCollectionStartDate": "2020-11-30T14:00:00-08:00",
              "modelStatus": "ACTIVE",
              "datapointsCollectionPercentage": 29.332,
              "lastModelRefreshDate": "2020-12-07T14:30:44.113000-08:00"
          },
          {
              "securityProfileName": "security-profile-for-smart-lights",
              "behaviorName": "Connection_attempts_ML_behavior",
              "trainingDataCollectionStartDate": "2020-11-30T14:00:00-08:00",
              "modelStatus": "ACTIVE",
              "datapointsCollectionPercentage": 32.891999999999996,
              "lastModelRefreshDate": "2020-12-07T14:29:43.121000-08:00"
          },
          {
              "securityProfileName": "security-profile-for-smart-lights",
              "behaviorName": "Disconnects_ML_behavior",
              "trainingDataCollectionStartDate": "2020-11-30T14:00:00-08:00",
              "modelStatus": "ACTIVE",
              "datapointsCollectionPercentage": 35.46,
              "lastModelRefreshDate": "2020-12-07T14:29:55.556000-08:00"
          }
      ]
  }
  ```

**Note**  
If your model doesn't progress as expected, make sure your devices are meeting the [ML Detect minimum requirements](dd-detect-ml.md#dd-detect-ml-requirements)\.

### Review your ML Detect alarms<a name="review-ml-alarms-cli"></a>

After your ML models are built and ready for data evaluations, you can regularly view any alarms that are inferred by the models\. The following procedure shows you how to view your alarms in the AWS CLI\.
+ To see all active alarms, use the `[list\-active\-violations](https://docs.aws.amazon.com/cli/latest/reference/iot/list-active-violations.html)` command\.

  ```
  aws iot list-active-violations \
  --max-results 2
  ```

  Output:

  ```
  {
      "activeViolations": []
  }
  ```

  Alternatively, you can view all violations discovered during a given time period by using the `[list\-violation\-events](https://docs.aws.amazon.com/cli/latest/reference/iot/list-violation-events.html)` command\. The following example lists violation events from September 22, 2020 5:42:13 GMT to October 26, 2020 5:42:13 GMT\.

  ```
  aws iot list-violation-events \
      --start-time 1599500533 \ 
      --end-time 1600796533 \
      --max-results 2
  ```

  Output:

  ```
  {
      "violationEvents": [
          {
              "violationId": "1448be98c09c3d4ab7cb9b6f3ece65d6",
              "thingName": "lightbulb-1",
              "securityProfileName": "security-profile-for-smart-lights",
              "behavior": {
                  "name": "LowConfidence_MladBehavior_MessagesSent",
                  "metric": "aws:num-messages-sent",
                  "criteria": {
                      "mlDetectionConfig": {
                          "confidenceLevel": "MEDIUM"
                      }
                  },
                  "suppressAlerts": false
              },
              "violationEventType": "alarm-invalidated",
              "violationEventTime": 1600780245.29
          },
          {
              "violationId": "df4537569ef23efb1c029a433ae84b52",
              "thingName": "lightbulb-2",
              "securityProfileName": "security-profile-for-smart-lights",
              "behavior": {
                  "name": "LowConfidence_MladBehavior_MessagesSent",
                  "metric": "aws:num-messages-sent",
                  "criteria": {
                      "mlDetectionConfig": {
                          "confidenceLevel": "MEDIUM"
                      }
                  },
                  "suppressAlerts": false
              },
              "violationEventType": "alarm-invalidated",
              "violationEventTime": 1600780245.281
          }
      ],
      "nextToken": "Amo6XIUrsOohsojuIG6TuwSR3X9iUvH2OCksBZg6bed2j21VSnD1uP1pflxKX1+a3cvBRSosIB0xFv40kM6RYBknZ/vxabMe/ZW31Ps/WiZHlr9Wg7R7eEGli59IJ/U0iBQ1McP/ht0E2XA2TTIvYeMmKQQPsRj/eoV9j7P/wveu7skNGepU/mvpV0O2Ap7hnV5U+Prx/9+iJA/341va+pQww7jpUeHmJN9Hw4MqW0ysw0Ry3w38hOQWEpz2xwFWAxAARxeIxCxt5c37RK/lRZBlhYqoB+w2PZ74730h8pICGY4gktJxkwHyyRabpSM/G/f5DFrD9O5v8idkTZzBxW2jrbzSUIdafPtsZHL/yAMKr3HAKtaABz2nTsOBNre7X2d/jIjjarhon0Dh9l+8I9Y5Ey+DIFBcqFTvhibKAafQt3gs6CUiqHdWiCenfJyb8whmDE2qxvdxGElGmRb+k6kuN5jrZxxw95gzfYDgRHv11iEn8h1qZLD0czkIFBpMppHj9cetHPvM+qffXGAzKi8tL6eQuCdMLXmVE3jbqcJcjk9ItnaYJi5zKDz9FVbrz9qZZPtZJFHp"
  }
  ```

### Fine\-tune your ML alarms<a name="fine-tune-ml-models-cli"></a>

Once your ML models are built and ready for data evaluations, you can update your Security Profile's ML behavior settings to change the configuration\. The following procedure shows you how to update your Security Profile's ML behavior settings in the AWS CLI\.
+ Use the `[update\-security\-profile](https://docs.aws.amazon.com/cli/latest/reference/iot/update-security-profile.html)` command to change your Security Profile's ML behavior settings\. The following example updates the *security\-profile\-for\-smart\-lights* Security Profile's behaviors by changing the `confidenceLevel` of a few of the behaviors and unsuppresses notifications\.

  ```
  aws iot update-security-profile \
      --security-profile-name security-profile-for-smart-lights \
      --behaviors 
       '[{
        "name": "num-messages-sent-ml-behavior",
        "metric": "aws:num-messages-sent",
        "criteria": {
            "mlDetectionConfig": {
                "confidenceLevel" : "HIGH"
            }
        },
    },
    {
        "name": "num-authorization-failures-ml-behavior",
        "metric": "aws:num-authorization-failures",
        "criteria": {
            "mlDetectionConfig": {
                "confidenceLevel" : "HIGH"
            }
        },
    },
    {
        "name": "num-connection-attempts-ml-behavior",
        "metric": "aws:num-connection-attempts",
        "criteria": {
            "mlDetectionConfig": {
                "confidenceLevel" : "HIGH"
            }
        },
        "suppressAlerts": false
    },
    {
        "name": "num-disconnects-ml-behavior",
        "metric": "aws:num-disconnects",
        "criteria": {
            "mlDetectionConfig": {
                "confidenceLevel" : "LOW"
            }
        },
        "suppressAlerts": true
  
    }]'
  ```

  Output:

  ```
   {
      "securityProfileName": "security-profile-for-smart-lights",
      "securityProfileArn": "arn:aws:iot:eu-west-1:123456789012:securityprofile/security-profile-for-smart-lights",
      "behaviors": [
          {
              "name": "num-messages-sent-ml-behavior",
              "metric": "aws:num-messages-sent",
              "criteria": {
                  "mlDetectionConfig": {
                      "confidenceLevel": "HIGH"
                  }
              }
          },
          {
              "name": "num-authorization-failures-ml-behavior",
              "metric": "aws:num-authorization-failures",
              "criteria": {
                  "mlDetectionConfig": {
                      "confidenceLevel": "HIGH"
                  }
              }
          },
          {
              "name": "num-connection-attempts-ml-behavior",
              "metric": "aws:num-connection-attempts",
              "criteria": {
                  "mlDetectionConfig": {
                      "confidenceLevel": "HIGH"
                  }
              },
              "suppressAlerts": false
          },
          {
              "name": "num-disconnects-ml-behavior",
              "metric": "aws:num-disconnects",
              "criteria": {
                  "mlDetectionConfig": {
                      "confidenceLevel": "LOW"
                  }
              },
              "suppressAlerts": true
          }
      ],
      "version": 2,
      "creationDate": 1600799559.249,
      "lastModifiedDate": 1600800516.856
  }
  ```

### Mitigate identified device issues<a name="mitigate-issues-cli"></a>

1. Use the `[create\-thing\-group](https://docs.aws.amazon.com/cli/latest/reference/iot/create-thing-group.html)` command to create a thing group for the mitigation action\. In the following example we create a thing group called **ThingGroupForDetectMitigationAction**

   ```
   aws iot create-thing-group —thing-group-name ThingGroupForDetectMitigationAction
   ```

   Output:

   ```
   {
    "thingGroupName": "ThingGroupForDetectMitigationAction",
    "thingGroupArn": "arn:aws:iot:us-east-1:123456789012:thinggroup/ThingGroupForDetectMitigationAction",
    "thingGroupId": "4139cd61-10fa-4c40-b867-0fc6209dca4d"
   }
   ```

1. Next, use the `[create\-mitigation\-action](https://docs.aws.amazon.com/cli/latest/reference/iot/create-mitigation-action.html)` command to create a mitigation action\. In the following example, we create a mitigation action called **detect\_mitigation\_action** with the ARN of the IAM role that is used to apply the mitigation action\. We also define the type of action and the parameters for that action\. In this case, our mitigation will move things to our previously created thing group called **ThingGroupForDetectMitigationAction**\.

   ```
   aws iot create-mitigation-action --action-name detect_mitigation_action \
   --role-arn arn:aws:iam::123456789012:role/MitigationActionValidRole \
   --action-params  
   '{
        "addThingsToThingGroupParams": {
            "thingGroupNames": ["ThingGroupForDetectMitigationAction"],
            "overrideDynamicGroups": false
        }
    }'
   ```

   Output:

   ```
   {
    "actionArn": "arn:aws:iot:us-east-1:123456789012:mitigationaction/detect_mitigation_action",
    "actionId": "5939e3a0-bf4c-44bb-a547-1ab59ffe67c3"
   }
   ```

1. Use the `[start\-detect\-mitigation\-actions\-task](https://docs.aws.amazon.com/cli/latest/reference/iot/start-detect-mitigation-actions-task.html)` command to start your mitigation actions task\. `task-id`, `target` and `actions` are required parameters\.

   ```
   aws iot start-detect-mitigation-actions-task \
       --task-id taskIdForMitigationAction \
       --target '{ "violationIds" : [ "violationId-1", "violationId-2" ] }' \
       --actions "detect_mitigation_action" \
       --include-only-active-violations \
       --include-suppressed-alerts \
   ```

   Output:

   ```
   {
       "taskId": "taskIdForMitigationAction"
   }
   ```

1. \(Optional\) Use the `[describe\-detect\-mitigation\-actions\-task](https://docs.aws.amazon.com/cli/latest/reference/iot/describe-detect-mitigation-actions-task.html)` command to get information about a mitigation action task\.

   ```
   aws iot describe-detect-mitigation-actions-task \
       --task-id taskIdForMitigationAction
   ```

   Output:

   ```
   {
       "taskSummary": {
           "taskId": "taskIdForMitigationAction",
           "taskStatus": "SUCCESSFUL",
           "taskStartTime": 1609988361.224,
           "taskEndTime": 1609988362.281,
           "target": {
               "securityProfileName": "security-profile-for-smart-lights",
               "behaviorName": "num-messages-sent-ml-behavior"
           },
           "violationEventOccurrenceRange": {
               "startTime": 1609986633.0,
               "endTime": 1609987833.0
           },
           "onlyActiveViolationsIncluded": true,
           "suppressedAlertsIncluded": true,
           "actionsDefinition": [
               {
                   "name": "detect_mitigation_action",
                   "id": "5939e3a0-bf4c-44bb-a547-1ab59ffe67c3",
                   "roleArn": "arn:aws:iam::123456789012:role/MitigatioActionValidRole",
                   "actionParams": {
                       "addThingsToThingGroupParams": {
                           "thingGroupNames": [
                               "ThingGroupForDetectMitigationAction"
                           ],
                           "overrideDynamicGroups": false
                       }
                   }
               }
           ],
           "taskStatistics": {
               "actionsExecuted": 0,
               "actionsSkipped": 0,
               "actionsFailed": 0
           }
       }
   }
   ```

1. \(Optional\) To view mitigation actions executions included in a task, use the `[list\-detect\-mitigation\-actions\-executions](https://docs.aws.amazon.com/cli/latest/reference/iot/list-detect-mitigation-actions-executions.html)` command\.

   ```
   aws iot list-detect-mitigation-actions-executions \
       --task-id taskIdForMitigationAction \
       --max-items 5 \
       --page-size 4
   ```

   Output:

   ```
   {
       "actionsExecutions": [
           {
               "taskId": "e56ee95e - f4e7 - 459 c - b60a - 2701784290 af",
               "violationId": "214_fe0d92d21ee8112a6cf1724049d80",
               "actionName": "underTest_MAThingGroup71232127",
               "thingName": "cancelDetectMitigationActionsTaskd143821b",
               "executionStartDate": "Thu Jan 07 18: 35: 21 UTC 2021",
               "executionEndDate": "Thu Jan 07 18: 35: 21 UTC 2021",
               "status": "SUCCESSFUL",
           }
       ]
   }
   ```

1. \(Optional\) To get a list of your mitigation actions tasks, use the `[list\-detect\-mitigation\-actions\-tasks](https://docs.aws.amazon.com/cli/latest/reference/iot/list-detect-mitigation-actions-tasks.html)` command\.

   ```
   aws iot list-detect-mitigation-actions-tasks \
       --start-time 1609985315 \
       --end-time 1609988915 \
       --max-items 5 \
       --page-size 4
   ```

   Output:

   ```
   {
       "tasks": [
           {
               "taskId": "taskIdForMitigationAction",
               "taskStatus": "SUCCESSFUL",
               "taskStartTime": 1609988361.224,
               "taskEndTime": 1609988362.281,
               "target": {
                   "securityProfileName": "security-profile-for-smart-lights",
                   "behaviorName": "num-messages-sent-ml-behavior"
               },
               "violationEventOccurrenceRange": {
                   "startTime": 1609986633.0,
                   "endTime": 1609987833.0
               },
               "onlyActiveViolationsIncluded": true,
               "suppressedAlertsIncluded": true,
               "actionsDefinition": [
                   {
                       "name": "detect_mitigation_action",
                       "id": "5939e3a0-bf4c-44bb-a547-1ab59ffe67c3",
                       "roleArn": "arn:aws:iam::123456789012:role/MitigatioActionValidRole",
                       "actionParams": {
                           "addThingsToThingGroupParams": {
                               "thingGroupNames": [
                                   "ThingGroupForDetectMitigationAction"
                               ],
                               "overrideDynamicGroups": false
                           }
                       }
                   }
               ],
               "taskStatistics": {
                   "actionsExecuted": 0,
                   "actionsSkipped": 0,
                   "actionsFailed": 0
               }
           }
       ]
   }
   ```

1. \(Optional\) To cancel a mitigation actions task, use the `[cancel\-detect\-mitigation\-actions\-task](https://docs.aws.amazon.com/cli/latest/reference/iot/cancel-detect-mitigation-actions-task.html)` command\.

   ```
   aws iot cancel-detect-mitigation-actions-task \
       --task-id taskIdForMitigationAction
   ```

   Output:

   None\.