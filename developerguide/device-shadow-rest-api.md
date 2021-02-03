# Device Shadow REST API<a name="device-shadow-rest-api"></a>

A shadow exposes the following URI for updating state information:

```
https://endpoint/things/thingName/shadow
```

The endpoint is specific to your AWS account\. To find your endpoint, you can:
+ Use the [describe\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/iot/describe-endpoint.html) command from the AWS CLI\.
+ Use the AWS IoT console settings\. In **Settings**, the endpoint is listed under **Custom endpoint**
+ Use the AWS IoT console thing details\. Open **Manage**\. Under **Manage**, choose **Things** and, in the list of things, select and open a thing\. In the left nav of the Thing detail page, choose **Interact** and view the endpoint URI in the **HTTPS** section of the page\.

The format of the endpoint is as follows:

```
identifier.iot.region.amazonaws.com
```

The shadow REST API follows the same HTTPS protocols/port mappings as described in [Device communication protocols](protocols.md)\.

**Topics**
+ [GetThingShadow](#API_GetThingShadow)
+ [UpdateThingShadow](#API_UpdateThingShadow)
+ [DeleteThingShadow](#API_DeleteThingShadow)
+ [ListNamedShadowsForThing](#API_ListNamedShadowsForThing)

## GetThingShadow<a name="API_GetThingShadow"></a>

Gets the shadow for the specified thing\.

The response state document includes the delta between the `desired` and `reported` states\.

**Request**  
The request includes the standard HTTP headers plus the following URI:

```
HTTP GET https://endpoint/things/thingName/shadow?name=shadowName
Request body: (none)
```

The `name` query parameter is not required for unnamed \(classic\) shadows\.

**Response**  
Upon success, the response includes the standard HTTP headers plus the following code and body:

```
HTTP 200
Response Body: response state document
```

For more information, see [Example Response State Document](device-shadow-document.md#device-shadow-example-response-json)\.

**Authorization**  
Retrieving a shadow requires a policy that allows the caller to perform the `iot:GetThingShadow` action\. The Device Shadow service accepts two forms of authentication: Signature Version 4 with IAM credentials or TLS mutual authentication with a client certificate\.

The following is an example policy that allows a caller to retrieve a device's shadow:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "iot:GetThingShadow",
      "Resource": [
        "arn:aws:iot:region:account:thing/thing"
      ]
    }
  ]
}
```

## UpdateThingShadow<a name="API_UpdateThingShadow"></a>

Updates the shadow for the specified thing\.

Updates affect only the fields specified in the request state document\. Any field with a value of `null` is removed from the device's shadow\.

**Request**  
The request includes the standard HTTP headers plus the following URI and body:

```
HTTP POST https://endpoint/things/thingName/shadow?name=shadowName
Request body: request state document
```

The `name` query parameter is not required for unnamed \(classic\) shadows\.

For more information, see [Example Request State Document](device-shadow-document.md#device-shadow-example-request-json)\.

**Response**  
Upon success, the response includes the standard HTTP headers plus the following code and body:

```
HTTP 200
Response body: response state document
```

For more information, see [Example Response State Document](device-shadow-document.md#device-shadow-example-response-json)\.

**Authorization**  
Updating a shadow requires a policy that allows the caller to perform the `iot:UpdateThingShadow` action\. The Device Shadow service accepts two forms of authentication: Signature Version 4 with IAM credentials or TLS mutual authentication with a client certificate\.

The following is an example policy that allows a caller to update a device's shadow:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "iot:UpdateThingShadow",
      "Resource": [
        "arn:aws:iot:region:account:thing/thing"
      ]
    }
  ]
}
```

## DeleteThingShadow<a name="API_DeleteThingShadow"></a>

Deletes the shadow for the specified thing\.

**Request**  
The request includes the standard HTTP headers plus the following URI:

```
HTTP DELETE https://endpoint/things/thingName/shadow?name=shadowName
Request body: (none)
```

The `name` query parameter is not required for unnamed \(classic\) shadows\.

**Response**  
Upon success, the response includes the standard HTTP headers plus the following code and body:

```
HTTP 200
Response body: Empty response state document
```

Note that deleting a shadow does not reset its version number to 0\.

**Authorization**  
Deleting a device's shadow requires a policy that allows the caller to perform the `iot:DeleteThingShadow` action\. The Device Shadow service accepts two forms of authentication: Signature Version 4 with IAM credentials or TLS mutual authentication with a client certificate\.

The following is an example policy that allows a caller to delete a device's shadow:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "iot:DeleteThingShadow",
      "Resource": [
        "arn:aws:iot:region:account:thing/thing"
      ]
    }
  ]
}
```

## ListNamedShadowsForThing<a name="API_ListNamedShadowsForThing"></a>

Lists the shadows for the specified thing\.

**Request**  
The request includes the standard HTTP headers plus the following URI:

```
HTTP GET /api/things/shadow/ListNamedShadowsForThing/thingName?nextToken=nextToken&pageSize=pageSize
Request body: (none)
```

nextToken  
The token to retrieve the next set of results\.  
This value is returned on paged results and is used in the call that returns the next page\.

pageSize  
The number of shadow names to return in each call\. See also `nextToken`\.

thingName  
The name of the thing for which to list the named shadows\.

**Response**  
Upon success, the response includes the standard HTTP headers plus the following response code and a [Shadow name list response document](device-shadow-document.md#device-shadow-list-json)

**Note**  
The unnamed \(classic\) shadow does not appear in this list\.

```
HTTP 200
Response body: Shadow name list document
```

**Authorization**  
Deleting a device's shadow requires a policy that allows the caller to perform the `iot:ListNamedShadowsForThing` action\. The Device Shadow service accepts two forms of authentication: Signature Version 4 with IAM credentials or TLS mutual authentication with a client certificate\.

The following is an example policy that allows a caller to list a thing's named shadows:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "iot:ListNamedShadowsForThing",
      "Resource": [
        "arn:aws:iot:region:account:thing/thing"
      ]
    }
  ]
}
```