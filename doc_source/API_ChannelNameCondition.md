# ChannelNameCondition<a name="API_ChannelNameCondition"></a>

An optional input parameter for the `ListSignalingChannels` API\. When this parameter is specified while invoking `ListSignalingChannels`, the API returns only the channels that satisfy a condition specified in `ChannelNameCondition`\.

## Contents<a name="API_ChannelNameCondition_Contents"></a>

 **ComparisonOperator**   <a name="KinesisVideo-Type-ChannelNameCondition-ComparisonOperator"></a>
A comparison operator\. Currently, you can only specify the `BEGINS_WITH` operator, which finds signaling channels whose names begin with a given prefix\.  
Type: String  
Valid Values:` BEGINS_WITH`   
Required: No

 **ComparisonValue**   <a name="KinesisVideo-Type-ChannelNameCondition-ComparisonValue"></a>
A value to compare\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: No

## See Also<a name="API_ChannelNameCondition_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/ChannelNameCondition) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/ChannelNameCondition) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/ChannelNameCondition) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/ChannelNameCondition) 