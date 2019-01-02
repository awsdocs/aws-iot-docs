# Fleet Indexing Service<a name="iot-indexing"></a>

Fleet Indexing is a managed service that enables you to index and search your registry data, shadow data, and device connectivity data \(device life\-cycle events\) in the cloud\. After you set up your fleet index, the service manages the indexing of updates for your thing groups, thing registries and thing shadows\. You can use a simple query language to search across this data\. You can also create a [dynamic thing group](http://alpha-docs-aws.amazon.com/iot/latest/developerguide/dynamic-thing-groups.html) with a search query\.

`AWS_Things` is the index created for all of your *things*\. `AWS_ThingGroups` is the index that contains all of your *thing groups*\.

To get started, enable indexing and AWS IoT creates an index for your things or thing groups\. After it's active, you can run queries on your index, such as finding all devices that are handheld and have a battery life greater than 70%\. AWS IoT keeps it continuously updated with your latest data\.

You can use the [ AWS IoT console](https://console.aws.amazon.com/iot/home) to manage your indexing configuration and run your search queries\. Choose the indexes you would like to use in the console settings page\. If you prefer programmatic access, you can use the AWS SDKs or the AWS CLI\.

Please see the [AWS IoT Device Management Pricing](https://aws.amazon.com/iot-device-management/pricing) page for information about pricing this and other services\.