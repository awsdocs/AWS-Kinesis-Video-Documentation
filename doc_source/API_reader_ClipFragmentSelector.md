# ClipFragmentSelector<a name="API_reader_ClipFragmentSelector"></a>

Describes the timestamp range and timestamp origin of a range of fragments\.

Fragments that have duplicate producer timestamps are deduplicated\. This means that if producers are producing a stream of fragments with producer timestamps that are approximately equal to the true clock time, the clip will contain all of the fragments within the requested timestamp range\. If some fragments are ingested within the same time range and very different points in time, only the oldest ingested collection of fragments are returned\.

## Contents<a name="API_reader_ClipFragmentSelector_Contents"></a>

 **FragmentSelectorType**   <a name="KinesisVideo-Type-reader_ClipFragmentSelector-FragmentSelectorType"></a>
The origin of the timestamps to use \(Server or Producer\)\.  
Type: String  
Valid Values:` PRODUCER_TIMESTAMP | SERVER_TIMESTAMP`   
Required: Yes

 **TimestampRange**   <a name="KinesisVideo-Type-reader_ClipFragmentSelector-TimestampRange"></a>
The range of timestamps to return\.  
Type: [ClipTimestampRange](API_reader_ClipTimestampRange.md) object  
Required: Yes

## See Also<a name="API_reader_ClipFragmentSelector_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesis-video-reader-data-2017-09-30/ClipFragmentSelector) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesis-video-reader-data-2017-09-30/ClipFragmentSelector) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesis-video-reader-data-2017-09-30/ClipFragmentSelector) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesis-video-reader-data-2017-09-30/ClipFragmentSelector) 