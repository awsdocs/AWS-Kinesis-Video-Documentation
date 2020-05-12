# GetMedia<a name="API_dataplane_GetMedia"></a>

 Use this API to retrieve media content from a Kinesis video stream\. In the request, you identify the stream name or stream Amazon Resource Name \(ARN\), and the starting chunk\. Kinesis Video Streams then returns a stream of chunks in order by fragment number\.

**Note**  
You must first call the `GetDataEndpoint` API to get an endpoint\. Then send the `GetMedia` requests to this endpoint using the [\-\-endpoint\-url parameter](https://docs.aws.amazon.com/cli/latest/reference/)\. 

When you put media data \(fragments\) on a stream, Kinesis Video Streams stores each incoming fragment and related metadata in what is called a "chunk\." For more information, see [PutMedia](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_dataplane_PutMedia.html)\. The `GetMedia` API returns a stream of these chunks starting from the chunk that you specify in the request\. 

The following limits apply when using the `GetMedia` API:
+ A client can call `GetMedia` up to five times per second per stream\. 
+ Kinesis Video Streams sends media data at a rate of up to 25 megabytes per second \(or 200 megabits per second\) during a `GetMedia` session\. 

**Note**  
If an error is thrown after invoking a Kinesis Video Streams media API, in addition to the HTTP status code and the response body, it includes the following pieces of information:   
 `x-amz-ErrorType` HTTP header – contains a more specific error type in addition to what the HTTP status code provides\. 
 `x-amz-RequestId` HTTP header – if you want to report an issue to AWS, the support team can better diagnose the problem if given the Request Id\.
Both the HTTP status code and the ErrorType header can be utilized to make programmatic decisions about whether errors are retry\-able and under what conditions, as well as provide information on what actions the client programmer might need to take in order to successfully try again\.  
For more information, see the **Errors** section at the bottom of this topic, as well as [Common Errors](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/CommonErrors.html)\. 

## Request Syntax<a name="API_dataplane_GetMedia_RequestSyntax"></a>

```
POST /getMedia HTTP/1.1
Content-type: application/json

{
   "[StartSelector](#KinesisVideo-dataplane_GetMedia-request-StartSelector)": { 
      "[AfterFragmentNumber](API_dataplane_StartSelector.md#KinesisVideo-Type-dataplane_StartSelector-AfterFragmentNumber)": "string",
      "[ContinuationToken](API_dataplane_StartSelector.md#KinesisVideo-Type-dataplane_StartSelector-ContinuationToken)": "string",
      "[StartSelectorType](API_dataplane_StartSelector.md#KinesisVideo-Type-dataplane_StartSelector-StartSelectorType)": "string",
      "[StartTimestamp](API_dataplane_StartSelector.md#KinesisVideo-Type-dataplane_StartSelector-StartTimestamp)": number
   },
   "[StreamARN](#KinesisVideo-dataplane_GetMedia-request-StreamARN)": "string",
   "[StreamName](#KinesisVideo-dataplane_GetMedia-request-StreamName)": "string"
}
```

## URI Request Parameters<a name="API_dataplane_GetMedia_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_dataplane_GetMedia_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [StartSelector](#API_dataplane_GetMedia_RequestSyntax) **   <a name="KinesisVideo-dataplane_GetMedia-request-StartSelector"></a>
Identifies the starting chunk to get from the specified stream\.   
Type: [StartSelector](API_dataplane_StartSelector.md) object  
Required: Yes

 ** [StreamARN](#API_dataplane_GetMedia_RequestSyntax) **   <a name="KinesisVideo-dataplane_GetMedia-request-StreamARN"></a>
The ARN of the stream from where you want to get the media content\. If you don't specify the `streamARN`, you must specify the `streamName`\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+`   
Required: No

 ** [StreamName](#API_dataplane_GetMedia_RequestSyntax) **   <a name="KinesisVideo-dataplane_GetMedia-request-StreamName"></a>
The Kinesis video stream name from where you want to get the media content\. If you don't specify the `streamName`, you must specify the `streamARN`\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: No

## Response Syntax<a name="API_dataplane_GetMedia_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-Type: ContentType

Payload
```

## Response Elements<a name="API_dataplane_GetMedia_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The response returns the following HTTP headers\.

 ** [ContentType](#API_dataplane_GetMedia_ResponseSyntax) **   <a name="KinesisVideo-dataplane_GetMedia-response-ContentType"></a>
The content type of the requested media\.  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `^[a-zA-Z0-9_\.\-]+$` 

The response returns the following as the HTTP body\.

 ** [Payload](#API_dataplane_GetMedia_ResponseSyntax) **   <a name="KinesisVideo-dataplane_GetMedia-response-Payload"></a>
 The payload Kinesis Video Streams returns is a sequence of chunks from the specified stream\. For more information about the chunks, see [PutMedia](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_dataplane_PutMedia.html)\. The chunks that Kinesis Video Streams returns in the `GetMedia` call also include the following additional Matroska \(MKV\) tags:   
+ AWS\_KINESISVIDEO\_CONTINUATION\_TOKEN \(UTF\-8 string\) \- In the event your `GetMedia` call terminates, you can use this continuation token in your next request to get the next chunk where the last request terminated\.
+ AWS\_KINESISVIDEO\_MILLIS\_BEHIND\_NOW \(UTF\-8 string\) \- Client applications can use this tag value to determine how far behind the chunk returned in the response is from the latest chunk on the stream\. 
+ AWS\_KINESISVIDEO\_FRAGMENT\_NUMBER \- Fragment number returned in the chunk\.
+ AWS\_KINESISVIDEO\_SERVER\_TIMESTAMP \- Server timestamp of the fragment\.
+ AWS\_KINESISVIDEO\_PRODUCER\_TIMESTAMP \- Producer timestamp of the fragment\.
The following tags will be present if an error occurs:  
+ AWS\_KINESISVIDEO\_ERROR\_CODE \- String description of an error that caused GetMedia to stop\.
+ AWS\_KINESISVIDEO\_ERROR\_ID: Integer code of the error\.
The error codes are as follows:  
+ 3002 \- Error writing to the stream
+ 4000 \- Requested fragment is not found
+ 4500 \- Access denied for the stream's KMS key
+ 4501 \- Stream's KMS key is disabled
+ 4502 \- Validation error on the stream's KMS key
+ 4503 \- KMS key specified in the stream is unavailable
+ 4504 \- Invalid usage of the KMS key specified in the stream
+ 4505 \- Invalid state of the KMS key specified in the stream
+ 4506 \- Unable to find the KMS key specified in the stream
+ 5000 \- Internal error

## Errors<a name="API_dataplane_GetMedia_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ClientLimitExceededException**   
Kinesis Video Streams has throttled the request because you have exceeded the limit of allowed client calls\. Try making the call later\.  
HTTP Status Code: 400

 **ConnectionLimitExceededException**   
Kinesis Video Streams has throttled the request because you have exceeded the limit of allowed client connections\.  
HTTP Status Code: 400

 **InvalidArgumentException**   
The value for this input parameter is invalid\.  
HTTP Status Code: 400

 **InvalidEndpointException**   
Caller used wrong endpoint to write data to a stream\. On receiving such an exception, the user must call `GetDataEndpoint` with `APIName` set to `PUT_MEDIA` and use the endpoint from response to invoke the next `PutMedia` call\.   
HTTP Status Code: 400

 **NotAuthorizedException**   
The caller is not authorized to perform an operation on the given stream, or the token has expired\.  
HTTP Status Code: 401

 **ResourceNotFoundException**   
Status Code: 404, The stream with the given name does not exist\.  
HTTP Status Code: 404

## See Also<a name="API_dataplane_GetMedia_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesis-video-data-2017-09-30/GetMedia) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesis-video-data-2017-09-30/GetMedia) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesis-video-data-2017-09-30/GetMedia) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesis-video-data-2017-09-30/GetMedia) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesis-video-data-2017-09-30/GetMedia) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesis-video-data-2017-09-30/GetMedia) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesis-video-data-2017-09-30/GetMedia) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesis-video-data-2017-09-30/GetMedia) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesis-video-data-2017-09-30/GetMedia) 