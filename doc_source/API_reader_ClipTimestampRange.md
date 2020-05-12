# ClipTimestampRange<a name="API_reader_ClipTimestampRange"></a>

The range of timestamps for which to return fragments\.

The values in the ClipTimestampRange are `inclusive`\. Fragments that begin before the start time but continue past it, or fragments that begin before the end time but continue past it, are included in the session\. 

## Contents<a name="API_reader_ClipTimestampRange_Contents"></a>

 **EndTimestamp**   <a name="KinesisVideo-Type-reader_ClipTimestampRange-EndTimestamp"></a>
The end of the timestamp range for the requested media\.  
This value must be within 3 hours of the specified `StartTimestamp`, and it must be later than the `StartTimestamp` value\. If `FragmentSelectorType` for the request is `SERVER_TIMESTAMP`, this value must be in the past\.   
This value is inclusive\. The `EndTimestamp` is compared to the \(starting\) timestamp of the fragment\. Fragments that start before the `EndTimestamp` value and continue past it are included in the session\.   
Type: Timestamp  
Required: Yes

 **StartTimestamp**   <a name="KinesisVideo-Type-reader_ClipTimestampRange-StartTimestamp"></a>
The starting timestamp in the range of timestamps for which to return fragments\.   
This value is inclusive\. Fragments that start before the `StartTimestamp` and continue past it are included in the session\. If `FragmentSelectorType` is `SERVER_TIMESTAMP`, the `StartTimestamp` must be later than the stream head\.   
Type: Timestamp  
Required: Yes

## See Also<a name="API_reader_ClipTimestampRange_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesis-video-reader-data-2017-09-30/ClipTimestampRange) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesis-video-reader-data-2017-09-30/ClipTimestampRange) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesis-video-reader-data-2017-09-30/ClipTimestampRange) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesis-video-reader-data-2017-09-30/ClipTimestampRange) 