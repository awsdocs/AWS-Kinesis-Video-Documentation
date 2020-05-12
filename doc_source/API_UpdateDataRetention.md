# UpdateDataRetention<a name="API_UpdateDataRetention"></a>

 Increases or decreases the stream's data retention period by the value that you specify\. To indicate whether you want to increase or decrease the data retention period, specify the `Operation` parameter in the request body\. In the request, you must specify either the `StreamName` or the `StreamARN`\. 

**Note**  
The retention period that you specify replaces the current value\.

This operation requires permission for the `KinesisVideo:UpdateDataRetention` action\.

Changing the data retention period affects the data in the stream as follows:
+ If the data retention period is increased, existing data is retained for the new retention period\. For example, if the data retention period is increased from one hour to seven hours, all existing data is retained for seven hours\.
+ If the data retention period is decreased, existing data is retained for the new retention period\. For example, if the data retention period is decreased from seven hours to one hour, all existing data is retained for one hour, and any data older than one hour is deleted immediately\.

## Request Syntax<a name="API_UpdateDataRetention_RequestSyntax"></a>

```
POST /updateDataRetention HTTP/1.1
Content-type: application/json

{
   "[CurrentVersion](#KinesisVideo-UpdateDataRetention-request-CurrentVersion)": "string",
   "[DataRetentionChangeInHours](#KinesisVideo-UpdateDataRetention-request-DataRetentionChangeInHours)": number,
   "[Operation](#KinesisVideo-UpdateDataRetention-request-Operation)": "string",
   "[StreamARN](#KinesisVideo-UpdateDataRetention-request-StreamARN)": "string",
   "[StreamName](#KinesisVideo-UpdateDataRetention-request-StreamName)": "string"
}
```

## URI Request Parameters<a name="API_UpdateDataRetention_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_UpdateDataRetention_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [CurrentVersion](#API_UpdateDataRetention_RequestSyntax) **   <a name="KinesisVideo-UpdateDataRetention-request-CurrentVersion"></a>
The version of the stream whose retention period you want to change\. To get the version, call either the `DescribeStream` or the `ListStreams` API\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `[a-zA-Z0-9]+`   
Required: Yes

 ** [DataRetentionChangeInHours](#API_UpdateDataRetention_RequestSyntax) **   <a name="KinesisVideo-UpdateDataRetention-request-DataRetentionChangeInHours"></a>
The retention period, in hours\. The value you specify replaces the current value\. The maximum value for this parameter is 87600 \(ten years\)\.  
Type: Integer  
Valid Range: Minimum value of 1\.  
Required: Yes

 ** [Operation](#API_UpdateDataRetention_RequestSyntax) **   <a name="KinesisVideo-UpdateDataRetention-request-Operation"></a>
Indicates whether you want to increase or decrease the retention period\.  
Type: String  
Valid Values:` INCREASE_DATA_RETENTION | DECREASE_DATA_RETENTION`   
Required: Yes

 ** [StreamARN](#API_UpdateDataRetention_RequestSyntax) **   <a name="KinesisVideo-UpdateDataRetention-request-StreamARN"></a>
The Amazon Resource Name \(ARN\) of the stream whose retention period you want to change\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `arn:aws:kinesisvideo:[a-z0-9-]+:[0-9]+:[a-z]+/[a-zA-Z0-9_.-]+/[0-9]+`   
Required: No

 ** [StreamName](#API_UpdateDataRetention_RequestSyntax) **   <a name="KinesisVideo-UpdateDataRetention-request-StreamName"></a>
The name of the stream whose retention period you want to change\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: No

## Response Syntax<a name="API_UpdateDataRetention_ResponseSyntax"></a>

```
HTTP/1.1 200
```

## Response Elements<a name="API_UpdateDataRetention_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_UpdateDataRetention_Errors"></a>

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

 **ResourceInUseException**   
The signaling channel is currently not available for this operation\.  
HTTP Status Code: 400

 **ResourceNotFoundException**   
Amazon Kinesis Video Streams can't find the stream that you specified\.  
HTTP Status Code: 404

 **VersionMismatchException**   
The stream version that you specified is not the latest version\. To get the latest version, use the [DescribeStream](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_DescribeStream.html) API\.  
HTTP Status Code: 400

## See Also<a name="API_UpdateDataRetention_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesisvideo-2017-09-30/UpdateDataRetention) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesisvideo-2017-09-30/UpdateDataRetention) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesisvideo-2017-09-30/UpdateDataRetention) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesisvideo-2017-09-30/UpdateDataRetention) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesisvideo-2017-09-30/UpdateDataRetention) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesisvideo-2017-09-30/UpdateDataRetention) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesisvideo-2017-09-30/UpdateDataRetention) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesisvideo-2017-09-30/UpdateDataRetention) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesisvideo-2017-09-30/UpdateDataRetention) 