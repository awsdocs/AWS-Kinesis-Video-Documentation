# FragmentSelector<a name="API_reader_FragmentSelector"></a>

Describes the time stamp range and time stamp origin of a range of fragments\.

## Contents<a name="API_reader_FragmentSelector_Contents"></a>

 **FragmentSelectorType**   <a name="KinesisVideo-Type-reader_FragmentSelector-FragmentSelectorType"></a>
The origin of the time stamps to use \(Server or Producer\)\.  
Type: String  
Valid Values:` PRODUCER_TIMESTAMP | SERVER_TIMESTAMP`   
Required: Yes

 **TimestampRange**   <a name="KinesisVideo-Type-reader_FragmentSelector-TimestampRange"></a>
The range of time stamps to return\.  
Type: [TimestampRange](API_reader_TimestampRange.md) object  
Required: Yes

## See Also<a name="API_reader_FragmentSelector_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/kinesis-video-reader-data-2017-09-30/FragmentSelector) 
+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/kinesis-video-reader-data-2017-09-30/FragmentSelector) 
+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/kinesis-video-reader-data-2017-09-30/FragmentSelector) 
+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/kinesis-video-reader-data-2017-09-30/FragmentSelector) 