# Amazon Kinesis Video Streams: How It Works<a name="how-it-works"></a>

**Topics**
+ [Kinesis Video Streams API and Producer Libraries Support](how-it-works-kinesis-video-api-producer-sdk.md)
+ [Kinesis Video Streams Playback](how-playback.md)
+ [Using Streaming Metadata with Kinesis Video Streams](how-meta.md)
+ [Kinesis Video Streams Data Model](how-data.md)

Amazon Kinesis Video Streams is a fully managed AWS service that enables you to stream live video from devices to the AWS Cloud and durably store it\. You can then build your own applications for real\-time video processing or perform batch\-oriented video analytics\.

The following diagram provides an overview of how Kinesis Video Streams works\.

![\[Diagram showing interaction of producers and consumers in Kinesis Video Streams.\]](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/images/acuity-arch-3a.png)

The diagram demonstrates the interaction among the following components:
+ **Producer** – Any source that puts data into a Kinesis video stream\. A producer can be any video\-generating device, such as a security camera, a body\-worn camera, a smartphone camera, or a dashboard camera\. A producer can also send non\-video data, such as audio feeds, images, or RADAR data\.

  A single producer can generate one or more video streams\. For example, a video camera can push video data to one Kinesis video stream and audio data to another\.
  + **Kinesis Video Streams Producer libraries** – A set of easy\-to\-use software and libraries that you can install and configure on your devices\. These libraries make it easy to securely connect and reliably stream video in different ways, including in real time, after buffering it for a few seconds, or as after\-the\-fact media uploads\.
+ **Kinesis video stream** – A resource that enables you to transport live video data, optionally store it, and make the data available for consumption both in real time and on a batch or ad hoc basis\. In a typical configuration, a Kinesis video stream has only one producer publishing data into it\. 

  The stream can carry audio, video, and similar time\-encoded data streams, such as depth sensing feeds, RADAR feeds, and more\. You create a Kinesis video stream using the AWS Management Console or programmatically using the AWS SDKs\.

  Multiple independent applications can consume a Kinesis video stream in parallel\. 
+ **Consumer** – Gets data, such as fragments and frames, from a Kinesis video stream to view, process, or analyze it\. Generally these consumers are called Kinesis Video Streams applications\. You can write applications that consume and process data in Kinesis video streams in real time, or after the data is durably stored and time\-indexed when low latency processing is not required\. You can create these consumer applications to run on Amazon EC2 instances\.
  + [Kinesis Video Stream Parser Library](parser-library.md) – Enables Kinesis Video Streams applications to reliably get media from Kinesis video streams in a low\-latency manner\. Additionally, it parses the frame boundaries in the media so that applications can focus on processing and analyzing the frames themselves\.