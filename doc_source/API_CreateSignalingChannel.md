# CreateSignalingChannel<a name="API_CreateSignalingChannel"></a>

Creates a signaling channel\. 

 `CreateSignalingChannel` is an asynchronous operation\.

## Request Syntax<a name="API_CreateSignalingChannel_RequestSyntax"></a>

```
POST /createSignalingChannel HTTP/1.1
Content-type: application/json

{
   "[ChannelName](#KinesisVideo-CreateSignalingChannel-request-ChannelName)": "string",
   "[ChannelType](#KinesisVideo-CreateSignalingChannel-request-ChannelType)": "string",
   "[SingleMasterConfiguration](#KinesisVideo-CreateSignalingChannel-request-SingleMasterConfiguration)": { 
      "[MessageTtlSeconds](API_SingleMasterConfiguration.md#KinesisVideo-Type-SingleMasterConfiguration-MessageTtlSeconds)": number
   },
   "[Tags](#KinesisVideo-CreateSignalingChannel-request-Tags)": [ 
      { 
         "[Key](API_Tag.md#KinesisVideo-Type-Tag-Key)": "string",
         "[Value](API_Tag.md#KinesisVideo-Type-Tag-Value)": "string"
      }
   ]
}
```

## URI Request Parameters<a name="API_CreateSignalingChannel_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_CreateSignalingChannel_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [ChannelName](#API_CreateSignalingChannel_RequestSyntax) **   <a name="KinesisVideo-CreateSignalingChannel-request-ChannelName"></a>
A name for the signaling channel that you are creating\. It must be unique for each AWS account and AWS Region\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: Yes

 ** [ChannelType](#API_CreateSignalingChannel_RequestSyntax) **   <a name="KinesisVideo-CreateSignalingChannel-request-ChannelType"></a>
A type of the signaling channel that you are creating\. Currently, `SINGLE_MASTER` is the only supported channel type\.   
Type: String  
Valid Values:` SINGLE_MASTER`   
Required: No

 ** [SingleMasterConfiguration](#API_CreateSignalingChannel_RequestSyntax) **   <a name="KinesisVideo-CreateSignalingChannel-request-SingleMasterConfiguration"></a>
A structure containing the configuration for the `SINGLE_MASTER` channel type\.   
Type: [SingleMasterConfiguration](API_SingleMasterConfiguration.md) object  
Required: No

 ** [Tags](#API_CreateSignalingChannel_RequestSyntax) **   <a name="KinesisVideo-CreateSignalingChannel-request-Tags"></a>
A set of tags \(key\-value pairs\) that you want to associate with this channel\.  
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.  
Required: No

## Response Syntax<a name="API_CreateSignalingChannel_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-type: application/json

{
   "[ChannelARN](#KinesisVideo-CreateSignalingChannel-response-ChannelARN)": "string"
}
```

## Response Elements<a name="API_CreateSignalingChannel_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [ChannelARN](#API_CreateSignalingChannel_ResponseSyntax) **   <a name="KinesisVideo-CreateSignalingChannel-response-ChannelARN"></a>
The Amazon Resource Name \(ARN\) of the created channel\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+` 

## Errors<a name="API_CreateSignalingChannel_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **AccessDeniedException**   
You do not have required permissions to perform this operation\.  
HTTP Status Code: 401

 **AccountChannelLimitExceededException**   
You have reached the maximum limit of active signaling channels for this AWS account in this region\.  
HTTP Status Code: 400

 **ClientLimitExceededException**   
Kinesis Video Streams has throttled the request because you have exceeded the limit of allowed client calls\. Try making the call later\.  
HTTP Status Code: 400

 **InvalidArgumentException**   
The value for this input parameter is invalid\.  
HTTP Status Code: 400

 **ResourceInUseException**   
The signaling channel is currently not available for this operation\.  
HTTP Status Code: 400

 **TagsPerResourceExceededLimitException**   
You have exceeded the limit of tags that you can associate with the resource\. Kinesis video streams support up to 50 tags\.   
HTTP Status Code: 400

## See Also<a name="API_CreateSignalingChannel_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesisvideo-2017-09-30/CreateSignalingChannel) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesisvideo-2017-09-30/CreateSignalingChannel) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/CreateSignalingChannel) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/CreateSignalingChannel) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/CreateSignalingChannel) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesisvideo-2017-09-30/CreateSignalingChannel) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesisvideo-2017-09-30/CreateSignalingChannel) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesisvideo-2017-09-30/CreateSignalingChannel) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/CreateSignalingChannel) 