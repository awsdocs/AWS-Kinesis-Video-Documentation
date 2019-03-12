# Kinesis Video Streams Data Model<a name="how-data"></a>

The [Producer Libraries](producer-sdk.md) and [Stream Parser Library](parser-library.md) send and receive video data in a format that supports embedding information alongside video data\. This format is based on the Matroska \(MKV\) specification\.

The [MKV format](https://en.wikipedia.org/wiki/Matroska) is an open specification for media data\. All the libraries and code examples in the *Amazon Kinesis Video Streams Developer Guide* send or receive data in the MKV format\. 

The [Kinesis Video Streams Producer Libraries](producer-sdk.md) use the `StreamDefinition` and `Frame` types to produce MKV stream headers, frame headers, and frame data\.

For information about the full MKV specification, see [Matroska Specifications](https://www.matroska.org/technical/specs/index.html)\.

The following sections describe the components of MKV\-formatted data produced by the [C\+\+ Producer Library](producer-sdk-cpp.md)\.

**Topics**
+ [Stream Header Elements](#how-data-header-streamdefinition)
+ [Stream Track Data](#how-data-header-streamtrack)
+ [Frame Header Elements](#how-data-header-frame)
+ [MKV Frame Data](#how-data-frame)

## Stream Header Elements<a name="how-data-header-streamdefinition"></a>

The following MKV header elements are used by `StreamDefinition` \(defined in `StreamDefinition.h`\)\.


****  

| Element | Description | Typical Values | 
| --- | --- | --- | 
| stream\_name | Corresponds to the name of the Kinesis video stream\. | my\-stream | 
| retention\_period | The duration that stream data is persisted by Kinesis Video Streams\. Specify 0 for a stream that does not retain data\.  | 24 | 
| tags | A key\-value collection of user data\. This data is displayed in the AWS Management Console and can be read by client applications to filter or get information about a stream\. |  | 
| kms\_key\_id | If present, the user\-defined AWS KMS master key that is used to encrypt data on the stream\. If it is absent, the data is encrypted by the Kinesis\-supplied master key \(aws/kinesis\-video\)\. | 01234567\-89ab\-cdef\-0123\-456789ab | 
| streaming\_type | Currently, the only valid streaming type is STREAMING\_TYPE\_REALTIME\. | STREAMING\_TYPE\_REALTIME | 
| content\_type | The user\-defined content type\. For streaming video data to play in the console, the content type must be video/h264\. | video/h264 | 
| max\_latency | This value is not currently used and should be set to 0\. | 0 | 
| fragment\_duration | The estimate of how long your fragments should be, which is used for optimization\. The actual fragment duration is determined by the streaming data\. | 2 | 
| timecode\_scale | Indicates the scale used by frame time stamps\. The default is 1 millisecond\. Specifying `0` also assigns the default value of 1 millisecond\. This value can be between 100 nanoseconds and 1 second\. For more information, see [TimecodeScale](https://matroska.org/technical/specs/notes.html#TimecodeScale) in the Matroska documentation\. |  | 
| key\_frame\_fragmentation | If true, the stream starts a new cluster when a keyframe is received\. | true | 
| frame\_timecodes | If true, Kinesis Video Streams stamps the frames when they are received\. If false, Kinesis Video Streams uses the decode time of the received frames\. | true | 
| absolute\_fragment\_time |  If true, the cluster timecodes are interpreted as using absolute time \(for example, from the producer's system clock\)\. If false, the cluster timecodes are interpreted as being relative to the start time of the stream\. | true | 
| fragment\_acks |  If true, acknowledgements \(ACKs\) are sent when Kinesis Video Streams receives the data\. The ACKs can be received using the KinesisVideoStreamFragmentAck or KinesisVideoStreamParseFragmentAck callbacks\. | true | 
| restart\_on\_error | Indicates whether the stream should resume transmission after a stream error is raised\. | true | 
| nal\_adaptation\_flags | Indicates whether NAL \(Network Abstraction Layer\) adaptation or codec private data is present in the content\. Valid flags include NAL\_ADAPTATION\_ANNEXB\_NALS and NAL\_ADAPTATION\_ANNEXB\_CPD\_NALS\. | NAL\_ADAPTATION\_ANNEXB\_NALS | 
| frame\_rate | An estimate of the content frame rate\. This value is used for optimization; the actual frame rate is determined by the rate of incoming data\. Specifying 0 assigns the default of 24\. | 24 | 
| avg\_bandwidth\_bps | An estimate of the content bandwidth\. This value is used for optimization; the actual rate is determined by the bandwidth of incoming data\. For example, for a 720 p resolution video stream running at 25 FPS, you can expect the average bandwidth to be 5 Mbps\. | 5 | 
| buffer\_duration | The duration that content is to be buffered on the producer\. If there is low network latency, this value can be reduced; if network latency is high, increasing this value prevents frames from being dropped before they can be sent, due to allocation failing to put frames into the smaller buffer\. |  | 
| replay\_duration | The amount of time the video data stream is "rewound" in the case of connection loss\. This value can be zero if lost frames due to connection loss are not a concern; the value can be increased if the consuming application can eliminate redundant frames\. This value should be less than the buffer duration; otherwise the buffer duration is used\. |  | 
| connection\_staleness | The duration that a connection is maintained when no data is received\. |  | 
| codec\_id | The codec used by the content\. For more information, see [CodecID](https://matroska.org/technical/specs/codecid/index.html) in the Matroska specification\. | V\_MPEG2 | 
| track\_name | The user\-defined name of the track\. | my\_track | 
| codecPrivateData | Data provided by the encoder used to decode the frame data, such as the frame width and height in pixels, which is needed by many downstream consumers\. In the [C\+\+ Producer Library](producer-sdk-cpp.md), the gMkvTrackVideoBits array in MkvStatics\.cpp includes pixel width and height for the frame\. |  | 
| codecPrivateDataSize | The size of the data in the codecPrivateData parameter\. |  | 
| track\_type | The type of the track for the stream\. | MKV\_TRACK\_INFO\_TYPE\_AUDIO or MKV\_TRACK\_INFO\_TYPE\_VIDEO | 
| segment\_uuid | User\-defined segment uuid \(16 bytes\)\. |  | 
| default\_track\_id | Unique non\-zero number for the track\. | 1 | 

## Stream Track Data<a name="how-data-header-streamtrack"></a>

The following MKV track elements are used by `StreamDefinition` \(defined in `StreamDefinition.h`\)\.


****  

| Element | Description | Typical Values | 
| --- | --- | --- | 
| track\_name  | User\-defined track name\. For example, "audio" for the audio track\.  | audio | 
| codec\_id | Codec id for the track\. For example, "A\_AAC" for an audio track\. | A\_AAC | 
| cpd | Data provided by the encoder used to decode the frame data\. This data can include frame width and height in pixels, which is needed by many downstream consumers\. In the [C\+\+ Producer Library](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producer-sdk-cpp.html), the gMkvTrackVideoBits array in MkvStatics\.cpp includes pixel width and height for the frame\.  |  | 
| cpd\_size | The size of the data in the codecPrivateData parameter\. |  | 
| track\_type | The type of the track\. For example, you can use the enum value of MKV\_TRACK\_INFO\_TYPE\_AUDIO for audio\. | MKV\_TRACK\_INFO\_TYPE\_AUDIO | 

## Frame Header Elements<a name="how-data-header-frame"></a>

The following MKV header elements are used by `Frame` \(defined in the `KinesisVideoPic` package, in `mkvgen/Include.h`\):
+ **Frame Index:** A monotonically increasing value\.
+ **Flags:** The type of frame\. Valid values include the following:
  + `FRAME_FLAGS_NONE`
  + `FRAME_FLAG_KEY_FRAME`: If `key_frame_fragmentation` is set on the stream, key frames start a new fragment\.
  + `FRAME_FLAG_DISCARDABLE_FRAME`: Tells the decoder that it can discard this frame if decoding is slow\.
  + `FRAME_FLAG_INVISIBLE_FRAME`: Duration of this block is 0\.
+ **Decoding Timestamp:** The time stamp of when this frame was decoded\. If previous frames depend on this frame for decoding, this time stamp might be earlier than that of earlier frames\. This value is relative to the start of the fragment\.
+ **Presentation Timestamp:** The time stamp of when this frame is displayed\. This value is relative to the start of the fragment\.
+ **Duration:** The playback duration of the frame\.
+ **Size:** The size of the frame data in bytes

## MKV Frame Data<a name="how-data-frame"></a>

The data in `frame.frameData` might contain only media data for the frame, or it might contain further nested header information, depending on the encoding schema used\. To be displayed in the AWS Management Console, the data must be encoded in the [H\.264](https://en.wikipedia.org/wiki/H.264/MPEG-4_AVC) codec, but Kinesis Video Streams can receive time\-serialized data streams in any format\.