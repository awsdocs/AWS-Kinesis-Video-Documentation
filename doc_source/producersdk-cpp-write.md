# Step 2: Write and Examine the Code<a name="producersdk-cpp-write"></a>

In this section of the [C\+\+ Producer Library procedure](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producer-sdk-cpp.html), you examine the code in the C\+\+ test harness \(`tst/ProducerTestFixture.h` and other files\)\. You downloaded this code in the previous section\.

The **Platform Independent** C\+\+ example shows the following coding pattern:
+ Create an instance of `KinesisVideoProducer` to access Kinesis Video Streams\.
+ Create an instance of `KinesisVideoStream`\. This creates a Kinesis video stream in your AWS account if a stream of the same name doesn't already exist\.
+ Call `putFrame` on the `KinesisVideoStream` for every frame of data, as it becomes available, to send it to the stream\.

The following sections provide details:

## Creating an Instance of KinesisVideoProducer<a name="producersdk-cpp-write-create-producer"></a>

You create the `KinesisVideoProducer` object by calling the `KinesisVideoProducer::createSync` method\. The following example creates the `KinesisVideoProducer` in the `ProducerTestFixture.h` file:

```
kinesis_video_producer_ = KinesisVideoProducer::createSync(move(device_provider_),
    move(client_callback_provider_),
    move(stream_callback_provider_),
    move(credential_provider_),
    defaultRegion_);
```

The `createSync` method takes the following parameters:
+ A `DeviceInfoProvider` object, which returns a `DeviceInfo` object containing information about the device or storage configuration\.
**Note**  
You configure your content store size using the `deviceInfo.storageInfo.storageSize` parameter\. Your content streams share the content store\. To determine your storage size requirement, multiply the average frame size by the number of frames stored for the max duration for all the streams\. Then multiply by 1\.2 to account for defragmentation\. For example, suppose that your application has the following configuration:  
Three streams
3 minutes of maximum duration
Each stream is 30 frames per second \(FPS\)
Each frame is 10,000 KB in size
The content store requirement for this application is **3 \(streams\) \* 3 \(minutes\) \* 60 \(seconds in a minute\) \* 10000 \(kb\) \* 1\.2 \(defragmentation allowance\) = 194\.4 Mb \~ 200Mb**\.
+ A `ClientCallbackProvider` object, which returns function pointers that report client\-specific events\.
+ A `StreamCallbackProvider` object, which returns function pointers that are called back when stream\-specific events occur\.
+ A `CredentialProvider` object, which provides access to AWS credential environment variables\.
+ The AWS Region \("us\-west\-2"\)\. The service endpoint is determined from the Region\.

## Creating an Instance of KinesisVideoStream<a name="producersdk-cpp-write-create-stream"></a>

You create the `KinesisVideoStream` object by calling the `KinesisVideoProducer::CreateStream` method with a `StreamDefinition` parameter\. The example creates the `KinesisVideoStream` in the `ProducerTestFixture.h` file:

```
auto stream_definition = make_unique<StreamDefinition>(stream_name,
                                               hours(2),
                                               tags,
                                               "",
                                               STREAMING_TYPE_REALTIME,
                                               "video/h264",
                                               milliseconds::zero(),
                                               seconds(2),
                                               milliseconds(1),
                                               true,
                                               true,
                                               true);
return kinesis_video_producer_->createStream(move(stream_definition));
```

The `StreamDefinition` object has the following fields:
+ Stream name\.
+ Data retention period\.
+ Tags for the stream\. These tags can be used by consumer applications to find the correct stream, or to get more information about the stream\. The tags can also be viewed in the AWS Management Console\.
+ AWS KMS encryption key for the stream\. For more information, see [Using Server\-Side Encryption with Kinesis Video Streams](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/how-kms.html)\.
+ Streaming type\. Currently, the only valid value is `STREAMING_TYPE_REALTIME`\.
+ Media content type\. To view the stream in the console viewer, set this value to `"video/h264"`\.
+ Media latency\. This value is not currently used, and should be set to 0\.
+ Playback duration of each fragment\.
+ Media timecode scale\.
+ Whether the media uses key frame fragmentation\.
+ Whether the media uses timecodes\.
+ Whether the media uses absolute fragment times\.

## Putting a Frame into the Kinesis Video Stream<a name="producersdk-cpp-write-putframe"></a>

You put media into the Kinesis video stream using `KinesisVideoStream::putFrame`, passing in a `Frame` object that contains the header and media data\. The example calls `putFrame` in the `ProducerApiTest.cpp` file:

```
frame.duration = FRAME_DURATION_IN_MICROS * HUNDREDS_OF_NANOS_IN_A_MICROSECOND;
    frame.size = SIZEOF(frameBuffer_);
    frame.frameData = frameBuffer_;
    MEMSET(frame.frameData, 0x55, frame.size);

    while (!stop_producer_) {
        // Produce frames
        timestamp = std::chrono::duration_cast<std::chrono::nanoseconds>(
                std::chrono::system_clock::now().time_since_epoch()).count() / DEFAULT_TIME_UNIT_IN_NANOS;
        frame.index = index++;
        frame.decodingTs = timestamp;
        frame.presentationTs = timestamp;

        // Key frame every 50th
        frame.flags = (frame.index % 50 == 0) ? FRAME_FLAG_KEY_FRAME : FRAME_FLAG_NONE;
    ...

    EXPECT_TRUE(kinesis_video_stream->putFrame(frame));
```

**Note**  
The preceding C\+\+ Producer example sends a buffer of test data\. In a real\-world application, you should obtain the frame buffer and size from the frame data from a media source \(such as a camera\)\.

The `Frame` object has the following fields:
+ Frame index\. This should be a monotonically incrementing value\.
+ Flags associated with the frame\. For example, if the encoder were configured to produce a key frame, this frame would be assigned the `FRAME_FLAG_KEY_FRAME` flag\.
+ Decoding time stamp\.
+ Presentation time stamp\.
+ Duration of the frame \(to 100 ns units\)\.
+ Size of the frame in bytes\.
+ Frame data\.

For more information about the format of the frame, see [Kinesis Video Streams Data Model](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/how-data.html)\.

## Metrics and Metric Logging<a name="producersdk-cpp-write-metrics"></a>

The C\+\+ Producer SDK includes functionality for metrics and metric logging\. 

You can use the `getKinesisVideoMetrics` and `getKinesisVideoStreamMetrics` APIs to retrieve information about Kinesis Video Streams and your active streams\.

The following code is from the `kinesis-video-pic/src/client/include/com/amazonaws/kinesis/video/client/Include.h` file\.

```
/**
* Gets information about the storage availability.
*
* @param 1 CLIENT_HANDLE - the client object handle.
* @param 2 PKinesisVideoMetrics - OUT - Kinesis Video metrics to be filled.
*
* @return Status of the function call.
*/
PUBLIC_API STATUS getKinesisVideoMetrics(CLIENT_HANDLE, PKinesisVideoMetrics);

/**
* Gets information about the stream content view.
*
* @param 1 STREAM_HANDLE - the stream object handle.
* @param 2 PStreamMetrics - Stream metrics to fill.
*
* @return Status of the function call.
*/
PUBLIC_API STATUS getKinesisVideoStreamMetrics(STREAM_HANDLE, PStreamMetrics);
```

The `PClientMetrics` object filled by `getKinesisVideoMetrics` contains the following information:
+ **contentStoreSize:** The overall size in bytes of the content store \(the memory used to store streaming data\)\.
+ **contentStoreAvailableSize:** The free memory in the content store, in bytes\.
+ **contentStoreAllocatedSize:** The allocated memory in the content store\.
+ **totalContentViewsSize:** The total memory used for the content view\. \(The content view is a series of indices of information in the content store\.\)
+ **totalFrameRate:** The aggregate number of frames per second across all active streams\.
+ **totalTransferRate:** The total bits per second \(bps\) being sent in all streams\.

The `PStreamMetrics` object filled by `getKinesisVideoStreamMetrics` contains the following information:
+ **currentViewDuration:** The difference in 100 ns units between the head of the content view \(when frames are encoded\) and the current position \(when frame data is being sent to Kinesis Video Streams\)\.
+ **overallViewDuration:** The difference in 100 ns units between the head of the content view \(when frames are encoded\) to the tail \(when frames are flushed from memory, either because the total allocated space for the content view is exceeded, or because a `PersistedAck` message is received from Kinesis Video Streams, and frames known to be persisted are flushed\)\.
+ **currentViewSize:** The size in bytes of the content view from the head \(when frames are encoded\) to the current position \(when frames are sent to Kinesis Video Streams\)\.
+ **overallViewSize:** The total size in bytes of the content view\.
+ **currentFrameRate:** The last measured rate of the stream, in frames per second\.
+ **currentTransferRate:** The last measured rate of the stream, in bytes per second\.

## Next Step<a name="producersdk-cpp-write-next"></a>

[Step 3: Run and Verify the Code](producersdk-cpp-test.md)