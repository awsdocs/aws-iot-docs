# Fleet Indexing Service<a name="iot-indexing"></a>

Fleet Indexing is a managed service that you can use to index, search, and aggregate your registry data, shadow data, and device connectivity data \(device lifecycle events\) in the cloud\. After you set up your fleet index, the service manages the indexing of updates for your thing groups, thing registries, and device shadows\. For more information about aggregation queries, see [Querying for Aggregate Data](index-aggregate.md)\. You can use a simple query language to search across this data\. You can also create a [dynamic thing group](https://docs.aws.amazon.com/iot/latest/developerguide/dynamic-thing-groups.html) with a search query\.

**Note**  
It can take up to 30 seconds for the Fleet Indexing service to update the fleet index after a thing is created or deleted\.

When you enable indexing, AWS IoT creates an index for your things or thing groups\. After it's active, you can run queries on your index, such as finding all devices that are handheld and have more than 70 percent battery life\. AWS IoT keeps it continuously updated with your latest data\.

`AWS_Things` is the index created for all of your *things*\. `AWS_ThingGroups` is the index that contains all of your *thing groups*\.

You can use the [ AWS IoT console](https://console.aws.amazon.com/iot/home) to manage your indexing configuration and run your search queries\. Choose the indexes you would like to use in the console settings page\. If you prefer programmatic access, you can use the AWS SDKs or the AWS CLI\.

For information about pricing this and other services, see the [AWS IoT Device Management Pricing](https://aws.amazon.com/iot-device-management/pricing) page\.