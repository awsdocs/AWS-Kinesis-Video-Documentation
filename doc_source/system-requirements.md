# Kinesis Video Streams System Requirements<a name="system-requirements"></a>

The following sections contain hardware, software, and storage requirements for Amazon Kinesis Video Streams\.

**Topics**
+ [Camera Requirements](#system-requirements-cameras)
+ [SDK Storage Requirements](#system-requirements-sdk)

## Camera Requirements<a name="system-requirements-cameras"></a>

Cameras that are used for running the Kinesis Video Streams Producer SDK and samples have the following memory requirements:
+ The SDK content view requires 16 MB of memory\.
+ The sample application default configuration is 512 MB\. This value is appropriate for producers that have good network connectivity and no requirements for additional buffering\. If the network connectivity is poor and more buffering is required, you can calculate the memory requirement per second of buffering by multiplying the frame rate per second by the frame memory size\. For more information about allocating memory, see [StorageInfo](producer-reference-structures-producer.md#producer-reference-structures-producer-storageinfo)\.

We recommend using USB or RTSP \(Real Time Streaming Protocol\) cameras that encode data using H\.264 because this removes the encoding workload from the CPU\.

Currently, the demo application does not support the User Datagram Protocol \(UDP\) for RTSP streaming\. This capability will be added in the future\.

The Producer SDK supports the following types of cameras:
+ Web cameras\.
+ USB cameras\.
+ Cameras with H\.264 encoding \(preferred\)\.
+ Cameras without H\.264 encoding\.
+ Raspberry Pi camera module\. This is preferred for Raspberry Pi devices because it connects to the GPU for video data transfer, so there is no overhead for CPU processing\.
+ RTSP \(network\) cameras\. These cameras are preferred because the video streams are already encoded with H\.264\.

### Tested Cameras<a name="system-requirements-cameras-tested"></a>

We have tested the following USB cameras with Kinesis Video Streams:
+ Logitech 1080p
+ Logitech C930
+ Logitech C920 \(H\.264\) 
+ Logitech Brio \(4K\) 
+ SVPRO USB Camera 170degree Fisheye Lens Wide Angle 1080P 2mp Sony IMX322 HD H\.264 30fps Mini Aluminum USB Webcam Camera

We have tested the following IP cameras with Kinesis Video Streams:
+ Vivotek FD9371 – HTV/EHTV
+ Vivotek IB9371 – HT
+ Hikvision 3MP IP Camera DS\-2CD2035FWD\-I
+ Sricam SP012 IP 
+ VStarcam 720P WiFi IP Camera \(TCP\)
+ Ipccam Security Surveillance IP Camera 1080P
+ AXIS P3354 Fixed Dome Network Camera 

Of the cameras that were tested with Kinesis Video Streams, the Vivotek cameras have the most consistent RTSP stream\. The Sricam camera has the least consistent RTSP stream\.

### Tested Operating Systems<a name="system-requirements-cameras-os"></a>

We have tested web cameras and RTSP cameras with the following devices and operating systems:
+ Mac mini
  + High Sierra
+ MacBook Pro laptops
  + Sierra \(10\.12\)
  + El Capitan \(10\.11\)
+ HP laptops running Ubuntu 16\.04
+ Ubuntu 17\.10 \(Docker container\)
+ Raspberry Pi 3

## SDK Storage Requirements<a name="system-requirements-sdk"></a>

Installing the [Kinesis Video Streams Producer Libraries](producer-sdk.md) has a minimum storage requirement of 170 MB and a recommended storage requirement of 512 MB\.