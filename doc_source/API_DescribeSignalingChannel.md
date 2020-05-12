# DescribeSignalingChannel<a name="API_DescribeSignalingChannel"></a>

Returns the most current information about the signaling channel\. You must specify either the name or the Amazon Resource Name \(ARN\) of the channel that you want to describe\.

## Request Syntax<a name="API_DescribeSignalingChannel_RequestSyntax"></a>

```
POST /describeSignalingChannel HTTP/1.1
Content-type: application/json

{
   "[ChannelARN](#KinesisVideo-DescribeSignalingChannel-request-ChannelARN)": "string",
   "[ChannelName](#KinesisVideo-DescribeSignalingChannel-request-ChannelName)": "string"
}
```

## URI Request Parameters<a name="API_DescribeSignalingChannel_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_DescribeSignalingChannel_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [ChannelARN](#API_DescribeSignalingChannel_RequestSyntax) **   <a name="KinesisVideo-DescribeSignalingChannel-request-ChannelARN"></a>
The ARN of the signaling channel that you want to describe\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+`   
Required: No

 ** [ChannelName](#API_DescribeSignalingChannel_RequestSyntax) **   <a name="KinesisVideo-DescribeSignalingChannel-request-ChannelName"></a>
The name of the signaling channel that you want to describe\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: No

## Response Syntax<a name="API_DescribeSignalingChannel_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-type: application/json

{
   "[ChannelInfo](#KinesisVideo-DescribeSignalingChannel-response-ChannelInfo)": { 
      "[ChannelARN](API_ChannelInfo.md#KinesisVideo-Type-ChannelInfo-ChannelARN)": "string",
      "[ChannelName](API_ChannelInfo.md#KinesisVideo-Type-ChannelInfo-ChannelName)": "string",
      "[ChannelStatus](API_ChannelInfo.md#KinesisVideo-Type-ChannelInfo-ChannelStatus)": "string",
      "[ChannelType](API_ChannelInfo.md#KinesisVideo-Type-ChannelInfo-ChannelType)": "string",
      "[CreationTime](API_ChannelInfo.md#KinesisVideo-Type-ChannelInfo-CreationTime)": number,
      "[SingleMasterConfiguration](API_ChannelInfo.md#KinesisVideo-Type-ChannelInfo-SingleMasterConfiguration)": { 
         "[MessageTtlSeconds](API_SingleMasterConfiguration.md#KinesisVideo-Type-SingleMasterConfiguration-MessageTtlSeconds)": number
      },
      "[Version](API_ChannelInfo.md#KinesisVideo-Type-ChannelInfo-Version)": "string"
   }
}
```

## Response Elements<a name="API_DescribeSignalingChannel_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [ChannelInfo](#API_DescribeSignalingChannel_ResponseSyntax) **   <a name="KinesisVideo-DescribeSignalingChannel-response-ChannelInfo"></a>
A structure that encapsulates the specified signaling channel's metadata and properties\.  
Type: [ChannelInfo](API_ChannelInfo.md) object

## Errors<a name="API_DescribeSignalingChannel_Errors"></a>

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

 **ResourceNotFoundException**   
Amazon Kinesis Video Streams can't find the stream that you specified\.  
HTTP Status Code: 404

## See Also<a name="API_DescribeSignalingChannel_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesisvideo-2017-09-30/DescribeSignalingChannel) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesisvideo-2017-09-30/DescribeSignalingChannel) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/DescribeSignalingChannel) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/DescribeSignalingChannel) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/DescribeSignalingChannel) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesisvideo-2017-09-30/DescribeSignalingChannel) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesisvideo-2017-09-30/DescribeSignalingChannel) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesisvideo-2017-09-30/DescribeSignalingChannel) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/DescribeSignalingChannel) 