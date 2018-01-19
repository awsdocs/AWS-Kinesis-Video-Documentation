# Fragment<a name="API_reader_Fragment"></a>

Represents a segment of video or other time\-delimited data\.

## Contents<a name="API_reader_Fragment_Contents"></a>

 **FragmentLengthInMilliseconds**   
The playback duration or other time value associated with the fragment\.  
Type: Long  
Required: No

 **FragmentNumber**   
The index value of the fragment\.  
Type: String  
Length Constraints: Minimum length of 1\.  
Required: No

 **FragmentSizeInBytes**   
The total fragment size, including information about the fragment and contained media data\.  
Type: Long  
Required: No

 **ProducerTimestamp**   
The time stamp from the producer corresponding to the fragment\.  
Type: Timestamp  
Required: No

 **ServerTimestamp**   
The time stamp from the AWS server corresponding to the fragment\.  
Type: Timestamp  
Required: No

## See Also<a name="API_reader_Fragment_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/kinesis-video-reader-data-2017-09-30/Fragment) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/kinesis-video-reader-data-2017-09-30/Fragment) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/kinesis-video-reader-data-2017-09-30/Fragment) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/kinesis-video-reader-data-2017-09-30/Fragment) 