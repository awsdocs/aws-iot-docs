# Using AWS IoT MQTT\-based file delivery in devices<a name="mqtt-based-file-delivery-in-devices"></a>

To initiate the data transfer process, a device must receive an **initial data set**, which includes a stream ID at minimum\. You can use an [Jobs](iot-jobs.md) to schedule data transfer tasks for your devices by including the initial data set in the job document\. When a device receives the initial data set, it should then start the interaction with AWS IoT MQTT\-based file delivery\. To exchange data with AWS IoT MQTT\-based file delivery, a device should:
+ Use the MQTT protocol to subscribe to the [MQTT\-based file delivery topics](reserved-topics.md#reserved-topics-mqtt-based-file-delivery)\.
+ Send requests and then wait to receive the responses using MQTT messages\.

You can optionally include a stream file ID and a stream version in the initial data set\. Sending a stream file ID to a device can simplify the programming of the device's firmware/software, because it eliminates the need to make a `DescribeStream` request from the device to get this ID\. The device can specify the stream version in a `GetStream` request to enforce a consistency check in case the stream has been updated unexpectedly\.

## Use DescribeStream to get stream data<a name="mqtt-based-file-delivery-describe-stream"></a>

AWS IoT MQTT\-based file delivery provides the `DescribeStream` API to send stream data to a device\. The stream data returned by this API includes the stream ID, stream version, stream description and a list of stream files, each of which has a file ID and the file size in bytes\. With this information, a device can select arbitrary files to initiate the data transfer process\.

**Note**  
You don't need to use the `DescribeStream` API if your device receives all required stream file IDs in the initial data set\.

Follow these steps to make a `DescribeStream` request\.

1. Subscribe to the "accepted" topic filter `$aws/things/ThingName/streams/StreamId/description/json`\. 

1. Subscribe to the "rejected" topic filter `$aws/things/ThingName/streams/StreamId/rejected/json`\. 

1. Publish a `DescribeStream` request by sending a message to `$aws/things/ThingName/streams/StreamId/describe/json`\. 

1. If the request was accepted, your device receives a `DescribeStream` response on the "accepted" topic filter\.

1. If the request was rejected, your device receives the error response on the "rejected" topic filter\.

**Note**  
If you replace `json` with `cbor` in the topics and topic filters shown, your device receives messages in the CBOR format, which is more compact than JSON\.

### DescribeStream request<a name="mqtt-based-file-delivery-describe-stream-request"></a>

A typical `DescribeStream` request in JSON looks like the following example\.

```
{
    "c": "ec944cfb-1e3c-49ac-97de-9dc4aaad0039"
}
```
+ \(Optional\) "`c`" is the client token field\.

  The client token can't be longer than 64 bytes\. A client token that is longer than 64 bytes causes an error response and an `InvalidRequest` error message\.

### DescribeStream response<a name="mqtt-based-file-delivery-describe-stream-response"></a>

A `DescribeStream` response in JSON looks like the following example\.

```
{
    "c": "ec944cfb-1e3c-49ac-97de-9dc4aaad0039",
    "s": 1,
    "d": "This is the description of stream ABC.",
    "r": [
        {
            "f": 0,
            "z": 131072
        },
        {
            "f": 1,
            "z": 51200
        }
    ]
}
```
+ "`c`" is the client token field\. This is returned if it was given in the `DescribeStream` request\. Use the client token to associate the response with its request\.
+ "`s`" is the stream version as an integer\. You can use this version to perform a consistency check with your `GetStream` requests\.
+ "`r`" contains a list of the files in the stream\.
  + "`f`" is the stream file ID as an integer\.
  + "`z`" is the stream file size in number of bytes\.
+ "`d`" contains the description of the stream\.

## Get data blocks from a stream file<a name="mqtt-based-file-delivery-get-getstream"></a>

You can use the `GetStream` API so that a device can receive stream files in small data blocks, so it can be used by those devices that have constraints on processing large block sizes\. To receive an entire data file, a device might need to send or receive multiple requests and responses until all data blocks are received and processed\.

### GetStream request<a name="mqtt-based-file-delivery-getstream-request"></a>

Follow these steps to make a `GetStream` request\.

1. Subscribe to the "accepted" topic filter `$aws/things/ThingName/streams/StreamId/data/json`\. 

1. Subscribe to the "rejected" topic filter `$aws/things/ThingName/streams/StreamId/rejected/json`\. 

1. Publish a `GetStream` request to the topic `$aws/things/ThingName/streams/StreamId/get/json`\. 

1. If the request was accepted, your device will receive one or more `GetStream` responses on the "accepted" topic filter\. Each response message contains basic information and a data payload for a single block\.

1. Repeat steps 3 and 4 to receive all data blocks\. You must repeat these steps if the amount of data requested is larger than 128 KB\. You must program your device to use multiple `GetStream` requests to receive all of the data requested\.

1. If the request was rejected, your device will receive the error response on the "rejected" topic filter\.

**Note**  
If you replace "json" with "cbor" in the topics and topic filters shown, your device will receive messages in the CBOR format, which is more compact than JSON\.
AWS IoT MQTT\-based file delivery limits the size of a block to 128 KB\. If you make a request for a block that is more than 128 KB, the request will fail\.
You can make a request for multiple blocks whose total size is greater than 128 KB \(for example, if you make a request for 5 blocks of 32 KB each for a total of 160 KB of data\)\. In this case, the request doesn't fail, but your device must make multiple requests in order to receive all of the data requested\. The service will send additional blocks as your device makes additional requests\. We recommend that you continue with a new request only after the previous response has been correctly received and processed\.
Regardless of the total size of data requested, you should program your device to initiate retries when blocks are not received, or not received correctly\.

A typical `GetStream` request in JSON looks like the following example\.

```
{
    "c": "1bb8aaa1-5c18-4d21-80c2-0b44fee10380",
    "s": 1,
    "f": 0,
    "l": 4098,
    "o": 2,
    "n": 100,
    "b": "..."
}
```
+ \[optional\] "`c`" is the client token field\.

  The client token can be no longer than 64 bytes\. A client token that is longer than 64 bytes causes an error response and an `InvalidRequest` error message\.
+ \[optional\] "`s`" is the stream version field \(an integer\)\.

  MQTT\-based file delivery applies a consistency check based on this requested version and the latest stream version in the cloud\. If the stream version sent from a device in a `GetStream` request doesn't match the latest stream version in the cloud, the service sends an error response and a `VersionMismatch` error message\. Typically, a device receives the expected \(latest\) stream version in the initial data set or in the response to `DescribeStream`\.
+ "`f`" is the stream file ID \(an integer in the range 0 to 255\)\.

  The stream file ID is required when you create or update a stream using the AWS CLI or SDK\. If a device requests a stream file with an ID that doesn't exist, the service sends an error response and a `ResourceNotFound` error message\.
+ "`l`" is the data block size in bytes \(an integer in the range 256 to 131,072\)\.

  Refer to [Build a bitmap for a GetStream request](#mqtt-based-file-delivery-build-a-bitmap) for instructions on how to use the bitmap fields to specify what portion of the stream file will be returned in the `GetStream` response\. If a device specifies a block size that is out of range, the service sends an error response and a `BlockSizeOutOfBounds` error message\.
+ \[optional\] "`o`" is the offset of the block in the stream file \(an integer in the range 0 to 98,304\)\. 

  Refer to [Build a bitmap for a GetStream request](#mqtt-based-file-delivery-build-a-bitmap) for instructions on how to use the bitmap fields to specify what portion of the stream file will be returned in the `GetStream` response\. The maximum value of 98,304 is based on a 24 MB stream file size limit and 256 bytes for the minimum block size\. The default is 0 if not specified\.
+ \[optional\] "`n`" is the number of blocks requested \(an integer in the range 0 to 98,304\)\. 

  The "n" field specifies either \(1\) the number of blocks requested, or \(2\) when the bitmap field \("b"\) is used, a limit on the number of blocks that will be returned by the bitmap request\. This second use is optional\. If not defined, it defaults to 131072/*DataBlockSize*\.
+ \[optional\] "`b`" is a bitmap that represents the blocks being requested\.

  Using a bitmap, your device can request non\-consecutive blocks, which makes handling retries following an error more convenient\. Refer to [Build a bitmap for a GetStream request](#mqtt-based-file-delivery-build-a-bitmap) for instructions on how to use the bitmap fields to specify which portion of the stream file will be returned in the `GetStream` response\. For this field, convert the bitmap to a string representing the bitmap's value in hexadecimal notation\. The bitmap must be less than 12,288 bytes\.

### GetStream response<a name="mqtt-based-file-delivery-getstream-response"></a>

A `GetStream` response in JSON looks like this example for each data block that is requested\.

```
{
    "c": "1bb8aaa1-5c18-4d21-80c2-0b44fee10380",
    "f": 0,
    "l": 4098,
    "i": 2,
    "p": "..."
}
```
+ "`c`" is the client token field\. This is returned if it was given in the `GetStream` request\. Use the client token to associate the response with its request\.
+ "`f`" is the ID of the stream file to which the current data block payload belongs\.
+ "`l`" is the size of the data block payload in bytes\.
+ "`i`" is the ID of the data block contained in the payload\. Data blocks are numbered starting from 0\.
+ "`p`" contains the data block payload\. This field is a string which represents the value of the data block in hexadecimal notation\.

### Build a bitmap for a GetStream request<a name="mqtt-based-file-delivery-build-a-bitmap"></a>

You can use the bitmap field \(`b`\) in a `GetStream` request to get non\-consecutive blocks from a stream file\. This helps devices with limited RAM capacity deal with network delivery issues\. A device can request only those blocks that were not received or not received correctly\. The bitmap determines which blocks of the stream file will be returned\. For each bit which is set to 1 in the bitmap, a corresponding block of the stream file will be returned\.

Here's an example of how to specify a bitmap and its supporting fields in a `GetStream` request\. For example, you want to receive a stream file in chunks of 256 bytes \(the block size\)\. Think of each block of 256 bytes as having a number that specifies its position in the file, starting from 0\. So block 0 is the first block of 256 bytes in the file, block 1 is the second, and so on\. You want to request blocks 20, 21, 24 and 43 from the file\.

**Block offset**  
Because the first block is number 20, specify the offset \(field `o`\) as 20 to save space in the bitmap\.

**Number of blocks**  
To ensure that your device doesn't receive more blocks than it can handle with limited memory resources, you can specify the maximum number of blocks that should be returned in each message sent by MQTT\-based file delivery\. Note that this value is disregarded if the bitmap itself specifies less than this number of blocks, or if it would make the total size of the response messages sent by MQTT\-based file delivery greater than the service limit of 128 KB per `GetStream` request\.

**Block bitmap**  
The bitmap itself is an array of unsigned bytes expressed in hexadecimal notation, and included in the `GetStream` request as a string representation of the number\. But to construct this string, let's start by thinking of the bitmap as a long sequence of bits \(a binary number\)\. If a bit in this sequence is set to 1, the corresponding block from the stream file will be sent back to the device\. For our example, we want to receive blocks 20, 21, 24, and 43, so we must set bits 20, 21, 24, and 43 in our bitmap\. We can use the block offset to save space, so after we subtract the offset from each block number, we want to set bits 0, 1, 4, and 23, like the following example\.  

```
 1 1 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 
```
Taking one byte \(8 bits\) at a time, this is conventionally written as: "0b00010011", "0b00000000", and "0b10000000"\. Bit 0 shows up in our binary representation at the end of the first byte, and bit 23 at the beginning of the last\. This can be confusing unless you know the conventions\. The first byte contains bits 7\-0 \(in that order\), the second byte contains bits 15\-8, the third byte contains bits 23\-16, and so on\. In hexadecimal notation, this converts to "0x130080"\.  
You can convert the standard binary to hexadecimal notation\. Take four binary digits at a time and convert these to their hexadecimal equivalent\. For example, "0001" becomes "1", "0011" becomes "3" and so on\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/iot/latest/developerguide/images/blockBitmap.png)
Putting this all together, the JSON for our `GetStream` request looks like the following\.  

```
{
    "c" : "1",       // client token
    "s" : 1,         // expected stream version
    "l" : 256,       // block size
    "f" : 1,         // source file index id
    "o" : 20,        // block offset
    "n" : 32,        // number of blocks
    "b" : "0x130080" // A missing blockId bitmap starting from the offset
}
```

## Handling errors from AWS IoT MQTT\-based file delivery<a name="mqtt-based-file-delivery-errors"></a>

An error response that is sent to a device for both `DescribeStream` and `GetStream` APIs contains a client token, an error code and an error message\. A typical error response looks like the following example\.

```
{
    "o": "BlockSizeOutOfBounds", 
    "m": "The block size is out of bounds", 
    "c": "1bb8aaa1-5c18-4d21-80c2-0b44fee10380" 
}
```
+ "`o`" is the error code that indicates the reason an error occurred\. Refer to the error codes later in this section for more details\.
+ "`m`" is the error message that contains details of the error\.
+ "`c`" is the client token field\. This may be returned if it was given in the `DescribeStream` request\. You can use the client token to associate the response with its request\.

  The client token field is not always included in an error response\. When the client token given in the request isn't valid or is malformed, it's not returned in the error response\.

**Note**  
For backward compatibility, fields in the error response may be in non\-abbreviated form\. For example, the error code might be designated by either "code" or "o" fields and the client token field may be designated by either "clientToken" or "c" fields\. We recommend that you use the abbreviation form shown above\.

**InvalidTopic**  
The MQTT topic of the stream message is invalid\.

**InvalidJson**  
The Stream request is not a valid JSON document\.

**InvalidCbor**  
The Stream request is not valid CBOR document\.

**InvalidRequest**  
The request is generally identified as malformed\. For more information, see the error message\.

**Unauthorized**  
The request is not authorized to access the stream data files in the storage medium, such as Amazon S3\. For more information, see the error message\.

**BlockSizeOutOfBounds**  
The block size is out of bounds\. Refer to the "**MQTT\-based File Delivery**" section in [AWS IoT Core Service Quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#limits_iot)\.

**OffsetOutOfBounds**  
The offset is out of bounds\. Refer to the "**MQTT\-based File Delivery**" section in [AWS IoT Core Service Quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#limits_iot)\.

**BlockCountLimitExceeded**  
The number of request block\(s\) is out of bounds\. Refer to the "**MQTT\-based File Delivery**" section in [AWS IoT Core Service Quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#limits_iot)\.

**BlockBitmapLimitExceeded**  
The size of the request bitmap is out of bounds\. Refer to the "**MQTT\-based File Delivery**" section in [AWS IoT Core Service Quotas](https://docs.aws.amazon.com/general/latest/gr/iot-core.html#limits_iot)\.

**ResourceNotFound**  
The requested stream, files, file versions or blocks were not found\. Refer to the error message for more details\.

**VersionMismatch**  
The stream version in the request doesn't match with the stream version in the MQTT\-based file delivery feature\. This indicates that the stream data had been modified since the stream version was initially received by the device\.

**ETagMismatch**  
The S3 ETag in the stream doesn't match with the ETag of the latest S3 object version\.

**InternalError**  
An internal error occurred in MQTT\-based file delivery\.