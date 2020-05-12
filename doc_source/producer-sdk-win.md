# Using the C\+\+ Producer SDK on Windows<a name="producer-sdk-win"></a>

This tutorial demonstrates how to build and run the [Producer Libraries](producer-sdk.md) on Microsoft Windows\. You can then stream video to Kinesis Video Streams from sources such as webcams, USB cameras, or RTSP \(Real Time Streaming Protocol\) cameras\. When you start streaming from your media source to a Kinesis video stream, you can view the video in the Kinesis Video Streams console\. You can also build applications that operate on the streaming video that is available in your Kinesis video stream\.

**Topics**
+ [Building and Running the Producer SDK: MinGW](#producer-sdk-win-mingw)
+ [Building and Running the Producer SDK: MSVC](#producer-sdk-win-msvc)

## Building and Running the Producer SDK: Minimalist GNU for Windows \(MinGW\)<a name="producer-sdk-win-mingw"></a>

Minimalist GNU for Windows \(MinGW\) is an open\-source programming toolchain for developing native Windows applications\. You can use MinGW to build the Kinesis Video Streams Producer SDK for Windows and then run one of the sample applications to start streaming video\.

This section describes prerequisites and steps needed to build the Amazon Kinesis Video Streams Producer SDK using the MinGW compiler\.

### Prerequisites<a name="producer-sdk-win-mingw-prerequisites"></a>

Before you start, ensure that you have the following:
+ Download and install the [MSYS2](https://www.msys2.org/) version for your specific Windows platform\. MSYS2 provides all the tools to build native Windows applications using MinGW toolchains\.

### Building the Producer SDK Using MinGW<a name="producer-sdk-win-mingw-build"></a>

Follow these steps to use the MinGW runtime environment to compile the Kinesis Video StreamsProducer SDK on Windows\.

1. Launch the MinGW shell \(`mingw64.exe`\) from the `C:\msys32` or `C:\msys64` directory\. Make sure that you are opening the *mingw64\.exe* or *mingw32\.exe* based on your platform, and not the MSYS2 application\. The MSYS2 application is the default application that is opened after you finish installing MSYS2\.

1. Install Git by running the following command in the MinGW shell:

   ```
   pacman -S git
   ```

1. Download the Kinesis Video Streams Producer SDK from GitHub:

   ```
   git clone --recursive https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp.git
   ```

1. Run the following commands to create a `build` directory in your [downloaded SDK](https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp), and execute `cmake` from it:

   ```
   mkdir -p amazon-kinesis-video-streams-producer-c/build; 
   cd amazon-kinesis-video-streams-producer-c/build; 
   cmake ..
   ```

   You can pass the following options to `cmake ..`
   + `-DBUILD_DEPENDENCIES` \- whether or not to build depending libraries from source
   + `-DBUILD_TEST=TRUE` \- build unit/integration tests, may be useful for confirm support for your device\. 

     `./tst/webrtc_client_test`
   + `-DCODE_COVERAGE` \-enable coverage reporting
   + `-DCOMPILER_WARNINGS` \- enable all compiler warnings
   + `-DADDRESS_SANITIZER` \- build with AddressSanitizer
   + `-DMEMORY_SANITIZER` \- build with MemorySanitizer
   + `-DTHREAD_SANITIZER` \- build with ThreadSanitizer
   + `-DUNDEFINED_BEHAVIOR_SANITIZER` \- build with UndefinedBehaviorSanitizer
   + `-DALIGNED_MEMORY_MODEL` \- build for aligned memory model only devices\. Default is `OFF`\.

1. Navigate to the `build` directory you just created with the step above, and run `nmake` to build the SDK and its provided samples\. 

   ```
   nmake
   ```

### Running the Producer SDK to Send Video to Kinesis Video Streams<a name="producer-sdk-win-mingw-run"></a>

After compiling the Kinesis Video Streams Producer SDK using MinGW, follow these steps to run it:

#### Step 1: Set Environment Variables<a name="producer-sdk-win-mingw-run-1"></a>
+ In the MinGW shell, set the following environment variables:

  ```
  export AWS_ACCESS_KEY_ID=YOUR_ACCESS_KEY
  export AWS_SECRET_ACCESS_KEY=YOUR_SECRET_ACCESS_KEY
  export GST_PLUGIN_PATH=$PWD
  ```
**Note**  
*YOUR\_ACCESS\_KEY* and *YOUR\_SECRET\_ACCESS\_KEY* are the access keys for your AWS account used for signing programmatic requests that you make to AWS\. For more information, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)\. 

#### Step 2: Run the Sample Application for Your Media Source<a name="producer-sdk-win-mingw-run-2"></a>

1. To stream video from your PC webcam, run the sample application from the `samples` folder using the following command:

   ```
   kinesis_video_gstreamer_sample_app.exe my-stream-name
   ```

1. To stream video from your PC webcam using a custom configuration, such as a specific bitrate or resolution, run the Kinesis Video Streams Producer SDK GStreamer plugin using the `gst-launch-1.0` command:

   ```
   gst-launch-1.0 ksvideosrc do-timestamp=TRUE ! video/x-raw,width=640,height=480,framerate=30/1 ! videoconvert ! x264enc bframes=0 key-int-max=45 bitrate=512 ! video/x-h264,profile=baseline,stream-format=avc,alignment=au,width=640,height=480,framerate=30/1 ! kvssink stream-name="your-stream-name" access-key=your_accesskey_id secret-key=your_secret_access_key
   ```

   For information about how to determine the parameters for the `gst-launch-1.0` command, see [GStreamer Element Parameter Reference](examples-gstreamer-plugin-parameters.md)\.
**Note**  
If you are using IoT credentials instead of your access key and secret key to authenticate, you can supply IoT credentials as parameters to the `gst-launch-1.0` command\.   
The following example demonstrates using IoT parameters to stream video from an RTSP camera:  

   ```
   gst-launch-1.0 rtspsrc location=rtsp://YourCameraRtspUrl short-header=TRUE ! rtph264depay ! video/x-h264, format=avc,alignment=au ! kvssink stream-name="your-iot-stream" iot-certificate="iot-certificate,endpoint=endpoint,cert-path=/path/to/certificate,key-path=/path/to/private/key,ca-path=/path/to/ca-cert,role-aliases=role-aliases"
   ```

1. To stream video from an RTSP \(network\) camera, run the sample application from the `samples` folder using the following command:

   ```
   kinesis_video_gstreamer_sample_rtsp_app.exe RTSP-camera-URL my-test-rtsp-stream
   ```

## Building and Running the Producer SDK: Microsoft Visual C\+\+ Compiler \(MSVC\)<a name="producer-sdk-win-msvc"></a>

The Microsoft Visual C\+\+ Compiler \(MSVC\) is the compiler for Microsoft Visual Studio\. The following sections include the prerequisites and steps that are required to build the Kinesis Video Streams Producer SDK using MSVC\.

### Prerequisites<a name="producer-sdk-win-msvc-prerequisites"></a>

Before you start, ensure that you have the following:
+ Microsoft Windows version 7 or later\.
+ [Microsoft \.NET Framework](https://www.microsoft.com/net/download/dotnet-framework-runtime) version 4\.6\.1 or later\.
+ Windows PowerShell version 5\.1 \(included in Windows 10\)\. On Windows 7, [update Windows PowerShell](https://www.microsoft.com/en-us/download/details.aspx?id=54616)\.
+ [Git](https://git-scm.com/download/win)\. In the **Adjusting your PATH environment** installation step, choose **Use Git from the Windows Command Prompt**\.

### Building the Producer SDK Using MSVC<a name="producer-sdk-win-msvc-build"></a>

Follow these steps to use MSVC to compile the Producer SDK on Windows\.

**Note**  
If you previously installed the Producer SDK for Windows using MinGW, do the following cleanup steps before building the SDK using MSVC:   
Delete the files in the `kinesis-video-native-build/downloads` directory\.
Remove the `CMakeFiles` directory and the `CMakeCachedList.txt` file in the `kinesis-video-native-build` directory\.

1. Open a Windows command prompt as an administrator\.

1. Download the Producer SDK:

   ```
   git clone https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp.git
   ```

1. After the download is complete, change to the `kinesis-video-native-build` directory within the downloaded project\.

1. Run the Visual Studio build tools install script:

   ```
   vs-buildtools-install.bat
   ```

1. After the install script completes, if you are using Windows 10 or Windows 7, reboot your computer\. Then re\-open a Windows command prompt as an administrator\.

1. In the `kinesis-video-native-build` directory, run `windows-install.bat`, specifying your system's bit width \(32 or 64\): 

   ```
   windows-install.bat 32
   or
   windows-install.bat 64
   ```
**Note**  
This script builds the following components:  
The [C\+\+ Producer Library](producer-sdk-cpp.md) libraries\.
The C\+\+ Producer SDK [GStreamer](examples-gstreamer-plugin.md) \(`kvssink`\)\.
The [RTSP and Docker](examples-rtsp.md) demo, which shows how to stream data from an RTSP \(network\) camera\.

#### Running the Producer SDK to Send Video to Kinesis Video Streams<a name="producer-sdk-win-mingw-run"></a>

After compiling the Kinesis Video Streams Producer SDK using MSVC, follow these steps to run it as a GStreamer plugin\.

You have several options for starting the SDK\. We recommend that you use the [GStreamer](examples-gstreamer-plugin.md), which you can run using the example executables available in the `kinesis-video-native-build/start` directory\.

1. Add the following directories to your path \(specify the location for the Producer SDK, including the drive\):

   ```
   set PATH=%PATH%;install directory\amazon-kinesis-video-streams-producer-sdk-cpp\kinesis-video-native-build\downloads\gstreamer\1.0\x86_64\bin;
   ```

1. Set the following environment variables \(replace *install directory* with the location for the Producer SDK, including the drive\):

   ```
   set GST_PLUGIN_PATH=install directory\amazon-kinesis-video-streams-producer-sdk-cpp\kinesis-video-native-build\Release
   set GST_PLUGIN_SYSTEM_PATH=install directory\amazon-kinesis-video-streams-producer-sdk-cpp\kinesis-video-native-build\downloads\gstreamer\1.0\x86_64\lib\gstreamer-1.0
   ```

1. Stream video from the webcam on the PC to the Kinesis Video Streams service using the `gst-launch-1.0` command:

   ```
   gst-launch-1.0 ksvideosrc do-timestamp=TRUE ! video/x-raw,width=640,height=480,framerate=30/1 ! videoconvert ! x264enc bframes=0 key-int-max=45 bitrate=512 ! video/x-h264,profile=baseline,stream-format=avc,alignment=au,width=640,height=480,framerate=30/1 ! kvssink stream-name="your-stream-name" access-key=your_accesskey_id secret-key=your_secret_access_key
   ```

   For information about how to determine the parameters for the `gst-launch-1.0` command, see [GStreamer Element Parameter Reference](examples-gstreamer-plugin-parameters.md)\.
**Note**  
If you are using IoT credentials instead of your access key and secret key, you can supply them as parameters to the `gst-launch-1.0` command\. The following example demonstrates using IoT parameters to stream video from an RTSP camera:  

   ```
   gst-launch-1.0 rtspsrc location=rtsp://YourCameraRtspUrl short-header=TRUE ! rtph264depay ! video/x-h264, format=avc,alignment=au ! kvssink stream-name="your-iot-stream" iot-certificate="iot-certificate,endpoint=endpoint,cert-path=/path/to/certificate,key-path=/path/to/private/key,ca-path=/path/to/ca-cert,role-aliases=role-aliases"
   ```

1. Alternatively, you can set the following environment variables and use one of our pre\-build sample applications\.

   ```
   export AWS_ACCESS_KEY_ID=YOUR_ACCESS_KEY
   export AWS_SECRET_ACCESS_KEY=YOUR_SECRET_ACCESS_KEY
   ```

1. To stream video from a PC webcam, run the sample application from the `kinesis-video-native-build\Release` directory using the following command:

   ```
   kinesis_video_gstreamer_sample_app.exe my-stream-name
   ```

1. To stream video from an RTSP \(network\) camera, run the sample application from the `kinesis-video-native-build\Release` directory using the following command:

   ```
   kinesis_video_gstreamer_sample_rtsp_app.exe RTSP-camera-URL my-test-rtsp-stream
   ```