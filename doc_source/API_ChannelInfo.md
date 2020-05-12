# ChannelInfo<a name="API_ChannelInfo"></a>

A structure that encapsulates a signaling channel's metadata and properties\.

## Contents<a name="API_ChannelInfo_Contents"></a>

 **ChannelARN**   <a name="KinesisVideo-Type-ChannelInfo-ChannelARN"></a>
The Amazon Resource Name \(ARN\) of the signaling channel\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+`   
Required: No

 **ChannelName**   <a name="KinesisVideo-Type-ChannelInfo-ChannelName"></a>
The name of the signaling channel\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: No

 **ChannelStatus**   <a name="KinesisVideo-Type-ChannelInfo-ChannelStatus"></a>
Current status of the signaling channel\.  
Type: String  
Valid Values:` CREATING | ACTIVE | UPDATING | DELETING`   
Required: No

 **ChannelType**   <a name="KinesisVideo-Type-ChannelInfo-ChannelType"></a>
The type of the signaling channel\.  
Type: String  
Valid Values:` SINGLE_MASTER`   
Required: No

 **CreationTime**   <a name="KinesisVideo-Type-ChannelInfo-CreationTime"></a>
The time at which the signaling channel was created\.  
Type: Timestamp  
Required: No

 **SingleMasterConfiguration**   <a name="KinesisVideo-Type-ChannelInfo-SingleMasterConfiguration"></a>
A structure that contains the configuration for the `SINGLE_MASTER` channel type\.  
Type: [SingleMasterConfiguration](API_SingleMasterConfiguration.md) object  
Required: No

 **Version**   <a name="KinesisVideo-Type-ChannelInfo-Version"></a>
The current version of the signaling channel\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `[a-zA-Z0-9]+`   
Required: No

## See Also<a name="API_ChannelInfo_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/ChannelInfo) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/ChannelInfo) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/ChannelInfo) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/ChannelInfo) 