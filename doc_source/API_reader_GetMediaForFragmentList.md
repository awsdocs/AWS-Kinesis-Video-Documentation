# GetMediaForFragmentList<a name="API_reader_GetMediaForFragmentList"></a>

Gets media for a list of fragments \(specified by fragment number\) from the archived data in an Amazon Kinesis video stream\.

**Note**  
You must first call the `GetDataEndpoint` API to get an endpoint\. Then send the `GetMediaForFragmentList` requests to this endpoint using the [\-\-endpoint\-url parameter](https://docs.aws.amazon.com/cli/latest/reference/)\. 

The following limits apply when using the `GetMediaForFragmentList` API:
+ A client can call `GetMediaForFragmentList` up to five times per second per stream\. 
+ Kinesis Video Streams sends media data at a rate of up to 25 megabytes per second \(or 200 megabits per second\) during a `GetMediaForFragmentList` session\. 

**Important**  
If an error is thrown after invoking a Kinesis Video Streams archived media API, in addition to the HTTP status code and the response body, it includes the following pieces of information:   
 `x-amz-ErrorType` HTTP header – contains a more specific error type in addition to what the HTTP status code provides\. 
 `x-amz-RequestId` HTTP header – if you want to report an issue to AWS, the support team can better diagnose the problem if given the Request Id\.
Both the HTTP status code and the ErrorType header can be utilized to make programmatic decisions about whether errors are retry\-able and under what conditions, as well as provide information on what actions the client programmer might need to take in order to successfully try again\.  
For more information, see the **Errors** section at the bottom of this topic, as well as [Common Errors](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/CommonErrors.html)\. 

## Request Syntax<a name="API_reader_GetMediaForFragmentList_RequestSyntax"></a>

```
POST /getMediaForFragmentList HTTP/1.1
Content-type: application/json

{
   "[Fragments](#KinesisVideo-reader_GetMediaForFragmentList-request-Fragments)": [ "string" ],
   "[StreamName](#KinesisVideo-reader_GetMediaForFragmentList-request-StreamName)": "string"
}
```

## URI Request Parameters<a name="API_reader_GetMediaForFragmentList_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_reader_GetMediaForFragmentList_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [Fragments](#API_reader_GetMediaForFragmentList_RequestSyntax) **   <a name="KinesisVideo-reader_GetMediaForFragmentList-request-Fragments"></a>
A list of the numbers of fragments for which to retrieve media\. You retrieve these values with [ListFragments](API_reader_ListFragments.md)\.  
Type: Array of strings  
Array Members: Minimum number of 1 item\. Maximum number of 1000 items\.  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `^[0-9]+$`   
Required: Yes

 ** [StreamName](#API_reader_GetMediaForFragmentList_RequestSyntax) **   <a name="KinesisVideo-reader_GetMediaForFragmentList-request-StreamName"></a>
The name of the stream from which to retrieve fragment media\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: Yes

## Response Syntax<a name="API_reader_GetMediaForFragmentList_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-Type: ContentType

Payload
```

## Response Elements<a name="API_reader_GetMediaForFragmentList_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The response returns the following HTTP headers\.

 ** [ContentType](#API_reader_GetMediaForFragmentList_ResponseSyntax) **   <a name="KinesisVideo-reader_GetMediaForFragmentList-response-ContentType"></a>
The content type of the requested media\.  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `^[a-zA-Z0-9_\.\-]+$` 

The response returns the following as the HTTP body\.

 ** [Payload](#API_reader_GetMediaForFragmentList_ResponseSyntax) **   <a name="KinesisVideo-reader_GetMediaForFragmentList-response-Payload"></a>
The payload that Kinesis Video Streams returns is a sequence of chunks from the specified stream\. For information about the chunks, see [PutMedia](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_dataplane_PutMedia.html)\. The chunks that Kinesis Video Streams returns in the `GetMediaForFragmentList` call also include the following additional Matroska \(MKV\) tags:   
+ AWS\_KINESISVIDEO\_FRAGMENT\_NUMBER \- Fragment number returned in the chunk\.
+ AWS\_KINESISVIDEO\_SERVER\_SIDE\_TIMESTAMP \- Server\-side timestamp of the fragment\.
+ AWS\_KINESISVIDEO\_PRODUCER\_SIDE\_TIMESTAMP \- Producer\-side timestamp of the fragment\.
The following tags will be included if an exception occurs:  
+ AWS\_KINESISVIDEO\_FRAGMENT\_NUMBER \- The number of the fragment that threw the exception
+ AWS\_KINESISVIDEO\_EXCEPTION\_ERROR\_CODE \- The integer code of the exception
+ AWS\_KINESISVIDEO\_EXCEPTION\_MESSAGE \- A text description of the exception

## Errors<a name="API_reader_GetMediaForFragmentList_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ClientLimitExceededException**   
Kinesis Video Streams has throttled the request because you have exceeded the limit of allowed client calls\. Try making the call later\.  
HTTP Status Code: 400

 **InvalidArgumentException**   
A specified parameter exceeds its restrictions, is not supported, or can't be used\.  
HTTP Status Code: 400

 **NotAuthorizedException**   
Status Code: 403, The caller is not authorized to perform an operation on the given stream, or the token has expired\.  
HTTP Status Code: 401

 **ResourceNotFoundException**   
 `GetMedia` throws this error when Kinesis Video Streams can't find the stream that you specified\.  
 `GetHLSStreamingSessionURL` and `GetDASHStreamingSessionURL` throw this error if a session with a `PlaybackMode` of `ON_DEMAND` or `LIVE_REPLAY`is requested for a stream that has no fragments within the requested time range, or if a session with a `PlaybackMode` of `LIVE` is requested for a stream that has no fragments within the last 30 seconds\.  
HTTP Status Code: 404

## See Also<a name="API_reader_GetMediaForFragmentList_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesis-video-reader-data-2017-09-30/GetMediaForFragmentList) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesis-video-reader-data-2017-09-30/GetMediaForFragmentList) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesis-video-reader-data-2017-09-30/GetMediaForFragmentList) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesis-video-reader-data-2017-09-30/GetMediaForFragmentList) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesis-video-reader-data-2017-09-30/GetMediaForFragmentList) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesis-video-reader-data-2017-09-30/GetMediaForFragmentList) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesis-video-reader-data-2017-09-30/GetMediaForFragmentList) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesis-video-reader-data-2017-09-30/GetMediaForFragmentList) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesis-video-reader-data-2017-09-30/GetMediaForFragmentList) 