# How do I use AWS IoT metrics?<a name="how_to_use_metrics"></a>

The metrics reported by AWS IoT provide information that you can analyze in different ways\. The following use cases are based on a scenario where you have ten things that connect to the internet once a day\. Each day:
+ Ten things connect to AWS IoT at roughly the same time\.
+ Each thing subscribes to a topic filter, and then waits for an hour before disconnecting\. During this period, things communicate with one another and learn more about the state of the world\.
+ Each thing publishes some perception it has based on its newly found data using `UpdateThingShadow`\.
+ Each thing disconnects from AWS IoT\.

These are suggestions to get you started, not a comprehensive list\.
+ [How can I be notified if my things do not connect successfully each day?](creating_alarms.md#how_to_detect_connection_failures)
+ [How can I be notified if my things are not publishing data each day?](creating_alarms.md#how_to_detect_publish_failures)
+ [How can I be notified if my thing's shadow updates are being rejected each day?](creating_alarms.md#detect_rejected_updates)