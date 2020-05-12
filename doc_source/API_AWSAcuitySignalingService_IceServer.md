# IceServer<a name="API_AWSAcuitySignalingService_IceServer"></a>

A structure for the ICE server connection data\.

## Contents<a name="API_AWSAcuitySignalingService_IceServer_Contents"></a>

 **Password**   <a name="KinesisVideo-Type-AWSAcuitySignalingService_IceServer-Password"></a>
A password to login to the ICE server\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: No

 **Ttl**   <a name="KinesisVideo-Type-AWSAcuitySignalingService_IceServer-Ttl"></a>
The period of time, in seconds, during which the user name and password are valid\.  
Type: Integer  
Valid Range: Minimum value of 30\. Maximum value of 86400\.  
Required: No

 **Uris**   <a name="KinesisVideo-Type-AWSAcuitySignalingService_IceServer-Uris"></a>
An array of URIs, in the form specified in the [I\-D\.petithuguenin\-behave\-turn\-uris](https://tools.ietf.org/html/draft-petithuguenin-behave-turn-uris-03) spec\. These URIs provide the different addresses and/or protocols that can be used to reach the TURN server\.  
Type: Array of strings  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Required: No

 **Username**   <a name="KinesisVideo-Type-AWSAcuitySignalingService_IceServer-Username"></a>
A user name to login to the ICE server\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: No

## See Also<a name="API_AWSAcuitySignalingService_IceServer_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesis-video-signaling-2019-12-04/IceServer) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesis-video-signaling-2019-12-04/IceServer) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesis-video-signaling-2019-12-04/IceServer) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesis-video-signaling-2019-12-04/IceServer) 