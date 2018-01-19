# Using the C\+\+ Producer Library<a name="producer-sdk-cpp"></a>

Amazon Kinesis Video Streams provides the C\+\+ Producer Library, which you can use to write application code to send media data from a device to a Kinesis video stream\. 

## Object Model<a name="producer-sdk-cpp-objectmodel"></a>

The C\+\+ library provides the following objects to manage sending data to a Kinesis video stream:

+ **KinesisVideoProducer:** Contains information about your media source and AWS credentials, and maintains callbacks to report on Kinesis Video Streams events\.

+ **KinesisVideoStream:** Represents the Kinesis video stream\. Contains information about the video stream's parameters, such as name, data retention period, media content type, etc\.

## Putting Media into the Stream<a name="producer-sdk-cpp-putframe"></a>

The C\+\+ library provides methods \(for example, `PutFrame`\) that you can use to put data into the `KinesisVideoStream` object\. The library then manages the internal state of the data, which can include the following tasks: 

+ Performing authentication\.

+ Watching for network latency\. If the latency is too high, the library might choose to drop frames\.

+ Tracking status of streaming in progress\.

## Callback Interfaces<a name="producer-sdk-cpp-callbacks"></a>

This layer exposes a set of callback interfaces, which enable it to talk to the application layer\. These callback interfaces include the following:

+ Service callbacks interface \(`CallbackProvider`\): The library invokes events obtained through this interface when it creates a stream, obtains a stream description, deletes a stream, etc\.

+ Client\-ready state or low storage events interface \(`ClientCallbackProvider`\): The library invokes events on this interface when the client is ready, or when it detects that it might run out of available storage or memory\.

+ Stream events callback interface \(`StreamCallbackProvider`\): The library invokes events on this interface when stream events occur, such as the stream entering the ready state, dropped frames, or stream errors\.

Kinesis Video Streams provides default implementations for these interfaces\. You can also provide your own custom implementation—for example, if you need custom networking logic or you want to expose a low storage condition to the user interface\.

## Procedure: Using the C\+\+ Producer SDK<a name="producer-sdk-cpp-using"></a>

This procedure demonstrates how to use the Kinesis Video Streams client and media sources in a C\+\+ application to send data to your Kinesis video stream\.

**Note**  
The C\+\+ library includes a sample build script for macOS\. The C\+\+ Producer Library is not currently available for Windows\.

The procedure includes the following steps:

+ [Step 1: Download and Configure the Code](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producersdk-cpp-download.html)

+ [Step 2: Write and Examine the Code](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producersdk-cpp-write.html)

+ [Step 3: Run and Verify the Code](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producersdk-cpp-test.html)

### Prerequisites<a name="producer-sdk-cpp-prerequisites"></a>

+ **Credentials:** In the sample code, you provide credentials by specifying a profile that you set up in your AWS credentials profile file\. If you haven't already done so, first set up your credentials profile\. 

  For more information, see [Set up AWS Credentials and Region for Development](http://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html)\.

+ **Certificate store integration:** The Kinesis Video Streams Producer Library must establish trust with the service it calls\. This is done through validating the certification authorities \(CAs\) in the public certificate store\. On Linux\-based models, this store is located in the `/etc/ssl`/ directory\. 

  Download the certificate from the following location to your certificate store:

  [https://www\.amazontrust\.com/repository/SFSRootCAG2\.pem](https://www.amazontrust.com/repository/SFSRootCAG2.pem)

+ Install the following build dependencies:

  + [Autoconf 2\.69](http://www.gnu.org/software/autoconf/autoconf.html) \(License GPLv3\+/Autoconf: GNU GPL version 3 or later\) 

  + [CMake 3\.7 or 3\.8](https://cmake.org/)

  + [Bison 2\.4](https://www.gnu.org/software/bison/) \(GNU License\)

  + [Automake 1\.15\.1](https://www.gnu.org/software/automake/) \(GNU License\)

  + Libtool \(Apple Inc\. version cctools\-898\)

  + xCode \(macOS\) / clang / gcc \(xcode\-select version 2347\)

  + Java Development Kit \(JDK\) \(for Java JNI compilation\)