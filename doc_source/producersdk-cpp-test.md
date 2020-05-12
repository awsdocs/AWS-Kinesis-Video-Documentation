# Step 3: Run and Verify the Code<a name="producersdk-cpp-test"></a>

To run and verify the code for the [C\+\+ Producer Library procedure](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producer-sdk-cpp.html), do the following:

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

1. Navigate to the `build` directory you created with the step above, and run `make` to build the WebRTC C\+\+ SDK and its provided samples\. 

   ```
   make
   ```

1. To enable verbose logs, define the `HEAP_DEBUG` and `LOG_STREAMING` C\-defines by uncommenting the appropriate lines in `CMakeList.txt`\.

You can monitor the progress of the test suite in the debug output in your IDE\. You can also monitor the traffic on your stream by watching the metrics that are associated with your stream in the Amazon CloudWatch console, such as `PutMedia.IncomingBytes`\.

**Note**  
Because the test harness only sends frames of empty bytes, the console doesn't display the data as a video stream\.