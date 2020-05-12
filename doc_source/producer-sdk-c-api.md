# Using the C Producer Library<a name="producer-sdk-c-api"></a>

Amazon Kinesis Video Streams provides the C Producer Library, which you can use to write application code to send media data from a device to a Kinesis video stream\. 

## Object Model<a name="producer-sdk-c-objectmodel"></a>

The Kinesis Video Streams C producer library is based on a common component called Platform Independent Codebase \(PIC\), which is available on GitHub at [https://github\.com/awslabs/amazon\-kinesis\-video\-streams\-pic/](https://github.com/awslabs/amazon-kinesis-video-streams-pic/)\. The PIC contains platform\-independent business logic for the low\-levels\. The Kinesis Video Streams C producer library wraps PIC with additional layer of API that allows scenario\- and platform\-specific callbacks and events\. The Kinesis Video Streams C producer library has the following components built on top of PIC:
+ **Device info providers** – Exposes the `DeviceInfo` structure that can be directly supplied to the PIC API\. There is a set of easy\-to\-configure providers, including application scenario\-optimized provider that can optimize the content store based on the number and types of streams that your application handles and the amount of required buffering configured based on the amount of available RAM\.
+ **Stream info provider** – Exposes the `StreamInfo` structure that can be directly supplied to the PIC API\. There is a set of easy\-to\-configure providers which are specific to the application types and the common types of streaming scenarios\. These include providers such as video, audio, audio/video multitrack, etc\. Each of these scenarios has defaults that you can customize according to your application's requirements\. 
+ **Callback provider** – Exposes the `ClientCallbacks` structure that can be directly supplied to the PIC API\. This includes a set of easy\-to\-configure callback providers for networking \(CURL\-based API callbacks\), authorization \(AWS credentials API\), retry streaming on errors callbacks, etc\. The Callback Provider API takes a number of arguments to configure, such as the AWS Region and authorization information \(via IoT certificates or through AWS AccessKeyId, SecretKey, SessionToken\)\. You can enhance Callback Provider with custom callbacks if your application needs further processing of a particular callback to achieve some application\-specific logic\.
+ **FrameOrderCoordinator** – Helps handle audio and video synchronization for multi\-track scenarios\. It has default behavior which you can customize to handle your application's specific logic\. It also simplifies the frame metadata packaging in PIC Frame structure before submitting it to the lower\-layer PIC API\. For non\-multitrack scenarios, this component is a pass\-through to PIC putFrame API\.

The C library provides the following objects to manage sending data to a Kinesis video stream:
+ **KinesisVideoClient** – Contains information about your device information and maintains callbacks to report on Kinesis Video Streams events\.
+ **KinesisVideoStream** – Represents information about the video stream's parameters, such as name, data retention period, media content type, and so on\.

## Putting Media Into the Stream<a name="producer-sdk-c-putframe"></a>

The C library provides methods \(for example, `PutKinesisVideoFrame`\) that you can use to put data into the `KinesisVideoStream` object\. The library then manages the internal state of the data, which can include the following tasks: 
+ Performing authentication\.
+ Watching for network latency\. If the latency is too high, the library might choose to drop frames\.
+ Tracking status of streaming in progress\.

## Procedure: Using the C Producer SDK<a name="producer-sdk-c-using"></a>

This procedure demonstrates how to use the Kinesis Video Streams client and media sources in a C application to send H\.264\-encoded video frames to your Kinesis video stream\.

The procedure includes the following steps:
+ [Step 1: Download the C Producer Library Code](producersdk-c-download.md)
+ [Step 2: Write and Examine the Code](producersdk-c-write.md)
+ [Step 3: Run and Verify the Code](producersdk-c-test.md)

### Prerequisites<a name="producer-sdk-cpp-prerequisites"></a>
+ **Credentials** – In the sample code, you provide credentials by specifying a profile that you set up in your AWS credentials profile file\. If you haven't already done so, first set up your credentials profile\. 

  For more information, see [Set up AWS Credentials and Region for Development](http://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html)\.
+ **Certificate store integration** – The Kinesis Video Streams Producer Library must establish trust with the service it calls\. This is done through validating the certification authorities \(CAs\) in the public certificate store\. On Linux\-based models, this store is located in the `/etc/ssl`/ directory\. 

  Download the certificate from the following location to your certificate store:

  [https://www\.amazontrust\.com/repository/SFSRootCAG2\.pem](https://www.amazontrust.com/repository/SFSRootCAG2.pem)
+ Install the following build dependencies for macOS:
  + [Autoconf 2\.69](http://www.gnu.org/software/autoconf/autoconf.html) \(License GPLv3\+/Autoconf: GNU GPL version 3 or later\) 
  + [CMake 3\.7 or 3\.8](https://cmake.org/)
  + [Pkg\-Config](https://www.freedesktop.org/wiki/Software/pkg-config/)
  + [Flex 2\.5\.35 Apple \(flex\-31\) or later](https://github.com/westes/flex/releases)
  + [Bison 2\.4](https://www.gnu.org/software/bison/) \(GNU License\)
  + [Automake 1\.15\.1](https://www.gnu.org/software/automake/) \(GNU License\)
  + GNU Libtool \(Apple Inc\. version cctools\-898\)
  + xCode \(macOS\) / clang / gcc \(xcode\-select version 2347\)
  + Java Development Kit \(JDK\) \(for Java JNI compilation\)
  + [Lib\-Pkg](https://github.com/freebsd/pkg/tree/master/libpkg)
+ Install the following build dependencies for Ubuntu \(responses to version commands are truncated\):
  + Install Git: `sudo apt-get install git`

    ```
    $ git --version
    git version 2.14.1
    ```
  + Install [CMake](http://kitware.com/cmake): `sudo apt-get install cmake`

    ```
    $ cmake --version
    cmake version 3.9.1
    ```
  + Install Libtool: `sudo apt-get install libtool`

    ```
    2.4.6-2
    ```
  + Install libtool\-bin: `sudo apt-get install libtool-bin`

    ```
    $ libtool --version
    libtool (GNU libtool) 2.4.6
    Written by Gordon Matzigkeit, 1996
    ```
  + Install GNU Automake: `sudo apt-get install automake`

    ```
    $ automake --version
    automake (GNU automake) 1.15
    ```
  + Install GNU Bison: `sudo apt-get install bison`

    ```
    $ bison -V
    bison (GNU Bison) 3.0.4
    ```
  + Install G\+\+: `sudo apt-get install g++`

    ```
    g++ --version
    g++ (Ubuntu 7.2.0-8ubuntu3) 7.2.0
    ```
  + Install curl: `sudo apt-get install curl`

    ```
    $ curl --version
    curl 7.55.1 (x86_64-pc-linux-gnu) libcurl/7.55.1 OpenSSL/1.0.2g zlib/1.2.11 libidn2/2.0.2 libpsl/0.18.0 (+libidn2/2.0.2) librtmp/2.3
    ```
  + Install pkg\-config: `sudo apt-get install pkg-config`

    ```
    $ pkg-config --version
    0.29.1
    ```
  + Install Flex: `sudo apt-get install flex`

    ```
    $ flex --version
    flex 2.6.1
    ```
  + Install OpenJDK: `sudo apt-get install openjdk-8-jdk`

    ```
    $ java -version
    openjdk version "1.8.0_171"
    ```
  + Set the `JAVA_HOME` environment variable: `export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/`
  + Run the build script: `./install-script`

### Next Step<a name="producer-sdk-c-prerequisites-next-step"></a>

[Step 1: Download the C Producer Library Code](producersdk-c-download.md)