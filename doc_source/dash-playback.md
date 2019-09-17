# Video Playback with MPEG\-DASH<a name="dash-playback"></a>

To view a Kinesis video stream using MPEG\-DASH, you first create a streaming session using [GetDASHStreamingSessionURL](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_reader_GetDASHStreamingSessionURL.html)\. This action returns a URL \(containing a session token\) for accessing the MPEG\-DASH session\. You can then use the URL in a media player or a standalone application to display the stream\. 

An Amazon Kinesis video stream has the following requirements for providing video through MPEG\-DASH:
+ The media must contain h\.264 or h\.265 encoded video and, optionally, AAC or G\.711 encoded audio\. Specifically, the codec ID of track 1 should be `V_MPEG/ISO/AVC` \(for h\.264\) or V\_MPEGH/ISO/HEVC \(for H\.265\)\. Optionally, the codec ID of track 2 should be `A_AAC` \(for AAC\) or A\_MS/ACM \(for G\.711\)\.
+ Data retention must be greater than 0\.
+ The video track of each fragment must contain codec private data in the Advanced Video Coding \(AVC\) for H\.264 format and HEVC for H\.265 format\. For more information, see [MPEG\-4 specification ISO/IEC 14496\-15](https://www.iso.org/standard/55980.html)\. For information about adapting stream data to a given format, see [NAL Adaptation Flags](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producer-reference-nal.html)\.
+ The audio track \(if present\) of each fragment must contain codec private data in the AAC format \([AAC specification ISO/IEC 13818\-7](https://www.iso.org/standard/43345.html)\) or the [MS Wave format](http://www-mmsp.ece.mcgill.ca/Documents/AudioFormats/WAVE/WAVE.html)\.

## Example: Using MPEG\-DASH in HTML and JavaScript<a name="dash-examp1e"></a>

The following example shows how to retrieve an MPEG\-DASH streaming session for a Kinesis video stream and play it back in a webpage\. The example shows how to play back video in the following players:
+ [Google Shaka Player](https://github.com/google/shaka-player)
+ [dash\.js](https://github.com/video-dev/hls.js/)

**Topics**
+ [Set Up the Kinesis Video Streams Client for MPEG\-DASH Playback](#dash-example-setup)
+ [Retrieve the Kinesis Video Streams Archived Content Endpoint for MPEG\-DASH Playback](#dash-example-endpoint)
+ [Retrieve the MPEG\-DASH Streaming Session URL](#dash-example-session)
+ [Display the Streaming Video with MPEG\-DASH Playback](#dash-example-display)
+ [Completed Example](#dash-example-complete)

### Set Up the Kinesis Video Streams Client for MPEG\-DASH Playback<a name="dash-example-setup"></a>

To access streaming video with MPEG\-DASH, first create and configure the Kinesis Video Streams client \(to retrieve the service endpoint\) and archived media client \(to retrieve the MPEG\-DASH streaming session\)\. The application retrieves the necessary values from input boxes on the HTML page\.

```
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

### Retrieve the Kinesis Video Streams Archived Content Endpoint for MPEG\-DASH Playback<a name="dash-example-endpoint"></a>

After the clients are initiated, retrieve the Kinesis Video Streams archived content endpoint so that you can retrieve the MPEG\-DASH streaming session URL as follows:

```
 // Step 2: Get a data endpoint for the stream
console.log('Fetching data endpoint');
kinesisVideo.getDataEndpoint({
    StreamName: streamName,
    APIName: "GET_DASH_STREAMING_SESSION_URL" 
}, function(err, response) {
    if (err) { return console.error(err); }
    console.log('Data endpoint: ' + response.DataEndpoint);
    kinesisVideoArchivedContent.endpoint = new AWS.Endpoint(response.DataEndpoint);
```

### Retrieve the MPEG\-DASH Streaming Session URL<a name="dash-example-session"></a>

When you have the archived content endpoint, call the [GetDASHStreamingSessionURL](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_reader_GetDASHStreamingSessionURL.html) API to retrieve the MPEG\-DASH streaming session URL as follows:

```
// Step 3: Get a Streaming Session URL
var consoleInfo = 'Fetching ' + protocol + ' Streaming Session URL';
console.log(consoleInfo);

if (protocol === 'DASH') {
    kinesisVideoArchivedContent.getDASHStreamingSessionURL({
        StreamName: streamName,
        PlaybackMode: $('#playbackMode').val(),
        DASHFragmentSelector: {
            FragmentSelectorType: $('#fragmentSelectorType').val(),
            TimestampRange: $('#playbackMode').val() === "LIVE" ? undefined : {
                StartTimestamp: new Date($('#startTimestamp').val()),
                EndTimestamp: new Date($('#endTimestamp').val())
            }
        },
        DisplayFragmentTimestamp: $('#displayFragmentTimestamp').val(),
        DisplayFragmentNumber: $('#displayFragmentNumber').val(),
        MaxManifestFragmentResults: parseInt($('#maxResults').val()),
        Expires: parseInt($('#expires').val())
    }, function(err, response) {
        if (err) { return console.error(err); }
        console.log('DASH Streaming Session URL: ' + response.DASHStreamingSessionURL);
```

### Display the Streaming Video with MPEG\-DASH Playback<a name="dash-example-display"></a>

When you have the MPEG\-DASH streaming session URL, provide it to the video player\. The method for providing the URL to the video player is specific to the player that you use\.

The following code example shows how to provide the streaming session URL to a [Google Shaka](https://github.com/google/shaka-player) player: 

```
// Step 4: Give the URL to the video player.

//Shaka Player elements 
<video id="shaka" class="player" controls autoplay></video>
<script src="https://cdnjs.cloudflare.com/ajax/libs/shaka-player/2.4.1/shaka-player.compiled.js">
</script>
...

var playerName = $('#player').val();

if (playerName === 'Shaka Player') {
    var playerElement = $('#shaka');
    playerElement.show();

    var player = new shaka.Player(playerElement[0]);
    console.log('Created Shaka Player');

    player.load(response.DASHStreamingSessionURL).then(function() {
        console.log('Starting playback');
    });
    console.log('Set player source');
}
```

The following code example shows how to provide the streaming session URL to an [dash\.js](https://github.com/Dash-Industry-Forum/dash.js/wiki) player: 

```
<!-- dash.js Player elements -->
<video id="dashjs" class="player" controls autoplay=""></video>
<script src="https://cdn.dashjs.org/latest/dash.all.min.js"></script>

...

var playerElement = $('#dashjs');
playerElement.show();

var player = dashjs.MediaPlayer().create();
console.log('Created DASH.js Player');

player.initialize(document.querySelector('#dashjs'), response.DASHStreamingSessionURL, true);
console.log('Starting playback');
console.log('Set player source');
}
```

### Completed Example<a name="dash-example-complete"></a>

You can [download or view the completed example code](https://github.com/aws-samples/amazon-kinesis-video-streams-hls-viewer/blob/master/index.html) on GitHub\.