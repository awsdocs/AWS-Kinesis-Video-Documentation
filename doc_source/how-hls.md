# Kinesis Video Streams Playback with HLS<a name="how-hls"></a>

[HTTP Live Streaming \(HLS\)](https://en.wikipedia.org/wiki/HTTP_Live_Streaming) is an industry\-standard HTTP\-based media streaming communications protocol\. You can use HLS to view an Amazon Kinesis video stream, either for live playback or to view archived video\.

You can view a Kinesis video stream using either HLS or the [GetMedia](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_dataplane_GetMedia.html) API\. The differences between these methods are as follows:
+ **GetMedia**: You use the `GetMedia` API to build your own applications to process Kinesis video streams\. `GetMedia` is a real\-time API with low latency\. If you want to create a player that uses `GetMedia`, you have to build it yourself\. For information about how to develop an application that displays a Kinesis video stream using `GetMedia`, see [Stream Parser Library](parser-library.md)\.
+ **HLS**: You can use HLS for live playback\. Latency is typically between 3 and 5 seconds, but it can be between 1 and 10 seconds, depending on the use case, player, and network conditions\. You can use a third\-party player \(such as [Video\.js](https://github.com/videojs/video.js/) or [Google Shaka Player](https://github.com/google/shaka-player)\) to display the video stream by providing the HLS streaming session URL, either programmatically or manually\. You can also play back video by typing the HLS streaming session URL in the **Location** bar of the [Apple Safari](https://www.apple.com/safari/) or [Microsoft Edge](https://www.microsoft.com/en-us/windows/microsoft-edge) browsers\.

To view a Kinesis video stream using HLS, you first create a streaming session using [GetHLSStreamingSessionURL](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_reader_GetHLSStreamingSessionURL.html)\. This action returns a URL \(containing a session token\) for accessing the HLS session\. You can then use the URL in a media player or a standalone application to display the stream\. 

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
<script src="aws-sdk/dist/aws-sdk-all.js"></script>
...

// Step 1: Configure SDK Clients
var options = {
    accessKeyId: 'ACCESS_KEY_ID',
    secretAccessKey: 'SECRET_KEY',
    region: 'REGION'
}
var kinesisVideo = new AWS.KinesisVideo(options);
var kinesisVideoArchivedContent = new AWS.KinesisVideoArchivedMedia(options);
```

### Retrieve the Kinesis Video Streams Archived Content Endpoint<a name="how-hls-ex1-endpoint"></a>

After the clients are initiated, retrieve the Kinesis Video Streams archived content endpoint to retrieve the HLS streaming session URL:

```
// Step 2: Get a data endpoint for the stream
kinesisVideo.getDataEndpoint({
    StreamName: 'your_kvs_stream_name',
    APIName: "GET_HLS_STREAMING_SESSION_URL"
}, function(err, response) {
    if (err) { return console.error(err); }
    kinesisVideoArchivedContent.endpoint = new AWS.Endpoint(response.DataEndpoint);
});
```

### Retrieve the HLS Streaming Session URL<a name="how-hls-ex1-session"></a>

When you have the archived content endpoint, call the [GetHLSStreamingSessionURL](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_reader_GetHLSStreamingSessionURL.html) API to retrieve the HLS streaming session URL:

```
kinesisVideoArchivedContent.getHLSStreamingSessionURL({
    StreamName: 'your_kvs_stream_name',
    PlaybackMode: 'LIVE'
    // Additional parameters can go here
}, function(err, response) {
    if (err) { return console.error(err); }
    var streamingSessionURL = response.HLSStreamingSessionURL;
});
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

var player = videojs('videojs');
player.src({
    src: streamingSessionURL,
    type: 'application/x-mpegURL'
});
player.play();
```

The following code example shows how to provide the streaming session URL to a [Google Shaka](https://github.com/google/shaka-player) player: 

```
<!-- Shaka Player elements -->
<video id="shaka" controls autoplay></video>
<script src="https://cdnjs.cloudflare.com/ajax/libs/shaka-player/2.4.1/shaka-player.compiled.js"></script>
                
...

var playerElement = $('#shaka');

var player = new shaka.Player(document.getElementById('shaka'));
console.log('Created Shaka Player');

player.load(streamingSessionURL);
```

### Troubleshooting<a name="how-hls-ex1-ts"></a>

If the video stream does not play back correctly, see [Troubleshooting HLS Issues](troubleshooting.md#troubleshooting-hls)\.

### Completed Example<a name="how-hls-ex1-complete"></a>

The following example shows the completed HTML page\. You can view the example code [here](https://github.com/aws-samples/amazon-kinesis-video-streams-hls-viewer/blob/master/index.html), or view the completed page [here](https://aws-samples.github.io/amazon-kinesis-video-streams-hls-viewer/)\.

**Example**  

```
<html>
    <head>
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css">
        <link rel="stylesheet" href="http://vjs.zencdn.net/6.6.3/video-js.css">
        <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.slim.min.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js"></script>
        <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js"></script>
        <script src="aws-sdk-all.js"></script>
    </head>
    <body>
        <div class="container">
            <h1 style="margin: 20px 0;">Kinesis Video Streams HLS Sample</h1>
            <div class="row">
                <div class="col-md-4">
                    <div class="form-group">
                        <label>Player</label>
                        <select id="player" class="form-control form-control-sm">
                            <option selected>VideoJS</option>
                            <option>Shaka Player</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Region</label>
                        <select id="region" class="form-control form-control-sm">
                            <option>ap-northeast-1</option>
                            <option>eu-central-1</option>
                            <option>eu-west-1</option>
                            <option selected>us-east-1</option>
                            <option>us-west-2</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>AWS Access Key</label>
                        <input id="accessKeyId" type="password" class="form-control form-control-sm"/>
                    </div>
                    <div class="form-group">
                        <label>AWS Secret Key</label>
                        <input id="secretAccessKey" type="password" class="form-control form-control-sm"/>
                    </div>
                    <div class="form-group">
                        <label>AWS Session Token (Optional)</label>
                        <input id="sessionToken" type="password" class="form-control form-control-sm" />
                    </div>
                    <div class="form-group">
                        <label>Endpoint (Optional)</label>
                        <input id="endpoint" type="text" class="form-control form-control-sm" />
                    </div>
                    <div class="form-group">
                        <label>Stream name</label>
                        <input id="streamName" type="text" class="form-control form-control-sm"/>
                    </div>
                    <div class="form-group">
                        <label>Playback Mode</label>
                        <select id="playbackMode" class="form-control form-control-sm">
                            <option selected>LIVE</option>
                            <option>ON_DEMAND</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Start Timestamp</label>
                        <input id="startTimestamp" type="datetime-local" class="form-control form-control-sm"/>
                    </div>
                    <div class="form-group">
                        <label>End Timestamp</label>
                        <input id="endTimestamp" type="datetime-local" class="form-control form-control-sm"/>
                    </div>
                    <div class="form-group">
                        <label>Fragment Selector Type</label>
                        <select id="fragmentSelectorType" class="form-control form-control-sm">
                            <option selected>SERVER_TIMESTAMP</option>
                            <option>PRODUCER_TIMESTAMP</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Discontinuity Mode</label>
                        <select id="discontinuityMode" class="form-control form-control-sm">
                            <option selected>ALWAYS</option>
                            <option>NEVER</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Max Media Playlist Fragment Results</label>
                        <input id="maxMediaPlaylistFragmentResults" type="text" class="form-control form-control-sm"/>
                    </div>
                    <div class="form-group">
                        <label>Expires (seconds)</label>
                        <input id="expires" type="text" class="form-control form-control-sm"/>
                    </div>
                    <button id="start" type="submit" class="btn btn-primary">Start Playback</button>
                </div>
                <div class="col-md-8">
                    <div id="playerContainer">

                        <!-- VideoJS elements -->
                        <video id="videojs" class="player video-js vjs-default-skin" controls autoplay></video>
                        <script src="http://vjs.zencdn.net/6.6.3/video.js"></script>
                        <script src="https://cdnjs.cloudflare.com/ajax/libs/videojs-contrib-hls/5.14.1/videojs-contrib-hls.js"></script>

                        <!-- Shaka Player elements -->
                        <video id="shaka" class="player" controls autoplay></video>
                        <script src="https://cdnjs.cloudflare.com/ajax/libs/shaka-player/2.4.1/shaka-player.compiled.js"></script>
                    </div>

                    <h3 style="margin-top: 20px;">Logs</h3>
                    <div class="card bg-light mb-3">
                        <pre id="logs" class="card-body text-monospace" style="font-family: monospace; white-space: pre-wrap;"></pre>
                    </div>
                </div>
            </div>
        </div>
        <script>
            configureLogging();

            $('#start').click(function() {
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

                // Step 2: Get a data endpoint for the stream
                console.log('Fetching data endpoint');
                kinesisVideo.getDataEndpoint({
                    StreamName: streamName,
                    APIName: "GET_HLS_STREAMING_SESSION_URL"
                }, function(err, response) {
                    if (err) { return console.error(err); }
                    console.log('Data endpoint: ' + response.DataEndpoint);
                    kinesisVideoArchivedContent.endpoint = new AWS.Endpoint(response.DataEndpoint);

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
                        } else if (playerName === 'Shaka Player') {
                            var playerElement = $('#shaka');
                            playerElement.show();

                            var player = new shaka.Player(playerElement[0]);
                            console.log('Created Shaka Player');

                            player.load(response.HLSStreamingSessionURL).then(function() {
                                console.log('Starting playback');
                            });
                            console.log('Set player source');
                        }
                    });
                });

                $('.player').hide();
            });

            // Read/Write all of the fields to/from localStorage so that fields are not lost on refresh.
            [
                'player',
                'region',
                'accessKeyId',
                'secretAccessKey',
                'sessionToken',
                'endpoint',
                'streamName',
                'playbackMode',
                'startTimestamp',
                'endTimestamp',
                'fragmentSelectorType',
                'discontinuityMode',
                'maxMediaPlaylistFragmentResults',
                'expires'
            ].forEach(function(field) {
                var id = "#" + field;

                // Read field from localStorage
                try {
                    var localStorageValue = localStorage.getItem(field);
                    if (localStorageValue) { $(id).val(localStorageValue); }
                } catch (e) { /* Don't use localStorage */ }

                // Write field to localstorage on change event
                $(id).change(function() {
                    try {
                        localStorage.setItem(field, $(id).val());
                    } catch (e) { /* Don't use localStorage */ }
                });
            });

            // Setup disabled/enabled state for timestamp fields
            $('#playbackMode').change(function() {
                updateTimestampFieldState();
            });
            updateTimestampFieldState();

            // Initially hide the player elements
            $('.player').hide();

            function configureLogging() {
                console._error = console.error;
                console.error = function(messages) {
                    log('ERROR', Array.prototype.slice.call(arguments));
                    console._error.apply(this, arguments);
                }

                console._log = console.log;
                console.log = function(messages) {
                    log('INFO', Array.prototype.slice.call(arguments));
                    console._log.apply(this, arguments);
                }

                function log(level, messages) {
                    var text = '';
                    for (message of messages) {
                        if (typeof message === 'object') { message = JSON.stringify(message, null, 2); }
                        text += ' ' + message;
                    }
                    $('#logs').append($('<div>').text('[' + level + ']' + text + '\n'));
                }
            }

            function updateTimestampFieldState() {
                var isLive = $('#playbackMode').val() === 'LIVE';
                $('#startTimestamp').prop('disabled', isLive);
                $('#endTimestamp').prop('disabled', isLive);
            }

            console.log("Page loaded")
        </script>
        <style>
            #playerContainer, .player { width: 100%; height: auto; min-height: 400px; background-color: black; }
            .vjs-big-play-button { display: none !important; }
        </style>
    </body>
</html>
```