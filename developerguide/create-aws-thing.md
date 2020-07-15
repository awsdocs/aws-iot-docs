# Create a thing<a name="create-aws-thing"></a>

Devices connected to AWS IoT are represented by *things* in the AWS IoT registry\. A *thing* represents a specific device or logical entity\. It can be a physical device or sensor \(for example, a light bulb or a switch on the wall\)\. It can also be a logical entity, like an instance of an application or physical entity that does not connect to AWS IoT, but is related to other devices that do \(for example, a car that has engine sensors or a control panel\)\.

**To create a thing**

1. On the **Welcome to the AWS IoT Console** page, in the navigation pane, choose **Manage**\.

1. On the **You don't have any things yet** page, choose **Register a thing**\.

1. On the **Creating AWS IoT things** page, choose **Create a single thing**\.

1. On the **Create a thing** page, in the **Name** field, enter a name for your thing, such as **MyIotThing**\. Choose **Next**\. To change a thing's name, you must create a new thing, give it the new name, and then delete the old thing\.

**When naming your thing objects:**
+ You should not use personally identifiable information in your thing name\. The thing name can appear in unencrypted communications and reports\. 
+  You should not use a colon character \( : \) in a thing name\. The colon character is used as a delimiter by other AWS IoT services and this can cause them to parse strings with thing names incorrectly\. 