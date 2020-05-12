# TagResource<a name="API_TagResource"></a>

Adds one or more tags to a signaling channel\. A *tag* is a key\-value pair \(the value is optional\) that you can define and assign to AWS resources\. If you specify a tag that already exists, the tag value is replaced with the value that you specify in the request\. For more information, see [Using Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) in the *AWS Billing and Cost Management User Guide*\.

## Request Syntax<a name="API_TagResource_RequestSyntax"></a>

```
POST /TagResource HTTP/1.1
Content-type: application/json

{
   "[ResourceARN](#KinesisVideo-TagResource-request-ResourceARN)": "string",
   "[Tags](#KinesisVideo-TagResource-request-Tags)": [ 
      { 
         "[Key](API_Tag.md#KinesisVideo-Type-Tag-Key)": "string",
         "[Value](API_Tag.md#KinesisVideo-Type-Tag-Value)": "string"
      }
   ]
}
```

## URI Request Parameters<a name="API_TagResource_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_TagResource_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [ResourceARN](#API_TagResource_RequestSyntax) **   <a name="KinesisVideo-TagResource-request-ResourceARN"></a>
The Amazon Resource Name \(ARN\) of the signaling channel to which you want to add tags\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+`   
Required: Yes

 ** [Tags](#API_TagResource_RequestSyntax) **   <a name="KinesisVideo-TagResource-request-Tags"></a>
A list of tags to associate with the specified signaling channel\. Each tag is a key\-value pair\.  
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 50 items\.  
Required: Yes

## Response Syntax<a name="API_TagResource_ResponseSyntax"></a>

```
HTTP/1.1 200
```

## Response Elements<a name="API_TagResource_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_TagResource_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **AccessDeniedException**   
You do not have required permissions to perform this operation\.  
HTTP Status Code: 401

 **ClientLimitExceededException**   
Kinesis Video Streams has throttled the request because you have exceeded the limit of allowed client calls\. Try making the call later\.  
HTTP Status Code: 400

 **InvalidArgumentException**   
The value for this input parameter is invalid\.  
HTTP Status Code: 400

 **ResourceNotFoundException**   
Amazon Kinesis Video Streams can't find the stream that you specified\.  
HTTP Status Code: 404

 **TagsPerResourceExceededLimitException**   
You have exceeded the limit of tags that you can associate with the resource\. Kinesis video streams support up to 50 tags\.   
HTTP Status Code: 400

## See Also<a name="API_TagResource_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesisvideo-2017-09-30/TagResource) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesisvideo-2017-09-30/TagResource) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/TagResource) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/TagResource) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/TagResource) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesisvideo-2017-09-30/TagResource) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesisvideo-2017-09-30/TagResource) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesisvideo-2017-09-30/TagResource) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/TagResource) 