# UpdateSignalingChannel<a name="API_UpdateSignalingChannel"></a>

Updates the existing signaling channel\. This is an asynchronous operation and takes time to complete\. 

If the `MessageTtlSeconds` value is updated \(either increased or reduced\), it only applies to new messages sent via this channel after it's been updated\. Existing messages are still expired as per the previous `MessageTtlSeconds` value\.

## Request Syntax<a name="API_UpdateSignalingChannel_RequestSyntax"></a>

```
POST /updateSignalingChannel HTTP/1.1
Content-type: application/json

{
   "[ChannelARN](#KinesisVideo-UpdateSignalingChannel-request-ChannelARN)": "string",
   "[CurrentVersion](#KinesisVideo-UpdateSignalingChannel-request-CurrentVersion)": "string",
   "[SingleMasterConfiguration](#KinesisVideo-UpdateSignalingChannel-request-SingleMasterConfiguration)": { 
      "[MessageTtlSeconds](API_SingleMasterConfiguration.md#KinesisVideo-Type-SingleMasterConfiguration-MessageTtlSeconds)": number
   }
}
```

## URI Request Parameters<a name="API_UpdateSignalingChannel_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_UpdateSignalingChannel_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [ChannelARN](#API_UpdateSignalingChannel_RequestSyntax) **   <a name="KinesisVideo-UpdateSignalingChannel-request-ChannelARN"></a>
The Amazon Resource Name \(ARN\) of the signaling channel that you want to update\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+`   
Required: Yes

 ** [CurrentVersion](#API_UpdateSignalingChannel_RequestSyntax) **   <a name="KinesisVideo-UpdateSignalingChannel-request-CurrentVersion"></a>
The current version of the signaling channel that you want to update\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `[a-zA-Z0-9]+`   
Required: Yes

 ** [SingleMasterConfiguration](#API_UpdateSignalingChannel_RequestSyntax) **   <a name="KinesisVideo-UpdateSignalingChannel-request-SingleMasterConfiguration"></a>
The structure containing the configuration for the `SINGLE_MASTER` type of the signaling channel that you want to update\.   
Type: [SingleMasterConfiguration](API_SingleMasterConfiguration.md) object  
Required: No

## Response Syntax<a name="API_UpdateSignalingChannel_ResponseSyntax"></a>

```
HTTP/1.1 200
```

## Response Elements<a name="API_UpdateSignalingChannel_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_UpdateSignalingChannel_Errors"></a>

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

## See Also<a name="API_UpdateSignalingChannel_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesisvideo-2017-09-30/UpdateSignalingChannel) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesisvideo-2017-09-30/UpdateSignalingChannel) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/UpdateSignalingChannel) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/UpdateSignalingChannel) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/UpdateSignalingChannel) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesisvideo-2017-09-30/UpdateSignalingChannel) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesisvideo-2017-09-30/UpdateSignalingChannel) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesisvideo-2017-09-30/UpdateSignalingChannel) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/UpdateSignalingChannel) 