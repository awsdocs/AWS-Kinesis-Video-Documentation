# Kinesis Video Stream Structures<a name="producer-reference-structures-stream"></a>

You can use the following structures to provide data to an instance of a Kinesis video stream\.

**Topics**
+ [StreamDefinition/ StreamInfo](#producer-reference-structures-stream-streaminfo)
+ [ClientMetrics](#producer-reference-structures-stream-clientmetrics)
+ [StreamMetrics](#producer-reference-structures-stream-streammetrics)

## StreamDefinition/ StreamInfo<a name="producer-reference-structures-stream-streaminfo"></a>

The `StreamDefinition` object in the C\+\+ layer wraps the `StreamInfo` object in the platform\-independent code, and provides some default values in the constructor\. 

### Member Fields<a name="producer-reference-structures-stream-streaminfo-fields"></a>


****  

| Field | Data Type | Description | Default Value | 
| --- | --- | --- | --- | 
| stream\_name | string | An optional stream name\. For more information about the length of the stream name, see [Producer SDK Limits](producer-sdk-limits.md) \. Each stream should have a unique name\. | If no name is specified, a name is generated randomly\. | 
| retention\_period | duration<uint64\_t, ratio<3600>> | The retention period for the stream, in seconds\. Specifying 0 indicates no retention\. | 3600 \(One hour\) | 
| tags | const map<string, string>\*  | A map of key\-value pairs that contain user information\. If the stream already has a set of tags, the new tags are appended to the existing set of tags\.  | No tags | 
| kms\_key\_id | string | The AWS KMS key ID to be used for encrypting the stream\. For more information, see [Data Protection in Kinesis Video Streams](how-kms.md)\. | The default KMS key \(aws/kinesis\-video\.\) | 
| streaming\_type | STREAMING\_TYPE enumeration | The only supported value is STREAMING\_TYPE\_REALTIME\. |  | 
| content\_type | string | The content format of the stream\. The Kinesis Video Streams console can play back content in the video/h264 format\. | video/h264 | 
| max\_latency | duration<uint64\_t, milli> | The maximum latency in milliseconds for the stream\. The stream latency pressure callback \(if specified\) is called when the buffer duration exceeds this amount of time\. Specifying 0 indicates that no stream latency pressure callback will be called\.  | milliseconds::zero\(\) | 
| fragment\_duration | duration<uint64\_t>  | The fragment duration that you want, in seconds\. This value is used in combination with the key\_frame\_fragmentation value\. If this value is false, Kinesis Video Streams generates fragments on a key frame after this duration elapses\. For example, an Advanced Audio Coding \(AAC\) audio stream has each frame as a key frame\. Specifying key\_frame\_fragmentation = false causes fragmentation to happen on a key frame after this duration expires, resulting in 2\-second fragments\.  | 2 | 
| timecode\_scale | duration<uint64\_t, milli>  |  The MKV timecode scale in milliseconds, which specifies the granularity of the timecodes for the frames within the MKV cluster\. The MKV frame timecode is always relative to the start of the cluster\. MKV uses a signed 16\-bit value \(0\-32767\) to represent the timecode within the cluster \(fragment\)\. Therefore, you should ensure that the frame timecode can be represented with the given timecode scale\. The default timecode scale value of 1 ms ensures that the largest frame that can be represented is 32767 ms \~= 32 seconds\. This is over the maximum fragment duration that is specified in [Kinesis Video Streams Limits](limits.md), which is 10 seconds\. | 1 | 
| key\_frame\_fragmentation | bool | Whether to produce fragments on a key frame\. If true, the SDK produces a start of the fragment every time there is a key frame\. If false, Kinesis Video Streams waits for at least fragment\_duration and produces a new fragment on the key frame following it\. | true | 
| frame\_timecodes | bool | Whether to use frame timecodes or generate timestamps using the current time callback\. Many encoders don't produce timestamps with the frames\. So specifying false for this parameter ensures that the frames are timestamped as they are put into Kinesis Video Streams\. | true | 
| absolute\_fragment\_times | bool | Kinesis Video Streams uses MKV as its underlying packaging mechanism\. The MKV specification is strict about frame timecodes being relative to the beginning of the cluster \(fragment\)\. However, the cluster timecodes can be either absolute or relative to the starting time for the stream\. If the timestamps are relative, the PutMedia service API call uses the optional stream start timestamp and adjust the cluster timestamps\. The service always stores the fragments with their absolute timestamps\. | true | 
| fragment\_acks | bool | Whether to receive application level fragment ACKs \(acknowledgements\) or not\.  | true, meaning that the SDK will receive the ACKs and act accordingly\. | 
| restart\_on\_error | bool | Whether to restart on specific errors\. | true, meaning that the SDK tries to restart the streaming if any errors occur\. | 
| recalculate\_metrics | bool | Whether to recalculate the metrics\. Each call to retrieve the metrics can recalculate those to get the latest "running" value, which might create a small CPU impact\. You might need to set this to false on extremely low\-power/footprint devices to spare the CPU cycles\. Otherwise, it’s not advised to use false for this value\. | true | 
| nal\_adaptation\_flags | uint32\_t  |  Specifies the Network Abstraction Layer unit \(NALU\) adaptation flags\. If the bitstream is H\.264 encoded, it can then be processed as raw or packaged in NALUs\. Those are either in the Annex\-B or AVCC format\. Most of the elementary stream producers/consumers \(read encoders/decoders\) use the Annex\-B format because it has some advantages, such as error recovery\. Higher\-level systems use the AVCC format, which is the default format for MPEG, HLS, DASH, and so on\. The console playback uses the browser's MSE \(media source extensions\) to decode and play back the stream that uses the AVCC format\. For H\.264 \(and for M\-JPEG and H\.265\), the SDK provides adaptation capabilities\. Many elementary streams are in the following format\. In this example, `Ab` is the Annex\-B start code \(001 or 0001\)\. <pre>Ab(Sps)Ab(Pps)Ab(I-frame)Ab(P/B-frame) Ab(P/B-frame)…. Ab(Sps)Ab(Pps)Ab(I-frame)Ab(P/B-frame) Ab(P/B-frame)</pre> In the case of H\.264, the codec private data \(CPD\) is in the SPS \(sequence parameter set\) and PPS \(picture parameter set\) parameters, and it can be adapted to the AVCC format\. Unless the media pipeline gives the CPD separately, the application can extract the CPD from the frame\. It can do this by looking for the first IDR frame \(which should contain the SPS/PPS\), extract the two NALUs \(which are `Ab(Sps)Ab(Pps)`\), and set it in the CPD in `StreamDefinition`\. For more information, see [NAL Adaptation Flags](producer-reference-nal.md)\.  | The default is to adapt Annex\-B format to AVCC format for both the frame data and for the codec private data\.  | 
| frame\_rate | uint32\_t  | The expected frame rate\. This value is used to better calculate buffering needs\. | 25 | 
| avg\_bandwidth\_bps | uint32\_t  | The expected average bandwidth for the stream\. This value is used to better calculate buffering needs\. | 4 \* 1024 \* 1024  | 
| buffer\_duration | duration<uint64\_t>  | The stream buffer duration, in seconds\. The SDK keeps the frames in the content store for up to the buffer\_duration, after which the older frames are dropped as the window moves forward\. If the frame that is being dropped has not been sent to the backend, the dropped frame callback is called\. If the current buffer duration is greater than max\_latency, then the stream latency pressure callback is called\. The buffer is trimmed to the next fragment start when the fragment persisted ACK is received\. This indicates that the content has been durably persisted in the cloud, so storing the content on the local device is no longer needed\. | 120 | 
| replay\_duration | duration<uint64\_t> | The duration to roll the current reader backward to replay during an error if restarting is enabled, in seconds\. The rollback stops at the buffer start \(in case it has just started streaming or the persisted ACK has come along\)\. The rollback tries to land on a key frame that indicates a fragment start\. If the error that is causing the restart is not indicative of a dead host \(that is, the host is still alive and contains the frame data in its internal buffers\), the rollback stops at the last received ACK frame\. It then rolls forward to the next key frame, because the entire fragment is already stored in the host memory\.  | 40 | 
| connection\_staleness | duration<uint64\_t> | The time, in seconds, after which the stream staleness callback is called if the SDK does not receive the buffering ACK\. It indicates that the frames are being sent from the device, but the backend is not acknowledging them\. This condition indicates a severed connection at the intermediate hop or at the load balancer\.  | 30 | 
| codec\_id | string | The codec ID for the MKV track\. | V\_MPEG4/ISO/AVC | 
| track\_name | string | The MKV track name\. | kinesis\_video | 
| codecPrivateData | unsigned char\*  | The codec private data \(CPD\) buffer\. If the media pipeline has the information about the CPD before the stream starts, it can be set in StreamDefinition\.codecPrivateData\. The bits are copied, and the buffer can be reused or freed after the call to create the stream\. However, if the data is not available when the stream is created, it can be set in one of the overloads of the KinesisVideoStream\.start\(cpd\) function\. | null | 
| codecPrivateDataSize | uint32\_t  | The codec private data buffer size\. | 0 | 

## ClientMetrics<a name="producer-reference-structures-stream-clientmetrics"></a>

The **ClientMetrics** object is filled by calling `getKinesisVideoMetrics`\.

### Member Fields<a name="producer-reference-structures-stream-clientmetrics-fields"></a>


****  

| Field | Data Type | Description | 
| --- | --- | --- | 
| version | UINT32 | The version of the structure, defined in the CLIENT\_METRICS\_CURRENT\_VERSION macro\. | 
| contentStoreSize  | UINT64 | The overall content store size in bytes\. This is the value specified in DeviceInfo\.StorageInfo\.storageSize\. | 
| contentStoreAvailableSize  | UINT64 | Currently available storage size in bytes\.  | 
| contentStoreAllocatedSize  | UINT64 | Currently allocated size\. The allocated plus the available sizes should be slightly smaller than the overall storage size, due to the internal bookkeeping and the implementation of the content store\. | 
| totalContentViewsSize  | UINT64 | The size of the memory allocated for all content views for all streams\. This is not counted against the storage size\. This memory is allocated using the MEMALLOC macro, which can be overwritten to provide a custom allocator\. | 
| totalFrameRate | UINT64 | The total observed frame rate across all the streams\. | 
| totalTransferRate | UINT64 | The total observed stream rate in bytes per second across all the streams\. | 

## StreamMetrics<a name="producer-reference-structures-stream-streammetrics"></a>

The **StreamMetrics** object is filled by calling `getKinesisVideoMetrics`\.

### Member Fields<a name="producer-reference-structures-stream-clientmetrics-fields"></a>


****  

| Field | Data Type | Description | 
| --- | --- | --- | 
| version | UINT32 | The version of the structure, defined in the STREAM\_METRICS\_CURRENT\_VERSION macro\. | 
| currentViewDuration  | UINT64 | The duration of the accumulated frames\. In the fast networking case, this duration is either 0 or the frame duration \(as the frame is being transmitted\)\. If the duration becomes longer than the max\_latency specified in the StreamDefinition, the stream latency callback is called if it is specified\. The duration is specified in 100 ns units, which is the default time unit for the PIC layer\. | 
| overallViewDuration  | UINT64 | The overall view duration\. If the stream is configured with no ACKs or persistence, this value grows as the frames are put into the Kinesis video stream and becomes equal to the buffer\_duration in the StreamDefinition\. When ACKs are enabled and the persisted ACK is received, the buffer is trimmed to the next key frame, because the ACK timestamp indicates the beginning of the entire fragment\. The duration is specified in 100\-ns units, which is the default time unit for the PIC layer\. | 
| currentViewSize  | UINT64 | The size in bytes of the current buffer\. | 
| overallViewSize  | UINT64 | The overall view size in bytes\. | 
| currentFrameRate  | UINT64 | The observed frame rate for the current stream\. | 
| currentTransferRate | UINT64 | The observed transfer rate in bytes per second for the current stream\. | 