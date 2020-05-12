# GetClip<a name="API_reader_GetClip"></a>

Downloads an MP4 file \(clip\) containing the archived, on\-demand media from the specified video stream over the specified time range\. 

Both the StreamName and the StreamARN parameters are optional, but you must specify either the StreamName or the StreamARN when invoking this API operation\. 

As a prerequsite to using GetCLip API, you must obtain an endpoint using `GetDataEndpoint`, specifying GET\_CLIP for`` the `APIName` parameter\. 

An Amazon Kinesis video stream has the following requirements for providing data through MP4:
+ The media must contain h\.264 or h\.265 encoded video and, optionally, AAC or G\.711 encoded audio\. Specifically, the codec ID of track 1 should be `V_MPEG/ISO/AVC` \(for h\.264\) or V\_MPEGH/ISO/HEVC \(for H\.265\)\. Optionally, the codec ID of track 2 should be `A_AAC` \(for AAC\) or A\_MS/ACM \(for G\.711\)\.
+ Data retention must be greater than 0\.
+ The video track of each fragment must contain codec private data in the Advanced Video Coding \(AVC\) for H\.264 format and HEVC for H\.265 format\. For more information, see [MPEG\-4 specification ISO/IEC 14496\-15](https://www.iso.org/standard/55980.html)\. For information about adapting stream data to a given format, see [NAL Adaptation Flags](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producer-reference-nal.html)\.
+ The audio track \(if present\) of each fragment must contain codec private data in the AAC format \([AAC specification ISO/IEC 13818\-7](https://www.iso.org/standard/43345.html)\) or the [MS Wave format](http://www-mmsp.ece.mcgill.ca/Documents/AudioFormats/WAVE/WAVE.html)\.

You can monitor the amount of outgoing data by monitoring the `GetClip.OutgoingBytes` Amazon CloudWatch metric\. For information about using CloudWatch to monitor Kinesis Video Streams, see [Monitoring Kinesis Video Streams](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/monitoring.html)\. For pricing information, see [Amazon Kinesis Video Streams Pricing](https://aws.amazon.com/kinesis/video-streams/pricing/) and [AWS Pricing](https://aws.amazon.com/pricing/)\. Charges for outgoing AWS data apply\.

## Request Syntax<a name="API_reader_GetClip_RequestSyntax"></a>

```
POST /getClip HTTP/1.1
Content-type: application/json

{
   "[ClipFragmentSelector](#KinesisVideo-reader_GetClip-request-ClipFragmentSelector)": { 
      "[FragmentSelectorType](API_reader_ClipFragmentSelector.md#KinesisVideo-Type-reader_ClipFragmentSelector-FragmentSelectorType)": "string",
      "[TimestampRange](API_reader_ClipFragmentSelector.md#KinesisVideo-Type-reader_ClipFragmentSelector-TimestampRange)": { 
         "[EndTimestamp](API_reader_ClipTimestampRange.md#KinesisVideo-Type-reader_ClipTimestampRange-EndTimestamp)": number,
         "[StartTimestamp](API_reader_ClipTimestampRange.md#KinesisVideo-Type-reader_ClipTimestampRange-StartTimestamp)": number
      }
   },
   "[StreamARN](#KinesisVideo-reader_GetClip-request-StreamARN)": "string",
   "[StreamName](#KinesisVideo-reader_GetClip-request-StreamName)": "string"
}
```

## URI Request Parameters<a name="API_reader_GetClip_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_reader_GetClip_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [ClipFragmentSelector](#API_reader_GetClip_RequestSyntax) **   <a name="KinesisVideo-reader_GetClip-request-ClipFragmentSelector"></a>
The time range of the requested clip and the source of the timestamps\.  
Type: [ClipFragmentSelector](API_reader_ClipFragmentSelector.md) object  
Required: Yes

 ** [StreamARN](#API_reader_GetClip_RequestSyntax) **   <a name="KinesisVideo-reader_GetClip-request-StreamARN"></a>
The Amazon Resource Name \(ARN\) of the stream for which to retrieve the media clip\.   
You must specify either the StreamName or the StreamARN\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+`   
Required: No

 ** [StreamName](#API_reader_GetClip_RequestSyntax) **   <a name="KinesisVideo-reader_GetClip-request-StreamName"></a>
The name of the stream for which to retrieve the media clip\.   
You must specify either the StreamName or the StreamARN\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: No

## Response Syntax<a name="API_reader_GetClip_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-Type: ContentType

Payload
```

## Response Elements<a name="API_reader_GetClip_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The response returns the following HTTP headers\.

 ** [ContentType](#API_reader_GetClip_ResponseSyntax) **   <a name="KinesisVideo-reader_GetClip-response-ContentType"></a>
The content type of the media in the requested clip\.  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `^[a-zA-Z0-9_\.\-]+$` 

The response returns the following as the HTTP body\.

 ** [Payload](#API_reader_GetClip_ResponseSyntax) **   <a name="KinesisVideo-reader_GetClip-response-Payload"></a>
Traditional MP4 file that contains the media clip from the specified video stream\. The output will contain the first 100 MB or the first 200 fragments from the specified start timestamp\. For more information, see [Kinesis Video Streams Limits](Kinesis Video Streams Limits)\. 

## Errors<a name="API_reader_GetClip_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ClientLimitExceededException**   
Kinesis Video Streams has throttled the request because you have exceeded the limit of allowed client calls\. Try making the call later\.  
HTTP Status Code: 400

 **InvalidArgumentException**   
A specified parameter exceeds its restrictions, is not supported, or can't be used\.  
HTTP Status Code: 400

 **InvalidCodecPrivateDataException**   
The codec private data in at least one of the tracks of the video stream is not valid for this operation\.  
HTTP Status Code: 400

 **InvalidMediaFrameException**   
One or more frames in the requested clip could not be parsed based on the specified codec\.  
HTTP Status Code: 400

 **MissingCodecPrivateDataException**   
No codec private data was found in at least one of tracks of the video stream\.  
HTTP Status Code: 400

 **NoDataRetentionException**   
A streaming session was requested for a stream that does not retain data \(that is, has a `DataRetentionInHours` of 0\)\.   
HTTP Status Code: 400

 **NotAuthorizedException**   
Status Code: 403, The caller is not authorized to perform an operation on the given stream, or the token has expired\.  
HTTP Status Code: 401

 **ResourceNotFoundException**   
 `GetMedia` throws this error when Kinesis Video Streams can't find the stream that you specified\.  
 `GetHLSStreamingSessionURL` and `GetDASHStreamingSessionURL` throw this error if a session with a `PlaybackMode` of `ON_DEMAND` or `LIVE_REPLAY`is requested for a stream that has no fragments within the requested time range, or if a session with a `PlaybackMode` of `LIVE` is requested for a stream that has no fragments within the last 30 seconds\.  
HTTP Status Code: 404

 **UnsupportedStreamMediaTypeException**   
The type of the media \(for example, h\.264 or h\.265 video or ACC or G\.711 audio\) could not be determined from the codec IDs of the tracks in the first fragment for a playback session\. The codec ID for track 1 should be `V_MPEG/ISO/AVC` and, optionally, the codec ID for track 2 should be `A_AAC`\.  
HTTP Status Code: 400

## See Also<a name="API_reader_GetClip_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesis-video-reader-data-2017-09-30/GetClip) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesis-video-reader-data-2017-09-30/GetClip) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesis-video-reader-data-2017-09-30/GetClip) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesis-video-reader-data-2017-09-30/GetClip) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesis-video-reader-data-2017-09-30/GetClip) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesis-video-reader-data-2017-09-30/GetClip) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesis-video-reader-data-2017-09-30/GetClip) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesis-video-reader-data-2017-09-30/GetClip) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesis-video-reader-data-2017-09-30/GetClip) 