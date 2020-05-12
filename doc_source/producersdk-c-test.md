# Step 3: Run and Verify the Code<a name="producersdk-c-test"></a>

To run and verify the code for the [ Producer Library procedure](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producer-sdk-cpp.html), do the following:

1. Run the following commands to create a `build` directory in your [downloaded C SDK](https://github.com/awslabs/amazon-kinesis-video-streams-producer-c.git), and execute `cmake` from it:

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

1. Navigate to the `build` directory you just created with the step above, and run `make` to build the WebRTC C SDK and its provided samples\. 

   ```
   make
   ```

1. The sample application `kinesis_video_cproducer_video_only_sample` sends h\.264\-encoded video frames inside the folder `samples/h264SampleFrames` to Kinesis Video Streams\. The following command sends the video frames in a loop for ten seconds to Kinesis Video Streams:

   ```
   ./kinesis_video_cproducer_video_only_sample YourStreamName 10                   
   ```

   If you want to send H\.264\-encoded frames from another folder \(for example, `MyH264FramesFolder`\), you can run the sample with the following arguments:

   ```
   ./kinesis_video_cproducer_video_only_sample YourStreamName 10 MyH264FramesFolder
   ```

1. To enable verbose logs, define the `HEAP_DEBUG` and `LOG_STREAMING` C\-defines by uncommenting the appropriate lines in `CMakeList.txt`\.

You can monitor the progress of the test suite in the debug output in your IDE\. You can also monitor the traffic on your stream by watching the metrics that are associated with your stream in the Amazon CloudWatch console, such as `PutMedia.IncomingBytes`\.

**Note**  
The console doesn't display the data as a video stream because the test harness only sends frames of empty bytes\.