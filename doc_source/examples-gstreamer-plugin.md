# Example: Kinesis Video Streams Producer SDK GStreamer Plugin<a name="examples-gstreamer-plugin"></a>

This topic shows how to build the Amazon Kinesis Video Streams Producer SDK to use as a GStreamer plugin\. 

**Topics**
+ [Download, Build, and Configure the GStreamer Element](#examples-gstreamer-plugin-download)
+ [Run the GStreamer Element](#examples-gstreamer-plugin-run)
+ [Example GStreamer Launch Commands](#examples-gstreamer-plugin-launch)
+ [GStreamer Element Parameter Reference](examples-gstreamer-plugin-parameters.md)

[GStreamer](https://gstreamer.freedesktop.org/) is a popular media framework used by a multitude of cameras and video sources to create custom media pipelines by combining modular plugins\. The Kinesis Video Streams GStreamer plugin greatly simplifies the integration of your existing GStreamer media pipeline with Kinesis Video Streams\. After integrating GStreamer, you can get started with streaming video from a webcam or RTSP \(Real Time Streaming Protocol\) camera to Kinesis Video Streams for real\-time or later playback, storage, and further analysis\. 

The GStreamer plugin automatically manages the transfer of your video stream to Kinesis Video Streams by encapsulating the functionality provided by the Kinesis Video Streams Producer SDK in a GStreamer sink element, `kvssink`\. The GStreamer framework provides a standard managed environment for constructing media flow from a device such as a camera or other video source for further processing, rendering, or storage\. 

The GStreamer pipeline typically consists of the link between a source \(video camera\) and the sink element \(either a player to render the video, or storage for offline retrieval\)\. In this example, you use the Producer SDK element as a *sink*, or media destination, for your video source \(webcam or IP camera\)\. The plugin element that encapsulates the SDK then manages sending the video stream to Kinesis Video Streams\. 

This topic shows how to construct a GStreamer media pipeline capable of streaming video from a video source, such as a web camera or RTSP stream, typically connected through intermediate encoding stages \(using H\.264 encoding\) to Kinesis Video Streams\. When your video stream is available as a Kinesis video stream, you can use the Kinesis Video Stream Parser Library for further processing, playback, storage, or analysis of your video stream\.

![\[Functional view of the GStreamer media pipeline for streaming video from a camera to the Kinesis Video Streams service.\]](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/images/gstreamer-pipeline.png)

## Download, Build, and Configure the GStreamer Element<a name="examples-gstreamer-plugin-download"></a>

The GStreamer Plugin example is included with the Kinesis Video Streams C\+\+ Producer SDK\. For information about SDK prerequisites and downloading, see [Step 1: Download and Configure the C\+\+ Producer Library Code](producersdk-cpp-download.md)\.

To build the Producer SDK GStreamer sink as a dynamic library on macOS, Ubuntu, or Raspberry Pi, execute the following command in the `kinesis-video-native-build` directory:

```
./gstreamer-plugin-install-script
```

After the sink is built, you can execute `gst-launch-1.0` from the following directory:

```
<YourSdkFolderPath>/kinesis-video-native-build/downloads/local/bin
```

You can either run `gst-launch-1.0` from this directory, or add it to the `PATH` environment variable:

```
$ export PATH=<YourSdkFolderPath>/kinesis-video-native-build/downloads/local/bin:$PATH
```

Add the library directory to your path so that the GStreamer plugin can be found:

```
export GST_PLUGIN_PATH=<YourSdkFolderPath>/kinesis-video-native-build/downloads/local/lib:$GST_PLUGIN_PATH
```

Set the library path for the SDK:

```
export LD_LIBRARY_PATH=<YourSdkFolderPath>/kinesis-video-native-build/downloads/local/lib
```

## Run the GStreamer Element<a name="examples-gstreamer-plugin-run"></a>

To run GStreamer with the Kinesis Video Streams Producer SDK element as a sink, execute the `gst-launch-1.0` command\. Use settings that are appropriate for the GStreamer plugin to use—for example, [v412src](https://gstreamer.freedesktop.org/data/doc/gstreamer/head/gst-plugins-good/html/gst-plugins-good-plugins-v4l2src.html) for v412 devices on Linux systems, or [rtspsrc](https://gstreamer.freedesktop.org/data/doc/gstreamer/head/gst-plugins-good/html/gst-plugins-good-plugins-rtspsrc.html) for RTSP devices\. Specify `kvssink` as the sink \(final destination of the pipeline\) to send video to the Producer SDK\. 

The `kvssink` element has the following required parameters:
+ `stream-name`: The name of the destination Kinesis video stream\.
+ `storage-size`: The storage size of the device in kilobytes\. For information about configuring device storage, see [StorageInfo](producer-reference-structures-producer.md#producer-reference-structures-producer-storageinfo)\.

For information about `kvssink` optional parameters, see [GStreamer Element Parameter Reference](examples-gstreamer-plugin-parameters.md)\.

For the latest information about GStreamer plugins and parameters, see [GStreamer Plugins](https://gstreamer.freedesktop.org/data/doc/gstreamer/head/), or execute the following command to list options:

```
gst-inspect-1.0 kvssink
```

## Example GStreamer Launch Commands<a name="examples-gstreamer-plugin-launch"></a>

These examples demonstrate how to use a GStreamer plugin to stream video from different types of devices\.

### Example 1: Stream Video from an RTSP Camera on Ubuntu<a name="examples-gstreamer-plugin-launch-ex1"></a>

The following command creates a GStreamer pipeline on Ubuntu that streams from a network RTSP camera, using the [rtspsrc](https://gstreamer.freedesktop.org/data/doc/gstreamer/head/gst-plugins-good/html/gst-plugins-good-plugins-rtspsrc.html) GStreamer plugin:

```
$ gst-launch-1.0 rtspsrc location=“rtsp://YourCameraRtspUrl” short-header=TRUE ! rtph264depay ! video/x-h264, format=avc,alignment=au ! kvssink stream-name=“YourStreamName” storage-size=512 
```

### Example 2: Encode and Stream Video from a USB Camera on Ubuntu<a name="examples-gstreamer-plugin-launch-ex2"></a>

The following command creates a GStreamer pipeline on Ubuntu that encodes the stream from a USB camera in H\.264 format, and streams it to Kinesis Video Streams\. This example uses the [v412src](https://gstreamer.freedesktop.org/data/doc/gstreamer/head/gst-plugins-good/html/gst-plugins-good-plugins-v4l2src.html) GStreamer plugin\.

```
$ gst-launch-1.0 v4l2src do-timestamp=TRUE device=/dev/video0 ! videoconvert ! video/x-raw,format=I420,width=640,height=480,framerate=30/1 ! x264enc  bframes=0 key-int-max=45 bitrate=500 ! video/x-h264,stream-format=avc,alignment=au ! kvssink stream-name="YourStreamName" storage-size=512 
```

### Example 3: Stream Pre\-Encoded Video from a USB Camera on Ubuntu<a name="examples-gstreamer-plugin-launch-ex3"></a>

The following command creates a GStreamer pipeline on Ubuntu that streams video that the camera has already encoded in H\.264 format to Kinesis Video Streams\. This example uses the [v412src](https://gstreamer.freedesktop.org/data/doc/gstreamer/head/gst-plugins-good/html/gst-plugins-good-plugins-v4l2src.html) GStreamer plugin\.

```
$ gst-launch-1.0 v4l2src do-timestamp=TRUE device=/dev/video0 ! h264parse ! video/x-h264,stream-format=avc,alignment=au ! kvssink stream-name="plugin" storage-size=512 
```

### Example 4: Stream Video from a Camera on macOS<a name="examples-gstreamer-plugin-launch-ex4"></a>

The following command creates a GStreamer pipeline on macOS that streams video to Kinesis Video Streams\. This example uses the [autovideosrc](https://gstreamer.freedesktop.org/data/doc/gstreamer/head/gst-plugins-good/html/gst-plugins-good-plugins-autovideosrc.html) GStreamer plugin\.

```
$ gst-launch-1.0 autovideosrc ! videoconvert ! video/x-raw,format=I420,width=640,height=480,framerate=30/1 ! vtenc_h264_hw allow-frame-reordering=FALSE realtime=TRUE max-keyframe-interval=45 bitrate=500 ! h264parse ! video/x-h264,stream-format=avc,alignment=au,width=640,height=480,framerate=30/1 ! kvssink stream-name="YourStreamName" storage-size=512
```

### Example 5: Stream Video from a Camera on Raspberry Pi<a name="examples-gstreamer-plugin-launch-ex5"></a>

The following command creates a GStreamer pipeline on Raspberry Pi that streams video to Kinesis Video Streams\. This example uses the [v412src](https://gstreamer.freedesktop.org/data/doc/gstreamer/head/gst-plugins-good/html/gst-plugins-good-plugins-v4l2src.html) GStreamer plugin\.

```
$ gst-launch-1.0 v4l2src do-timestamp=TRUE device=/dev/video0 ! videoconvert ! video/x-raw,format=I420,width=640,height=480,framerate=30/1 ! omxh264enc control-rate=1 target-bitrate=5120000 periodicity-idr=45 inline-header=FALSE ! h264parse ! video/x-h264,stream-format=avc,alignment=au,width=640,height=480,framerate=30/1 ! kvssink stream-name="YourStreamName" frame-timestamp=KVS_SINK_TIMESTAMP_DTS_ONLY access-key="YourAccessKey" secret-key="YourSecretKey"
```