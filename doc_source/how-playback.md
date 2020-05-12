# Kinesis Video Streams Playback<a name="how-playback"></a>

You can view a Kinesis video stream using the following methods:
+ **GetMedia**: You can use the `GetMedia` API to build your own applications to process Kinesis video streams\. `GetMedia` is a real\-time API with low latency\. If you want to create a player that uses `GetMedia`, you have to build it yourself\. For information about how to develop an application that displays a Kinesis video stream using `GetMedia`, see [Stream Parser Library](parser-library.md)\.
+ **HLS**: [HTTP Live Streaming \(HLS\)](https://en.wikipedia.org/wiki/HTTP_Live_Streaming) is an industry\-standard HTTP\-based media streaming communications protocol\. You can use HLS to view an Amazon Kinesis video stream, either for live playback or to view archived video\. 

  You can use HLS for live playback\. Latency is typically between 3 and 5 seconds, but it can be between 1 and 10 seconds, depending on the use case, player, and network conditions\. You can use a third\-party player \(such as [Video\.js](https://github.com/videojs/video.js/) or [Google Shaka Player](https://github.com/google/shaka-player)\) to display the video stream by providing the HLS streaming session URL, either programmatically or manually\. You can also play back video by typing the HLS streaming session URL in the **Location** bar of the [Apple Safari](https://www.apple.com/safari/) or [Microsoft Edge](https://www.microsoft.com/en-us/windows/microsoft-edge) browsers\.
+ **MPEG\-DASH**: [Dynamic Adaptive Streaming over HTTP \(DASH\)](https://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP), also known as MPEG\-DASH, is an adaptive bitrate streaming protocol that enables high quality streaming of media content over the Internet delivered from conventional HTTP web servers\.

  You can use MPEG\-DASH for live playback\. Latency is typically between 3 and 5 seconds, but it can be between 1 and 10 seconds, depending on the use case, player, and network conditions\. You can use a third\-party player \(such as [dash\.js](https://github.com/Dash-Industry-Forum/dash.js/wiki) or [Google Shaka Player](https://github.com/google/shaka-player) to display the video stream by providing the MPEG\-DASH streaming session URL, either programmatically or manually\.
+ **GetClip API**: You can use the `GetClip` API to download a clip \(in an MP4 file\) containing the archived, on\-demand media from the specified video stream over the specified time range\. For more information, see the [GetClip](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_reader_GetClip.html) API reference\.

**Topics**
+ [Video Playback with HLS](hls-playback.md)
+ [Video Playback with MPEG\-DASH](dash-playback.md)