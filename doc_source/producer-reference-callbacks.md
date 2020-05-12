# Producer SDK Callbacks<a name="producer-reference-callbacks"></a>

The classes and methods in the Amazon Kinesis Video Streams Producer SDK do not maintain their own processes\. Instead, they use the incoming function calls and events to schedule callbacks to communicate with the application\.

There are two callback patterns that the application can use to interact with the SDK:
+ `CallbackProvider`: This object exposes every callback from the platform\-independent code \(PIC\) component to the application\. This pattern allows full functionality, but it also means that the implementation must handle all of the public API methods and signatures in the C\+\+ layer\.
+ [StreamCallbackProvider](#producer-reference-callbacks-streamcallbackprovider) and [ClientCallbackProvider](#producer-reference-callbacks-clientcallbackprovider): These objects expose the stream\-specific and client\-specific callbacks, and the C\+\+ layer of the SDK exposes the rest of the callbacks\. This is the preferred callback pattern for interacting with the Producer SDK\.

The following diagram illustrates the object model of the callback objects:

![\[Diagram showing interaction of producers and consumers in Kinesis Video Streams.\]](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/images/callbacks-10.png)

In the preceding diagram, `DefaultCallbackProvider` derives from `CallbackProvider` \(which exposes all of the callbacks in the PIC\) and contains `StreamCallbackProvider` and `ClientCallbackProvider`\.

**Topics**
+ [ClientCallbackProvider](#producer-reference-callbacks-clientcallbackprovider)
+ [StreamCallbackProvider](#producer-reference-callbacks-streamcallbackprovider)
+ [ClientCallbacks Structure](#producer-reference-callbacks-clientcallbacks)
+ [Callback Implementations to Retry Streaming](#producer-reference-callbacks-retry)

## ClientCallbackProvider<a name="producer-reference-callbacks-clientcallbackprovider"></a>

The `ClientCallbackProvider` object exposes client\-level callback functions\. The details of the functions are described in the [ClientCallbacks](#producer-reference-callbacks-clientcallbacks) section\.

**Callback methods:**
+ `getClientReadyCallback`: Reports a ready state for the client\.
+ `getStorageOverflowPressureCallback`: Reports storage overflow or pressure\. This callback is called when the storage utilization drops below the `STORAGE_PRESSURE_NOTIFICATION_THRESHOLD` value, which is 5 percent of the overall storage size\. For more information, see [StorageInfo](producer-reference-structures-producer.md#producer-reference-structures-producer-storageinfo)\.

## StreamCallbackProvider<a name="producer-reference-callbacks-streamcallbackprovider"></a>

The `StreamCallbackProvider` object exposes stream\-level callback functions\.

**Callback methods:**
+ `getDroppedFragmentReportCallback`: Reports a dropped fragment\.
+ `getDroppedFrameReportCallback`: Reports a dropped frame\.
+ `getFragmentAckReceivedCallback`: Reports that a fragment ACK is received for the stream\.
+ `getStreamClosedCallback`: Reports a stream closed condition\.
+ `getStreamConnectionStaleCallback`: Reports a stale connection condition\. In this condition, the producer is sending data to the service but is not receiving acknowledgements\.
+ `getStreamDataAvailableCallback`: Reports that data is available in the stream\.
+ `getStreamErrorReportCallback`: Reports a stream error condition\.
+ `getStreamLatencyPressureCallback`: Reports a stream latency condition, which is when the accumulated buffer size is larger than the `max_latency` value\. For more information, see [StreamDefinition/ StreamInfo](producer-reference-structures-stream.md#producer-reference-structures-stream-streaminfo)\.
+ `getStreamReadyCallback`: Reports a stream ready condition\.
+ `getStreamUnderflowReportCallback`: Reports a stream underflow condition\. This function is not currently used and is reserved for future use\.

For the source code for `StreamCallbackProvider`, see [StreamCallbackProvider\.h](https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp/blob/d1684599a141785752582c16264e3123866f3cf8/kinesis-video-producer/src/StreamCallbackProvider.h)\.

## ClientCallbacks Structure<a name="producer-reference-callbacks-clientcallbacks"></a>

The `ClientCallbacks` structure contains the callback function entry points that the PIC calls when specific events occur\. The structure also contains version information in the `CALLBACKS_CURRENT_VERSION` field, and a `customData` field for user\-defined data that is returned with the individual callback functions\.

The client application can use a `this` pointer for the `custom_data` field to map member functions to the static `ClientCallback` functions at runtime, as shown in the following code example: 

```
STATUS TestStreamCallbackProvider::streamClosedHandler(UINT64 custom_data, STREAM_HANDLE stream_handle, UINT64 stream_upload_handle) {
    LOG_INFO("Reporting stream stopped.");

TestStreamCallbackProvider* streamCallbackProvider = reinterpret_cast<TestStreamCallbackProvider*> (custom_data);
streamCallbackProvider->streamClosedHandler(...);
```


**Events**  

| Function | Description | Type | 
| --- | --- | --- | 
| CreateDeviceFunc | Not currently implemented on the backend\. This call fails when called from Java or C\+\+\. Other clients perform platform\-specific initialization\. | Backend API | 
| CreateStreamFunc | Called when the stream is created\. | Backend API | 
| DescribeStreamFunc | Called when DescribeStream is called\. | Backend API | 
| GetStreamingEndpointFunc | Called when GetStreamingEndpoint is called\. | Backend API | 
| GetStreamingTokenFunc | Called when GetStreamingToken is called\. | Backend API | 
| PutStreamFunc | Called when PutStream is called\. | Backend API | 
| TagResourceFunc | Called when TagResource is called\. | Backend API | 
|   |   |   | 
| CreateMutexFunc | Creates a synchronization mutex\. | Synchronization | 
| FreeMutexFunc | Frees the mutex\. | Synchronization | 
| LockMutexFunc | Locks the synchronization mutex\. | Synchronization | 
| TryLockMutexFunc | Tries to lock the mutex\. Not currently implemented\. | Synchronization | 
| UnlockMutexFunc | Unlocks the mutex\. | Synchronization | 
|   |   |   | 
| ClientReadyFunc | Called when the client enters a ready state\. | Notification | 
| DroppedFrameReportFunc | Reports when a frame is dropped\. | Notification | 
| DroppedFragmentReportFunc | Reports when a fragment is dropped\. This function is not currently used and is reserved for future use\. | Notification | 
| FragmentAckReceivedFunc | Called when a fragment ACK \(buffering, received, persisted, and error\) is received\. | Notification | 
| StorageOverflowPressureFunc | Called when the storage utilization drops below the STORAGE\_PRESSURE\_NOTIFICATION\_THRESHOLD value, which is defined as 5 percent of the overall storage size\. | Notification | 
| StreamClosedFunc | Called when the last bits of the remaining frames are streamed\. | Notification | 
| StreamConnectionStaleFunc | Called when the stream enters a stale connection state\. In this condition, the producer is sending data to the service but is not receiving acknowledgements\. | Notification | 
| StreamDataAvailableFunc | Called when stream data is available\. | Notification | 
| StreamErrorReportFunc | Called when a stream error occurs\. The PIC automatically closes the stream under this condition\. | Notification | 
| StreamLatencyPressureFunc | Called when the stream enters a latency condition, which is when the accumulated buffer size is larger than the max\_latency value\. For more information, see [StreamDefinition/ StreamInfo](producer-reference-structures-stream.md#producer-reference-structures-stream-streaminfo)\. | Notification | 
| StreamReadyFunc | Called when the stream enters the ready state\. | Notification | 
| StreamUnderflowReportFunc | This function is not currently used and is reserved for future use\. | Notification | 
|   |   |   | 
| DeviceCertToTokenFunc | Returns the connection certificate as a token\. | Platform integration | 
| GetCurrentTimeFunc | Returns the current time\. | Platform integration | 
| GetDeviceCertificateFunc | Returns the device certificate\. This function is not currently used and is reserved for future use\. | Platform integration | 
| GetDeviceFingerprintFunc | Returns the device fingerprint\. This function is not currently used and is reserved for future use\. | Platform integration | 
| GetRandomNumberFunc | Returns a random number between 0 and RAND\_MAX\. | Platform integration | 
| GetSecurityTokenFunc | Returns the security token that is passed to the functions that communicate with the backend API\. The implementation can specify the serialized AccessKeyId, SecretKeyId, and the session token\. | Platform integration | 
| LogPrintFunc | Logs a line of text with the tag and the log level\. For more information, see PlatformUtils\.h\. | Platform integration | 

For the platform integration functions in the preceding table, the last parameter is a `ServiceCallContext` structure, which has the following fields:
+ `version`: The version of the struct\.
+ `callAfter`: An absolute time after which to call the function\.
+ `timeout`: The timeout of the operation in 100 nanosecond units\.
+ `customData`: A user\-defined value to be passed back to the client\.
+ `pAuthInfo`: The credentials for the call\. For more information, see the following \(`__AuthInfo`\) structure\.

The authorization information is provided using the `__AuthInfo` structure, which can be either serialized credentials or a provider\-specific authentication token\. This structure has the following fields:
+ `version`: The version of the `__AuthInfo` structure\.
+ `type`: An `AUTH_INFO_TYPE` value defining the type of the credential \(certificate or security token\)\.
+ `data`: A byte array containing the authentication information\.
+ `size`: The size of the `data` parameter\.
+ `expiration`: The expiration of the credentials in 100 nanosecond units\.

## Callback Implementations to Retry Streaming<a name="producer-reference-callbacks-retry"></a>

The Kinesis Video Producer SDK provides the status of streaming through callback functions\. It is recommended that you implement the following callback mechanisms to recover from any momentary network issues encountered during streaming\.
+ **Stream latency pressure callback** \- this callback mechanism gets triggered when the SDK encounters a stream latency condition\. This happens when the accumulated buffer size is larger than the MAX\_LATENCY value\. When the stream is created, the streaming application sets MAX\_LATENCY to the default value of 60 seconds\. The typical implementation for this callback is to reset the connection\. You can use the sample implementation at [https://github\.com/awslabs/amazon\-kinesis\-video\-streams\-producer\-sdk\-cpp/blob/master/kinesis\-video\-c\-producer/src/source/StreamLatencyStateMachine\.c](https://github.com/awslabs/amazon-kinesis-video-streams-producer-c/blob/master/src/source/StreamLatencyStateMachine.c) as needed\. Note that there is no option to store the frames undelivered due to network outage into a secondary storage for back\-fill\. 
+ **Stream staleness callback** \- this callback gets triggered when the producer can send data to the AWS KVS service \(uplink\) but it’s not able to get the acknowledgements \(buffered ACK\) back in time \(default is 60 seconds\)\. Depending on the network settings, either the stream latency pressure callback or the stream staleness callback, or both can get triggered\. Similar to the stream latency pressure callback retry implementation, the typical implementation is to reset the connection and start a new connection for streaming\. You can use the sample implementation at [https://github\.com/awslabs/amazon\-kinesis\-video\-streams\-producer\-c/blob/master/src/source/ConnectionStaleStateMachine\.c](https://github.com/awslabs/amazon-kinesis-video-streams-producer-c/blob/master/src/source/ConnectionStaleStateMachine.c) as needed\. 
+ **Stream error callback** \- this callback gets triggered when the SDK encounters a timeout on the network connection or other errors during the call to the KVS API service calls\. To recover from network timeout errors, you can refer to and use the sample implementation for recreating the stream at [https://github\.com/awslabs/amazon\-kinesis\-video\-streams\-producer\-sdk\-cpp/blob/master/samples/kinesis\_video\_gstreamer\_sample\_app\.cpp](https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp/blob/master/samples/kinesis_video_gstreamer_sample_app.cpp) \.
+ **Dropped frame callback** \- this callback gets triggered when the storage size is full either due to slow network speed or a stream error\. If the network speed results in dropped frames, then you can either increase the storage size, reduce the video frame size or frame rate to match the network speed\.