# Fragment<a name="API_reader_Fragment"></a>

Represents a segment of video or other time\-delimited data\.

## Contents<a name="API_reader_Fragment_Contents"></a>

 **FragmentLengthInMilliseconds**   <a name="KinesisVideo-Type-reader_Fragment-FragmentLengthInMilliseconds"></a>
The playback duration or other time value associated with the fragment\.  
Type: Long  
Required: No

 **FragmentNumber**   <a name="KinesisVideo-Type-reader_Fragment-FragmentNumber"></a>
The unique identifier of the fragment\. This value monotonically increases based on the ingestion order\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `^[0-9]+$`   
Required: No

 **FragmentSizeInBytes**   <a name="KinesisVideo-Type-reader_Fragment-FragmentSizeInBytes"></a>
The total fragment size, including information about the fragment and contained media data\.  
Type: Long  
Required: No

 **ProducerTimestamp**   <a name="KinesisVideo-Type-reader_Fragment-ProducerTimestamp"></a>
The timestamp from the producer corresponding to the fragment\.  
Type: Timestamp  
Required: No

 **ServerTimestamp**   <a name="KinesisVideo-Type-reader_Fragment-ServerTimestamp"></a>
The timestamp from the AWS server corresponding to the fragment\.  
Type: Timestamp  
Required: No

## See Also<a name="API_reader_Fragment_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesis-video-reader-data-2017-09-30/Fragment) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesis-video-reader-data-2017-09-30/Fragment) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesis-video-reader-data-2017-09-30/Fragment) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesis-video-reader-data-2017-09-30/Fragment) 