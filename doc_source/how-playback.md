# Kinesis Video Streams Playback<a name="how-playback"></a>

You can view a Kinesis video stream using the following methods:
+ **GetMedia**: You use the `GetMedia` API to build your own applications to process Kinesis video streams\. `GetMedia` is a real\-time API with low latency\. If you want to create a player that uses `GetMedia`, you have to build it yourself\. For information about how to develop an application that displays a Kinesis video stream using `GetMedia`, see [Stream Parser Library](parser-library.md)\.
+ **HLS**: [HTTP Live Streaming \(HLS\)](https://en.wikipedia.org/wiki/HTTP_Live_Streaming) is an industry\-standard HTTP\-based media streaming communications protocol\. You can use HLS to view an Amazon Kinesis video stream, either for live playback or to view archived video\. 

  You can use HLS for live playback\. Latency is typically between 3 and 5 seconds, but it can be between 1 and 10 seconds, depending on the use case, player, and network conditions\. You can use a third\-party player \(such as [Video\.js](https://github.com/videojs/video.js/) or [Google Shaka Player](https://github.com/google/shaka-player)\) to display the video stream by providing the HLS streaming session URL, either programmatically or manually\. You can also play back video by typing the HLS streaming session URL in the **Location** bar of the [Apple Safari](https://www.apple.com/safari/) or [Microsoft Edge](https://www.microsoft.com/en-us/windows/microsoft-edge) browsers\.
+ **MPEG\-DASH**: [Dynamic Adaptive Streaming over HTTP \(DASH\)](https://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP), also known as MPEG\-DASH, is an adaptive bitrate streaming protocol that enables high quality streaming of media content over the Internet delivered from conventional HTTP web servers\.

  You can use MPEG\-DASH for live playback\. Latency is typically between 3 and 5 seconds, but it can be between 1 and 10 seconds, depending on the use case, player, and network conditions\. You can use a third\-party player \(such as [dash\.js](https://github.com/Dash-Industry-Forum/dash.js/wiki) or [Google Shaka Player](https://github.com/google/shaka-player) to display the video stream by providing the MPEG\-DASH streaming session URL, either programmatically or manually\.

**Topics**
+ [Video and Audio Encodings Supported in Kinesis Video Streams](#playback-encoding)
+ [Video Playback with HLS](hls-playback.md)
+ [Video Playback with MPEG\-DASH](dash-playback.md)

## Video and Audio Encodings Supported in Kinesis Video Streams<a name="playback-encoding"></a>

By supporting the HLS protocol, Kinesis Video Streams enables you to use off\-the\-shelf media players to play your H\.264/5 and AAC encoded media archived in Kinesis Video Streams\. The HLS protocol features near universal support on web and mobile platforms\. However, HLS also has very limited support for encodings outside of H\.264/5 and AAC\. MPEG\-DASH, on the other hand, can support a much wider set of encodings, for example, a combination of H\.264 video and G711 audio encoding\.

 The following table describes all of the combinations of audio and video codecs that are currently supported by Kinesis Video Streams: 


****  

| Code | Message | Description | 
| --- | --- | --- | 
|  0x00000001  | STATUS\_NULL\_ARG  | NULL was passed for a mandatory argument\.  | 
|  0x00000002  | STATUS\_INVALID\_ARG  | An invalid value was specified for an argument\.  | 
|  0x00000003  | STATUS\_INVALID\_ARG\_LEN  | An invalid argument length was specified\. | 
|  0x00000004  | STATUS\_NOT\_ENOUGH\_MEMORY  | Could not allocate enough memory\.  | 
|  0x00000005  | STATUS\_BUFFER\_TOO\_SMALL  | The specified buffer size is too small\.  | 
|  0x00000006  | STATUS\_UNEXPECTED\_EOF  | An unexpected end of file was reached\.  | 
|  0x00000007  | STATUS\_FORMAT\_ERROR  | An invalid format was encountered\.  | 
|  0x00000008  | STATUS\_INVALID\_HANDLE\_ERROR  | Invalid handle value\.  | 
|  0x00000009  | STATUS\_OPEN\_FILE\_FAILED  | Failed to open a file\.  | 
|  0x0000000a  | STATUS\_READ\_FILE\_FAILED | Failed to read from a file\.  | 
|  0x0000000b  | STATUS\_WRITE\_TO\_FILE\_FAILED  | Failed to write to a file\.  | 
|  0x0000000c  | STATUS\_INTERNAL\_ERROR  | An internal error that normally doesn't occur and might indicate an SDK or service API bug\.  | 
|  0x0000000d  | STATUS\_INVALID\_OPERATION  | There was an invalid operation, or the operation is not permitted\.  | 
|  0x0000000e  | STATUS\_NOT\_IMPLEMENTED  | The feature is not implemented\.  | 
|  0x0000000f  | STATUS\_OPERATION\_TIMED\_OUT  | The operation timed out\.  | 
|  0x00000010  | STATUS\_NOT\_FOUND  | A required resource was not found\.  | 