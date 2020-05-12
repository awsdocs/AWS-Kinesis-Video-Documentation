# CreateStream<a name="API_CreateStream"></a>

Creates a new Kinesis video stream\. 

When you create a new stream, Kinesis Video Streams assigns it a version number\. When you change the stream's metadata, Kinesis Video Streams updates the version\. 

 `CreateStream` is an asynchronous operation\.

For information about how the service works, see [How it Works](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/how-it-works.html)\. 

You must have permissions for the `KinesisVideo:CreateStream` action\.

## Request Syntax<a name="API_CreateStream_RequestSyntax"></a>

```
POST /createStream HTTP/1.1
Content-type: application/json

{
   "[DataRetentionInHours](#KinesisVideo-CreateStream-request-DataRetentionInHours)": number,
   "[DeviceName](#KinesisVideo-CreateStream-request-DeviceName)": "string",
   "[KmsKeyId](#KinesisVideo-CreateStream-request-KmsKeyId)": "string",
   "[MediaType](#KinesisVideo-CreateStream-request-MediaType)": "string",
   "[StreamName](#KinesisVideo-CreateStream-request-StreamName)": "string",
   "[Tags](#KinesisVideo-CreateStream-request-Tags)": { 
      "string" : "string" 
   }
}
```

## URI Request Parameters<a name="API_CreateStream_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_CreateStream_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [DataRetentionInHours](#API_CreateStream_RequestSyntax) **   <a name="KinesisVideo-CreateStream-request-DataRetentionInHours"></a>
The number of hours that you want to retain the data in the stream\. Kinesis Video Streams retains the data in a data store that is associated with the stream\.  
The default value is 0, indicating that the stream does not persist data\.  
When the `DataRetentionInHours` value is 0, consumers can still consume the fragments that remain in the service host buffer, which has a retention time limit of 5 minutes and a retention memory limit of 200 MB\. Fragments are removed from the buffer when either limit is reached\.  
Type: Integer  
Valid Range: Minimum value of 0\.  
Required: No

 ** [DeviceName](#API_CreateStream_RequestSyntax) **   <a name="KinesisVideo-CreateStream-request-DeviceName"></a>
The name of the device that is writing to the stream\.   
In the current implementation, Kinesis Video Streams does not use this name\.
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: No

 ** [KmsKeyId](#API_CreateStream_RequestSyntax) **   <a name="KinesisVideo-CreateStream-request-KmsKeyId"></a>
The ID of the AWS Key Management Service \(AWS KMS\) key that you want Kinesis Video Streams to use to encrypt stream data\.  
If no key ID is specified, the default, Kinesis Video\-managed key \(`aws/kinesisvideo`\) is used\.  
 For more information, see [DescribeKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_DescribeKey.html#API_DescribeKey_RequestParameters)\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 2048\.  
Pattern: `.+`   
Required: No

 ** [MediaType](#API_CreateStream_RequestSyntax) **   <a name="KinesisVideo-CreateStream-request-MediaType"></a>
The media type of the stream\. Consumers of the stream can use this information when processing the stream\. For more information about media types, see [Media Types](http://www.iana.org/assignments/media-types/media-types.xhtml)\. If you choose to specify the `MediaType`, see [Naming Requirements](https://tools.ietf.org/html/rfc6838#section-4.2) for guidelines\.  
Example valid values include "video/h264" and "video/h264,audio/aac"\.  
This parameter is optional; the default value is `null` \(or empty in JSON\)\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `[\w\-\.\+]+/[\w\-\.\+]+(,[\w\-\.\+]+/[\w\-\.\+]+)*`   
Required: No

 ** [StreamName](#API_CreateStream_RequestSyntax) **   <a name="KinesisVideo-CreateStream-request-StreamName"></a>
A name for the stream that you are creating\.  
The stream name is an identifier for the stream, and must be unique for each account and region\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: Yes

 ** [Tags](#API_CreateStream_RequestSyntax) **   <a name="KinesisVideo-CreateStream-request-Tags"></a>
A list of tags to associate with the specified stream\. Each tag is a key\-value pair \(the value is optional\)\.  
Type: String to string map  
Key Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Key Pattern: `^([\p{L}\p{Z}\p{N}_.:/=+\-@]*)$`   
Value Length Constraints: Minimum length of 0\. Maximum length of 256\.  
Value Pattern: `[\p{L}\p{Z}\p{N}_.:/=+\-@]*`   
Required: No

## Response Syntax<a name="API_CreateStream_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-type: application/json

{
   "[StreamARN](#KinesisVideo-CreateStream-response-StreamARN)": "string"
}
```

## Response Elements<a name="API_CreateStream_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [StreamARN](#API_CreateStream_ResponseSyntax) **   <a name="KinesisVideo-CreateStream-response-StreamARN"></a>
The Amazon Resource Name \(ARN\) of the stream\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+` 

## Errors<a name="API_CreateStream_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **AccountStreamLimitExceededException**   
The number of streams created for the account is too high\.  
HTTP Status Code: 400

 **ClientLimitExceededException**   
Kinesis Video Streams has throttled the request because you have exceeded the limit of allowed client calls\. Try making the call later\.  
HTTP Status Code: 400

 **DeviceStreamLimitExceededException**   
Not implemented\.   
HTTP Status Code: 400

 **InvalidArgumentException**   
The value for this input parameter is invalid\.  
HTTP Status Code: 400

 **InvalidDeviceException**   
Not implemented\.  
HTTP Status Code: 400

 **ResourceInUseException**   
The signaling channel is currently not available for this operation\.  
HTTP Status Code: 400

 **TagsPerResourceExceededLimitException**   
You have exceeded the limit of tags that you can associate with the resource\. Kinesis video streams support up to 50 tags\.   
HTTP Status Code: 400

## See Also<a name="API_CreateStream_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesisvideo-2017-09-30/CreateStream) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesisvideo-2017-09-30/CreateStream) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/CreateStream) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/CreateStream) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/CreateStream) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesisvideo-2017-09-30/CreateStream) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesisvideo-2017-09-30/CreateStream) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesisvideo-2017-09-30/CreateStream) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/CreateStream) 