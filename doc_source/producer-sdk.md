# Kinesis Video Streams Producer Libraries<a name="producer-sdk"></a>

The Amazon Kinesis Video Streams Producer libraries are a set of easy\-to\-use libraries that are part of the Kinesis Video Streams Producer SDK\. The client uses the libraries and SDK to build the on\-device application for securely connecting to Kinesis Video Streams and streaming video and other media data that can be viewed in the console or client applications in real time\. 

Media data can be streamed in the following ways:
+ Streaming media data in real time
+ Streaming media data after buffering it for a few seconds
+ Streaming after\-the\-fact media uploads

After you create a Kinesis Video Streams stream, you can start sending data to the stream\. You can use the SDK to create application code that extracts the video data \(frames\) from the media source and uploads it to Kinesis Video Streams\. These applications are also referred to as *producer* applications\.

The Producer libraries contain the following components:
+ [Kinesis Video Streams Producer Client](#producer-sdk-client)
+ [Kinesis Video Streams Producer Library](#producer-sdk-library)

## Kinesis Video Streams Producer Client<a name="producer-sdk-client"></a>

The Kinesis Video Streams Producer Client includes a single `KinesisVideoClient` class\. This class manages media sources, receives data from the sources, and manages the stream lifecycle as data flows from a media source to Kinesis Video Streams\. Furthermore, it provides a `MediaSource` interface for defining the interaction between Kinesis Video Streams and your proprietary hardware and software\.

A media source can be almost anything\. For example, you can use a camera media source or a microphone media source\. Media sources are not limited to audio and video sources only\. For example, data logs might be text files, but they can still be sent as a stream of data\. You could also have multiple cameras on your phone that stream data simultaneously\.

To get data from any of these sources, you can implement the `MediaSource` interface\. This interface enables additional scenarios for which we don’t provide built\-in support\. For example, you might choose to send the following to Kinesis Video Streams:
+ A diagnostic data stream \(for example, application logs and events\)
+ Data from infrared cameras, RADARs, or depth cameras

Kinesis Video Streams does not provide built\-in implementations for media\-producing devices such as cameras\. To extract data from these devices, you must implement code, thus creating your own custom media source implementation\. You can then explicitly register your custom media sources with `KinesisVideoClient`, which uploads the data to Kinesis Video Streams\.

The Kinesis Video Streams Producer Client is available for Java and Android applications\. For more information, see [Using the Java Producer Library](producer-sdk-javaapi.md) and [Using the Android Producer Library](producer-sdk-android.md)\.

## Kinesis Video Streams Producer Library<a name="producer-sdk-library"></a>

The Kinesis Video Streams Producer Library is contained within the Kinesis Video Streams Producer Client\. The library is also available to use directly for those who want a deeper integration with Kinesis Video Streams\. It enables integration from devices with proprietary operating systems, network stacks, or limited on\-device resources\.

The Kinesis Video Streams Producer Library implements the state machine for streaming to Kinesis Video Streams\. It provides callback hooks, which require that you provide your own transport implementation and explicitly handle each message going to and from the service\.

You might choose to use the Kinesis Video Streams Producer Library directly for the following reasons: 
+ The device on which you want to run the application doesn't have a Java virtual machine\.
+ You want to write application code in languages other than Java\.
+ You might have Java on the device, but you want to reduce the amount of overhead in your code and limit it to the bare minimum level of abstraction, due to limitations such as memory and processing power\. 

Currently, the Kinesis Video Streams Producer Library is available for C\+\+ applications\. For more information, see [Using the C\+\+ Producer Library](producer-sdk-cpp.md)\.

## Related Topics<a name="producer-sdk-related-topics"></a>

 [Using the Java Producer Library](producer-sdk-javaapi.md) 

 [Using the Android Producer Library](producer-sdk-android.md) 

 [Using the C\+\+ Producer Library](producer-sdk-cpp.md) 

 [](producer-sdk-c-api.md) 