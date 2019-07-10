# Video Playback with HLS<a name="hls-playback"></a>

To view a Kinesis video stream using HLS, you first create a streaming session using [GetHLSStreamingSessionURL](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_reader_GetHLSStreamingSessionURL.html)\. This action returns a URL \(containing a session token\) for accessing the HLS session\. You can then use the URL in a media player or a standalone application to display the stream\. 

An Amazon Kinesis video stream has the following requirements for providing video through HLS:
+ Data retention must be greater than 0\.
+ Track 1 of the stream must have a codec ID of `V_MPEG/ISO/AVC` and contain H\.264 encoded media\. If there is an audio track \(optional\), it must be track number 2 and have a codec ID of `A_AAC` and contain AAC encoded audio\.
+ The fragments must contain codec private data in the Advanced Video Coding \(AVC\) for H\.264 format \([ MPEG\-4 specification ISO/IEC 14496\-15](https://www.iso.org/standard/55980.html)\) for the video media\. They must also contain codec private data for ACC \([AAC specification ISO/IEC 13818\-7](https://www.iso.org/standard/43345.html)\) for the audio media \(if present\)\. For information about adapting stream data to a given format, see [NAL Adaptation Flags](producer-reference-nal.md)\. 

## Example: Using HLS in HTML and JavaScript<a name="how-hls-ex1"></a>

The following example shows how to retrieve an HLS streaming session for a Kinesis video stream and play it back in a webpage\. The example shows how to play back video in the following players:
+ [Video\.js](https://github.com/videojs/video.js/) 
+ [Google Shaka Player](https://github.com/google/shaka-player)
+ [hls\.js](https://github.com/video-dev/hls.js/)

**Topics**
+ [Set Up the Kinesis Video Streams Client for HLS Playback](#how-hls-ex1-setup)
+ [Retrieve the Kinesis Video Streams Archived Content Endpoint for HLS Playback](#how-hls-ex1-endpoint)
+ [Retrieve the HLS Streaming Session URL](#how-hls-ex1-session)
+ [Display the Streaming Video with HLS Playback](#how-hls-ex1-display)
+ [Troubleshooting HLS Issues](#how-hls-ex1-ts)
+ [Completed Example for HLS Playback](#how-hls-ex1-complete)

### Set Up the Kinesis Video Streams Client for HLS Playback<a name="how-hls-ex1-setup"></a>

To access streaming video with HLS, first create and configure the Kinesis Video Streams client \(to retrieve the service endpoint\) and archived media client \(to retrieve the HLS streaming session\)\. The application retrieves the necessary values from input boxes on the HTML page\.

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/aws-sdk/2.278.1/aws-sdk.min.js"></script>
...

var protocol = $('#protocol').val();
var streamName = $('#streamName').val();

// Step 1: Configure SDK Clients
var options = {
    accessKeyId: $('#accessKeyId').val(),
    secretAccessKey: $('#secretAccessKey').val(),
    sessionToken: $('#sessionToken').val() || undefined,
    region: $('#region').val(),
    endpoint: $('#endpoint').val() || undefined
}
var kinesisVideo = new AWS.KinesisVideo(options);
var kinesisVideoArchivedContent = new AWS.KinesisVideoArchivedMedia(options);
```

### Retrieve the Kinesis Video Streams Archived Content Endpoint for HLS Playback<a name="how-hls-ex1-endpoint"></a>

After the clients are initiated, retrieve the Kinesis Video Streams archived content endpoint to retrieve the HLS streaming session URL:

```
// Step 2: Get a data endpoint for the stream
console.log('Fetching data endpoint');
kinesisVideo.getDataEndpoint({
    StreamName: streamName,
    APIName: "GET_HLS_STREAMING_SESSION_URL"
    }, function(err, response) {
        if (err) { return console.error(err); }
        console.log('Data endpoint: ' + response.DataEndpoint);
        kinesisVideoArchivedContent.endpoint = new AWS.Endpoint(response.DataEndpoint);
```

### Retrieve the HLS Streaming Session URL<a name="how-hls-ex1-session"></a>

When you have the archived content endpoint, call the [GetHLSStreamingSessionURL](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_reader_GetHLSStreamingSessionURL.html) API to retrieve the HLS streaming session URL:

```
// Step 3: Get a Streaming Session URL
var consoleInfo = 'Fetching ' + protocol + ' Streaming Session URL';
console.log(consoleInfo);
                    
...

else {
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
            ContainerFormat: $('#containerFormat').val(),
            DiscontinuityMode: $('#discontinuityMode').val(),
            DisplayFragmentTimestamp: $('#displayFragmentTimestamp').val(),
            MaxMediaPlaylistFragmentResults: parseInt($('#maxResults').val()),
            Expires: parseInt($('#expires').val())
        }, function(err, response) {
            if (err) { return console.error(err); }
            console.log('HLS Streaming Session URL: ' + response.HLSStreamingSessionURL);
```

### Display the Streaming Video with HLS Playback<a name="how-hls-ex1-display"></a>

When you have the HLS streaming session URL, provide it to the video player\. The method for providing the URL to the video player is specific to the player used\.

The following code example shows how to provide the streaming session URL to a [Video\.js](https://github.com/videojs/video.js/) player: 

```
// VideoJS elements
<video id="videojs" class="player video-js vjs-default-skin" controls autoplay></video>
<link rel="stylesheet" href="https://vjs.zencdn.net/6.6.3/video-js.css">
<script src="https://vjs.zencdn.net/6.6.3/video.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/videojs-contrib-hls/5.14.1/videojs-contrib-hls.js"></script>
...

else if (playerName === 'VideoJS') {
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
```

The following code example shows how to provide the streaming session URL to a [Google Shaka](https://github.com/google/shaka-player) player: 

```
// Shaka Player elements
<video id="shaka" class="player" controls autoplay></video>
<script src="https://cdnjs.cloudflare.com/ajax/libs/shaka-player/2.4.1/shaka-player.compiled.js">
</script>

...

else if (playerName === 'Shaka Player') {
var playerElement = $('#shaka');
playerElement.show();
var player = new shaka.Player(playerElement[0]);
console.log('Created Shaka Player');
player.load(response.HLSStreamingSessionURL).then(function() {
    console.log('Starting playback');
});
console.log('Set player source');
```

The following code example shows how to provide the streaming session URL to an [hls\.js](https://github.com/video-dev/hls.js/) player: 

```
// HLS.js elements
<video id="hlsjs" class="player" controls autoplay></video>
<script src="https://cdn.jsdelivr.net/npm/hls.js@latest"></script>

...

var playerName = $('#player').val();
if (playerName == 'HLS.js') {
    var playerElement = $('#hlsjs');
    playerElement.show();
    var player = new Hls();
    console.log('Created HLS.js Player');
    player.loadSource(response.HLSStreamingSessionURL);
    player.attachMedia(playerElement[0]);
    console.log('Set player source');
    player.on(Hls.Events.MANIFEST_PARSED, function() {
        video.play();
        console.log('Starting playback');
    });
}
```

### Troubleshooting HLS Issues<a name="how-hls-ex1-ts"></a>

If the video stream does not play back correctly, see [Troubleshooting HLS Issues](troubleshooting.md#troubleshooting-hls)\.

### Completed Example for HLS Playback<a name="how-hls-ex1-complete"></a>

You can [download or view the completed example code](https://github.com/aws-samples/amazon-kinesis-video-streams-hls-viewer/blob/master/index.html)\.