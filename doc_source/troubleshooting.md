# Troubleshooting Kinesis Video Streams<a name="troubleshooting"></a>

Use the following information to troubleshoot common issues encountered with Amazon Kinesis Video Streams\.


+ [General Troubleshooting](#troubleshooting-general)
+ [API Troubleshooting](#troubleshooting-api)
+ [Java Issues](#troubleshooting-java)
+ [Producer Library Issues](#troubleshooting-producer)
+ [Stream Parser Library Issues](#troubleshooting-parser)

## General Troubleshooting<a name="troubleshooting-general"></a>

This section describes general issues that you might encounter when working with Kinesis Video Streams\.


+ [Latency too high](#troubleshooting-general-latency)

### Latency too high<a name="troubleshooting-general-latency"></a>

Latency might be caused by the duration of fragments that are sent to the Kinesis Video Streams service\. One way to reduce the latency between the producer and the service is to configure the media pipeline to produce shorter fragment durations\.

To reduce the number of frames sent in each fragment, and thus reduce the amount of time for each fragment, reduce the following value in `kinesis_video_gstreamer_sample_app.cpp`:

```
g_object_set(G_OBJECT (data.encoder), "bframes", 0, "key-int-max", 45, "bitrate", 512, NULL);
```

## API Troubleshooting<a name="troubleshooting-api"></a>

This section describes API issues that you might encounter when working with Kinesis Video Streams\.


+ [Error: “Unable to determine service/operation name to be authorized”](#troubleshooting-api-name-auth)
+ [Error: “Failed to put a frame in the stream”](#troubleshooting-api-putframe)
+ [Error: “Service closed connection before final AckEvent was received”](#troubleshooting-api-closeconnection)

### Error: “Unable to determine service/operation name to be authorized”<a name="troubleshooting-api-name-auth"></a>

`GetMedia` can fail with the following error:

```
Unable to determine service/operation name to be authorized
```

This error might occur if the endpoint is not properly specified\. When getting the endpoint, be sure to include the following parameter in the `GetDataEndpoint` call, depending on the API to be called:

```
--api-name GET_MEDIA
--api-name PUT_MEDIA
--api-name GET_MEDIA_FOR_FRAGMENT_LIST
--api-name LIST_FRAGMENTS
```

### Error: “Failed to put a frame in the stream”<a name="troubleshooting-api-putframe"></a>

`PutMedia` can fail with the following error:

```
Failed to put a frame in the stream
```

This error might occur if connectivity or permissions are not available to the service\. Run the following in the AWS CLI, and verify that the stream information can be retrieved:

```
aws kinesisvideo describe-stream --stream-name StreamName --endpoint https://ServiceEndpoint.kinesisvideo.region.amazonaws.com
```

If the call fails, see [Troubleshooting AWS CLI Errors](http://docs.aws.amazon.com/cli/latest/userguide/troubleshooting.html) for more information\.

### Error: “Service closed connection before final AckEvent was received”<a name="troubleshooting-api-closeconnection"></a>

`PutMedia` can fail with the following error:

```
com.amazonaws.SdkClientException: Service closed connection before final AckEvent was received
```

This error might occur if `PushbackInputStream` is improperly implemented\. Ensure that the `unread()` methods are correctly implemented\.

## Java Issues<a name="troubleshooting-java"></a>

This section describes how to troubleshoot common Java issues encountered when working with Kinesis Video Streams\.


+ [Enabling Java logs](#troubleshooting-java-log)
+ [Java libraries or samples do not compile in Eclipse](#troubleshooting-java-compile)

### Enabling Java logs<a name="troubleshooting-java-log"></a>

To troubleshoot issues with Java samples and libraries, it is helpful to enable and examine the debug logs\. To enable debug logs, do the following:

1. Add `log4j` to the `pom.xml``` file, in the `dependencies` node: 

   ```
   <dependency>
       <groupId>log4j</groupId>
       <artifactId>log4j</artifactId>
       <version>1.2.17</version>
   </dependency>
   ```

1. In the `target/classes` directory, create a file named `log4j.properties` with the following contents:

   ```
   # Root logger option
   log4j.rootLogger=DEBUG, stdout
   
   # Redirect log messages to console
   log4j.appender.stdout=org.apache.log4j.ConsoleAppender
   log4j.appender.stdout.Target=System.out
   log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
   log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n
   
   log4j.logger.org.apache.http.wire=DEBUG
   ```

The debug logs then print to the IDE console\.

### Java libraries or samples do not compile in Eclipse<a name="troubleshooting-java-compile"></a>

If a library or sample fails to compile in the Eclipse IDE, verify that Lombok is properly configured\. For information about configuring Lombok, see the [Lombok configuration information](https://projectlombok.org/setup/eclipse) on the Lombok Project site\. Specifically, verify that you are using Java 8\. There are incompatibility issues between Lombok and Java 9\.

## Producer Library Issues<a name="troubleshooting-producer"></a>

This section describes issues that you might encounter when using the [Producer Libraries](producer-sdk.md)\.


+ [Error: "Security token included in the request is invalid" when streaming data using the GStreamer demo application](#troubleshooting-producer-general-securitytoken)
+ [GStreamer application stops with "streaming stopped, reason not\-negotiated" message on OS X](#troubleshooting-producer-failed-stream-osx)
+ [Error: "Failed to allocate heap" when creating Kinesis Video Client in GStreamer demo on Raspberry Pi](#troubleshooting-producer-raspberrypi-heap)
+ [Error: "Illegal Instruction" when running GStreamer demo on Raspberry Pi](#troubleshooting-producer-raspberrypi-illegalinstruction)
+ [Camera fails to load on Raspberry Pi](#troubleshooting-producer-raspberrypi-camera)
+ [Camera can't be found on macOS High Sierra](#troubleshooting-producer-sierra-camera)
+ [Time stamp/range assertion at run time on Raspberry Pi](#troubleshooting-producer-raspberrypi-timestamp-assert)
+ [Assertion on gst\_value\_set\_fraction\_range\_full on Raspberry Pi](#troubleshooting-producer-raspberrypi-gst-assert)

### Error: "Security token included in the request is invalid" when streaming data using the GStreamer demo application<a name="troubleshooting-producer-general-securitytoken"></a>

If this error occurs, there is an issue with your credentials\. Verify the following:

+ If you are using temporary credentials, you must specify the session token\.

+ Verify that your temporary credentials are not expired\.

+ Verify that you have the proper rights set up\.

+ On macOS, verify that you do not have credentials cached in Keychain\.

### GStreamer application stops with "streaming stopped, reason not\-negotiated" message on OS X<a name="troubleshooting-producer-failed-stream-osx"></a>

Streaming may stop on OS X with the following message:

```
Debugging information: gstbasesrc.c(2939): void gst_base_src_loop(GstPad *) (): /GstPipeline:test-pipeline/GstAutoVideoSrc:source/GstAVFVideoSrc:source-actual-src-avfvide:
streaming stopped, reason not-negotiated (-4)
```

A possible workaround for this is to remove the framerate parameters from the `gst_caps_new_simple` call in `kinesis_video_gstreamer_sample_app.cpp`:

```
GstCaps *h264_caps = gst_caps_new_simple("video/x-h264",
                                             "profile", G_TYPE_STRING, "baseline",
                                             "stream-format", G_TYPE_STRING, "avc",
                                             "alignment", G_TYPE_STRING, "au",
                                             "width", GST_TYPE_INT_RANGE, 320, 1920,
                                             "height", GST_TYPE_INT_RANGE, 240, 1080,
                                             "framerate", GST_TYPE_FRACTION_RANGE, 0, 1, 30, 1,
                                             NULL);
```

### Error: "Failed to allocate heap" when creating Kinesis Video Client in GStreamer demo on Raspberry Pi<a name="troubleshooting-producer-raspberrypi-heap"></a>

The GStreamer sample application tries to allocate 512 MB of RAM, which might not be available on your system\. You can reduce this allocation by reducing the following value in `KinesisVideoProducer.cpp`:

```
device_info.storageInfo.storageSize = 512 * 1024 * 1024;
```

### Error: "Illegal Instruction" when running GStreamer demo on Raspberry Pi<a name="troubleshooting-producer-raspberrypi-illegalinstruction"></a>

If you encounter the following error when executing the GStreamer demo, ensure that you have compiled the application for the correct version of your device\. \(For example, ensure that you are not compiling for Raspberry Pi 3 when you are running on Raspberry Pi 2\.\)

```
INFO - Initializing curl.
Illegal instruction
```

### Camera fails to load on Raspberry Pi<a name="troubleshooting-producer-raspberrypi-camera"></a>

To check whether the camera is loaded, run the following:

```
$ ls /dev/video*
```

If nothing is found, run the following:

```
$ vcgencmd get_camera
```

The output should look similar to the following:

```
supported=1 detected=1
```

If the driver does not detect the camera, do the following:

1. Check the physical camera setup and verify that it's connected properly\.

1. Run the following to upgrade the firmware:

   ```
   $ sudo rpi-update
   ```

1. Restart the device\.

1. Run the following to load the driver:

   ```
   $ sudo modprobe bcm2835-v4l2
   ```

1. Verify that the camera was detected:

   ```
   $ ls /dev/video*
   ```

### Camera can't be found on macOS High Sierra<a name="troubleshooting-producer-sierra-camera"></a>

On macOS High Sierra, the demo application can't find the camera if more than one camera is available\.

### Time stamp/range assertion at run time on Raspberry Pi<a name="troubleshooting-producer-raspberrypi-timestamp-assert"></a>

If a time stamp range assertion occurs at run time, update the firmware and restart the device:

```
$ sudo rpi-update 
$ sudo reboot
```

### Assertion on gst\_value\_set\_fraction\_range\_full on Raspberry Pi<a name="troubleshooting-producer-raspberrypi-gst-assert"></a>

The following assertion appears if the `uv4l` service is running: 

```
gst_util_fraction_compare (numerator_start, denominator_start, numerator_end, denominator_end) < 0' failed
```

If this occurs, stop the `uv4l` service and restart the application\.

## Stream Parser Library Issues<a name="troubleshooting-parser"></a>

This section describes issues that you might encounter when using the [Stream Parser Library](parser-library.md)\.

### Cannot access a single frame from the stream<a name="troubleshooting-parser-frame"></a>

To access a single frame from a streaming source in your consumer application, you must ensure that your stream contains the correct codec private data\. For information about the format of the data in a stream, see [Data Model](how-data.md)\. 

To learn how to use codec private data to access a frame, see the following test file on the GitHub website: [KinesisVideoRendererExampleTest\.java](https://github.com/aws/amazon-kinesis-video-streams-parser-library/blob/master/src/test/java/com/amazonaws/kinesisvideo/parser/examples/KinesisVideoRendererExampleTest.java)