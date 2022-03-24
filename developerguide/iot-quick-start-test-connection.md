# Testing connectivity with your device data endpoint<a name="iot-quick-start-test-connection"></a>

This topic describes how to test a device's connection with your account's *device data endpoint*, the endpoint that your IoT devices use to connect to AWS IoT\.

Perform these procedures on the device that you want to test or by using an SSH terminal session connected to the device you want to test\.

**Topics**
+ [Find your device data endpoint](#iot-quick-start-test-connection-endpoint)
+ [Test the connection quickly](#iot-quick-start-test-connection-ping)
+ [Get the app to test the connection to your device data endpoint and port](#iot-quick-start-test-connection-app)
+ [Test the connection to your device data endpoint and port](#iot-quick-start-test-connection-test)

## Find your device data endpoint<a name="iot-quick-start-test-connection-endpoint"></a>

**To find your device data endpoint**

1. In the [AWS IoT console](https://console.aws.amazon.com/iot/home), near the bottom of the navigation pane, choose**Settings**\.

1. In the **Settings** page, in the **Device data endpoint** container, locate the **Endpoint** value and copy it\.

1. Your endpoint value is unique to your AWS account and is similar to this example: `a3qEXAMPLEsffp-ats.iot.eu-west-1.amazonaws.com`\.

Save your device data endpoint to use in the following procedures\.

## Test the connection quickly<a name="iot-quick-start-test-connection-ping"></a>

This procedure tests general connectivity with your device data endpoint, but it doesn't test the specific port that your devices will use\. This test uses a common program and is usually sufficient to know if your devices can connect to AWS IoT\.

If you want to test connectivity with the specific port that your devices will use, skip this procedure and continue to [Get the app to test the connection to your device data endpoint and port](#iot-quick-start-test-connection-app)\.

**To test the device data endpoint quickly**

1. In a terminal or command line window on your device, replace the sample device data endpoint \(*`a3qEXAMPLEsffp-ats.iot.eu-west-1.amazonaws.com`*\) with the device data endpoint for your account, and then enter this command\.

   ```
   ping -c 5 a3qEXAMPLEsffp-ats.iot.eu-west-1.amazonaws.com
   ```

1. If `ping` displays an output similar to the following, it connected to your device data endpoint successfully\. While it didn't communicate with AWS IoT directly, it did find the server and it's likely that AWS IoT is available through this endpoint\.

   ```
   PING a3qEXAMPLEsffp-ats.iot.eu-west-1.amazonaws.com (xx.xx.xxx.xxx) 56(84) bytes of data.
   64 bytes from ec2-EXAMPLE-218.eu-west-1.compute.amazonaws.com (xx.xx.xxx.xxx): icmp_seq=1 ttl=231 time=127 ms
   64 bytes from ec2-EXAMPLE-218.eu-west-1.compute.amazonaws.com (xx.xx.xxx.xxx): icmp_seq=2 ttl=231 time=127 ms
   64 bytes from ec2-EXAMPLE-218.eu-west-1.compute.amazonaws.com (xx.xx.xxx.xxx): icmp_seq=3 ttl=231 time=127 ms
   64 bytes from ec2-EXAMPLE-218.eu-west-1.compute.amazonaws.com (xx.xx.xxx.xxx): icmp_seq=4 ttl=231 time=127 ms
   64 bytes from ec2-EXAMPLE-218.eu-west-1.compute.amazonaws.com (xx.xx.xxx.xxx): icmp_seq=5 ttl=231 time=127 ms
   ```

   If you are satisfied with this result, you can stop testing here\.

   If you want to test the connectivity with the specific port used by AWS IoT, continue to [Get the app to test the connection to your device data endpoint and port](#iot-quick-start-test-connection-app)\.

1. If `ping` didn't return a successful output, check the endpoint value to make sure you have the correct endpoint and check the device's connection with the internet\.

## Get the app to test the connection to your device data endpoint and port<a name="iot-quick-start-test-connection-app"></a>

A more thorough connectivity test can be performed by using `nmap`\. This procedure tests to see if `nmap` is installed on your device\.

**To check for `nmap` on the device**

1. In a terminal or command line window on the device you want to test, enter this command to see if `nmap` is installed\.

   ```
   nmap --version
   ```

1. If you see an output similar to the following, `nmap` is installed and you can continue to [Test the connection to your device data endpoint and port](#iot-quick-start-test-connection-test)\.

   ```
   Nmap version 6.40 ( http://nmap.org )
   Platform: x86_64-koji-linux-gnu
   Compiled with: nmap-liblua-5.2.2 openssl-1.0.2k libpcre-8.32 libpcap-1.5.3 nmap-libdnet-1.12 ipv6
   Compiled without:
   Available nsock engines: epoll poll select
   ```

1. If you don't see a response similar to the one shown in the preceding step, you must install `nmap` on the device\. Choose the procedure for your device's operating system\.

------
#### [ Linux ]

This procedure requires that you have permission to install software on the computer\.

**To install nmap on your Linux computer**

1. In a terminal or command line window on your device, enter the command that corresponds to the version of Linux it's running\.

   1. Debian or Ubuntu:

      ```
      sudo apt install nmap
      ```

   1. CentOS or RHEL:

      ```
      sudo yum install nmap
      ```

1. Test the installation with this command:

   ```
   nmap --version
   ```

1. If you see an output similar to the following, `nmap` is installed and you can continue to [Test the connection to your device data endpoint and port](#iot-quick-start-test-connection-test)\.

   ```
   Nmap version 6.40 ( http://nmap.org )
   Platform: x86_64-koji-linux-gnu
   Compiled with: nmap-liblua-5.2.2 openssl-1.0.2k libpcre-8.32 libpcap-1.5.3 nmap-libdnet-1.12 ipv6
   Compiled without:
   Available nsock engines: epoll poll select
   ```

------
#### [ macOS ]

This procedure requires that you have permission to install software on the computer\.

**To install nmap on your macOS computer**

1. In a browser, open [https://nmap\.org/download\#macosx](https://nmap.org/download#macosx) and download the **latest stable** installer\.

   When prompted, select **Open with DiskImageInstaller**\.

1. In the installation window, move the package to the **Applications** folder\.

1. In the **Finder**, locate the `nmap-xxxx-mpkg` package in the **Applications** folder\. Ctrl\-click the on package and select **Open** to open the package\.

1. Review the security dialog box\. If you are ready to install nmap, choose **Open** to install nmap\.

1. In Terminal, test the installation with this command\.

   ```
   nmap --version
   ```

1. If you see an output similar to the following, `nmap` is installed and you can continue to [Test the connection to your device data endpoint and port](#iot-quick-start-test-connection-test)\.

   ```
   Nmap version 7.92 ( https://nmap.org )
   Platform: x86_64-apple-darwin17.7.0
   Compiled with: nmap-liblua-5.3.5 openssl-1.1.1k nmap-libssh2-1.9.0 libz-1.2.11 nmap-libpcre-7.6 nmap-libpcap-1.9.1 nmap-libdnet-1.12 ipv6 Compiled without:
   Available nsock engines: kqueue poll select
   ```

------
#### [ Windows ]

This procedure requires that you have permission to install software on the computer\.

**To install nmap on your Windows computer**

1. In a browser, open [https://nmap\.org/download\#windows](https://nmap.org/download#windows) and download the **latest stable** release of the setup program\.

   If prompted, choose **Save file**\. After the file is downloaded, open it from the downloads folder\.

1.  After the setup file finishes downloading, open downloaded nmap\-xxxx\-setup\.exe to install the app\. 

1.  Accept the default settings as the program installs\.

   You don't need the Npcap app for this test\. You can deselect that option if you don't want to install it\.

1. In Command, test the installation with this command\.

   ```
   nmap --version
   ```

1. If you see an output similar to the following, `nmap` is installed and you can continue to [Test the connection to your device data endpoint and port](#iot-quick-start-test-connection-test)\.

   ```
   Nmap version 7.92 ( https://nmap.org )
   Platform: i686-pc-windows-windows
   Compiled with: nmap-liblua-5.3.5 openssl-1.1.1k nmap-libssh2-1.9.0 nmap-libz-1.2.11 nmap-libpcre-7.6 Npcap-1.50 nmap-libdnet-1.12 ipv6
   Compiled without:
   Available nsock engines: iocp poll select
   ```

------

## Test the connection to your device data endpoint and port<a name="iot-quick-start-test-connection-test"></a>

**To test your device data endpoint and port**

1. In a terminal or command line window on your device, replace the sample device data endpoint \(*`a3qEXAMPLEsffp-ats.iot.eu-west-1.amazonaws.com`*\) with the device data endpoint for your account, and then enter this command\.

   ```
   nmap -p 8443 a3qEXAMPLEsffp-ats.iot.eu-west-1.amazonaws.com
   ```

1. If `nmap` displays an output similar to the following, `nmap` was able to connect successfully to your device data endpoint at the selected port\.

   ```
   Starting Nmap 7.92 ( https://nmap.org ) at 2022-02-18 16:23 Pacific Standard Time
   Nmap scan report for a3qEXAMPLEsffp-ats.iot.eu-west-1.amazonaws.com (xx.xxx.147.160)
   Host is up (0.036s latency).
   Other addresses for a3qEXAMPLEsffp-ats.iot.eu-west-1.amazonaws.com (not scanned): xx.xxx.134.144 xx.xxx.55.139 xx.xxx.110.235 xx.xxx.174.233 xx.xxx.74.65 xx.xxx.122.179 xx.xxx.127.126
   rDNS record for xx.xxx.147.160: ec2-EXAMPLE-160.eu-west-1.compute.amazonaws.com
   
   PORT     STATE SERVICE
   8443/tcp open  https-alt
   MAC Address: 00:11:22:33:44:55 (Cimsys)
   
   Nmap done: 1 IP address (1 host up) scanned in 0.91 seconds
   ```

1. If `nmap` didn't return a successful output, check the endpoint value to make sure you have the correct endpoint and check your device's connection with the internet\.

You can test other ports on your device data endpoint, such as port 443, the primary HTTPS port, by replacing the port used in step 1, `8443`, with the port that you want to test\.