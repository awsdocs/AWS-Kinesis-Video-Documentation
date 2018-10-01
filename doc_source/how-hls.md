# Kinesis Video Streams Playback with HLS<a name="how-hls"></a>

[HTTP Live Streaming \(HLS\)](https://en.wikipedia.org/wiki/HTTP_Live_Streaming) is an industry\-standard HTTP\-based media streaming communications protocol\. You can use HLS to view an Amazon Kinesis video stream, either for live playback or to view archived video\.

You can view a Kinesis video stream using either HLS or the [GetMedia](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_dataplane_GetMedia.html) API\. The differences between these methods are as follows:
+ **GetMedia**: You use the `GetMedia` API to build your own applications to process Kinesis video streams\. `GetMedia` is a real\-time API with low latency\. If you want to create a player that uses `GetMedia`, you have to build it yourself\. For information about how to develop an application that displays a Kinesis video stream using `GetMedia`, see [Stream Parser Library](parser-library.md)\.
+ **HLS**: You can use HLS for live playback\. Latency is typically between 3 and 5 seconds, but it can be between 1 and 10 seconds, depending on the use case, player, and network conditions\. You can use a third\-party player \(such as [Video\.js](https://github.com/videojs/video.js/) or [Google Shaka Player](https://github.com/google/shaka-player)\) to display the video stream by providing the HLS streaming session URL, either programmatically or manually\. You can also play back video by typing the HLS streaming session URL in the **Location** bar of the [Apple Safari](https://www.apple.com/safari/) or [Microsoft Edge](https://www.microsoft.com/en-us/windows/microsoft-edge) browsers\.

To view a Kinesis video stream using HLS, you first create a streaming session using [GetHLSStreamingSessionURL](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_reader_GetHLSStreamingSessionURL.html)\. This action returns a URL \(containing a session token\) for accessing the HLS session\. You can then use the URL in a media player or a standalone application to display the stream\. 

An Amazon Kinesis video stream has the following requirements for providing video through HLS:
+ The media type must be `video/h264`\.
+ Data retention must be greater than 0\.
+ The fragments must contain codec private data in the AVC \(Advanced Video Coding\) for H\.264 format \([MPEG\-4 specification ISO/IEC 14496\-15](https://www.iso.org/standard/55980.html)\)\. For information about adapting stream data to a given format, see [NAL Adaptation Flags](producer-reference-nal.md)\. 

## Example: Using HLS in HTML and JavaScript<a name="how-hls-ex1"></a>

The following example shows how to retrieve an HLS streaming session for a Kinesis video stream and play it back in a webpage\. The example shows how to play back video in the following players:
+ [Video\.js](https://github.com/videojs/video.js/) 
+ [Google Shaka Player](https://github.com/google/shaka-player)

**Topics**
+ [Set Up the Kinesis Video Streams Client](#how-hls-ex1-setup)
+ [Retrieve the Kinesis Video Streams Archived Content Endpoint](#how-hls-ex1-endpoint)
+ [Retrieve the HLS Streaming Session URL](#how-hls-ex1-session)
+ [Display the Streaming Video](#how-hls-ex1-display)
+ [Troubleshooting](#how-hls-ex1-ts)
+ [Completed Example](#how-hls-ex1-complete)

### Set Up the Kinesis Video Streams Client<a name="how-hls-ex1-setup"></a>

To access streaming video with HLS, first create and configure the Kinesis Video Streams client \(to retrieve the service endpoint\) and archived media client \(to retrieve the HLS streaming session\)\. The application retrieves the necessary values from input boxes on the HTML page\.

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/aws-sdk/2.278.1/aws-sdk.min.js"></script>
...

var streamName = $('#streamName').val();

// Step 1: Configure SDK Clients
var options = {
    accessKeyId: 'ACCESS_KEY_ID',
    secretAccessKey: 'SECRET_KEY',
    sessionToken: $('#sessionToken').val() || undefined,
    region: 'REGION',
    endpoint: $('#endpoint').val() || undefined
}
var kinesisVideo = new AWS.KinesisVideo(options);
var kinesisVideoArchivedContent = new AWS.KinesisVideoArchivedMedia(options);
```

### Retrieve the Kinesis Video Streams Archived Content Endpoint<a name="how-hls-ex1-endpoint"></a>

After the clients are initiated, retrieve the Kinesis Video Streams archived content endpoint to retrieve the HLS streaming session URL:

```
// Step 2: Get a data endpoint for the stream
kinesisVideo.getDataEndpoint({
    StreamName: streamName,
    APIName: "GET_HLS_STREAMING_SESSION_URL"
}, function(err, response) {
    if (err) { return console.error(err); }
    console.log('Data endpoint: ' + response.DataEndpoint);
    kinesisVideoArchivedContent.endpoint = new AWS.Endpoint(response.DataEndpoint);
});
```

### Retrieve the HLS Streaming Session URL<a name="how-hls-ex1-session"></a>

When you have the archived content endpoint, call the [GetHLSStreamingSessionURL](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_reader_GetHLSStreamingSessionURL.html) API to retrieve the HLS streaming session URL:

```
// Step 3: Get an HLS Streaming Session URL
console.log('Fetching HLS Streaming Session URL');
kinesisVideoArchivedContent.getHLSStreamingSessionURL({
    StreamName: streamName,
    PlaybackMode: $('#playbackMode').val(),
    HLSFragmentSelector: {
        FragmentSelectorType: $('#fragmentSelectorType').val(),
        TimestampRange: $('#playbackMode').val() === "LIVE" ? undefined : {
            StartTimestamp: new Date($('#startTimestamp').val()),
            EndTimestamp: new Date($('#endTimestamp').val())
        }
    },
    DiscontinuityMode: $('#discontinuityMode').val(),
    MaxMediaPlaylistFragmentResults: parseInt($('#maxMediaPlaylistFragmentResults').val()),
    Expires: parseInt($('#expires').val())
}, function(err, response) {
    if (err) { return console.error(err); }
    console.log('HLS Streaming Session URL: ' + response.HLSStreamingSessionURL);
```

### Display the Streaming Video<a name="how-hls-ex1-display"></a>

When you have the HLS streaming session URL, provide it to the video player\. The method for providing the URL to the video player is specific to the player used\.

The following code example shows how to provide the streaming session URL to a [Video\.js](https://github.com/videojs/video.js/) player: 

```
<!-- VideoJS elements -->
<video id="videojs" class="video-js vjs-default-skin" controls autoplay></video>
<script src="https://vjs.zencdn.net/6.6.3/video.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/videojs-contrib-hls/5.14.1/videojs-contrib-hls.js"></script>

...

// Step 4: Give the URL to the video player.
var playerName = $('#player').val();
if (playerName === 'VideoJS') {
    var playerElement = $('#videojs');
    playerElement.show();
    var player = videojs('videojs');
    console.log('Created VideoJS Player');
    
    player.src({
        src: response.HLSStreamingSessionURL,
        type: 'application/x-mpegURL'
    });
    console.log('Set player source');
    
    player.play();
    console.log('Starting playback');
}
```

The following code example shows how to provide the streaming session URL to a [Google Shaka](https://github.com/google/shaka-player) player: 

```
<!-- Shaka Player elements -->
<video id="shaka" controls autoplay></video>
<script src="https://cdnjs.cloudflare.com/ajax/libs/shaka-player/2.4.1/shaka-player.compiled.js"></script>
                
...

if (playerName === 'Shaka Player') {
    var playerElement = $('#shaka');
    playerElement.show();
    var player = new shaka.Player(playerElement[0]);
    console.log('Created Shaka Player');
    player.load(response.HLSStreamingSessionURL).then(function() {
        console.log('Starting playback');
});
console.log('Set player source');
```

### Troubleshooting<a name="how-hls-ex1-ts"></a>

If the video stream does not play back correctly, see [Troubleshooting HLS Issues](troubleshooting.md#troubleshooting-hls)\.

### Completed Example<a name="how-hls-ex1-complete"></a>

You can download or view the completed example code [here](https://github.com/aws-samples/amazon-kinesis-video-streams-hls-viewer/blob/master/index.html)\.