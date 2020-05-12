# StreamInfo<a name="API_StreamInfo"></a>

An object describing a Kinesis video stream\.

## Contents<a name="API_StreamInfo_Contents"></a>

 **CreationTime**   <a name="KinesisVideo-Type-StreamInfo-CreationTime"></a>
A time stamp that indicates when the stream was created\.  
Type: Timestamp  
Required: No

 **DataRetentionInHours**   <a name="KinesisVideo-Type-StreamInfo-DataRetentionInHours"></a>
How long the stream retains data, in hours\.  
Type: Integer  
Valid Range: Minimum value of 0\.  
Required: No

 **DeviceName**   <a name="KinesisVideo-Type-StreamInfo-DeviceName"></a>
The name of the device that is associated with the stream\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: No

 **KmsKeyId**   <a name="KinesisVideo-Type-StreamInfo-KmsKeyId"></a>
The ID of the AWS Key Management Service \(AWS KMS\) key that Kinesis Video Streams uses to encrypt data on the stream\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 2048\.  
Pattern: `.+`   
Required: No

 **MediaType**   <a name="KinesisVideo-Type-StreamInfo-MediaType"></a>
The `MediaType` of the stream\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `[\w\-\.\+]+/[\w\-\.\+]+(,[\w\-\.\+]+/[\w\-\.\+]+)*`   
Required: No

 **Status**   <a name="KinesisVideo-Type-StreamInfo-Status"></a>
The status of the stream\.  
Type: String  
Valid Values:` CREATING | ACTIVE | UPDATING | DELETING`   
Required: No

 **StreamARN**   <a name="KinesisVideo-Type-StreamInfo-StreamARN"></a>
The Amazon Resource Name \(ARN\) of the stream\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+`   
Required: No

 **StreamName**   <a name="KinesisVideo-Type-StreamInfo-StreamName"></a>
The name of the stream\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: No

 **Version**   <a name="KinesisVideo-Type-StreamInfo-Version"></a>
The version of the stream\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `[a-zA-Z0-9]+`   
Required: No

## See Also<a name="API_StreamInfo_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/StreamInfo) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/StreamInfo) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/StreamInfo) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/StreamInfo) 