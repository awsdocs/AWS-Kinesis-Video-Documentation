# Step 3: Run and Verify the Code<a name="producersdk-cpp-test"></a>

To run and verify the code for the [C\+\+ Producer Library procedure](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producer-sdk-cpp.html), do the following:

1. See [Prerequisites](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producer-sdk-cpp.html#producer-sdk-cpp-prerequisites) for credential, certificate, and build requirements\.

1. Build the project by using the `/kinesis-video-native-build/install-script` script\. Running the install script installs the following open source dependencies:
   + [curl lib](https://curl.haxx.se/docs/copyright.html)
   + [openssl \(crypto and ssl\)](https://github.com/openssl/openssl/blob/master/LICENSE)
   + [log4cplus](https://github.com/log4cplus/log4cplus/blob/master/LICENSE)
   + [jsoncpp](https://github.com/open-source-parsers/jsoncpp/blob/master/LICENSE)
**Note**  
To configure **log4cplus**, set the following value in `PlatformUtils.h` to point to your logging function:  

   ```
   #define __LOG(p1, p2, p3, ...)     printf(p3, ##__VA_ARGS__)
   ```

1. The executable is built in `kinesis-video-native-build/start`\. Launch it to run the unit test and kick off dummy frame streaming\.

1. To enable verbose logs, define the `HEAP_DEBUG` and `LOG_STREAMING` C\-defines by uncommenting the appropriate lines in `CMakeList.txt`\.

You can monitor the progress of the test suite in the debug output in your IDE\. You can also monitor the traffic on your stream by watching the metrics that are associated with your stream in the Amazon CloudWatch console, such as `PutMedia.IncomingBytes`\.

**Note**  
Because the test harness only sends frames of empty bytes, the console doesn't display the data as a video stream\.