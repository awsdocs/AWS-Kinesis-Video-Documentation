# StartSelector<a name="API_dataplane_StartSelector"></a>

Identifies the chunk on the Kinesis video stream where you want the `GetMedia` API to start returning media data\. You have the following options to identify the starting chunk: 
+ Choose the latest \(or oldest\) chunk\.
+ Identify a specific chunk\. You can identify a specific chunk either by providing a fragment number or timestamp \(server or producer\)\. 
+ Each chunk's metadata includes a continuation token as a Matroska \(MKV\) tag \(`AWS_KINESISVIDEO_CONTINUATION_TOKEN`\)\. If your previous `GetMedia` request terminated, you can use this tag value in your next `GetMedia` request\. The API then starts returning chunks starting where the last API ended\.

## Contents<a name="API_dataplane_StartSelector_Contents"></a>

 **AfterFragmentNumber**   <a name="KinesisVideo-Type-dataplane_StartSelector-AfterFragmentNumber"></a>
Specifies the fragment number from where you want the `GetMedia` API to start returning the fragments\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `^[0-9]+$`   
Required: No

 **ContinuationToken**   <a name="KinesisVideo-Type-dataplane_StartSelector-ContinuationToken"></a>
Continuation token that Kinesis Video Streams returned in the previous `GetMedia` response\. The `GetMedia` API then starts with the chunk identified by the continuation token\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `^[a-zA-Z0-9_\.\-]+$`   
Required: No

 **StartSelectorType**   <a name="KinesisVideo-Type-dataplane_StartSelector-StartSelectorType"></a>
Identifies the fragment on the Kinesis video stream where you want to start getting the data from\.  
+ NOW \- Start with the latest chunk on the stream\.
+ EARLIEST \- Start with earliest available chunk on the stream\.
+ FRAGMENT\_NUMBER \- Start with the chunk after a specific fragment\. You must also specify the `AfterFragmentNumber` parameter\.
+ PRODUCER\_TIMESTAMP or SERVER\_TIMESTAMP \- Start with the chunk containing a fragment with the specified producer or server timestamp\. You specify the timestamp by adding `StartTimestamp`\.
+  CONTINUATION\_TOKEN \- Read using the specified continuation token\. 
If you choose the NOW, EARLIEST, or CONTINUATION\_TOKEN as the `startSelectorType`, you don't provide any additional information in the `startSelector`\.
Type: String  
Valid Values:` FRAGMENT_NUMBER | SERVER_TIMESTAMP | PRODUCER_TIMESTAMP | NOW | EARLIEST | CONTINUATION_TOKEN`   
Required: Yes

 **StartTimestamp**   <a name="KinesisVideo-Type-dataplane_StartSelector-StartTimestamp"></a>
A timestamp value\. This value is required if you choose the PRODUCER\_TIMESTAMP or the SERVER\_TIMESTAMP as the `startSelectorType`\. The `GetMedia` API then starts with the chunk containing the fragment that has the specified timestamp\.  
Type: Timestamp  
Required: No

## See Also<a name="API_dataplane_StartSelector_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesis-video-data-2017-09-30/StartSelector) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesis-video-data-2017-09-30/StartSelector) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesis-video-data-2017-09-30/StartSelector) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesis-video-data-2017-09-30/StartSelector) 