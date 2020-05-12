# UntagStream<a name="API_UntagStream"></a>

Removes one or more tags from a stream\. In the request, specify only a tag key or keys; don't specify the value\. If you specify a tag key that does not exist, it's ignored\.

In the request, you must provide the `StreamName` or `StreamARN`\.

## Request Syntax<a name="API_UntagStream_RequestSyntax"></a>

```
POST /untagStream HTTP/1.1
Content-type: application/json

{
   "[StreamARN](#KinesisVideo-UntagStream-request-StreamARN)": "string",
   "[StreamName](#KinesisVideo-UntagStream-request-StreamName)": "string",
   "[TagKeyList](#KinesisVideo-UntagStream-request-TagKeyList)": [ "string" ]
}
```

## URI Request Parameters<a name="API_UntagStream_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_UntagStream_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [StreamARN](#API_UntagStream_RequestSyntax) **   <a name="KinesisVideo-UntagStream-request-StreamARN"></a>
The Amazon Resource Name \(ARN\) of the stream that you want to remove tags from\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+`   
Required: No

 ** [StreamName](#API_UntagStream_RequestSyntax) **   <a name="KinesisVideo-UntagStream-request-StreamName"></a>
The name of the stream that you want to remove tags from\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: No

 ** [TagKeyList](#API_UntagStream_RequestSyntax) **   <a name="KinesisVideo-UntagStream-request-TagKeyList"></a>
A list of the keys of the tags that you want to remove\.  
Type: Array of strings  
Array Members: Minimum number of 1 item\. Maximum number of 50 items\.  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `^([\p{L}\p{Z}\p{N}_.:/=+\-@]*)$`   
Required: Yes

## Response Syntax<a name="API_UntagStream_ResponseSyntax"></a>

```
HTTP/1.1 200
```

## Response Elements<a name="API_UntagStream_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_UntagStream_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ClientLimitExceededException**   
Kinesis Video Streams has throttled the request because you have exceeded the limit of allowed client calls\. Try making the call later\.  
HTTP Status Code: 400

 **InvalidArgumentException**   
The value for this input parameter is invalid\.  
HTTP Status Code: 400

 **InvalidResourceFormatException**   
The format of the `StreamARN` is invalid\.  
HTTP Status Code: 400

 **NotAuthorizedException**   
The caller is not authorized to perform this operation\.  
HTTP Status Code: 401

 **ResourceNotFoundException**   
Amazon Kinesis Video Streams can't find the stream that you specified\.  
HTTP Status Code: 404

## See Also<a name="API_UntagStream_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesisvideo-2017-09-30/UntagStream) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesisvideo-2017-09-30/UntagStream) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/UntagStream) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/UntagStream) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/UntagStream) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesisvideo-2017-09-30/UntagStream) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesisvideo-2017-09-30/UntagStream) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesisvideo-2017-09-30/UntagStream) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/UntagStream) 