# ListFragments<a name="API_reader_ListFragments"></a>

Returns a list of [Fragment](API_reader_Fragment.md) objects from the specified stream and timestamp range within the archived data\.

Listing fragments is eventually consistent\. This means that even if the producer receives an acknowledgment that a fragment is persisted, the result might not be returned immediately from a request to `ListFragments`\. However, results are typically available in less than one second\.

**Note**  
You must first call the `GetDataEndpoint` API to get an endpoint\. Then send the `ListFragments` requests to this endpoint using the [\-\-endpoint\-url parameter](https://docs.aws.amazon.com/cli/latest/reference/)\. 

**Important**  
If an error is thrown after invoking a Kinesis Video Streams archived media API, in addition to the HTTP status code and the response body, it includes the following pieces of information:   
 `x-amz-ErrorType` HTTP header – contains a more specific error type in addition to what the HTTP status code provides\. 
 `x-amz-RequestId` HTTP header – if you want to report an issue to AWS, the support team can better diagnose the problem if given the Request Id\.
Both the HTTP status code and the ErrorType header can be utilized to make programmatic decisions about whether errors are retry\-able and under what conditions, as well as provide information on what actions the client programmer might need to take in order to successfully try again\.  
For more information, see the **Errors** section at the bottom of this topic, as well as [Common Errors](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/CommonErrors.html)\. 

## Request Syntax<a name="API_reader_ListFragments_RequestSyntax"></a>

```
POST /listFragments HTTP/1.1
Content-type: application/json

{
   "[FragmentSelector](#KinesisVideo-reader_ListFragments-request-FragmentSelector)": { 
      "[FragmentSelectorType](API_reader_FragmentSelector.md#KinesisVideo-Type-reader_FragmentSelector-FragmentSelectorType)": "string",
      "[TimestampRange](API_reader_FragmentSelector.md#KinesisVideo-Type-reader_FragmentSelector-TimestampRange)": { 
         "[EndTimestamp](API_reader_TimestampRange.md#KinesisVideo-Type-reader_TimestampRange-EndTimestamp)": number,
         "[StartTimestamp](API_reader_TimestampRange.md#KinesisVideo-Type-reader_TimestampRange-StartTimestamp)": number
      }
   },
   "[MaxResults](#KinesisVideo-reader_ListFragments-request-MaxResults)": number,
   "[NextToken](#KinesisVideo-reader_ListFragments-request-NextToken)": "string",
   "[StreamName](#KinesisVideo-reader_ListFragments-request-StreamName)": "string"
}
```

## URI Request Parameters<a name="API_reader_ListFragments_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_reader_ListFragments_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [FragmentSelector](#API_reader_ListFragments_RequestSyntax) **   <a name="KinesisVideo-reader_ListFragments-request-FragmentSelector"></a>
Describes the timestamp range and timestamp origin for the range of fragments to return\.  
Type: [FragmentSelector](API_reader_FragmentSelector.md) object  
Required: No

 ** [MaxResults](#API_reader_ListFragments_RequestSyntax) **   <a name="KinesisVideo-reader_ListFragments-request-MaxResults"></a>
The total number of fragments to return\. If the total number of fragments available is more than the value specified in `max-results`, then a [ListFragments:NextToken](#KinesisVideo-reader_ListFragments-response-NextToken) is provided in the output that you can use to resume pagination\.  
Type: Long  
Valid Range: Minimum value of 1\. Maximum value of 1000\.  
Required: No

 ** [NextToken](#API_reader_ListFragments_RequestSyntax) **   <a name="KinesisVideo-reader_ListFragments-request-NextToken"></a>
A token to specify where to start paginating\. This is the [ListFragments:NextToken](#KinesisVideo-reader_ListFragments-response-NextToken) from a previously truncated response\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 4096\.  
Pattern: `[a-zA-Z0-9+/]+={0,2}`   
Required: No

 ** [StreamName](#API_reader_ListFragments_RequestSyntax) **   <a name="KinesisVideo-reader_ListFragments-request-StreamName"></a>
The name of the stream from which to retrieve a fragment list\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: Yes

## Response Syntax<a name="API_reader_ListFragments_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-type: application/json

{
   "[Fragments](#KinesisVideo-reader_ListFragments-response-Fragments)": [ 
      { 
         "[FragmentLengthInMilliseconds](API_reader_Fragment.md#KinesisVideo-Type-reader_Fragment-FragmentLengthInMilliseconds)": number,
         "[FragmentNumber](API_reader_Fragment.md#KinesisVideo-Type-reader_Fragment-FragmentNumber)": "string",
         "[FragmentSizeInBytes](API_reader_Fragment.md#KinesisVideo-Type-reader_Fragment-FragmentSizeInBytes)": number,
         "[ProducerTimestamp](API_reader_Fragment.md#KinesisVideo-Type-reader_Fragment-ProducerTimestamp)": number,
         "[ServerTimestamp](API_reader_Fragment.md#KinesisVideo-Type-reader_Fragment-ServerTimestamp)": number
      }
   ],
   "[NextToken](#KinesisVideo-reader_ListFragments-response-NextToken)": "string"
}
```

## Response Elements<a name="API_reader_ListFragments_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [Fragments](#API_reader_ListFragments_ResponseSyntax) **   <a name="KinesisVideo-reader_ListFragments-response-Fragments"></a>
A list of archived [Fragment](API_reader_Fragment.md) objects from the stream that meet the selector criteria\. Results are in no specific order, even across pages\.  
Type: Array of [Fragment](API_reader_Fragment.md) objects

 ** [NextToken](#API_reader_ListFragments_ResponseSyntax) **   <a name="KinesisVideo-reader_ListFragments-response-NextToken"></a>
If the returned list is truncated, the operation returns this token to use to retrieve the next page of results\. This value is `null` when there are no more results to return\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 4096\.  
Pattern: `[a-zA-Z0-9+/]+={0,2}` 

## Errors<a name="API_reader_ListFragments_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ClientLimitExceededException**   
Kinesis Video Streams has throttled the request because you have exceeded the limit of allowed client calls\. Try making the call later\.  
HTTP Status Code: 400

 **InvalidArgumentException**   
A specified parameter exceeds its restrictions, is not supported, or can't be used\.  
HTTP Status Code: 400

 **NotAuthorizedException**   
Status Code: 403, The caller is not authorized to perform an operation on the given stream, or the token has expired\.  
HTTP Status Code: 401

 **ResourceNotFoundException**   
 `GetMedia` throws this error when Kinesis Video Streams can't find the stream that you specified\.  
 `GetHLSStreamingSessionURL` and `GetDASHStreamingSessionURL` throw this error if a session with a `PlaybackMode` of `ON_DEMAND` or `LIVE_REPLAY`is requested for a stream that has no fragments within the requested time range, or if a session with a `PlaybackMode` of `LIVE` is requested for a stream that has no fragments within the last 30 seconds\.  
HTTP Status Code: 404

## See Also<a name="API_reader_ListFragments_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/kinesis-video-reader-data-2017-09-30/ListFragments) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/kinesis-video-reader-data-2017-09-30/ListFragments) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/kinesis-video-reader-data-2017-09-30/ListFragments) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/kinesis-video-reader-data-2017-09-30/ListFragments) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/kinesis-video-reader-data-2017-09-30/ListFragments) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesis-video-reader-data-2017-09-30/ListFragments) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/kinesis-video-reader-data-2017-09-30/ListFragments) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/kinesis-video-reader-data-2017-09-30/ListFragments) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/kinesis-video-reader-data-2017-09-30/ListFragments) 