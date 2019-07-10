# Step 3: Send Data to a Kinesis Video Stream<a name="gs-send-data"></a>

This section describes how to send media data from a camera to the Kinesis video stream you created in the previous step\. This section uses the [C\+\+ Producer Library](producer-sdk-cpp.md) as a [GStreamer](examples-gstreamer-plugin.md) plugin\.

To easily send media from a variety of devices on a variety of operating systems, this tutorial uses [GStreamer](https://gstreamer.freedesktop.org/), an open\-source media framework that standardizes access to cameras and other media sources\. 

The GStreamer example application is supported on the following operating systems:
+ Ubuntu
+ macOS
+ Microsoft Windows
+ Raspbian \(Raspberry Pi\)

For more information about using the GStreamer plugin to stream video from a file or an RTSP stream from a camera, see [Example: Kinesis Video Streams Producer SDK GStreamer Plugin](examples-gstreamer-plugin.md)\.

## Download the C\+\+ Producer SDK<a name="gs-send-data-1"></a>

The GStreamer sample is included in the C\+\+ Producer SDK\. You can download the C\+\+ Producer SDK from Github using the following [Git](https://git-scm.com/downloads) command: 

```
$ git clone https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp 
```

For information about SDK prerequisites and downloading, see [Step 1: Download and Configure the C\+\+ Producer Library Code](producersdk-cpp-download.md)\.

## Compile the GStreamer Example<a name="gs-send-data-2"></a>

You can compile and install the GStreamer sample in the ` kinesis-video-native-build` directory using the following commands:
+ macOS:
  + Install homebrew
  + Run `brew install pkg-config openssl cmake gstreamer gst-plugins-base gst-plugins-good gst-plugins-bad gst-plugins-ugly log4cplus`
  + Go to kinesis\-video\-native\-build directory and run `./min-install-script`
+ Ubuntu and Raspbian:
  + Run the following:
    + `$ sudo apt-get update`
    + `$ sudo apt-get install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev gstreamer1.0-plugins-base-apps`
    + `$ sudo apt-get install gstreamer1.0-plugins-bad gstreamer1.0-plugins-good gstreamer1.0-plugins-ugly gstreamer1.0-tools`
  + If on Raspbian, run `$ sudo apt-get install gstreamer1.0-omx` after running previous commands\.
  + Go to kinesis\-video\-native\-build directory and run `./min-install-script`
+ Windows:
  + Inside mingw32 or mingw64 shell, go to kinesis\-video\-native\-build directory and run `./min-install-script`

## Run the GStreamer Example<a name="gs-send-data-3"></a>

The GStreamer application sends media from your camera to the Kinesis Video Streams service\. You can run the GStreamer example application for your operating system with the following commands\. Run the example application from the `kinesis-video-native-build/downloads/local/bin` directory\. Use the following parameters for the command:
+ **Access key:** The AWS access key you recorded in the first step of this tutorial\.
+ **Secret key:** The AWS secret key you recorded in the first step of this tutorial\.
+ **AWS Region:** A region that supports Kinesis Video Streams\. For information on supported regions, see [Amazon Kinesis Video Streams Regions](https://docs.aws.amazon.com/general/latest/gr/rande.html#akv_region)\.

### Run the GStreamer Example on Ubuntu<a name="gs-send-data-3-ubuntu"></a>

You can run the GStreamer example application on Ubuntu with the following command\. Specify your camera device with the `device` parameter\.

```
$ gst-launch-1.0 v4l2src device=/dev/video0 ! videoconvert ! video/x-raw,format=I420,width=640,height=480 ! x264enc bframes=0 key-int-max=45 bitrate=512 tune=zerolatency ! h264parse ! video/x-h264,stream-format=avc,alignment=au,profile=baseline ! kvssink stream-name="MyKinesisVideoStream" storage-size=512 access-key="YourAccessKey" secret-key="YourSecretKey" aws-region="YourAWSRegion"
```

### Run the GStreamer Example on macOS<a name="gs-send-data-3-macos"></a>

You can run the GStreamer example application on MacOS with the following command:

```
$ gst-launch-1.0 autovideosrc ! videoconvert ! video/x-raw,format=I420,width=1280,height=720 ! vtenc_h264_hw allow-frame-reordering=FALSE realtime=TRUE max-keyframe-interval=45 bitrate=512 ! h264parse ! video/x-h264,stream-format=avc,alignment=au,profile=baseline ! kvssink stream-name=MyKinesisVideoStream storage-size=512 access-key="YourAccessKey" secret-key="YourSecretKey" aws-region="YourAWSRegion"
```

### Run the GStreamer Example on Windows<a name="gs-send-data-3-windows"></a>

You can run the GStreamer example application on Windows with the following command:

```
$ gst-launch-1.0 ksvideosrc ! videoconvert ! video/x-raw,format=I420,width=640,height=480 ! x264enc bframes=0 key-int-max=45 bitrate=512 tune=zerolatency ! h264parse ! video/x-h264,stream-format=avc,alignment=au,profile=baseline ! kvssink stream-name="MyKinesisVideoStream" storage-size=512 access-key="YourAccessKey" secret-key="YourSecretKey" aws-region="YourAWSRegion"
```

### Run the GStreamer Example on Raspbian \(Raspberry Pi\)<a name="gs-send-data-3-rpi"></a>

You can run the GStreamer example application on Raspbian with the following command\. Specify your camera device with the `device` parameter\.

```
$ gst-launch-1.0 v4l2src device=/dev/video0 ! videoconvert ! video/x-raw,format=I420,width=640,height=480 ! omxh264enc control-rate=2 target-bitrate=512000 periodicity-idr=45 inline-header=FALSE ! h264parse ! video/x-h264,stream-format=avc,alignment=au,profile=baseline ! kvssink stream-name="MyKinesisVideoStream" access-key="YourAccessKey" secret-key="YourSecretKey" aws-region="YourAWSRegion"
```

## Consume Media Data<a name="gs-send-data-4"></a>

You can consume media data by either viewing it in the console, or by creating an application that reads media data from a stream using HLS\.

### View Media Data in the Console<a name="gs-send-data-4-1"></a>

To view the media data sent from your camera in the Kinesis Video Streams console, open the Kinesis Video Streams console at [https://console\.aws\.amazon\.com/kinesisvideo/](https://console.aws.amazon.com/kinesisvideo/), and choose the **MyKinesisVideoStream** stream on the Manage Streams page\. The video plays in the Video Preview pane\. 

### Consume Media Data using HLS<a name="gs-send-data-4-2"></a>

You can create a client application that consumes data from a Kinesis video stream using Hypertext Live Streaming \(HLS\)\. For information about creating an application that consumes media data using HLS, see [Kinesis Video Streams Playback](how-playback.md)\.

## Next Step<a name="gs-next-step-4"></a>

[What's Next? ](gs-console-whatnext.md)