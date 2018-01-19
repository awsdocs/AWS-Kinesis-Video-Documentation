# GStreamer Example<a name="examples-gstreamer"></a>

The [C\+\+ Producer Library](producer-sdk-cpp.md) contains a demo application for using [GStreamer](https://gstreamer.freedesktop.org/), an open source multimedia framework, with Amazon Kinesis Video Streams\. 

The following procedure demonstrates how to set up and use the GStreamer demo\. The application sends the video data to Kinesis Video Streams on the following platforms:

+ A built\-in camera on a macOS computer

+ A USB camera on a Linux or Raspberry Pi device

**Note**  
The GStreamer example is not currently available for Windows systems\.

## Prerequisites<a name="examples-gstreamer-prerequisites"></a>

The GStreamer example typically requires the following components, which you can install using the following commands:

+ [CMake:](https://cmake.org/) `brew install cmake`

+ [GStreamer:](https://gstreamer.freedesktop.org/) `brew install gstreamer gst-plugins-base gst-plugins-bad gst-plugins-good gst-plugins-ugly`

+ [GNU Bison:](https://www.gnu.org/software/bison/) `brew install bison && brew link bison --force`

For a more comprehensive list of requirements, see [Prerequisites](producer-sdk-cpp.md#producer-sdk-cpp-prerequisites) in the C\+\+ Producer SDK documentation, or the `README.md` file in the SDK itself\.

## Running the GStreamer Example<a name="examples-gstreamer-procedure"></a>

1. Follow the directions in [Step 1: Download and Configure the C\+\+ Producer Library Code](producersdk-cpp-download.md) to download the C\+\+ Producer SDK\.

1. Run the following script in the `/kinesis-video-native-build` folder to build the C\+\+ Producer SDK:

   ```
   ./install-script
   ```

   This script installs the prerequisites for the SDK and builds the binaries, including the GStreamer example\. For information about building the example on Ubuntu, see the "Troubleshooting" section of the `README.md` file in the SDK\.

1. After the C\+\+ Procedure SDK is installed and configured, run the application using the following command \(also in the `/kinesis-video-native-build` folder\): 

   ```
   EXPORT AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE 
   EXPORT AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY 
   EXPORT AWS_DEFAULT_REGION=<AWS region>
   ./kinesis_video_gstreamer_sample_app stream_name
   ```

   Supply the following information for the application parameters:

   + **AWS\_ACCESS\_KEY\_ID:** The access key ID for your account\. See [Prerequisites](examples.md#examples-prerequisites)\.

   + **AWS\_SECRET\_ACCESS\_KEY:** The secret access key for your account\. See [Prerequisites](examples.md#examples-prerequisites)\.

   + **AWS\_DEFAULT\_REGION:** Your AWS Region, for example, `us-west-2`\.

   + **stream\_name:** The name of the Kinesis video stream to send camera data to\. The stream will be created if it doesn't exist\.

1. The demo application starts\. In a few seconds, you will see video data from your camera in the Kinesis Video Streams console for your stream\.
**Note**  
If the application fails to acquire your camera, you might see a `Failed to negotiate pipeline` error\. For troubleshooting information, see the `README.md` file in the SDK\.