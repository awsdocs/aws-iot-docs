# Working with Topic Rule Destinations<a name="rule-destination"></a>

A destination is a resource that defines where rules engine can route data\. A destination can be reused across rules and might require confirmation or configuration before it can be used\. Destinations make it possible for the rules engine to send data to other services that are not natively integrated with AWS IoT\.

Topic rule destinations are used to verify that you own or have access to the endpoint to which you want to route data\. When you create a rule action with an endpoint \(for example, an `http` action\) or update an existing rule action's endpoint, AWS IoT sends a confirmation message to the endpoint that contains a unique token\. To verify your endpoint, you must call `ConfirmTopicRuleDestination` with the token to verify you own or have access to the endpoint\. The token is valid for three days, after which you need to create a new `http` action or call the `UpdateTopicRuleDestination` API to restart the confirmation process\. You can also confirm your topic rule destination by browsing to `enableUrl` or provide the token from the **Destination** page in the AWS IoT console\.

When AWS IoT receives the token sent to your endpoint, the destination is confirmed\. You must enable the destination before it can be used by rules engine\. A destination can be in one of the following states:

ENABLED  
The destination is enabled\. A destination must be in the `ENABLED` state for it to be used in a rule\. You can only enable a destinations in DISABLED status\.

DISABLED  
The destination has been confirmed but disabled\. This is useful if you want to temporarily prevent traffic to your endpoint without having to go through the confirmation process again\. You can only disable a destinations in ENABLED status\.

IN\_PROGRESS  
Confirmation of the destination is in progress\.

ERROR  
Destination confirmation timed out\.

After they are confirmed and enabled, destinations can be used with any rule in your account\.

## Creating a Topic Rule Destination<a name="create-destination"></a>

A destination is created when you use AWS IoT to create a rule with an `http` action\. You can also create a destination using the `CreateTopicRuleDestination` API or the AWS IoT console\.

When you create a destination, AWS IoT verifies the endpoint URL is valid\. If the URL is valid, a confirmation message is sent to that URL\. The confirmation request has the following format:

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

The fields of this message are defined as follows:

arn  
The ARN for the topic rule destination to confirm\.

confirmationToken  
The confirmation token\. The token in the example is truncated\. Your token will be longer\.

enableUrl  
The URL to which you browse to confirm a topic rule destination\.

messageType  
The type of message\.

## Confirming a Topic Rule Destination<a name="confirm-destination"></a>

You can confirm a destination by making an HTTP GET request to the `enableUrl` that is included in the confirmation message\. You can call `ConfirmTopicRuleDestination` with the token from the confirmation message or you can use the AWS IoT console\.

The token must be valid for the destination to be put in the `ENABLED` state\. If the token has expired, you must call `UpdateTopicRuleDestination` to restart the confirmation process\.

**Note**  
 if you confirm the topic rule destination by calling `ConfirmTopicRuleDestination`, the destination status will be `DISABLED` and you need to explicitly enable your destination by calling `UpdateTopicRuleDestination` before using it\.

## Disabling a Topic Rule Destination<a name="disable-destination"></a>

To disable a destination, call `UpdateTopicRuleDestination` and set the topic rule destination's status to `DISABLED`\.

## Enabling a Topic Rule Destination<a name="enable-destination"></a>

To enable a destination, call `UpdateTopicRuleDistination` and set the topic rule's status to `ENABLED`\. You do not need to re\-validate the URL\.

## Sending a New Confirmation Message<a name="trigger-confirm"></a>

To trigger a new confirmation message for a destination, call `UpdateTopicRuleDistination` and set the topic rule destination's status to `IN_PROGRESS`\. 

## Deleting a Topic Rule Destination<a name="delete-destination"></a>

To delete a topic rule destination, call `DeleteTopicRuleDestination`\.