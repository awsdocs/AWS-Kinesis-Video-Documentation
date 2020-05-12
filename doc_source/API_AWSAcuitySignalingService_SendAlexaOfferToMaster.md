# SendAlexaOfferToMaster<a name="API_AWSAcuitySignalingService_SendAlexaOfferToMaster"></a>

This API allows you to connect WebRTC\-enabled devices with Alexa display devices\. When invoked, it sends the Alexa Session Description Protocol \(SDP\) offer to the master peer\. The offer is delivered as soon as the master is connected to the specified signaling channel\. This API returns the SDP answer from the connected master\. If the master is not connected to the signaling channel, redelivery requests are made until the message expires\.

## Request Syntax<a name="API_AWSAcuitySignalingService_SendAlexaOfferToMaster_RequestSyntax"></a>

```
POST /v1/send-alexa-offer-to-master HTTP/1.1
Content-type: application/json

{
   "[ChannelARN](#KinesisVideo-AWSAcuitySignalingService_SendAlexaOfferToMaster-request-ChannelARN)": "string",
   "[MessagePayload](#KinesisVideo-AWSAcuitySignalingService_SendAlexaOfferToMaster-request-MessagePayload)": "string",
   "[SenderClientId](#KinesisVideo-AWSAcuitySignalingService_SendAlexaOfferToMaster-request-SenderClientId)": "string"
}
```

## URI Request Parameters<a name="API_AWSAcuitySignalingService_SendAlexaOfferToMaster_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_AWSAcuitySignalingService_SendAlexaOfferToMaster_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [ChannelARN](#API_AWSAcuitySignalingService_SendAlexaOfferToMaster_RequestSyntax) **   <a name="KinesisVideo-AWSAcuitySignalingService_SendAlexaOfferToMaster-request-ChannelARN"></a>
The Amazon Resource Name \(ARN\) of the signaling channel by which Alexa and the master peer communicate\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+`   
Required: Yes

 ** [MessagePayload](#API_AWSAcuitySignalingService_SendAlexaOfferToMaster_RequestSyntax) **   <a name="KinesisVideo-AWSAcuitySignalingService_SendAlexaOfferToMaster-request-MessagePayload"></a>
The base64\-encoded SDP offer content\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 10000\.  
Pattern: `[a-zA-Z0-9+/=]+`   
Required: Yes

 ** [SenderClientId](#API_AWSAcuitySignalingService_SendAlexaOfferToMaster_RequestSyntax) **   <a name="KinesisVideo-AWSAcuitySignalingService_SendAlexaOfferToMaster-request-SenderClientId"></a>
The unique identifier for the sender client\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: Yes

## Response Syntax<a name="API_AWSAcuitySignalingService_SendAlexaOfferToMaster_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-type: application/json

{
   "[Answer](#KinesisVideo-AWSAcuitySignalingService_SendAlexaOfferToMaster-response-Answer)": "string"
}
```

## Response Elements<a name="API_AWSAcuitySignalingService_SendAlexaOfferToMaster_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [Answer](#API_AWSAcuitySignalingService_SendAlexaOfferToMaster_ResponseSyntax) **   <a name="KinesisVideo-AWSAcuitySignalingService_SendAlexaOfferToMaster-response-Answer"></a>
The base64\-encoded SDP answer content\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 10000\.

## Errors<a name="API_AWSAcuitySignalingService_SendAlexaOfferToMaster_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ClientLimitExceededException**   
Your request was throttled because you have exceeded the limit of allowed client calls\. Try making the call later\.  
HTTP Status Code: 400

 **InvalidArgumentException**   
The value for this input parameter is invalid\.  
HTTP Status Code: 400

 **NotAuthorizedException**   
The caller is not authorized to perform this operation\.  
HTTP Status Code: 401

 **ResourceNotFoundException**   
The specified resource is not found\.  
HTTP Status Code: 404

## See Also<a name="API_AWSAcuitySignalingService_SendAlexaOfferToMaster_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesis-video-signaling-2019-12-04/SendAlexaOfferToMaster) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesis-video-signaling-2019-12-04/SendAlexaOfferToMaster) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesis-video-signaling-2019-12-04/SendAlexaOfferToMaster) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesis-video-signaling-2019-12-04/SendAlexaOfferToMaster) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesis-video-signaling-2019-12-04/SendAlexaOfferToMaster) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesis-video-signaling-2019-12-04/SendAlexaOfferToMaster) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesis-video-signaling-2019-12-04/SendAlexaOfferToMaster) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesis-video-signaling-2019-12-04/SendAlexaOfferToMaster) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesis-video-signaling-2019-12-04/SendAlexaOfferToMaster) 