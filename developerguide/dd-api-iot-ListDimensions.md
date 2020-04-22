# ListDimensions<a name="dd-api-iot-ListDimensions"></a>

Lists the dimensions defined in your AWS account that you can use in your AWS IoT Device Defender security profiles\.

To see all security profiles where a dimension is applied, use [ListSecurityProfiles](dd-api-iot-ListSecurityProfiles.md)\.

For more information, see [ListDimensions](https://docs.aws.amazon.com/iot/latest/apireference/API_ListDimensions.html) in the API reference\.

**Synopsis:**

```
aws iot  list-dimensions \
    [--next-token <value>] \
    [--max-results <value>]  \
    [--cli-input-json <value>] \
    [--generate-cli-skeleton]
```

**Example**  
This example lists all dimensions defined for your AWS account\.  

```
aws iot list-dimensions
```
`cli-input-json` format:  

```
{
  "nextToken": "string",
  "maxResults": "integer"
}
```

**Output:**

```
{
   "dimensionNames": [ "string" ],
   "nextToken": "string"
}
```