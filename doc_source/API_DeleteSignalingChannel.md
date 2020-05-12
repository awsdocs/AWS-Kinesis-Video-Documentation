# DeleteSignalingChannel<a name="API_DeleteSignalingChannel"></a>

Deletes a specified signaling channel\. `DeleteSignalingChannel` is an asynchronous operation\. If you don't specify the channel's current version, the most recent version is deleted\.

## Request Syntax<a name="API_DeleteSignalingChannel_RequestSyntax"></a>

```
POST /deleteSignalingChannel HTTP/1.1
Content-type: application/json

{
   "[ChannelARN](#KinesisVideo-DeleteSignalingChannel-request-ChannelARN)": "string",
   "[CurrentVersion](#KinesisVideo-DeleteSignalingChannel-request-CurrentVersion)": "string"
}
```

## URI Request Parameters<a name="API_DeleteSignalingChannel_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_DeleteSignalingChannel_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [ChannelARN](#API_DeleteSignalingChannel_RequestSyntax) **   <a name="KinesisVideo-DeleteSignalingChannel-request-ChannelARN"></a>
The Amazon Resource Name \(ARN\) of the signaling channel that you want to delete\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+`   
Required: Yes

 ** [CurrentVersion](#API_DeleteSignalingChannel_RequestSyntax) **   <a name="KinesisVideo-DeleteSignalingChannel-request-CurrentVersion"></a>
The current version of the signaling channel that you want to delete\. You can obtain the current version by invoking the `DescribeSignalingChannel` or `ListSignalingChannels` API operations\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `[a-zA-Z0-9]+`   
Required: No

## Response Syntax<a name="API_DeleteSignalingChannel_ResponseSyntax"></a>

```
HTTP/1.1 200
```

## Response Elements<a name="API_DeleteSignalingChannel_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_DeleteSignalingChannel_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **AccessDeniedException**   
You do not have required permissions to perform this operation\.  
HTTP Status Code: 401

 **ClientLimitExceededException**   
Kinesis Video Streams has throttled the request because you have exceeded the limit of allowed client calls\. Try making the call later\.  
HTTP Status Code: 400

 **InvalidArgumentException**   
The value for this input parameter is invalid\.  
HTTP Status Code: 400

 **ResourceInUseException**   
The signaling channel is currently not available for this operation\.  
HTTP Status Code: 400

 **ResourceNotFoundException**   
Amazon Kinesis Video Streams can't find the stream that you specified\.  
HTTP Status Code: 404

 **VersionMismatchException**   
The stream version that you specified is not the latest version\. To get the latest version, use the [DescribeStream](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_DescribeStream.html) API\.  
HTTP Status Code: 400

## See Also<a name="API_DeleteSignalingChannel_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesisvideo-2017-09-30/DeleteSignalingChannel) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesisvideo-2017-09-30/DeleteSignalingChannel) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/DeleteSignalingChannel) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/DeleteSignalingChannel) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/DeleteSignalingChannel) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesisvideo-2017-09-30/DeleteSignalingChannel) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesisvideo-2017-09-30/DeleteSignalingChannel) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesisvideo-2017-09-30/DeleteSignalingChannel) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/DeleteSignalingChannel) 