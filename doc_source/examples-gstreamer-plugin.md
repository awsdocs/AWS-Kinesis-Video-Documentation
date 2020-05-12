# Example: Kinesis Video Streams Producer SDK GStreamer Plugin<a name="examples-gstreamer-plugin"></a>

This topic shows how to build the Amazon Kinesis Video Streams Producer SDK to use as a GStreamer plugin\. 

**Topics**
+ [Download, Build, and Configure the GStreamer Element](#examples-gstreamer-plugin-download)
+ [Run the GStreamer Element](#examples-gstreamer-plugin-run)
+ [Example GStreamer Launch Commands](#examples-gstreamer-plugin-launch)
+ [Run the GStreamer Element in a Docker Container](#examples-gstreamer-plugin-docker)
+ [GStreamer Element Parameter Reference](examples-gstreamer-plugin-parameters.md)

[GStreamer](https://gstreamer.freedesktop.org/) is a popular media framework used by a multitude of cameras and video sources to create custom media pipelines by combining modular plugins\. The Kinesis Video Streams GStreamer plugin greatly simplifies the integration of your existing GStreamer media pipeline with Kinesis Video Streams\. After integrating GStreamer, you can get started with streaming video from a webcam or RTSP \(Real Time Streaming Protocol\) camera to Kinesis Video Streams for real\-time or later playback, storage, and further analysis\. 

The GStreamer plugin automatically manages the transfer of your video stream to Kinesis Video Streams by encapsulating the functionality provided by the Kinesis Video Streams Producer SDK in a GStreamer sink element, `kvssink`\. The GStreamer framework provides a standard managed environment for constructing media flow from a device such as a camera or other video source for further processing, rendering, or storage\. 

The GStreamer pipeline typically consists of the link between a source \(video camera\) and the sink element \(either a player to render the video, or storage for offline retrieval\)\. In this example, you use the Producer SDK element as a *sink*, or media destination, for your video source \(webcam or IP camera\)\. The plugin element that encapsulates the SDK then manages sending the video stream to Kinesis Video Streams\. 

This topic shows how to construct a GStreamer media pipeline capable of streaming video from a video source, such as a web camera or RTSP stream, typically connected through intermediate encoding stages \(using H\.264 encoding\) to Kinesis Video Streams\. When your video stream is available as a Kinesis video stream, you can use the Kinesis Video Stream Parser Library for further processing, playback, storage, or analysis of your video stream\.

![\[Functional view of the GStreamer media pipeline for streaming video from a camera to the Kinesis Video Streams service.\]](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/images/gstreamer-pipeline.png)

## Download, Build, and Configure the GStreamer Element<a name="examples-gstreamer-plugin-download"></a>

The GStreamer Plugin example is included with the Kinesis Video Streams C\+\+ Producer SDK\. For information about SDK prerequisites and downloading, see [Step 1: Download and Configure the C\+\+ Producer Library Code](producersdk-cpp-download.md)\.

You can build the Producer SDK GStreamer sink as a dynamic library on macOS, Ubuntu, Raspberry Pi, or Windows\. The GStreamer plugin is located in your `build` directory\. To load this plugin, it needs to be in your `GST_PLUGIN_PATH`\. Run the following command:

```
export GST_PLUGIN_PATH=`pwd`/build
```

## Run the GStreamer Element<a name="examples-gstreamer-plugin-run"></a>

To run GStreamer with the Kinesis Video Streams Producer SDK element as a sink, execute the `gst-launch-1.0` command\. Use settings that are appropriate for the GStreamer plugin to use\. For example, [v4l2src](https://gstreamer.freedesktop.org/data/doc/gstreamer/head/gst-plugins-good/html/gst-plugins-good-plugins-v4l2src.html) for v4l2 devices on Linux systems, or [rtspsrc](https://gstreamer.freedesktop.org/data/doc/gstreamer/head/gst-plugins-good/html/gst-plugins-good-plugins-rtspsrc.html) for RTSP devices\. Specify `kvssink` as the sink \(final destination of the pipeline\) to send video to the Producer SDK\. 

The `kvssink` element has the following required parameters:
+ `stream-name`: The name of the destination Kinesis video stream\.
+ `storage-size`: The storage size of the device in kilobytes\. For information about configuring device storage, see [StorageInfo](producer-reference-structures-producer.md#producer-reference-structures-producer-storageinfo)\.
+ `access-key`: The AWS access key that is used to access Kinesis Video Streams\. You must provide either this parameter or `credential-path`\.
+ `secret-key`: The AWS secret key that is used to access Kinesis Video Streams\. You must provide either this parameter or `credential-path`\.
+ `credential-path`: A path to a file containing your credentials for accessing Kinesis Video Streams\.  For more information on rotating credentials, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)\. You must provide either this parameter or `access-key` and `secret-key`\.

For information about `kvssink` optional parameters, see [GStreamer Element Parameter Reference](examples-gstreamer-plugin-parameters.md)\.

For the latest information about GStreamer plugins and parameters, see [GStreamer Plugins](https://gstreamer.freedesktop.org/data/doc/gstreamer/head/), or execute the following command to list options:

```
gst-inspect-1.0 kvssink
```

If the build failed or GST\_PLUGIN\_PATH is not proprely set, your output looks similar to this:

```
No such element or plugin 'kvssink'
```

## Example GStreamer Launch Commands<a name="examples-gstreamer-plugin-launch"></a>

These examples demonstrate how to use a GStreamer plugin to stream video from different types of devices\.

### Example 1: Stream Video from an RTSP Camera on Ubuntu<a name="examples-gstreamer-plugin-launch-ex1"></a>

The following command creates a GStreamer pipeline on Ubuntu that streams from a network RTSP camera, using the [rtspsrc](https://gstreamer.freedesktop.org/data/doc/gstreamer/head/gst-plugins-good/html/gst-plugins-good-plugins-rtspsrc.html) GStreamer plugin:

```
$ gst-launch-1.0 rtspsrc location="rtsp://YourCameraRtspUrl" short-header=TRUE ! rtph264depay ! video/x-h264, format=avc,alignment=au ! kvssink stream-name="YourStreamName" storage-size=512 access-key="YourAccessKey" secret-key="YourSecretKey" aws-region="YourAWSRegion"
```

### Example 2: Encode and Stream Video from a USB Camera on Ubuntu<a name="examples-gstreamer-plugin-launch-ex2"></a>

The following command creates a GStreamer pipeline on Ubuntu that encodes the stream from a USB camera in H\.264 format, and streams it to Kinesis Video Streams\. This example uses the [v412src](https://gstreamer.freedesktop.org/data/doc/gstreamer/head/gst-plugins-good/html/gst-plugins-good-plugins-v4l2src.html) GStreamer plugin\.

```
$ gst-launch-1.0 v4l2src do-timestamp=TRUE device=/dev/video0 ! videoconvert ! video/x-raw,format=I420,width=640,height=480,framerate=30/1 ! x264enc  bframes=0 key-int-max=45 bitrate=500 ! video/x-h264,stream-format=avc,alignment=au,profile=baseline ! kvssink stream-name="YourStreamName" storage-size=512 access-key="YourAccessKey" secret-key="YourSecretKey" aws-region="YourAWSRegion"
```

### Example 3: Stream Pre\-Encoded Video from a USB Camera on Ubuntu<a name="examples-gstreamer-plugin-launch-ex3"></a>

The following command creates a GStreamer pipeline on Ubuntu that streams video that the camera has already encoded in H\.264 format to Kinesis Video Streams\. This example uses the [v412src](https://gstreamer.freedesktop.org/data/doc/gstreamer/head/gst-plugins-good/html/gst-plugins-good-plugins-v4l2src.html) GStreamer plugin\.

```
$ gst-launch-1.0 v4l2src do-timestamp=TRUE device=/dev/video0 ! h264parse ! video/x-h264,stream-format=avc,alignment=au ! kvssink stream-name="plugin" storage-size=512 access-key="YourAccessKey" secret-key="YourSecretKey" aws-region="YourAWSRegion"
```

### Example 4: Stream Video from a Network Camera on macOS<a name="examples-gstreamer-plugin-launch-ex4"></a>

The following command creates a GStreamer pipeline on macOS that streams video to Kinesis Video Streams from a network camera\. This example uses the [rtspsrc](https://gstreamer.freedesktop.org/data/doc/gstreamer/head/gst-plugins-good/html/gst-plugins-good-plugins-rtspsrc.html) GStreamer plugin\.

```
$ gst-launch-1.0 rtspsrc location="rtsp://YourCameraRtspUrl" short-header=TRUE ! rtph264depay ! video/x-h264, format=avc,alignment=au ! kvssink stream-name="YourStreamName" storage-size=512  access-key="YourAccessKey" secret-key="YourSecretKey" aws-region="YourAWSRegion"
```

### Example 5: Stream Video from a Network Camera on Windows<a name="examples-gstreamer-plugin-launch-ex5"></a>

The following command creates a GStreamer pipeline on Windows that streams video to Kinesis Video Streams from a network camera\. This example uses the [rtspsrc](https://gstreamer.freedesktop.org/data/doc/gstreamer/head/gst-plugins-good/html/gst-plugins-good-plugins-rtspsrc.html) GStreamer plugin\.

```
$ gst-launch-1.0 rtspsrc location="rtsp://YourCameraRtspUrl" short-header=TRUE ! rtph264depay ! video/x-h264, format=avc,alignment=au ! kvssink stream-name="YourStreamName" storage-size=512  access-key="YourAccessKey" secret-key="YourSecretKey" aws-region="YourAWSRegion"
```

### Example 6: Stream Video from a Camera on Raspberry Pi<a name="examples-gstreamer-plugin-launch-ex6"></a>

The following command creates a GStreamer pipeline on Raspberry Pi that streams video to Kinesis Video Streams\. This example uses the [v412src](https://gstreamer.freedesktop.org/data/doc/gstreamer/head/gst-plugins-good/html/gst-plugins-good-plugins-v4l2src.html) GStreamer plugin\.

```
$ gst-launch-1.0 v4l2src do-timestamp=TRUE device=/dev/video0 ! videoconvert ! video/x-raw,format=I420,width=640,height=480,framerate=30/1 ! omxh264enc control-rate=1 target-bitrate=5120000 periodicity-idr=45 inline-header=FALSE ! h264parse ! video/x-h264,stream-format=avc,alignment=au,width=640,height=480,framerate=30/1,profile=baseline ! kvssink stream-name="YourStreamName" frame-timestamp=dts-only access-key="YourAccessKey" secret-key="YourSecretKey" aws-region="YourAWSRegion"
```

### Example 7: Stream Video from a Camera on Raspberry Pi and Specify Region<a name="examples-gstreamer-plugin-launch-ex7"></a>

The following command creates a GStreamer pipeline on Raspberry Pi that streams video to Kinesis Video Streams in the US East \(N\. Virginia\) region\. This example uses the [v412src](https://gstreamer.freedesktop.org/data/doc/gstreamer/head/gst-plugins-good/html/gst-plugins-good-plugins-v4l2src.html) GStreamer plugin\.

```
$ gst-launch-1.0 v4l2src do-timestamp=TRUE device=/dev/video0 ! videoconvert ! video/x-raw,format=I420,width=640,height=480,framerate=30/1 ! omxh264enc control-rate=1 target-bitrate=5120000 periodicity-idr=45 inline-header=FALSE ! h264parse ! video/x-h264,stream-format=avc,alignment=au,width=640,height=480,framerate=30/1,profile=baseline ! kvssink stream-name="YourStreamName" frame-timestamp=dts-only access-key="YourAccessKey" secret-key="YourSecretKey" aws-region="YourAWSRegion"
```

### Example 8: Stream both audio and video in Raspberry\-PI and Ubuntu<a name="examples-gstreamer-plugin-launch-ex8"></a>

See how to [run the gst\-launch\-1\.0 command to start streaming both audio and video in Raspberry\-PI and Ubuntu](https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp/blob/master/docs/linux.md#running-the-gst-launch-10-command-to-start-streaming-both-audio-and-video-in-raspberry-pi-and-ubuntu)\.

### Example 9: Stream both audio and video in MacOS<a name="examples-gstreamer-plugin-launch-ex9"></a>

See how to [run the gst\-launch\-1\.0 command to start streaming both audio and video in MacOS](https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp/blob/master/docs/macos.md#running-the-gst-launch-10-command-to-start-streaming-both-audio-and-raw-video-in-mac-os)\.

### Example 10: Upload MKV file that contains both audio and video<a name="examples-gstreamer-plugin-launch-ex10"></a>

See how to [run the gst\-launch\-1\.0 command to upload MKV file that contains both audio and video](https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp/blob/master/docs/windows.md#running-the-gst-launch-10-command-to-upload-mkv-file-that-contains-both-audio-and-video)\.

## Run the GStreamer Element in a Docker Container<a name="examples-gstreamer-plugin-docker"></a>

Docker is a platform for developing, deploying, and running applications using containers\. Using Docker to create the GStreamer pipeline standardizes the operating environment for Kinesis Video Streams, which greatly simplifies building and executing the application\.

To install and configure Docker, see the following:
+ [Docker download instructions](https://www.docker.com/community-edition#/download)
+ [Getting started with Docker](https://docs.docker.com/get-started/)

After installing Docker, you can download the Kinesis Video Streams C\+\+ Producer SDK \(and GStreamer plugin\) from Amazon Elastic Container Registry using the `docker pull` command\.

To run GStreamer with the Kinesis Video Streams Producer SDK element as a sink in a Docker container, do the following:

**Topics**
+ [Authenticate your Docker Client](#examples-gstreamer-plugin-docker-authenticate)
+ [Download the Docker Image for Ubuntu, macOS, Windows, or Raspberry Pi](#examples-gstreamer-plugin-docker-download)
+ [Run the Docker Image](#examples-gstreamer-plugin-docker-run)

### Authenticate your Docker Client<a name="examples-gstreamer-plugin-docker-authenticate"></a>

Authenticate your Docker client to the Amazon ECR registry that you intend to pull your image from\. You must get authentication tokens for each registry used, and the tokens are valid for 12 hours\. For more information, see [Registry Authentication](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html#registry_auth) in the *Amazon Elastic Container Registry User Guide*\.

**Example : Authenticate with Amazon ECR**  

```
aws ecr get-login --no-include-email --region us-west-2 --registry-ids 546150905175
```
The preceding command produces output similar to the following:  

```
docker login -u AWS -p <Password>   https://YourAccountId.dkr.ecr.us-west-2.amazonaws.com
```
The resulting output is a Docker login command that you use to authenticate your Docker client to your Amazon ECR registry\. 

### Download the Docker Image for Ubuntu, macOS, Windows, or Raspberry Pi<a name="examples-gstreamer-plugin-docker-download"></a>

Download the Docker image to your Docker environment using one the following commands, depending on your operating system:

#### Download the Docker Image for Ubuntu<a name="examples-gstreamer-plugin-docker-download-ubuntu"></a>

```
sudo docker pull 546150905175.dkr.ecr.us-west-2.amazonaws.com/kinesis-video-producer-sdk-cpp-amazon-linux:latest
```

#### Download the Docker Image for macOS<a name="examples-gstreamer-plugin-docker-download-macos"></a>

```
sudo docker pull 546150905175.dkr.ecr.us-west-2.amazonaws.com/kinesis-video-producer-sdk-cpp-amazon-linux:latest
```

#### Download the Docker Image for Windows<a name="examples-gstreamer-plugin-docker-download-windows"></a>

```
docker pull 546150905175.dkr.ecr.us-west-2.amazonaws.com/kinesis-video-producer-sdk-cpp-amazon-windows:latest
```

#### Download the Docker Image for Raspberry Pi<a name="examples-gstreamer-plugin-docker-download-rpi"></a>

```
sudo docker pull 546150905175.dkr.ecr.us-west-2.amazonaws.com/kinesis-video-producer-sdk-cpp-raspberry-pi:latest
```

To verify that the image was successfully added, use the following command:

```
docker images
```

### Run the Docker Image<a name="examples-gstreamer-plugin-docker-run"></a>

Use one of the following commands to run the Docker image, depending on your operating system:

#### Run the Docker Image on Ubuntu<a name="examples-gstreamer-plugin-docker-run-ubuntu"></a>

```
sudo docker run -it --network="host" --device=/dev/video0 546150905175
.dkr.ecr.us-west-2.amazonaws.com/kinesis-video-producer-sdk-cpp-amazon-linux /bin/bash
```

#### Run the Docker Image on macOS<a name="examples-gstreamer-plugin-docker-run-macos"></a>

```
sudo docker run -it --network="host" 546150905175.dkr.ecr.us-west-2.amazonaws.com/kinesis-video-producer-sdk-cpp-amazon-linux /bin/bash
```

#### Run the Docker Image on Windows<a name="examples-gstreamer-plugin-docker-run-windows"></a>

```
docker run -it 546150905175.dkr.ecr.us-west-2.amazonaws.com/kinesis-video-producer-sdk-cpp-windows <AWS_ACCESS_KEY_ID> <AWS_SECRET_ACCESS_KEY> <RTSP_URL> <STREAM_NAME>
```

#### Run the Docker Image on Raspberry Pi<a name="examples-gstreamer-plugin-docker-run-rpi"></a>

```
sudo docker run -it --device=/dev/video0 --device=/dev/vchiq -v /opt/vc:/opt/vc 546150905175
.dkr.ecr.us-west-2.amazonaws.com/kinesis-video-producer-sdk-cpp-raspberry-pi /bin/bash
```

Docker launches the container, and presents you with a command prompt for executing commands within the container\.

In the container, set the environment variables using the following command:

```
export LD_LIBRARY_PATH=/opt/awssdk/amazon-kinesis-video-streams-producer-sdk-cpp/kinesis-video-native-build/downloads/local/lib:$LD_LIBRARY_PATH
export PATH=/opt/awssdk/amazon-kinesis-video-streams-producer-sdk-cpp/kinesis-video-native-build/downloads/local/bin:$PATH
export GST_PLUGIN_PATH=/opt/awssdk/amazon-kinesis-video-streams-producer-sdk-cpp/kinesis-video-native-build/downloads/local/lib:$GST_PLUGIN_PATH
```

Start streaming from the camera using the `gst-launch-1.0` command that is appropriate for your device\. 

**Note**  
On macOS, you can only stream video from a network camera when running GStreamer in a Docker container\. Streaming video from a USB camera on macOS in a Docker container is not supported\. 

For examples of using the `gst-launch-1.0` command to connect to a local web camera or a network RTSP camera, see [Launch Commands](#examples-gstreamer-plugin-launch)\.