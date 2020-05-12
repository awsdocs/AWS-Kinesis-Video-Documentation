# PutMedia<a name="API_dataplane_PutMedia"></a>

 Use this API to send media data to a Kinesis video stream\. 

**Note**  
 Before using this API, you must call the `GetDataEndpoint` API to get an endpoint\. You then specify the endpoint in your `PutMedia` request\. 

 In the request, you use the HTTP headers to provide parameter information, for example, stream name, timestamp, and whether the timestamp value is absolute or relative to when the producer started recording\. You use the request body to send the media data\. Kinesis Video Streams supports only the Matroska \(MKV\) container format for sending media data using this API\. 

You have the following options for sending data using this API:
+ Send media data in real time: For example, a security camera can send frames in real time as it generates them\. This approach minimizes the latency between the video recording and data sent on the wire\. This is referred to as a continuous producer\. In this case, a consumer application can read the stream in real time or when needed\. 
+ Send media data offline \(in batches\): For example, a body camera might record video for hours and store it on the device\. Later, when you connect the camera to the docking port, the camera can start a `PutMedia` session to send data to a Kinesis video stream\. In this scenario, latency is not an issue\. 

When using this API, note the following considerations:
+ You must specify either `streamName` or `streamARN`, but not both\.
+ To be able to play the media on the console or via HLS, track 1 of each fragment should contain h\.264 encoded video, the CodecID in the fragment metadata should be "V\_MPEG/ISO/AVC", and the fragment metadata should include AVCC formatted h\.264 codec private data\. Optionally, track 2 of each fragment should contain AAC encoded audio, the CodecID in the fragment metadata should be "A\_AAC", and the fragment metadata should include AAC codec private data\.
+ You might find it easier to use a single long\-running `PutMedia` session and send a large number of media data fragments in the payload\. For each fragment received, Kinesis Video Streams sends one or more acknowledgements\. Potential network considerations might cause you to not get all these acknowledgements as they are generated\.
+ You might choose multiple consecutive `PutMedia` sessions, each with fewer fragments to ensure that you get all acknowledgements from the service in real time\.

**Note**  
If you send data to the same stream on multiple simultaneous `PutMedia` sessions, the media fragments get interleaved on the stream\. You should make sure that this is OK in your application scenario\. 

The following limits apply when using the `PutMedia` API:
+ A client can call `PutMedia` up to five times per second per stream\.
+ A client can send up to five fragments per second per stream\.
+ Kinesis Video Streams reads media data at a rate of up to 12\.5 MB/second, or 100 Mbps during a `PutMedia` session\. 

Note the following constraints\. In these cases, Kinesis Video Streams sends the Error acknowledgement in the response\. 
+ Fragments that have time codes spanning longer than 10 seconds and that contain more than 50 MB of data are not allowed\. 
+ Fragments containing more than three tracks are not allowed\. Each frame in every fragment must have the same track number as one of the tracks defined in the fragment header\. Additionally, every fragment must contain at least one frame for each track defined in the fragment header\.
+ Each fragment must contain at least one frame for each track defined in the fragment metadata\.
+ The earliest frame timestamp in a fragment must be after the latest frame timestamp in the previous fragment\.
+  An MKV stream containing more than one MKV segment or containing disallowed MKV elements \(like `track*`\) also results in the Error acknowledgement\. 

Kinesis Video Streams stores each incoming fragment and related metadata in what is called a "chunk\." The fragment metadata includes the following: 
+ The MKV headers provided at the start of the `PutMedia` request
+ The following Kinesis Video Streams\-specific metadata for the fragment:
  +  `server_timestamp` \- Timestamp when Kinesis Video Streams started receiving the fragment\. 
  +  `producer_timestamp` \- Timestamp, when the producer started recording the fragment\. Kinesis Video Streams uses three pieces of information received in the request to calculate this value\. 
    + The fragment timecode value received in the request body along with the fragment\.
    + Two request headers: `producerStartTimestamp` \(when the producer started recording\) and `fragmentTimeCodeType` \(whether the fragment timecode in the payload is absolute or relative\)\.

    Kinesis Video Streams then computes the `producer_timestamp` for the fragment as follows:

     If `fragmentTimeCodeType` is relative, then 

     `producer_timestamp` = `producerStartTimeStamp` \+ fragment timecode 

    If `fragmentTimeCodeType` is absolute, then 

     `producer_timestamp` = fragment timecode \(converted to milliseconds\)
  + Unique fragment number assigned by Kinesis Video Streams\.

  

**Note**  
 When you make the `GetMedia` request, Kinesis Video Streams returns a stream of these chunks\. The client can process the metadata as needed\. 

**Note**  
This operation is only available for the AWS SDK for Java\. It is not supported in AWS SDKs for other languages\.

**Note**  
Kinesis Video Streams does not parse and validate the codec private data during ingestion and archival via the PutMedia API\. KVS extracts and validates the necessary information from the codec private data for MPEG\-TS and MP4 fragment packaging when consuming the stream via the HLS APIs\.

**Note**  
If an error is thrown after invoking a Kinesis Video Streams media API, in addition to the HTTP status code and the response body, it includes the following pieces of information:   
 `x-amz-ErrorType` HTTP header – contains a more specific error type in addition to what the HTTP status code provides\. 
 `x-amz-RequestId` HTTP header – if you want to report an issue to AWS, the support team can better diagnose the problem if given the Request Id\.
Both the HTTP status code and the ErrorType header can be utilized to make programmatic decisions about whether errors are retry\-able and under what conditions, as well as provide information on what actions the client programmer might need to take in order to successfully try again\.  
For more information, see the **Errors** section at the bottom of this topic, as well as [Common Errors](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/CommonErrors.html)\. 

## Request Syntax<a name="API_dataplane_PutMedia_RequestSyntax"></a>

```
POST /putMedia HTTP/1.1
x-amzn-stream-name: StreamName
x-amzn-stream-arn: StreamARN
x-amzn-fragment-timecode-type: FragmentTimecodeType
x-amzn-producer-start-timestamp: ProducerStartTimestamp

Payload
```

## URI Request Parameters<a name="API_dataplane_PutMedia_RequestParameters"></a>

The request requires the following URI parameters\.

 ** [FragmentTimecodeType](#API_dataplane_PutMedia_RequestSyntax) **   <a name="KinesisVideo-dataplane_PutMedia-request-FragmentTimecodeType"></a>
You pass this value as the `x-amzn-fragment-timecode-type` HTTP header\.  
Indicates whether timecodes in the fragments \(payload, HTTP request body\) are absolute or relative to `producerStartTimestamp`\. Kinesis Video Streams uses this information to compute the `producer_timestamp` for the fragment received in the request, as described in the API overview\.  
Valid Values:` ABSOLUTE | RELATIVE` 

 ** [ProducerStartTimestamp](#API_dataplane_PutMedia_RequestSyntax) **   <a name="KinesisVideo-dataplane_PutMedia-request-ProducerStartTimestamp"></a>
You pass this value as the `x-amzn-producer-start-timestamp` HTTP header\.  
This is the producer timestamp at which the producer started recording the media \(not the timestamp of the specific fragments in the request\)\.

 ** [StreamARN](#API_dataplane_PutMedia_RequestSyntax) **   <a name="KinesisVideo-dataplane_PutMedia-request-StreamARN"></a>
You pass this value as the `x-amzn-stream-arn` HTTP header\.  
Amazon Resource Name \(ARN\) of the Kinesis video stream where you want to write the media content\. If you don't specify the `streamARN`, you must specify the `streamName`\.  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+` 

 ** [StreamName](#API_dataplane_PutMedia_RequestSyntax) **   <a name="KinesisVideo-dataplane_PutMedia-request-StreamName"></a>
You pass this value as the `x-amzn-stream-name` HTTP header\.  
Name of the Kinesis video stream where you want to write the media content\. If you don't specify the `streamName`, you must specify the `streamARN`\.  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+` 

## Request Body<a name="API_dataplane_PutMedia_RequestBody"></a>

The request accepts the following binary data\.

 ** [Payload](#API_dataplane_PutMedia_RequestSyntax) **   <a name="KinesisVideo-dataplane_PutMedia-request-Payload"></a>
 The media content to write to the Kinesis video stream\. In the current implementation, Kinesis Video Streams supports only the Matroska \(MKV\) container format with a single MKV segment\. A segment can contain one or more clusters\.   
Each MKV cluster maps to a Kinesis video stream fragment\. Whatever cluster duration you choose becomes the fragment duration\. 

## Response Syntax<a name="API_dataplane_PutMedia_ResponseSyntax"></a>

```
HTTP/1.1 200

Payload
```

## Response Elements<a name="API_dataplane_PutMedia_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The response returns the following as the HTTP body\.

 ** [Payload](#API_dataplane_PutMedia_ResponseSyntax) **   <a name="KinesisVideo-dataplane_PutMedia-response-Payload"></a>
 After Kinesis Video Streams successfully receives a `PutMedia` request, the service validates the request headers\. The service then starts reading the payload and first sends an HTTP 200 response\.   
The service then returns a stream containing a series of JSON objects \(`Acknowledgement` objects\) separated by newlines\. The acknowledgements are received on the same connection on which the media data is sent\. There can be many acknowledgements for a `PutMedia` request\. Each `Acknowledgement` consists of the following key\-value pairs:  
+  `AckEventType` \- Event type the acknowledgement represents\. 
  +  **Buffering:** Kinesis Video Streams has started receiving the fragment\. Kinesis Video Streams sends the first Buffering acknowledgement when the first byte of fragment data is received\. 
  +  **Received:** Kinesis Video Streams received the entire fragment\. If you did not configure the stream to persist the data, the producer can stop buffering the fragment upon receiving this acknowledgement\.
  +  **Persisted:** Kinesis Video Streams has persisted the fragment \(for example, to Amazon S3\)\. You get this acknowledgement if you configured the stream to persist the data\. After you receive this acknowledgement, the producer can stop buffering the fragment\.
  +  **Error:** Kinesis Video Streams ran into an error while processing the fragment\. You can review the error code and determine the next course of action\. 
  +  **Idle:** The `PutMedia` session is in\-progress\. However, Kinesis Video Streams is currently not receiving data\. Kinesis Video Streams sends this acknowledgement periodically for up to 30 seconds after the last received data\. If no data is received within the 30 seconds, Kinesis Video Streams closes the request\. 
**Note**  
 This acknowledgement can help a producer determine if the `PutMedia` connection is alive, even if it is not sending any data\. 
+  `FragmentTimeCode` \- Fragment timecode for which acknowledgement is sent\. 

  The element can be missing if the `AckEventType` is **Idle**\. 
+  `FragmentNumber` \- Kinesis Video Streams\-generated fragment number for which the acknowledgement is sent\.
+  `ErrorId` and `ErrorCode` \- If the `AckEventType` is ErrorId, this field provides corresponding error code\. The following is the list of error codes:
  + 4000 \- Error reading the data stream\.
  + 4001 \- Fragment size is greater than maximum limit, 50 MB, allowed\.
  + 4002 \- Fragment duration is greater than maximum limit, 10 seconds, allowed\.
  + 4003 \- Connection duration is greater than maximum allowed threshold\.
  + 4004 \- Fragment timecode is less than the timecode previous time code \(within a `PutMedia` call, you cannot send fragments out of order\)\.
  + 4005 \- More than one track is found in MKV\. \(deprecated\)
  + 4006 \- Failed to parse the input stream as valid MKV format\.
  + 4007 \- Invalid producer timestamp\.
  + 4008 \- Stream no longer exists \(deleted\)\.
  + 4009 \- Fragment metadata limit reached\.
  + 4010 \- The track number in an MKV frame did not match the tracks in the MKV header\.
  + 4011 \- The fragment did not contain any frames for at least one of the tracks in the MKV header\.
  + 4012 \- More than the allowed number of tracks found in the input MKV\.
  + 4500 \- Access to the stream's specified KMS key is denied\.
  + 4501 \- The stream's specified KMS key is disabled\.
  + 4502 \- The stream's specified KMS key failed validation\.
  + 4503 \- The stream's specified KMS key is unavailable\.
  + 4504 \- Invalid usage of the stream's specified KMS key\.
  + 4505 \- The stream's specified KMS key is in an invalid state\.
  + 4506 \- The stream's specified KMS key is not found\.
  + 5000 \- Internal service error
  + 5001 \- Kinesis Video Streams failed to persist fragments to the data store\.
The producer, while sending the payload for a long running `PutMedia` request, should read the response for acknowledgements\. A producer might receive chunks of acknowledgements at the same time, due to buffering on an intermediate proxy server\. A producer that wants to receive timely acknowledgements can send fewer fragments in each `PutMedia` request\. 

## Errors<a name="API_dataplane_PutMedia_Errors"></a>

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

## Example<a name="API_dataplane_PutMedia_Examples"></a>

### Acknowledgement Format<a name="API_dataplane_PutMedia_Example_1"></a>

The format of the acknowledgement is as follows:

#### <a name="w9aac38b4c13c11c51b3b5"></a>

```
{
       Acknowledgement : {
          "EventType": enum
          "FragmentTimecode": Long,
          "FragmentNumber": Long,
          "ErrorId" : String       
      }
}
```

## See Also<a name="API_dataplane_PutMedia_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesis-video-data-2017-09-30/PutMedia) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesis-video-data-2017-09-30/PutMedia) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesis-video-data-2017-09-30/PutMedia) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesis-video-data-2017-09-30/PutMedia) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesis-video-data-2017-09-30/PutMedia) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesis-video-data-2017-09-30/PutMedia) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesis-video-data-2017-09-30/PutMedia) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesis-video-data-2017-09-30/PutMedia) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesis-video-data-2017-09-30/PutMedia) 