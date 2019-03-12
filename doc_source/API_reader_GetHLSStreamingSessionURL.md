# GetHLSStreamingSessionURL<a name="API_reader_GetHLSStreamingSessionURL"></a>

Retrieves an HTTP Live Streaming \(HLS\) URL for the stream\. You can then open the URL in a browser or media player to view the stream contents\.

You must specify either the `StreamName` or the `StreamARN`\.

An Amazon Kinesis video stream has the following requirements for providing data through HLS:
+ The media must contain h\.264 encoded video and, optionally, AAC encoded audio\. Specifically, the codec id of track 1 should be `V_MPEG/ISO/AVC`\. Optionally, the codec id of track 2 should be `A_AAC`\.
+ Data retention must be greater than 0\.
+ The video track of each fragment must contain codec private data in the Advanced Video Coding \(AVC\) for H\.264 format \([MPEG\-4 specification ISO/IEC 14496\-15](https://www.iso.org/standard/55980.html)\)\. For information about adapting stream data to a given format, see [NAL Adaptation Flags](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producer-reference-nal.html)\.
+ The audio track \(if present\) of each fragment must contain codec private data in the AAC format \([AAC specification ISO/IEC 13818\-7](https://www.iso.org/standard/43345.html)\)\.

Kinesis Video Streams HLS sessions contain fragments in the fragmented MPEG\-4 form \(also called fMP4 or CMAF\), rather than the MPEG\-2 form \(also called TS chunks, which the HLS specification also supports\)\. For more information about HLS fragment types, see the [HLS specification](https://tools.ietf.org/html/draft-pantos-http-live-streaming-23)\.

The following procedure shows how to use HLS with Kinesis Video Streams:

1. Get an endpoint using [GetDataEndpoint](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_GetDataEndpoint.html), specifying `GET_HLS_STREAMING_SESSION_URL` for the `APIName` parameter\.

1. Retrieve the HLS URL using `GetHLSStreamingSessionURL`\. Kinesis Video Streams creates an HLS streaming session to be used for accessing content in a stream using the HLS protocol\. `GetHLSStreamingSessionURL` returns an authenticated URL \(that includes an encrypted session token\) for the session's HLS *master playlist* \(the root resource needed for streaming with HLS\)\.
**Note**  
Don't share or store this token where an unauthorized entity could access it\. The token provides access to the content of the stream\. Safeguard the token with the same measures that you would use with your AWS credentials\.

   The media that is made available through the playlist consists only of the requested stream, time range, and format\. No other media data \(such as frames outside the requested window or alternate bitrates\) is made available\.

1. Provide the URL \(containing the encrypted session token\) for the HLS master playlist to a media player that supports the HLS protocol\. Kinesis Video Streams makes the HLS media playlist, initialization fragment, and media fragments available through the master playlist URL\. The initialization fragment contains the codec private data for the stream, and other data needed to set up the video or audio decoder and renderer\. The media fragments contain H\.264\-encoded video frames or AAC\-encoded audio samples\.

1. The media player receives the authenticated URL and requests stream metadata and media data normally\. When the media player requests data, it calls the following actions:
   +  **GetHLSMasterPlaylist:** Retrieves an HLS master playlist, which contains a URL for the `GetHLSMediaPlaylist` action for each track, and additional metadata for the media player, including estimated bitrate and resolution\.
   +  **GetHLSMediaPlaylist:** Retrieves an HLS media playlist, which contains a URL to access the MP4 initialization fragment with the `GetMP4InitFragment` action, and URLs to access the MP4 media fragments with the `GetMP4MediaFragment` actions\. The HLS media playlist also contains metadata about the stream that the player needs to play it, such as whether the `PlaybackMode` is `LIVE` or `ON_DEMAND`\. The HLS media playlist is typically static for sessions with a `PlaybackType` of `ON_DEMAND`\. The HLS media playlist is continually updated with new fragments for sessions with a `PlaybackType` of `LIVE`\. There is a distinct HLS media playlist for the video track and the audio track \(if applicable\) that contains MP4 media URLs for the specific track\. 
   +  **GetMP4InitFragment:** Retrieves the MP4 initialization fragment\. The media player typically loads the initialization fragment before loading any media fragments\. This fragment contains the "`fytp`" and "`moov`" MP4 atoms, and the child atoms that are needed to initialize the media player decoder\.

     The initialization fragment does not correspond to a fragment in a Kinesis video stream\. It contains only the codec private data for the stream and respective track, which the media player needs to decode the media frames\.
   +  **GetMP4MediaFragment:** Retrieves MP4 media fragments\. These fragments contain the "`moof`" and "`mdat`" MP4 atoms and their child atoms, containing the encoded fragment's media frames and their timestamps\. 
**Note**  
After the first media fragment is made available in a streaming session, any fragments that don't contain the same codec private data cause an error to be returned when those different media fragments are loaded\. Therefore, the codec private data should not change between fragments in a session\. This also means that the session fails if the fragments in a stream change from having only video to having both audio and video\.

     Data retrieved with this action is billable\. See [Pricing](https://aws.amazon.com/kinesis/video-streams/pricing/) for details\.
   +  **GetTSFragment:** Retrieves MPEG TS fragments containing both initialization and media data for all tracks in the stream\.
**Note**  
If the `ContainerFormat` is `MPEG_TS`, this API is used instead of `GetMP4InitFragment` and `GetMP4MediaFragment` to retrieve stream media\.

     Data retrieved with this action is billable\. For more information, see [Kinesis Video Streams pricing](https://aws.amazon.com/kinesis/video-streams/pricing/)\.

**Note**  
The following restrictions apply to HLS sessions:  
A streaming session URL should not be shared between players\. The service might throttle a session if multiple media players are sharing it\. For connection limits, see [Kinesis Video Streams Limits](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/limits.html)\.
A Kinesis video stream can have a maximum of five active HLS streaming sessions\. If a new session is created when the maximum number of sessions is already active, the oldest \(earliest created\) session is closed\. The number of active `GetMedia` connections on a Kinesis video stream does not count against this limit, and the number of active HLS sessions does not count against the active `GetMedia` connection limit\.

You can monitor the amount of data that the media player consumes by monitoring the `GetMP4MediaFragment.OutgoingBytes` Amazon CloudWatch metric\. For information about using CloudWatch to monitor Kinesis Video Streams, see [Monitoring Kinesis Video Streams](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/monitoring.html)\. For pricing information, see [Amazon Kinesis Video Streams Pricing](https://aws.amazon.com/kinesis/video-streams/pricing/) and [AWS Pricing](https://aws.amazon.com/pricing/)\. Charges for both HLS sessions and outgoing AWS data apply\.

For more information about HLS, see [HTTP Live Streaming](https://developer.apple.com/streaming/) on the [Apple Developer site](https://developer.apple.com)\.

## Request Syntax<a name="API_reader_GetHLSStreamingSessionURL_RequestSyntax"></a>

```
POST /getHLSStreamingSessionURL HTTP/1.1
Content-type: application/json

{
   "[ContainerFormat](#KinesisVideo-reader_GetHLSStreamingSessionURL-request-ContainerFormat)": "string",
   "[DiscontinuityMode](#KinesisVideo-reader_GetHLSStreamingSessionURL-request-DiscontinuityMode)": "string",
   "[DisplayFragmentTimestamp](#KinesisVideo-reader_GetHLSStreamingSessionURL-request-DisplayFragmentTimestamp)": "string",
   "[Expires](#KinesisVideo-reader_GetHLSStreamingSessionURL-request-Expires)": number,
   "[HLSFragmentSelector](#KinesisVideo-reader_GetHLSStreamingSessionURL-request-HLSFragmentSelector)": { 
      "[FragmentSelectorType](API_reader_HLSFragmentSelector.md#KinesisVideo-Type-reader_HLSFragmentSelector-FragmentSelectorType)": "string",
      "[TimestampRange](API_reader_HLSFragmentSelector.md#KinesisVideo-Type-reader_HLSFragmentSelector-TimestampRange)": { 
         "[EndTimestamp](API_reader_HLSTimestampRange.md#KinesisVideo-Type-reader_HLSTimestampRange-EndTimestamp)": number,
         "[StartTimestamp](API_reader_HLSTimestampRange.md#KinesisVideo-Type-reader_HLSTimestampRange-StartTimestamp)": number
      }
   },
   "[MaxMediaPlaylistFragmentResults](#KinesisVideo-reader_GetHLSStreamingSessionURL-request-MaxMediaPlaylistFragmentResults)": number,
   "[PlaybackMode](#KinesisVideo-reader_GetHLSStreamingSessionURL-request-PlaybackMode)": "string",
   "[StreamARN](#KinesisVideo-reader_GetHLSStreamingSessionURL-request-StreamARN)": "string",
   "[StreamName](#KinesisVideo-reader_GetHLSStreamingSessionURL-request-StreamName)": "string"
}
```

## URI Request Parameters<a name="API_reader_GetHLSStreamingSessionURL_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_reader_GetHLSStreamingSessionURL_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [ContainerFormat](#API_reader_GetHLSStreamingSessionURL_RequestSyntax) **   <a name="KinesisVideo-reader_GetHLSStreamingSessionURL-request-ContainerFormat"></a>
Specifies which format should be used for packaging the media\. Specifying the `FRAGMENTED_MP4` container format packages the media into MP4 fragments \(fMP4 or CMAF\)\. This is the recommended packaging because there is minimal packaging overhead\. The other container format option is `MPEG_TS`\. HLS has supported MPEG TS chunks since it was released and is sometimes the only supported packaging on older HLS players\. MPEG TS typically has a 5\-25 percent packaging overhead\. This means MPEG TS typically requires 5\-25 percent more bandwidth and cost than fMP4\.  
The default is `FRAGMENTED_MP4`\.  
Type: String  
Valid Values:` FRAGMENTED_MP4 | MPEG_TS`   
Required: No

 ** [DiscontinuityMode](#API_reader_GetHLSStreamingSessionURL_RequestSyntax) **   <a name="KinesisVideo-reader_GetHLSStreamingSessionURL-request-DiscontinuityMode"></a>
Specifies when flags marking discontinuities between fragments will be added to the media playlists\. The default is `ALWAYS` when [HLSFragmentSelector](API_reader_HLSFragmentSelector.md) is `SERVER_TIMESTAMP`, and `NEVER` when it is `PRODUCER_TIMESTAMP`\.  
Media players typically build a timeline of media content to play, based on the timestamps of each fragment\. This means that if there is any overlap between fragments \(as is typical if [HLSFragmentSelector](API_reader_HLSFragmentSelector.md) is `SERVER_TIMESTAMP`\), the media player timeline has small gaps between fragments in some places, and overwrites frames in other places\. When there are discontinuity flags between fragments, the media player is expected to reset the timeline, resulting in the fragment being played immediately after the previous fragment\. We recommend that you always have discontinuity flags between fragments if the fragment timestamps are not accurate or if fragments might be missing\. You should not place discontinuity flags between fragments for the player timeline to accurately map to the producer timestamps\.  
Type: String  
Valid Values:` ALWAYS | NEVER`   
Required: No

 ** [DisplayFragmentTimestamp](#API_reader_GetHLSStreamingSessionURL_RequestSyntax) **   <a name="KinesisVideo-reader_GetHLSStreamingSessionURL-request-DisplayFragmentTimestamp"></a>
Specifies when the fragment start timestamps should be included in the HLS media playlist\. Typically, media players report the playhead position as a time relative to the start of the first fragment in the playback session\. However, when the start timestamps are included in the HLS media playlist, some media players might report the current playhead as an absolute time based on the fragment timestamps\. This can be useful for creating a playback experience that shows viewers the wall\-clock time of the media\.  
The default is `NEVER`\. When [HLSFragmentSelector](API_reader_HLSFragmentSelector.md) is `SERVER_TIMESTAMP`, the timestamps will be the server start timestamps\. Similarly, when [HLSFragmentSelector](API_reader_HLSFragmentSelector.md) is `PRODUCER_TIMESTAMP`, the timestamps will be the producer start timestamps\.   
Type: String  
Valid Values:` ALWAYS | NEVER`   
Required: No

 ** [Expires](#API_reader_GetHLSStreamingSessionURL_RequestSyntax) **   <a name="KinesisVideo-reader_GetHLSStreamingSessionURL-request-Expires"></a>
The time in seconds until the requested session expires\. This value can be between 300 \(5 minutes\) and 43200 \(12 hours\)\.  
When a session expires, no new calls to `GetHLSMasterPlaylist`, `GetHLSMediaPlaylist`, `GetMP4InitFragment`, or `GetMP4MediaFragment` can be made for that session\.  
The default is 300 \(5 minutes\)\.  
Type: Integer  
Valid Range: Minimum value of 300\. Maximum value of 43200\.  
Required: No

 ** [HLSFragmentSelector](#API_reader_GetHLSStreamingSessionURL_RequestSyntax) **   <a name="KinesisVideo-reader_GetHLSStreamingSessionURL-request-HLSFragmentSelector"></a>
The time range of the requested fragment, and the source of the timestamps\.  
This parameter is required if `PlaybackMode` is `ON_DEMAND`\. This parameter is optional if `PlaybackMode` is `LIVE`\. If `PlaybackMode` is `LIVE`, the `FragmentSelectorType` can be set, but the `TimestampRange` should not be set\. If `PlaybackMode` is `ON_DEMAND`, both `FragmentSelectorType` and `TimestampRange` must be set\.  
Type: [HLSFragmentSelector](API_reader_HLSFragmentSelector.md) object  
Required: No

 ** [MaxMediaPlaylistFragmentResults](#API_reader_GetHLSStreamingSessionURL_RequestSyntax) **   <a name="KinesisVideo-reader_GetHLSStreamingSessionURL-request-MaxMediaPlaylistFragmentResults"></a>
The maximum number of fragments that are returned in the HLS media playlists\.  
When the `PlaybackMode` is `LIVE`, the most recent fragments are returned up to this value\. When the `PlaybackMode` is `ON_DEMAND`, the oldest fragments are returned, up to this maximum number\.  
When there are a higher number of fragments available in a live HLS media playlist, video players often buffer content before starting playback\. Increasing the buffer size increases the playback latency, but it decreases the likelihood that rebuffering will occur during playback\. We recommend that a live HLS media playlist have a minimum of 3 fragments and a maximum of 10 fragments\.  
The default is 5 fragments if `PlaybackMode` is `LIVE`, and 1,000 if `PlaybackMode` is `ON_DEMAND`\.   
The maximum value of 1,000 fragments corresponds to more than 16 minutes of video on streams with 1\-second fragments, and more than 2 1/2 hours of video on streams with 10\-second fragments\.  
Type: Long  
Valid Range: Minimum value of 1\. Maximum value of 1000\.  
Required: No

 ** [PlaybackMode](#API_reader_GetHLSStreamingSessionURL_RequestSyntax) **   <a name="KinesisVideo-reader_GetHLSStreamingSessionURL-request-PlaybackMode"></a>
Whether to retrieve live or archived, on\-demand data\.  
Features of the two types of session include the following:  
+  ** `LIVE` **: For sessions of this type, the HLS media playlist is continually updated with the latest fragments as they become available\. We recommend that the media player retrieve a new playlist on a one\-second interval\. When this type of session is played in a media player, the user interface typically displays a "live" notification, with no scrubber control for choosing the position in the playback window to display\.
**Note**  
In `LIVE` mode, the newest available fragments are included in an HLS media playlist, even if there is a gap between fragments \(that is, if a fragment is missing\)\. A gap like this might cause a media player to halt or cause a jump in playback\. In this mode, fragments are not added to the HLS media playlist if they are older than the newest fragment in the playlist\. If the missing fragment becomes available after a subsequent fragment is added to the playlist, the older fragment is not added, and the gap is not filled\.
+  ** `ON_DEMAND` **: For sessions of this type, the HLS media playlist contains all the fragments for the session, up to the number that is specified in `MaxMediaPlaylistFragmentResults`\. The playlist must be retrieved only once for each session\. When this type of session is played in a media player, the user interface typically displays a scrubber control for choosing the position in the playback window to display\.
In both playback modes, if `FragmentSelectorType` is `PRODUCER_TIMESTAMP`, and if there are multiple fragments with the same start timestamp, the fragment that has the larger fragment number \(that is, the newer fragment\) is included in the HLS media playlist\. The other fragments are not included\. Fragments that have different timestamps but have overlapping durations are still included in the HLS media playlist\. This can lead to unexpected behavior in the media player\.  
The default is `LIVE`\.  
Type: String  
Valid Values:` LIVE | ON_DEMAND`   
Required: No

 ** [StreamARN](#API_reader_GetHLSStreamingSessionURL_RequestSyntax) **   <a name="KinesisVideo-reader_GetHLSStreamingSessionURL-request-StreamARN"></a>
The Amazon Resource Name \(ARN\) of the stream for which to retrieve the HLS master playlist URL\.  
You must specify either the `StreamName` or the `StreamARN`\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+`   
Required: No

 ** [StreamName](#API_reader_GetHLSStreamingSessionURL_RequestSyntax) **   <a name="KinesisVideo-reader_GetHLSStreamingSessionURL-request-StreamName"></a>
The name of the stream for which to retrieve the HLS master playlist URL\.  
You must specify either the `StreamName` or the `StreamARN`\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: No

## Response Syntax<a name="API_reader_GetHLSStreamingSessionURL_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-type: application/json

{
   "[HLSStreamingSessionURL](#KinesisVideo-reader_GetHLSStreamingSessionURL-response-HLSStreamingSessionURL)": "string"
}
```

## Response Elements<a name="API_reader_GetHLSStreamingSessionURL_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [HLSStreamingSessionURL](#API_reader_GetHLSStreamingSessionURL_ResponseSyntax) **   <a name="KinesisVideo-reader_GetHLSStreamingSessionURL-response-HLSStreamingSessionURL"></a>
The URL \(containing the session token\) that a media player can use to retrieve the HLS master playlist\.  
Type: String

## Errors<a name="API_reader_GetHLSStreamingSessionURL_Errors"></a>

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

 **MissingCodecPrivateDataException**   
No codec private data was found in at least one of tracks of the video stream\.  
HTTP Status Code: 400

 **NoDataRetentionException**   
A `PlaybackMode` of `ON_DEMAND` was requested for a stream that does not retain data \(that is, has a `DataRetentionInHours` of 0\)\.   
HTTP Status Code: 400

 **NotAuthorizedException**   
Status Code: 403, The caller is not authorized to perform an operation on the given stream, or the token has expired\.  
HTTP Status Code: 401

 **ResourceNotFoundException**   
 `GetMedia` throws this error when Kinesis Video Streams can't find the stream that you specified\.  
 `GetHLSStreamingSessionURL` throws this error if a session with a `PlaybackMode` of `ON_DEMAND` is requested for a stream that has no fragments within the requested time range, or if a session with a `PlaybackMode` of `LIVE` is requested for a stream that has no fragments within the last 30 seconds\.  
HTTP Status Code: 404

 **UnsupportedStreamMediaTypeException**   
The type of the media \(for example, h\.264 video or ACC audio\) could not be determined from the codec IDs of the tracks in the first fragment for a playback session\. The codec ID for track 1 should be `V_MPEG/ISO/AVC` and, optionally, the codec ID for track 2 should be `A_AAC`\.  
HTTP Status Code: 400

## See Also<a name="API_reader_GetHLSStreamingSessionURL_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesis-video-reader-data-2017-09-30/GetHLSStreamingSessionURL) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesis-video-reader-data-2017-09-30/GetHLSStreamingSessionURL) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesis-video-reader-data-2017-09-30/GetHLSStreamingSessionURL) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesis-video-reader-data-2017-09-30/GetHLSStreamingSessionURL) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/kinesis-video-reader-data-2017-09-30/GetHLSStreamingSessionURL) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesis-video-reader-data-2017-09-30/GetHLSStreamingSessionURL) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesis-video-reader-data-2017-09-30/GetHLSStreamingSessionURL) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesis-video-reader-data-2017-09-30/GetHLSStreamingSessionURL) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesis-video-reader-data-2017-09-30/GetHLSStreamingSessionURL) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/kinesis-video-reader-data-2017-09-30/GetHLSStreamingSessionURL) 