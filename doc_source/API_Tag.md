# Tag<a name="API_Tag"></a>

A key and value pair that is associated with the specified signaling channel\.

## Contents<a name="API_Tag_Contents"></a>

 **Key**   <a name="KinesisVideo-Type-Tag-Key"></a>
The key of the tag that is associated with the specified signaling channel\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `^([\p{L}\p{Z}\p{N}_.:/=+\-@]*)$`   
Required: Yes

 **Value**   <a name="KinesisVideo-Type-Tag-Value"></a>
The value of the tag that is associated with the specified signaling channel\.  
Type: String  
Length Constraints: Minimum length of 0\. Maximum length of 256\.  
Pattern: `[\p{L}\p{Z}\p{N}_.:/=+\-@]*`   
Required: Yes

## See Also<a name="API_Tag_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/Tag) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/Tag) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/Tag) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/Tag) 