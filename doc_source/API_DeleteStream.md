# DeleteStream<a name="API_DeleteStream"></a>

Deletes a Kinesis video stream and the data contained in the stream\. 

This method marks the stream for deletion, and makes the data in the stream inaccessible immediately\.

 

 To ensure that you have the latest version of the stream before deleting it, you can specify the stream version\. Kinesis Video Streams assigns a version to each stream\. When you update a stream, Kinesis Video Streams assigns a new version number\. To get the latest stream version, use the `DescribeStream` API\. 

This operation requires permission for the `KinesisVideo:DeleteStream` action\.

## Request Syntax<a name="API_DeleteStream_RequestSyntax"></a>

```
POST /deleteStream HTTP/1.1
Content-type: application/json

{
   "[CurrentVersion](#KinesisVideo-DeleteStream-request-CurrentVersion)": "string",
   "[StreamARN](#KinesisVideo-DeleteStream-request-StreamARN)": "string"
}
```

## URI Request Parameters<a name="API_DeleteStream_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_DeleteStream_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [CurrentVersion](#API_DeleteStream_RequestSyntax) **   <a name="KinesisVideo-DeleteStream-request-CurrentVersion"></a>
Optional: The version of the stream that you want to delete\.   
Specify the version as a safeguard to ensure that your are deleting the correct stream\. To get the stream version, use the `DescribeStream` API\.  
If not specified, only the `CreationTime` is checked before deleting the stream\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `[a-zA-Z0-9]+`   
Required: No

 ** [StreamARN](#API_DeleteStream_RequestSyntax) **   <a name="KinesisVideo-DeleteStream-request-StreamARN"></a>
The Amazon Resource Name \(ARN\) of the stream that you want to delete\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+`   
Required: Yes

## Response Syntax<a name="API_DeleteStream_ResponseSyntax"></a>

```
HTTP/1.1 200
```

## Response Elements<a name="API_DeleteStream_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_DeleteStream_Errors"></a>

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

## See Also<a name="API_DeleteStream_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesisvideo-2017-09-30/DeleteStream) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesisvideo-2017-09-30/DeleteStream) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/DeleteStream) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/DeleteStream) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/DeleteStream) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesisvideo-2017-09-30/DeleteStream) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesisvideo-2017-09-30/DeleteStream) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesisvideo-2017-09-30/DeleteStream) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/DeleteStream) 