# FragmentSelector<a name="API_reader_FragmentSelector"></a>

Describes the timestamp range and timestamp origin of a range of fragments\.

Only fragments with a start timestamp greater than or equal to the given start time and less than or equal to the end time are returned\. For example, if a stream contains fragments with the following start timestamps: 
+ 00:00:00
+ 00:00:02
+ 00:00:04
+ 00:00:06

 A fragment selector range with a start time of 00:00:01 and end time of 00:00:04 would return the fragments with start times of 00:00:02 and 00:00:04\. 

## Contents<a name="API_reader_FragmentSelector_Contents"></a>

 **FragmentSelectorType**   <a name="KinesisVideo-Type-reader_FragmentSelector-FragmentSelectorType"></a>
The origin of the timestamps to use \(Server or Producer\)\.  
Type: String  
Valid Values:` PRODUCER_TIMESTAMP | SERVER_TIMESTAMP`   
Required: Yes

 **TimestampRange**   <a name="KinesisVideo-Type-reader_FragmentSelector-TimestampRange"></a>
The range of timestamps to return\.  
Type: [TimestampRange](API_reader_TimestampRange.md) object  
Required: Yes

## See Also<a name="API_reader_FragmentSelector_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesis-video-reader-data-2017-09-30/FragmentSelector) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesis-video-reader-data-2017-09-30/FragmentSelector) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesis-video-reader-data-2017-09-30/FragmentSelector) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesis-video-reader-data-2017-09-30/FragmentSelector) 