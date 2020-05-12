# GetDataEndpoint<a name="API_GetDataEndpoint"></a>

Gets an endpoint for a specified stream for either reading or writing\. Use this endpoint in your application to read from the specified stream \(using the `GetMedia` or `GetMediaForFragmentList` operations\) or write to it \(using the `PutMedia` operation\)\. 

**Note**  
The returned endpoint does not have the API name appended\. The client needs to add the API name to the returned endpoint\.

In the request, specify the stream either by `StreamName` or `StreamARN`\.

## Request Syntax<a name="API_GetDataEndpoint_RequestSyntax"></a>

```
POST /getDataEndpoint HTTP/1.1
Content-type: application/json

{
   "[APIName](#KinesisVideo-GetDataEndpoint-request-APIName)": "string",
   "[StreamARN](#KinesisVideo-GetDataEndpoint-request-StreamARN)": "string",
   "[StreamName](#KinesisVideo-GetDataEndpoint-request-StreamName)": "string"
}
```

## URI Request Parameters<a name="API_GetDataEndpoint_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_GetDataEndpoint_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [APIName](#API_GetDataEndpoint_RequestSyntax) **   <a name="KinesisVideo-GetDataEndpoint-request-APIName"></a>
The name of the API action for which to get an endpoint\.  
Type: String  
Valid Values:` PUT_MEDIA | GET_MEDIA | LIST_FRAGMENTS | GET_MEDIA_FOR_FRAGMENT_LIST | GET_HLS_STREAMING_SESSION_URL | GET_DASH_STREAMING_SESSION_URL | GET_CLIP`   
Required: Yes

 ** [StreamARN](#API_GetDataEndpoint_RequestSyntax) **   <a name="KinesisVideo-GetDataEndpoint-request-StreamARN"></a>
The Amazon Resource Name \(ARN\) of the stream that you want to get the endpoint for\. You must specify either this parameter or a `StreamName` in the request\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+`   
Required: No

 ** [StreamName](#API_GetDataEndpoint_RequestSyntax) **   <a name="KinesisVideo-GetDataEndpoint-request-StreamName"></a>
The name of the stream that you want to get the endpoint for\. You must specify either this parameter or a `StreamARN` in the request\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: No

## Response Syntax<a name="API_GetDataEndpoint_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-type: application/json

{
   "[DataEndpoint](#KinesisVideo-GetDataEndpoint-response-DataEndpoint)": "string"
}
```

## Response Elements<a name="API_GetDataEndpoint_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [DataEndpoint](#API_GetDataEndpoint_ResponseSyntax) **   <a name="KinesisVideo-GetDataEndpoint-response-DataEndpoint"></a>
The endpoint value\. To read data from the stream or to write data to it, specify this endpoint in your application\.  
Type: String

## Errors<a name="API_GetDataEndpoint_Errors"></a>

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

 **ResourceNotFoundException**   
Amazon Kinesis Video Streams can't find the stream that you specified\.  
HTTP Status Code: 404

## See Also<a name="API_GetDataEndpoint_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesisvideo-2017-09-30/GetDataEndpoint) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesisvideo-2017-09-30/GetDataEndpoint) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/GetDataEndpoint) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/GetDataEndpoint) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/GetDataEndpoint) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesisvideo-2017-09-30/GetDataEndpoint) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesisvideo-2017-09-30/GetDataEndpoint) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesisvideo-2017-09-30/GetDataEndpoint) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/GetDataEndpoint) 