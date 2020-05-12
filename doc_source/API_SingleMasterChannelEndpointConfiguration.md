# SingleMasterChannelEndpointConfiguration<a name="API_SingleMasterChannelEndpointConfiguration"></a>

An object that contains the endpoint configuration for the `SINGLE_MASTER` channel type\. 

## Contents<a name="API_SingleMasterChannelEndpointConfiguration_Contents"></a>

 **Protocols**   <a name="KinesisVideo-Type-SingleMasterChannelEndpointConfiguration-Protocols"></a>
This property is used to determine the nature of communication over this `SINGLE_MASTER` signaling channel\. If `WSS` is specified, this API returns a websocket endpoint\. If `HTTPS` is specified, this API returns an `HTTPS` endpoint\.  
Type: Array of strings  
Array Members: Minimum number of 1 item\. Maximum number of 5 items\.  
Valid Values:` WSS | HTTPS`   
Required: No

 **Role**   <a name="KinesisVideo-Type-SingleMasterChannelEndpointConfiguration-Role"></a>
This property is used to determine messaging permissions in this `SINGLE_MASTER` signaling channel\. If `MASTER` is specified, this API returns an endpoint that a client can use to receive offers from and send answers to any of the viewers on this signaling channel\. If `VIEWER` is specified, this API returns an endpoint that a client can use only to send offers to another `MASTER` client on this signaling channel\.   
Type: String  
Valid Values:` MASTER | VIEWER`   
Required: No

## See Also<a name="API_SingleMasterChannelEndpointConfiguration_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/SingleMasterChannelEndpointConfiguration) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/SingleMasterChannelEndpointConfiguration) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/SingleMasterChannelEndpointConfiguration) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/SingleMasterChannelEndpointConfiguration) 