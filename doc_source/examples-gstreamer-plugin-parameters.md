# GStreamer Element Parameter Reference<a name="examples-gstreamer-plugin-parameters"></a>

To send video to the Amazon Kinesis Video Streams Producer SDK, you specify `kvssink` as the *sink*, or final destination of the pipeline\. This reference provides information about `kvssink` required and optional parameters\. For more information, see [Example: Kinesis Video Streams Producer SDK GStreamer Plugin](examples-gstreamer-plugin.md)\.

The `kvssink` element has the following required parameters:
+ `stream-name`: The name of the destination Kinesis video stream\.
+ `storage-size`: The storage size of the device in kilobytes\. For information about configuring device storage, see [StorageInfo](producer-reference-structures-producer.md#producer-reference-structures-producer-storageinfo)\.
+ `access-key`: The AWS access key that is used to access Kinesis Video Streams\. You must provide either this parameter or `credential-path`\.
+ `secret-key`: The AWS secret key that is used to access Kinesis Video Streams\. You must provide either this parameter or `credential-path`\.
+ `credential-path`: A path to a file containing your credentials for accessing Kinesis Video Streams\. For example credential files, see [Sample Static Credential](https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp/blob/master/kinesis-video-gstreamer-plugin/sample_static_credential) and [Sample Rotating Credential](https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp/blob/master/kinesis-video-gstreamer-plugin/sample_rotating_credential)\. You must provide either this parameter or `access-key` and `secret-key`\.

The `kvssink` element has the following optional parameters\. For more information about these parameters, see [Kinesis Video Stream Structures](producer-reference-structures-stream.md)\.


****  

| Parameter | Description | Unit/ Type | Default | 
| --- | --- | --- | --- | 
| retention\-period | The length of time the stream is preserved\. | Hours | 2 | 
| streaming\-type | The streaming type\. Valid values include: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/examples-gstreamer-plugin-parameters.html) | Enum GstKvsSinkStreamingType | 0: real time | 
| content\-type | The content type of the stream\. | String | "video/h264" | 
| max\-latency | The maximum latency for the stream\. | Seconds | 60 | 
| fragment\-duration | The fragment duration that you want\. | Milliseconds | 2000 | 
| timecode\-scale | The MKV timecode scale\. | Milliseconds | 1 | 
| key\-frame\-fragmentation | Whether to produce fragments on a key frame\. | Boolean | true | 
| frame\-timecodes | Whether to use frame timecodes or generate time stamps using the current time callback\.  | Boolean | true | 
| absolute\-fragment\-times | Whether to use absolute fragment times\. | Boolean | true | 
| fragment\-acks | Whether to use fragment ACKs\. | Boolean | true | 
| restart\-on\-error | Whether to restart when an error occurs\. | Boolean | true | 
| recalculate\-metrics | Whether to recalculate the metrics\. | Boolean | true | 
| framerate | The expected frame rate\. | Frames per second | 25 | 
| avg\-bandwidth\-bps | The expected average bandwidth for the stream\.  | Bytes per second | 4194304 | 
| buffer\-duration | The stream buffer duration\.  | Seconds | 180 | 
| replay\-duration | The duration to roll the current reader backward to replay during an error if restarting is enabled\. | Seconds | 40 | 
| connection\-staleness | The time after which the stream staleness callback is called\. | Seconds | 60 | 
| codec\-id | The codec ID of the stream\. | String | "V\_MPEG4/ISO/AVC" | 
| track\-name | The MKV track name\. | String | "kinesis\_video" | 
| rotation\-period | The key rotation period\. For more information, see [Rotating Customer Master Keys](http://docs.aws.amazon.com/kms/latest/developerguide/rotate-keys.html)\. | Seconds | 2400 | 
| log\-config | The log configuration path\. | String | "\./kvs\_log\_configuration" | 
| frame\-timestamp |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/examples-gstreamer-plugin-parameters.html)  | Enum GstKvsSinkFrameTimestampType | default\-timestamp | 