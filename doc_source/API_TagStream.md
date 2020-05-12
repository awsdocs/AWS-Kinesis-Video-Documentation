# TagStream<a name="API_TagStream"></a>

Adds one or more tags to a stream\. A *tag* is a key\-value pair \(the value is optional\) that you can define and assign to AWS resources\. If you specify a tag that already exists, the tag value is replaced with the value that you specify in the request\. For more information, see [Using Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) in the *AWS Billing and Cost Management User Guide*\. 

You must provide either the `StreamName` or the `StreamARN`\.

This operation requires permission for the `KinesisVideo:TagStream` action\.

Kinesis video streams support up to 50 tags\.

## Request Syntax<a name="API_TagStream_RequestSyntax"></a>

```
POST /tagStream HTTP/1.1
Content-type: application/json

{
   "[StreamARN](#KinesisVideo-TagStream-request-StreamARN)": "string",
   "[StreamName](#KinesisVideo-TagStream-request-StreamName)": "string",
   "[Tags](#KinesisVideo-TagStream-request-Tags)": { 
      "string" : "string" 
   }
}
```

## URI Request Parameters<a name="API_TagStream_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_TagStream_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [StreamARN](#API_TagStream_RequestSyntax) **   <a name="KinesisVideo-TagStream-request-StreamARN"></a>
The Amazon Resource Name \(ARN\) of the resource that you want to add the tag or tags to\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+`   
Required: No

 ** [StreamName](#API_TagStream_RequestSyntax) **   <a name="KinesisVideo-TagStream-request-StreamName"></a>
The name of the stream that you want to add the tag or tags to\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: No

 ** [Tags](#API_TagStream_RequestSyntax) **   <a name="KinesisVideo-TagStream-request-Tags"></a>
A list of tags to associate with the specified stream\. Each tag is a key\-value pair \(the value is optional\)\.  
Type: String to string map  
Key Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Key Pattern: `^([\p{L}\p{Z}\p{N}_.:/=+\-@]*)$`   
Value Length Constraints: Minimum length of 0\. Maximum length of 256\.  
Value Pattern: `[\p{L}\p{Z}\p{N}_.:/=+\-@]*`   
Required: Yes

## Response Syntax<a name="API_TagStream_ResponseSyntax"></a>

```
HTTP/1.1 200
```

## Response Elements<a name="API_TagStream_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_TagStream_Errors"></a>

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

 **TagsPerResourceExceededLimitException**   
You have exceeded the limit of tags that you can associate with the resource\. Kinesis video streams support up to 50 tags\.   
HTTP Status Code: 400

## See Also<a name="API_TagStream_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesisvideo-2017-09-30/TagStream) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesisvideo-2017-09-30/TagStream) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/TagStream) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/TagStream) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/TagStream) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesisvideo-2017-09-30/TagStream) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesisvideo-2017-09-30/TagStream) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesisvideo-2017-09-30/TagStream) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/TagStream) 