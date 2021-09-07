# Working with HTTP topic rule destinations<a name="rule-destination"></a>

An HTTP topic rule destination is a web service to which the rules engine can route data from a topic rule\. An AWS IoT Core resource describes the web service for AWS IoT\. Topic rule destination resources can be shared by different rules\.

Before AWS IoT Core can send data to another web service, it must confirm that it can access the service's endpoint\.

## HTTP topic rule destination overview<a name="rule-destination-http"></a>

An HTTP topic rule destination refers to a web service that supports a confirmation URL and one or more data collection URLs\. The HTTP topic rule destination resource contains the confirmation URL of your web service\. When you configure an HTTP topic rule action, you specify the actual URL of the endpoint that should receive the data along with the web service's confirmation URL\. After your destination has been confirmed, the topic rule sends the result of the SQL statement to the HTTPS endpoint \(and not to the confirmation URL\)\.

An HTTP topic rule destination can be in one of the following states:

ENABLED  
The destination has been confirmed and can be used by a rule action\. A destination must be in the `ENABLED` state for it to be used in a rule\. You can only enable a destination that's in DISABLED status\.

DISABLED  
The destination has been confirmed but it can't be used by a rule action\. This is useful if you want to temporarily prevent traffic to your endpoint without having to go through the confirmation process again\. You can only disable a destination that's in ENABLED status\.

IN\_PROGRESS  
Confirmation of the destination is in progress\.

ERROR  
Destination confirmation timed out\.

After an HTTP topic rule destination has been confirmed and enabled, it can be used with any rule in your account\.

The following sections describe common actions on HTTP topic rule destinations\.

## Creating and confirming HTTP topic rule destinations<a name="rule-destination-http-creating"></a>

You create an HTTP topic rule destination by calling the `CreateTopicRuleDestination` operation or by using the AWS IoT console\.

After you create a destination, AWS IoT sends a confirmation request to the confirmation URL\. The confirmation request has the following format:

```
HTTP POST {confirmationUrl}/?confirmationToken={confirmationToken}
Headers:
x-amz-rules-engine-message-type: DestinationConfirmation
x-amz-rules-engine-destination-arn:"arn:aws:iot:us-east-1:123456789012:ruledestination/http/7a280e37-b9c6-47a2-a751-0703693f46e4"
Content-Type: application/json
Body:
{
    "arn":"arn:aws:iot:us-east-1:123456789012:ruledestination/http/7a280e37-b9c6-47a2-a751-0703693f46e4",  
    "confirmationToken": "AYADeMXLrPrNY2wqJAKsFNn-…NBJndA",
    "enableUrl": "https://iot.us-east-1.amazonaws.com/confirmdestination/AYADeMXLrPrNY2wqJAKsFNn-…NBJndA",
    "messageType": "DestinationConfirmation"
}
```

The content of the confirmation request includes the following information:

arn  
The Amazon Resource Name \(ARN\) for the topic rule destination to confirm\.

confirmationToken  
The confirmation token sent by AWS IoT Core\. The token in the example is truncated\. Your token will be longer\. You'll need this token to confirm your destination with AWS IoT Core\.

enableUrl  
The URL to which you browse to confirm a topic rule destination\.

messageType  
The type of message\.

To complete the endpoint confirmation process, you must do one of the following after your confirmation URL receives the confirmation request\.
+ Call the `enableUrl` in the confirmation request, and then call `UpdateTopicRuleDestination` to set the topic rule's status to `ENABLED`\. 
+ Call the `ConfirmTopicRuleDestination` operation and passing the `confirmationToken` from the confirmation request\.
+ Copy the `confirmationToken` and paste it into the destination's confirmation dialog in the AWS IoT console\.

## Sending a new confirmation request<a name="trigger-confirm"></a>

To trigger a new confirmation message for a destination, call `UpdateTopicRuleDestination` and set the topic rule destination's status to `IN_PROGRESS`\. 

You'll need to repeat the confirmation process after you send a new confirmation request

## Disabling and deleting a topic rule destination<a name="rule-destination-http-deleting"></a>

To disable a destination, call `UpdateTopicRuleDestination` and set the topic rule destination's status to `DISABLED`\. A topic rule in the DISABLED state can be enabled again without the need to send a new confirmation request\.

To delete a topic rule destination, call `DeleteTopicRuleDestination`\.