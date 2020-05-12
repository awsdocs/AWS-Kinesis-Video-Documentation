# Step 2: Write and Examine the Code<a name="producersdk-c-write"></a>

In this section, you examine the code of the sample application `KvsVideoOnlyStreamingSample.c` in the `samples` folder of the [https://github\.com/awslabs/amazon\-kinesis\-video\-streams\-producer\-c](https://github.com/awslabs/amazon-kinesis-video-streams-producer-c) repo on GitHub\. You downloaded this code in the previous step\. This sample demonstrates how to use the C producer library to send H\.264\-encoded video frames inside the folder `samples/h264SampleFrames` to your Kinesis video stream\.

This sample application has three sections:
+ Initialization and configuration:
  + Initializing and configuring the platform\-specific media pipeline\.
  + Initializing and configuring KinesisVideoClient and KinesisVideoStream for the pipeline, setting the callbacks, integrating scenario\-specific authentication, extracting and submitting codec private data, and getting the stream to READY state\.
+ Main loop:
  + Getting the frame from the media pipeline with the timestamps and flags\.
  + Submitting the frame to the KinesisVideoStream\.
+ Teardown:
  + Stopping \(sync\) KinesisVideoStream, freeing KinesisVideoStream, freeing KinesisVideoClient\.

This sample application completes the following tasks:
+ Call the `createDefaultDeviceInfo` API to create the `deviceInfo` object that contains information about the device or storage configuration\.

  ```
  // default storage size is 128MB. Use setDeviceInfoStorageSize after create to change storage size.
  CHK_STATUS(createDefaultDeviceInfo(&pDeviceInfo));
  // adjust members of pDeviceInfo here if needed
      pDeviceInfo->clientInfo.loggerLogLevel = LOG_LEVEL_DEBUG;
  ```
+ Call the `createRealtimeVideoStreamInfoProvider` API to create the `StreamInfo` object\.

  ```
  CHK_STATUS(createRealtimeVideoStreamInfoProvider(streamName, DEFAULT_RETENTION_PERIOD, DEFAULT_BUFFER_DURATION, &pStreamInfo));
  // adjust members of pStreamInfo here if needed
  ```
+ Call the `createDefaultCallbacksProviderWithAwsCredentials` API to create the default callbacks provider based on static AWS credentials\.

  ```
  CHK_STATUS(createDefaultCallbacksProviderWithAwsCredentials(accessKey,
                                                                  secretKey,
                                                                  sessionToken,
                                                                  MAX_UINT64,
                                                                  region,
                                                                  cacertPath,
                                                                  NULL,
                                                                  NULL,
                                                                  FALSE,
                                                                  &pClientCallbacks));
  ```
+ Call the `createKinesisVideoClient` API to create the `KinesisVideoClient` object that contains information about your device storage and maintains callbacks to report on Kinesis Video Streams events\.

  ```
  CHK_STATUS(createKinesisVideoClient(pDeviceInfo, pClientCallbacks, &clientHandle));
  ```
+ Call the `createKinesisVideoStreamSync` API to create the `KinesisVideoStream` object\.

  ```
  CHK_STATUS(createKinesisVideoStreamSync(clientHandle, pStreamInfo, &streamHandle));
  ```
+ Set up a sample frame and call `PutKinesisVideoFrame` API to send that frame to the `KinesisVideoStream` object\.

  ```
   // setup sample frame
      MEMSET(frameBuffer, 0x00, frameSize);
      frame.frameData = frameBuffer;
      frame.version = FRAME_CURRENT_VERSION;
      frame.trackId = DEFAULT_VIDEO_TRACK_ID;
      frame.duration = HUNDREDS_OF_NANOS_IN_A_SECOND / DEFAULT_FPS_VALUE;
      frame.decodingTs = defaultGetTime(); // current time
      frame.presentationTs = frame.decodingTs;
  
      while(defaultGetTime() > streamStopTime) {
          frame.index = frameIndex;
          frame.flags = fileIndex % DEFAULT_KEY_FRAME_INTERVAL == 0 ? FRAME_FLAG_KEY_FRAME : FRAME_FLAG_NONE;
          frame.size = SIZEOF(frameBuffer);
  
          CHK_STATUS(readFrameData(&frame, frameFilePath));
  
          CHK_STATUS(putKinesisVideoFrame(streamHandle, &frame));
          defaultThreadSleep(frame.duration);
  
          frame.decodingTs += frame.duration;
          frame.presentationTs = frame.decodingTs;
          frameIndex++;
          fileIndex++;
          fileIndex = fileIndex % NUMBER_OF_FRAME_FILES;
      }
  ```
+ Teardown:

  ```
  CHK_STATUS(stopKinesisVideoStreamSync(streamHandle));
  CHK_STATUS(freeKinesisVideoStream(&streamHandle));
  CHK_STATUS(freeKinesisVideoClient(&clientHandle));
  ```

## Next Step<a name="producersdk-c-write-next"></a>

[Step 3: Run and Verify the Code](producersdk-c-test.md)