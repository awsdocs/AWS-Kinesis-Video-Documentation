# UpdateStream<a name="API_UpdateStream"></a>

Updates stream metadata, such as the device name and media type\.

You must provide the stream name or the Amazon Resource Name \(ARN\) of the stream\.

To make sure that you have the latest version of the stream before updating it, you can specify the stream version\. Kinesis Video Streams assigns a version to each stream\. When you update a stream, Kinesis Video Streams assigns a new version number\. To get the latest stream version, use the `DescribeStream` API\. 

 `UpdateStream` is an asynchronous operation, and takes time to complete\.

## Request Syntax<a name="API_UpdateStream_RequestSyntax"></a>

```
POST /updateStream HTTP/1.1
Content-type: application/json

{
   "[CurrentVersion](#KinesisVideo-UpdateStream-request-CurrentVersion)": "string",
   "[DeviceName](#KinesisVideo-UpdateStream-request-DeviceName)": "string",
   "[MediaType](#KinesisVideo-UpdateStream-request-MediaType)": "string",
   "[StreamARN](#KinesisVideo-UpdateStream-request-StreamARN)": "string",
   "[StreamName](#KinesisVideo-UpdateStream-request-StreamName)": "string"
}
```

## URI Request Parameters<a name="API_UpdateStream_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_UpdateStream_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [CurrentVersion](#API_UpdateStream_RequestSyntax) **   <a name="KinesisVideo-UpdateStream-request-CurrentVersion"></a>
The version of the stream whose metadata you want to update\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `[a-zA-Z0-9]+`   
Required: Yes

 ** [DeviceName](#API_UpdateStream_RequestSyntax) **   <a name="KinesisVideo-UpdateStream-request-DeviceName"></a>
The name of the device that is writing to the stream\.   
 In the current implementation, Kinesis Video Streams does not use this name\. 
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: No

 ** [MediaType](#API_UpdateStream_RequestSyntax) **   <a name="KinesisVideo-UpdateStream-request-MediaType"></a>
The stream's media type\. Use `MediaType` to specify the type of content that the stream contains to the consumers of the stream\. For more information about media types, see [Media Types](http://www.iana.org/assignments/media-types/media-types.xhtml)\. If you choose to specify the `MediaType`, see [Naming Requirements](https://tools.ietf.org/html/rfc6838#section-4.2)\.  
To play video on the console, you must specify the correct video type\. For example, if the video in the stream is H\.264, specify `video/h264` as the `MediaType`\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `[\w\-\.\+]+/[\w\-\.\+]+(,[\w\-\.\+]+/[\w\-\.\+]+)*`   
Required: No

 ** [StreamARN](#API_UpdateStream_RequestSyntax) **   <a name="KinesisVideo-UpdateStream-request-StreamARN"></a>
The ARN of the stream whose metadata you want to update\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+`   
Required: No

 ** [StreamName](#API_UpdateStream_RequestSyntax) **   <a name="KinesisVideo-UpdateStream-request-StreamName"></a>
The name of the stream whose metadata you want to update\.  
The stream name is an identifier for the stream, and must be unique for each account and region\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: No

## Response Syntax<a name="API_UpdateStream_ResponseSyntax"></a>

```
HTTP/1.1 200
```

## Response Elements<a name="API_UpdateStream_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_UpdateStream_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ClientLimitExceededException**   
Kinesis Video Streams has throttled the request because you have exceeded the limit of allowed client calls\. Try making the call later\.  
HTTP Status Code: 400

 **InvalidArgumentException**   
The value for this input parameter is invalid\.  
HTTP Status Code: 400

 **NotAuthorizedException**   
The caller is not authorized to perform this operation\.  
HTTP Status Code: 401

 **ResourceInUseException**   
The signaling channel is currently not available for this operation\.  
HTTP Status Code: 400

 **ResourceNotFoundException**   
Amazon Kinesis Video Streams can't find the stream that you specified\.  
HTTP Status Code: 404

 **VersionMismatchException**   
The stream version that you specified is not the latest version\. To get the latest version, use the [DescribeStream](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_DescribeStream.html) API\.  
HTTP Status Code: 400

## See Also<a name="API_UpdateStream_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesisvideo-2017-09-30/UpdateStream) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesisvideo-2017-09-30/UpdateStream) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/UpdateStream) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/UpdateStream) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/UpdateStream) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesisvideo-2017-09-30/UpdateStream) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesisvideo-2017-09-30/UpdateStream) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesisvideo-2017-09-30/UpdateStream) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/UpdateStream) 