# ListSignalingChannels<a name="API_ListSignalingChannels"></a>

Returns an array of `ChannelInfo` objects\. Each object describes a signaling channel\. To retrieve only those channels that satisfy a specific condition, you can specify a `ChannelNameCondition`\.

## Request Syntax<a name="API_ListSignalingChannels_RequestSyntax"></a>

```
POST /listSignalingChannels HTTP/1.1
Content-type: application/json

{
   "[ChannelNameCondition](#KinesisVideo-ListSignalingChannels-request-ChannelNameCondition)": { 
      "[ComparisonOperator](API_ChannelNameCondition.md#KinesisVideo-Type-ChannelNameCondition-ComparisonOperator)": "string",
      "[ComparisonValue](API_ChannelNameCondition.md#KinesisVideo-Type-ChannelNameCondition-ComparisonValue)": "string"
   },
   "[MaxResults](#KinesisVideo-ListSignalingChannels-request-MaxResults)": number,
   "[NextToken](#KinesisVideo-ListSignalingChannels-request-NextToken)": "string"
}
```

## URI Request Parameters<a name="API_ListSignalingChannels_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_ListSignalingChannels_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [ChannelNameCondition](#API_ListSignalingChannels_RequestSyntax) **   <a name="KinesisVideo-ListSignalingChannels-request-ChannelNameCondition"></a>
Optional: Returns only the channels that satisfy a specific condition\.  
Type: [ChannelNameCondition](API_ChannelNameCondition.md) object  
Required: No

 ** [MaxResults](#API_ListSignalingChannels_RequestSyntax) **   <a name="KinesisVideo-ListSignalingChannels-request-MaxResults"></a>
The maximum number of channels to return in the response\. The default is 500\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 10000\.  
Required: No

 ** [NextToken](#API_ListSignalingChannels_RequestSyntax) **   <a name="KinesisVideo-ListSignalingChannels-request-NextToken"></a>
If you specify this parameter, when the result of a `ListSignalingChannels` operation is truncated, the call returns the `NextToken` in the response\. To get another batch of channels, provide this token in your next request\.  
Type: String  
Length Constraints: Minimum length of 0\. Maximum length of 512\.  
Pattern: `[a-zA-Z0-9+/=]*`   
Required: No

## Response Syntax<a name="API_ListSignalingChannels_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-type: application/json

{
   "[ChannelInfoList](#KinesisVideo-ListSignalingChannels-response-ChannelInfoList)": [ 
      { 
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
   ],
   "[NextToken](#KinesisVideo-ListSignalingChannels-response-NextToken)": "string"
}
```

## Response Elements<a name="API_ListSignalingChannels_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [ChannelInfoList](#API_ListSignalingChannels_ResponseSyntax) **   <a name="KinesisVideo-ListSignalingChannels-response-ChannelInfoList"></a>
An array of `ChannelInfo` objects\.  
Type: Array of [ChannelInfo](API_ChannelInfo.md) objects

 ** [NextToken](#API_ListSignalingChannels_ResponseSyntax) **   <a name="KinesisVideo-ListSignalingChannels-response-NextToken"></a>
If the response is truncated, the call returns this element with a token\. To get the next batch of streams, use this token in your next request\.  
Type: String  
Length Constraints: Minimum length of 0\. Maximum length of 512\.  
Pattern: `[a-zA-Z0-9+/=]*` 

## Errors<a name="API_ListSignalingChannels_Errors"></a>

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

## See Also<a name="API_ListSignalingChannels_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesisvideo-2017-09-30/ListSignalingChannels) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesisvideo-2017-09-30/ListSignalingChannels) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/ListSignalingChannels) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/ListSignalingChannels) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/ListSignalingChannels) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesisvideo-2017-09-30/ListSignalingChannels) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesisvideo-2017-09-30/ListSignalingChannels) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesisvideo-2017-09-30/ListSignalingChannels) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/ListSignalingChannels) 