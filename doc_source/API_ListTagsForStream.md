# ListTagsForStream<a name="API_ListTagsForStream"></a>

Returns a list of tags associated with the specified stream\.

In the request, you must specify either the `StreamName` or the `StreamARN`\. 

## Request Syntax<a name="API_ListTagsForStream_RequestSyntax"></a>

```
POST /listTagsForStream HTTP/1.1
Content-type: application/json

{
   "NextToken": "string",
   "StreamARN": "string",
   "StreamName": "string"
}
```

## URI Request Parameters<a name="API_ListTagsForStream_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_ListTagsForStream_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** NextToken **   
If you specify this parameter and the result of a `ListTagsForStream` call is truncated, the response includes a token that you can use in the next request to fetch the next batch of tags\.  
Type: String  
Length Constraints: Minimum length of 0\. Maximum length of 512\.  
Required: No

 ** StreamARN **   
The Amazon Resource Name \(ARN\) of the stream that you want to list tags for\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+`   
Required: No

 ** StreamName **   
The name of the stream that you want to list tags for\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: No

## Response Syntax<a name="API_ListTagsForStream_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-type: application/json

{
   "NextToken": "string",
   "Tags": { 
      "string" : "string" 
   }
}
```

## Response Elements<a name="API_ListTagsForStream_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** NextToken **   
If you specify this parameter and the result of a `ListTags` call is truncated, the response includes a token that you can use in the next request to fetch the next set of tags\.  
Type: String  
Length Constraints: Minimum length of 0\. Maximum length of 512\.

 ** Tags **   
A map of tag keys and values associated with the specified stream\.  
Type: String to string map  
Key Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Value Length Constraints: Minimum length of 0\. Maximum length of 256\.

## Errors<a name="API_ListTagsForStream_Errors"></a>

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

## See Also<a name="API_ListTagsForStream_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/kinesisvideo-2017-09-30/ListTagsForStream) 

+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/kinesisvideo-2017-09-30/ListTagsForStream) 

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/ListTagsForStream) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/ListTagsForStream) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/ListTagsForStream) 

+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesisvideo-2017-09-30/ListTagsForStream) 

+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/kinesisvideo-2017-09-30/ListTagsForStream) 

+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/kinesisvideo-2017-09-30/ListTagsForStream) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/kinesisvideo-2017-09-30/ListTagsForStream) 