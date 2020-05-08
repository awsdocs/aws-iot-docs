# Resilience in AWS IoT Core<a name="disaster-recovery-resiliency"></a>

The AWS global infrastructure is built around AWS Regions and Availability Zones\. AWS Regions provide multiple physically separated and isolated Availability Zones, which are connected with low\-latency, high\-throughput, and highly redundant networking\. With Availability Zones, you can design and operate applications and databases that automatically fail over between Availability Zones without interruption\. Availability Zones are more highly available, fault tolerant, and scalable than traditional single or multiple data center infrastructures\. 

For more information about AWS Regions and Availability Zones, see [AWS Global Infrastructure](http://aws.amazon.com/about-aws/global-infrastructure/)\.

AWS IoT Core stores information about your devices in the device registry\. It also stores CA certificates, device certificates, and device shadow data\. This data is not automatically replicated in the event of hardware or network failures\. AWS IoT Core publishes MQTT events when the device registry is updated\. You can use these messages to back up your registry data and save it somewhere, like a DynamoDB table\. You are responsible for saving certificates that AWS IoT Core creates for you or those you create yourself\. Device shadow stores state data about your devices and can be resent when a device comes back online\.

AWS IoT Core resources are Region\-specific and aren't replicated across AWS Regions unless you specifically do so\.