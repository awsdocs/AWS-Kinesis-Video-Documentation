# Using the C\+\+ Producer SDK on Raspberry Pi<a name="producersdk-cpp-rpi"></a>

The Raspberry Pi is a small, inexpensive computer that can be used to teach and learn basic computer programming skills\. This tutorial describes how you can set up and use the Amazon Kinesis Video Streams C\+\+ Producer SDK on a Raspberry Pi device\. The steps also include how to verify the installation using the GStreamer demo application\.

**Topics**
+ [Prerequisites](#producersdk-cpp-rpi-prerequisites)
+ [Create an IAM User with Permission to Write to Kinesis Video Streams](#producersdk-cpp-rpi-iam)
+ [Join Your Raspberry Pi to Your Wi\-Fi Network](#producersdk-cpp-rpi-wifi)
+ [Connect Remotely to Your Raspberry Pi](#producersdk-cpp-rpi-connect)
+ [Configure the Raspberry Pi Camera](#producersdk-cpp-rpi-camera)
+ [Install Software Prerequisites](#producersdk-cpp-rpi-software)
+ [Download and Build the Kinesis Video Streams C\+\+ Producer SDK](#producersdk-cpp-rpi-download)
+ [Stream Video to Your Kinesis Video Stream and View the Live Stream](#producersdk-cpp-rpi-run)

## Prerequisites<a name="producersdk-cpp-rpi-prerequisites"></a>

Before you set up the C\+\+ Producer SDK on your Raspberry Pi, ensure that you have the following prerequisites: 
+ A Raspberry Pi device with the following configuration:
  + Board version: 3 Model B or later\.
  + A connected camera module\.
  + An SD card with a capacity of at least 8 GB\.
  + The Raspbian operating system \(kernel version 4\.9 or later\) installed\. You can download the latest Raspbian image from the [Raspberry Pi Foundation website](https://www.raspberrypi.org/downloads/raspbian/)\. Follow the Raspberry Pi instructions to [install the downloaded image on an SD card](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)\. 
+ An AWS account with a Kinesis video stream\. For more information, see [Getting Started with Kinesis Video Streams](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/getting-started.html)\.
**Note**  
The C\+\+ Producer SDK uses the US West \(Oregon\) \(`us-west-2`\) Region by default\. To use the default AWS Region, create your Kinesis video stream in the US West \(Oregon\) Region\.   
To use a different Region for your Kinesis video stream, do one of the following:  
Set the following environment variable to your Region \(for example, *us\-east\-1*\):  

    ```
    export AWS_DEFAULT_REGION=us-east-1 
    ```

## Create an IAM User with Permission to Write to Kinesis Video Streams<a name="producersdk-cpp-rpi-iam"></a>

If you haven't already done so, set up an AWS Identity and Access Management \(IAM\) user with permissions to write to a Kinesis video stream\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation menu on the left, choose **Users**\.

1. To create a new user, choose **Add user**\.

1. Provide a descriptive **User name** for the user, such as **kinesis\-video\-raspberry\-pi\-producer**\.

1. Under **Access type**, choose **Programmatic access**\.

1. Choose **Next: Permissions**\.

1. Under **Set permissions for kinesis\-video\-raspberry\-pi\-producer**, choose **Attach existing policies directly**\.

1. Choose **Create policy**\. The **Create policy** page opens in a new web browser tab\.

1. Choose the **JSON** tab\.

1. Copy the following JSON policy and paste it into the text area\. This policy gives your user permission to create and write data to Kinesis video streams\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [{
       "Effect": "Allow",
         "Action": [
         "kinesisvideo:DescribeStream",
         "kinesisvideo:CreateStream",
         "kinesisvideo:GetDataEndpoint",
         "kinesisvideo:PutMedia"
       ],
       "Resource": [
         "*"
       ]
     }]
   }
   ```

1. Choose **Review policy**\.

1. Provide a **Name** for your policy, such as **kinesis\-video\-stream\-write\-policy**\.

1. Choose **Create policy**\.

1. Return to the **Add user** tab in your browser, and choose **Refresh**\.

1. In the search box, type the name of the policy you created\.

1. Select the check box next to your new policy in the list\.

1. Choose **Next: Review**\.

1. Choose **Create user**\.

1. The console displays the **Access key ID** for your new user\. Choose **Show** to display the **Secret access key**\. Record these values; they are required when you configure the application\.

## Join Your Raspberry Pi to Your Wi\-Fi Network<a name="producersdk-cpp-rpi-wifi"></a>

You can use the Raspberry Pi in *headless* mode, that is, without an attached keyboard, monitor, or network cable\. If you are using an attached monitor and keyboard, proceed to [Configure the Raspberry Pi Camera](#producersdk-cpp-rpi-camera)\. 

1. On your computer, create a file named `wpa_supplicant.conf`\.

1. Copy the following text and paste it into the `wpa_supplicant.conf` file: 

   ```
   country=US
   ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
   update_config=1
   
   network={
   ssid="<YOUR_WIFI_SSID>"
   scan_ssid=1
   key_mgmt=WPA-PSK
   psk="<YOUR_WIFI_PASSWORD>"
   }
   ```

   Replace the `ssid` and `psk` values with the information for your Wi\-Fi network\.

1. Copy the `wpa_supplicant.conf` file to the SD card\. It must be copied to the root of the `boot` volume\.

1. Insert the SD card into the Raspberry Pi, and power the device\. It joins your Wi\-Fi network, and SSH is enabled\.

## Connect Remotely to Your Raspberry Pi<a name="producersdk-cpp-rpi-connect"></a>

You can connect remotely to your Raspberry Pi in headless mode\. If you are using your Raspberry Pi with a connected monitor and keyboard, proceed to [Configure the Raspberry Pi Camera](#producersdk-cpp-rpi-camera)\. 

1. Before connecting to your Raspberry Pi device remotely, do one of the following to determine its IP address:
   + If you have access to your network's Wi\-Fi router, look at the connected Wi\-Fi devices\. Find the device named `Raspberry Pi` to find your device's IP address\.
   + If you don't have access to your network's Wi\-Fi router, you can use other software to find devices on your network\. [Fing](https://www.fing.io/) is a popular application that is available for both Android and iOS devices\. You can use the free version of this application to find the IP addresses of devices on your network\.

1. When you know the IP address of the Raspberry Pi device, you can use any terminal application to connect\.
   + On macOS or Linux, use `ssh`:

     ```
     $   ssh pi@<IP address>
     ```
   + On Windows, use [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), a free SSH client for Windows\.

   For a new installation of Raspbian, the user name is **pi**, and the password is **raspberry**\. We recommend that you [change the default password](https://www.raspberrypi.org/documentation/linux/usage/users.md)\.

## Configure the Raspberry Pi Camera<a name="producersdk-cpp-rpi-camera"></a>

Follow these steps to configure the Raspberry Pi camera to send video from the device to a Kinesis video stream\. 

1. Open an editor to update the `modules` file with the following command:

   ```
   $   sudo nano /etc/modules
   ```

1. Add the following line to the end of the file, if it's not already there:

   ```
   bcm2835-v4l2
   ```

1. Save the file and exit the editor \(Ctrl\-X\)\.

1. Reboot the Raspberry Pi:

   ```
   $   sudo reboot
   ```

1. When the device reboots, connect to it again through your terminal application if you are connecting remotely\.

1. Open `raspi-config`:

   ```
   $   sudo raspi-config
   ```

1. Choose **Interfacing Options**, **Camera**\. Enable the camera if it's not already enabled, and reboot if prompted\.

1. Verify that the camera is working by typing the following command:

   ```
   $   raspistill -v -o test.jpg
   ```

   The display shows a five\-second preview from the camera, takes a picture \(saved to `test.jpg`\), and displays informational messages\.

## Install Software Prerequisites<a name="producersdk-cpp-rpi-software"></a>

The C\+\+ Producer SDK requires that you install the following software prerequisites on Raspberry Pi\.

1. Install Git:

   ```
   $   sudo apt-get update
   $   sudo apt-get install git
   ```

1. Install Yacc, Lex, and OpenJDK \(Open Java Development Kit\):

   ```
   $   sudo apt-get install byacc flex
   $   sudo apt-get install openjdk-8-jdk
   ```

1. Set the `JAVA_HOME` environment variable:

   ```
   $   export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-armhf/
   ```
**Note**  
If you reboot the device before building the SDK, you must repeat this step\. You can also set this environment variable in your \~/\.profile file\.

1. CMake is used to build the SDK\. Install CMake with the following command:

   ```
   $   sudo apt-get install cmake
   ```

1. Copy the following PEM file to `/etc/ssl/cert.pem`:

   [https://www\.amazontrust\.com/repository/SFSRootCAG2\.pem](https://www.amazontrust.com/repository/SFSRootCAG2.pem)

## Download and Build the Kinesis Video Streams C\+\+ Producer SDK<a name="producersdk-cpp-rpi-download"></a>

You can also download and build the Kinesis Video Streams C\+\+ Producer SDK using the following procedure\. This approach takes longer time to build, depending on network connectivity and processor speed\. 

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

1. Navigate to the `build` directory you just created with the step above, and run `make` to build the SDK and its provided samples\. 

   ```
   make
   ```

1. Make the install script executable:

   ```
   $   chmod +x install-script
   ```

1. Run the install script\. The script downloads the source and builds several open\-source projects\. It might take several hours to run the first time it is executed:

   ```
   ./install-script
   ```

1. Type **Y** to verify\. Then the build script runs\.

## Stream Video to Your Kinesis Video Stream and View the Live Stream<a name="producersdk-cpp-rpi-run"></a>

1. To run the sample application, you need the following information:
   + The name of the stream you created in the [Prerequisites](#producersdk-cpp-rpi-prerequisites) section\.
   + The account credentials \(access key ID and secret access key\) that you created in [Create an IAM User with Permission to Write to Kinesis Video Streams](#producersdk-cpp-rpi-iam)\.

1. Run the sample application using the following command:

   ```
   $  export AWS_ACCESS_KEY_ID=<Access Key ID> 
   export AWS_SECRET_ACCESS_KEY=<Secret Access Key> 
   ./kinesis_video_gstreamer_sample_app Stream Name
   ```

1. You can specify the image size, framerate, and bitrate as follows:

   ```
   $  export AWS_ACCESS_KEY_ID=<Access Key ID> 
   export AWS_SECRET_ACCESS_KEY=<Secret Access Key> 
   ./kinesis_video_gstreamer_sample_app -w <width> -h <height> -f <framerate> 
                       -b <bitrateInKBPS> Stream Name
   ```

1. If the sample application exits with a `library not found` error, type the following commands to verify that the project is correctly linked to its open\-source dependencies:

   ```
   $   rm -rf ./build/CMakeCache.txt ./build/CMakeFiles
   $   ./build/install-script
   ```

1. Open the Kinesis Video Streams console at [https://console\.aws\.amazon\.com/kinesisvideo/](https://console.aws.amazon.com/kinesisvideo/)\.

1. Choose the **Stream name** of the stream you created\.

The video stream that is sent from the Raspberry Pi appears in the console\. 

When the stream is playing, you can experiment with the following features of the Kinesis Video Streams console:
+ In the **Video preview** section, use the navigation controls to rewind or fast\-forward the stream\.
+ In the **Stream info** section, notice the codec, resolution, and bitrate of the stream\. The resolution and bitrate values are set purposefully low on the Raspberry Pi to minimize bandwidth usage for this tutorial\. To view the Amazon CloudWatch metrics that are being created for your stream, choose **View stream metrics in CloudWatch**\.
+ Under **Data retention period**, notice that the video stream is retained for one day\. You can edit this value and set it to **No data retention**, or set a value from one day to several years\.

  Under server\-side encryption, notice that your data is being encrypted at rest using a key maintained by the AWS Key Management Service \(AWS KMS\)\.