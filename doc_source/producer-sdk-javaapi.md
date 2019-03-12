# Using the Java Producer Library<a name="producer-sdk-javaapi"></a>

Amazon Kinesis Video Streams provides the Java Producer Library, which you can use to write application code, with minimal configuration, to send media data from a device to a Kinesis video stream\. 

You must perform the following steps to integrate your code with Kinesis Video Streams, so that your application can start streaming data to your Kinesis video stream:

1. Create an instance of the `KinesisVideoClient` object\.

1. Create a `MediaSource` object by providing media source information\. For example, when creating a camera media source, you provide information such as identifying the camera and specifying the encoding the camera uses\.

   When you want to start streaming, you must create a custom media source\. 

1. Register the media source with `KinesisVideoClient`\. 

   After you register the media source with `KinesisVideoClient`, whenever the data becomes available with the media source, it calls `KinesisVideoClient` with the data\.

## Procedure: Using the Java Producer SDK<a name="producer-sdk-java-using"></a>

This procedure demonstrates how to use the Kinesis Video Streams Java Producer Client in your Java application to send data to your Kinesis video stream\. 

These steps don't require you to have a media source, such as a camera or microphone\. Instead, for testing purposes, the code generates sample frames that consist of a series of bytes\. You can use the same coding pattern when you send media data from real sources such as cameras and microphones\. 

The procedure includes the following steps:
+ [Download and Configure the Code](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producersdk-javaapi-downloadcode.html)
+ [Write and Examine the Code](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producersdk-javaapi-writecode.html)
+ [Run and Verify the Code](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producersdk-javaapi-reviewcode.html)

### Prerequisites<a name="producersdk-javaapi-prerequisites"></a>
+ In the sample code, you provide credentials by specifying a profile that you set up in your AWS credentials profile file\. If you haven't already done so, first set up your credentials profile\. For more information, see [ Set up AWS Credentials and Region for Development](http://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html) in the *AWS SDK for Java*\.
**Note**  
The Java example uses a `SystemPropertiesCredentialsProvider` object to obtain your AWS credentials\. The provider retrieves these credentials from the `aws.accessKeyId` and `aws.secretKey` Java system properties\. You set these system properties in your Java development environment\. For information about how to set Java system properties, see the documentation for your particular integrated development environment \(IDE\)\.
+ Your `NativeLibraryPath` must contain your `KinesisVideoProducerJNI` file, available at [https://github\.com/awslabs/amazon\-kinesis\-video\-streams\-producer\-sdk\-cpp](https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp)\. The file name extension for this file depends on your operating system: 
  + **KinesisVideoProducerJNI\.so** for Linux
  + **KinesisVideoProducerJNI\.dylib** for macOS
  + **KinesisVideoProducerJNI\.dll** for Windows
**Note**  
Pre\-built libraries for macOS, Ubuntu, Windows, and Raspbian are available in `src/main/resources/lib` at [https://github\.com/awslabs/amazon\-kinesis\-video\-streams\-producer\-sdk\-java](https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-java)\. For other environments, compile the [C\+\+ Producer Library](producer-sdk-cpp.md)\.