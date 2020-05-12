# StreamNameCondition<a name="API_StreamNameCondition"></a>

Specifies the condition that streams must satisfy to be returned when you list streams \(see the `ListStreams` API\)\. A condition has a comparison operation and a value\. Currently, you can specify only the `BEGINS_WITH` operator, which finds streams whose names start with a given prefix\. 

## Contents<a name="API_StreamNameCondition_Contents"></a>

 **ComparisonOperator**   <a name="KinesisVideo-Type-StreamNameCondition-ComparisonOperator"></a>
A comparison operator\. Currently, you can specify only the `BEGINS_WITH` operator, which finds streams whose names start with a given prefix\.  
Type: String  
Valid Values:` BEGINS_WITH`   
Required: No

 **ComparisonValue**   <a name="KinesisVideo-Type-StreamNameCondition-ComparisonValue"></a>
A value to compare\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: No

## See Also<a name="API_StreamNameCondition_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/StreamNameCondition) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/StreamNameCondition) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/StreamNameCondition) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/StreamNameCondition) 