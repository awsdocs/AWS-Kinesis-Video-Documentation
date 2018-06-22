# Amazon Kinesis Video Streams Examples<a name="examples"></a>

The following code examples demonstrate how to work with the Kinesis Video Streams API:
+ [GStreamer Plugin](examples-gstreamer-plugin.md): Shows how to build the Kinesis Video Streams Producer SDK to use as a GStreamer destination\.
+ [Example: Sending Data to Kinesis Video Streams Using the PutMedia API](examples-putmedia.md): Send data that is already in a container format \(MKV\) using the [PutMedia](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_dataplane_PutMedia.html) API\.
+ [Example: Streaming from an RTSP Source](examples-rtsp.md): Sample application that runs in a Docker container and streams video from an RTSP source\.
+ [Example: Using GStreamer with Kinesis Video Streams](examples-gstreamer.md): Send video data to Kinesis Video Streams using the [GStreamer](https://gstreamer.freedesktop.org/) open source multimedia framework\.
+ [Example: Parsing and Rendering Kinesis Video Streams Fragments](examples-renderer.md): Parse and render Kinesis video stream fragments using [JCodec](http://jcodec.org/) and [JFrame](https://docs.oracle.com/javase/7/docs/api/javax/swing/JFrame.html)\.
+ [KinesisVideoExample](parser-library-write.md#parser-library-write-example): Parse and log video fragments using the Kinesis Video Streams Parser Library\.

## Prerequisites<a name="examples-prerequisites"></a>
+ In the sample code, you provide credentials by specifying a profile that you set in your AWS credentials profile file, or by providing credentials in the Java system properties of your integrated development environment \(IDE\)\. So if you haven't already done so, first set up your credentials\. For more information, see [Set up AWS Credentials and Region for Development](http://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html)\.
+ We recommend that you use a Java IDE to view and run the code, such as one of the following:
  + [Eclipse Java Neon](http://www.eclipse.org/downloads/packages/eclipse-ide-java-developers/neon3)
  + [JetBrains IntelliJ IDEA](https://www.jetbrains.com/idea/)