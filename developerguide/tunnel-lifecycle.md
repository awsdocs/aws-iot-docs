# Secure tunnel lifecycle<a name="tunnel-lifecycle"></a>

Tunnels can have one of the following statuses:
+ `OPEN`
+ `CLOSED`

Connections can have one of the following statuses:
+ `CONNECTED`
+ `DISCONNECTED`

When you open a tunnel, it has a status of `OPEN`\. The tunnel's source and destination connection status is set to `DISCONNECTED`\. When a device \(source or destination\) connects to the tunnel, the corresponding connection status changes to `CONNECTED`, while the tunnel status remains `OPEN`\. When a device disconnects from the tunnel, the corresponding connection status changes back to `DISCONNECTED`\. A device can connect to and disconnect from a tunnel repeatedly as long as the tunnel remains `OPEN`\.

A tunnel's status becomes `CLOSED` when you call `CloseTunnel` or the tunnel remains `OPEN` for longer than the `MaxLifetimeTimeout` value\. You can configure `MaxLifetimeTimeout` when calling `OpenTunnel`\. `MaxLifetimeTimeout` defaults to 12 hours if you do not specify a value\.

After a tunnel is `CLOSED`, it might be visible in the AWS IoT console for at least three hours before it is deleted\. While the tunnel is visible, you can call `DescribeTunnel` and `ListTunnels` to view tunnel metadata\. A tunnel cannot be reopened when it is `CLOSED`\. The Secure Tunneling service rejects attempts to connect to a `CLOSED` tunnel\. 