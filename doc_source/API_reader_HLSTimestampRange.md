# HLSTimestampRange<a name="API_reader_HLSTimestampRange"></a>

The start and end of the time stamp range for the requested media\.

This value should not be present if `PlaybackType` is `LIVE`\.

**Note**  
The values in the `HLSTimestampRange` are inclusive\. Fragments that begin before the start time but continue past it, or fragments that begin before the end time but continue past it, are included in the session\.

## Contents<a name="API_reader_HLSTimestampRange_Contents"></a>

 **EndTimestamp**   <a name="KinesisVideo-Type-reader_HLSTimestampRange-EndTimestamp"></a>
The end of the time stamp range for the requested media\. This value must be within three hours of the specified `StartTimestamp`, and it must be later than the `StartTimestamp` value\.  
If `FragmentSelectorType` for the request is `SERVER_TIMESTAMP`, this value must be in the past\.  
If the `HLSTimestampRange` value is specified, the `EndTimestamp` value is required\.  
This value is inclusive\. The `EndTimestamp` is compared to the \(starting\) time stamp of the fragment\. Fragments that start before the `EndTimestamp` value and continue past it are included in the session\.
Type: Timestamp  
Required: No

 **StartTimestamp**   <a name="KinesisVideo-Type-reader_HLSTimestampRange-StartTimestamp"></a>
The start of the time stamp range for the requested media\.  
If the `HLSTimestampRange` value is specified, the `StartTimestamp` value is required\.  
This value is inclusive\. Fragments that start before the `StartingTimestamp` and continue past it are included in the session\.
Type: Timestamp  
Required: No

## See Also<a name="API_reader_HLSTimestampRange_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesis-video-reader-data-2017-09-30/HLSTimestampRange) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesis-video-reader-data-2017-09-30/HLSTimestampRange) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesis-video-reader-data-2017-09-30/HLSTimestampRange) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/kinesis-video-reader-data-2017-09-30/HLSTimestampRange) 