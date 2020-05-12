# GetSignalingChannelEndpoint<a name="API_GetSignalingChannelEndpoint"></a>

Provides an endpoint for the specified signaling channel to send and receive messages\. This API uses the `SingleMasterChannelEndpointConfiguration` input parameter, which consists of the `Protocols` and `Role` properties\.

 `Protocols` is used to determine the communication mechanism\. For example, if you specify `WSS` as the protocol, this API produces a secure websocket endpoint\. If you specify `HTTPS` as the protocol, this API generates an HTTPS endpoint\. 

 `Role` determines the messaging permissions\. A `MASTER` role results in this API generating an endpoint that a client can use to communicate with any of the viewers on the channel\. A `VIEWER` role results in this API generating an endpoint that a client can use to communicate only with a `MASTER`\. 

## Request Syntax<a name="API_GetSignalingChannelEndpoint_RequestSyntax"></a>

```
POST /getSignalingChannelEndpoint HTTP/1.1
Content-type: application/json

{
   "[ChannelARN](#KinesisVideo-GetSignalingChannelEndpoint-request-ChannelARN)": "string",
   "[SingleMasterChannelEndpointConfiguration](#KinesisVideo-GetSignalingChannelEndpoint-request-SingleMasterChannelEndpointConfiguration)": { 
      "[Protocols](API_SingleMasterChannelEndpointConfiguration.md#KinesisVideo-Type-SingleMasterChannelEndpointConfiguration-Protocols)": [ "string" ],
      "[Role](API_SingleMasterChannelEndpointConfiguration.md#KinesisVideo-Type-SingleMasterChannelEndpointConfiguration-Role)": "string"
   }
}
```

## URI Request Parameters<a name="API_GetSignalingChannelEndpoint_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_GetSignalingChannelEndpoint_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [ChannelARN](#API_GetSignalingChannelEndpoint_RequestSyntax) **   <a name="KinesisVideo-GetSignalingChannelEndpoint-request-ChannelARN"></a>
The Amazon Resource Name \(ARN\) of the signalling channel for which you want to get an endpoint\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+`   
Required: Yes

 ** [SingleMasterChannelEndpointConfiguration](#API_GetSignalingChannelEndpoint_RequestSyntax) **   <a name="KinesisVideo-GetSignalingChannelEndpoint-request-SingleMasterChannelEndpointConfiguration"></a>
A structure containing the endpoint configuration for the `SINGLE_MASTER` channel type\.  
Type: [SingleMasterChannelEndpointConfiguration](API_SingleMasterChannelEndpointConfiguration.md) object  
Required: No

## Response Syntax<a name="API_GetSignalingChannelEndpoint_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-type: application/json

{
   "[ResourceEndpointList](#KinesisVideo-GetSignalingChannelEndpoint-response-ResourceEndpointList)": [ 
      { 
         "[Protocol](API_ResourceEndpointListItem.md#KinesisVideo-Type-ResourceEndpointListItem-Protocol)": "string",
         "[ResourceEndpoint](API_ResourceEndpointListItem.md#KinesisVideo-Type-ResourceEndpointListItem-ResourceEndpoint)": "string"
      }
   ]
}
```

## Response Elements<a name="API_GetSignalingChannelEndpoint_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [ResourceEndpointList](#API_GetSignalingChannelEndpoint_ResponseSyntax) **   <a name="KinesisVideo-GetSignalingChannelEndpoint-response-ResourceEndpointList"></a>
A list of endpoints for the specified signaling channel\.  
Type: Array of [ResourceEndpointListItem](API_ResourceEndpointListItem.md) objects

## Errors<a name="API_GetSignalingChannelEndpoint_Errors"></a>

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

## See Also<a name="API_GetSignalingChannelEndpoint_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesisvideo-2017-09-30/GetSignalingChannelEndpoint) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesisvideo-2017-09-30/GetSignalingChannelEndpoint) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/GetSignalingChannelEndpoint) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/GetSignalingChannelEndpoint) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/GetSignalingChannelEndpoint) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesisvideo-2017-09-30/GetSignalingChannelEndpoint) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesisvideo-2017-09-30/GetSignalingChannelEndpoint) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesisvideo-2017-09-30/GetSignalingChannelEndpoint) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/GetSignalingChannelEndpoint) 