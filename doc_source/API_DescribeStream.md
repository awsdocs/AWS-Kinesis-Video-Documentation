# DescribeStream<a name="API_DescribeStream"></a>

Returns the most current information about the specified stream\. You must specify either the `StreamName` or the `StreamARN`\. 

## Request Syntax<a name="API_DescribeStream_RequestSyntax"></a>

```
POST /describeStream HTTP/1.1
Content-type: application/json

{
   "[StreamARN](#KinesisVideo-DescribeStream-request-StreamARN)": "string",
   "[StreamName](#KinesisVideo-DescribeStream-request-StreamName)": "string"
}
```

## URI Request Parameters<a name="API_DescribeStream_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_DescribeStream_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [StreamARN](#API_DescribeStream_RequestSyntax) **   <a name="KinesisVideo-DescribeStream-request-StreamARN"></a>
The Amazon Resource Name \(ARN\) of the stream\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+`   
Required: No

 ** [StreamName](#API_DescribeStream_RequestSyntax) **   <a name="KinesisVideo-DescribeStream-request-StreamName"></a>
The name of the stream\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: No

## Response Syntax<a name="API_DescribeStream_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-type: application/json

{
   "[StreamInfo](#KinesisVideo-DescribeStream-response-StreamInfo)": { 
      "[CreationTime](API_StreamInfo.md#KinesisVideo-Type-StreamInfo-CreationTime)": number,
      "[DataRetentionInHours](API_StreamInfo.md#KinesisVideo-Type-StreamInfo-DataRetentionInHours)": number,
      "[DeviceName](API_StreamInfo.md#KinesisVideo-Type-StreamInfo-DeviceName)": "string",
      "[KmsKeyId](API_StreamInfo.md#KinesisVideo-Type-StreamInfo-KmsKeyId)": "string",
      "[MediaType](API_StreamInfo.md#KinesisVideo-Type-StreamInfo-MediaType)": "string",
      "[Status](API_StreamInfo.md#KinesisVideo-Type-StreamInfo-Status)": "string",
      "[StreamARN](API_StreamInfo.md#KinesisVideo-Type-StreamInfo-StreamARN)": "string",
      "[StreamName](API_StreamInfo.md#KinesisVideo-Type-StreamInfo-StreamName)": "string",
      "[Version](API_StreamInfo.md#KinesisVideo-Type-StreamInfo-Version)": "string"
   }
}
```

## Response Elements<a name="API_DescribeStream_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [StreamInfo](#API_DescribeStream_ResponseSyntax) **   <a name="KinesisVideo-DescribeStream-response-StreamInfo"></a>
An object that describes the stream\.  
Type: [StreamInfo](API_StreamInfo.md) object

## Errors<a name="API_DescribeStream_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ClientLimitExceededException**   
Kinesis Video Streams has throttled the request because you have exceeded the limit of allowed client calls\. Try making the call later\.  
HTTP Status Code: 400

 **InvalidArgumentException**   
The value for this input parameter is invalid\.  
HTTP Status Code: 400

 **NotAuthorizedException**   
The caller is not authorized to perform this operation\.  
HTTP Status Code: 401

 **ResourceNotFoundException**   
Amazon Kinesis Video Streams can't find the stream that you specified\.  
HTTP Status Code: 404

## See Also<a name="API_DescribeStream_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesisvideo-2017-09-30/DescribeStream) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesisvideo-2017-09-30/DescribeStream) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/DescribeStream) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/DescribeStream) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/DescribeStream) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesisvideo-2017-09-30/DescribeStream) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesisvideo-2017-09-30/DescribeStream) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesisvideo-2017-09-30/DescribeStream) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/DescribeStream) 