# Logging Kinesis Video Streams API Calls with AWS CloudTrail<a name="monitoring-cloudtrail"></a>

Amazon Kinesis Video Streams is integrated with AWS CloudTrail, which captures API calls made by or on behalf of Kinesis Video Streams\. CloudTrail then delivers the log files to the Amazon Simple Storage Service \(Amazon S3\) bucket that you specify\. You can make the API calls indirectly through the Kinesis Video Streams console, or directly by using the Kinesis Video Streams API\. You can use the information collected by CloudTrail to determine what request was made to Kinesis Video Streams, the source IP address from which the request was made, who made the request, when it was made, and so on\. To learn more about CloudTrail, including how to configure and enable it, see the *[AWS CloudTrail User Guide](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/)*\.

## Kinesis Video Streams and CloudTrail<a name="akvs-info-in-cloudtrail"></a>

CloudTrail logging is enabled by default\. Calls made to Kinesis Video Streams actions are tracked in log files\. Records for Kinesis Video Streams are written in a log file, together with records from any other AWS service that is enabled for CloudTrail logging\. CloudTrail determines when to create and write to a new file based on the specified time period and file size\.

The following actions are supported:
+ [CreateStream](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_CreateStream.html)
+ [DeleteStream](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_DeleteStream.html)
+ [DescribeStream](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_DescribeStream.html)
+ [GetDataEndpoint](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_GetDataEndpoint.html)
+ [ListStreams](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_ListStreams.html)
+ [ListTagsForStream](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_ListTagsForStream.html)
+ [TagStream](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_TagStream.html)
+ [UntagStream](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_UntagStream.html)
+ [UpdateDataRetention](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_UpdateDataRetention.html)
+ [UpdateStream](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_UpdateStream.html)

Each log entry contains information about who generated the request\. For example, if a request is made to create a stream \(CreateStream\), the user identity of the person or service that made the request is logged\. The user identity information helps you determine whether the request was made with root or IAM user credentials, with temporary security credentials for a role or federated user, or by another AWS service\. For more information, see the [userIdentity element](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/event_reference_user_identity.html) in the *AWS CloudTrail User Guide*\.

You can store your log files in your bucket for as long as you need to, but you can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. By default, your log files are encrypted with Amazon S3 server\-side encryption \(SSE\)\.

You can also aggregate Kinesis Video Streams log files from multiple AWS Regions and multiple AWS accounts into a single Amazon S3 bucket\. For more information, see [Aggregating CloudTrail Log Files to a Single Amazon S3 Bucket](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/aggregating_logs_top_level.html) in the *AWS CloudTrail User Guide*\.

You can have CloudTrail publish Amazon Simple Notification Service \(Amazon SNS\) notifications when new log files are delivered if you want to take quick action upon log file delivery\. For more information, see [Configuring Amazon SNS Notifications](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html) in the *AWS CloudTrail User Guide*\.

## Log File Entries for Kinesis Video Streams<a name="kinesis-log-entries"></a>

CloudTrail log files can contain one or more log entries, where each entry is made up of multiple JSON\-formatted events\. A log entry represents a single request from any source and includes information about the requested action, any parameters, the date and time of the action, and so on\. The log entries are not guaranteed to be in any particular order\. That is, this is not an ordered stack trace of API calls\.

The following is an example CloudTrail log entry\.

```
{
    "Records": [
        {
            "eventVersion": "1.05",
            "userIdentity": {
                "type": "IAMUser",
                "principalId": "EX_PRINCIPAL_ID",
                "arn": "arn:aws:iam::123456789012:user/Alice",
                "accountId": "123456789012",
                "accessKeyId": "EXAMPLE_KEY_ID",
                "userName": "Alice"
            },
            "eventTime": "2018-05-25T00:16:31Z",
            "eventSource": " kinesisvideo.amazonaws.com",
            "eventName": "CreateStream",
            "awsRegion": "us-east-1",
            "sourceIPAddress": "127.0.0.1",
            "userAgent": "aws-sdk-java/unknown-version Linux/x.xx",
            "requestParameters": {
                "streamName": "VideoStream",
                "dataRetentionInHours": 2,	
                "mediaType": "mediaType",
                "kmsKeyId": "arn:aws:kms::us-east-1:123456789012:alias",
		"deviceName": "my-device"
      		},
            "responseElements": {
		"streamARN":arn:aws:kinesisvideo:us-east-1:123456789012:stream/VideoStream/12345"
             },
            "requestID": "db6c59f8-c757-11e3-bc3b-57923b443c1c",
            "eventID": "b7acfcd0-6ca9-4ee1-a3d7-c4e8d420d99b"
        },
        {
            "eventVersion": "1.05",
            "userIdentity": {
                "type": "IAMUser",
                "principalId": "EX_PRINCIPAL_ID",
                "arn": "arn:aws:iam::123456789012:user/Alice",
                "accountId": "123456789012",
                "accessKeyId": "EXAMPLE_KEY_ID",
                "userName": "Alice"
            },
            "eventTime": "2018-05-25:17:06Z",
            "eventSource": " kinesisvideo.amazonaws.com",
            "eventName": "DeleteStream",
            "awsRegion": "us-east-1",
            "sourceIPAddress": "127.0.0.1",
            "userAgent": "aws-sdk-java/unknown-version Linux/x.xx",
            "requestParameters": {
                "streamARN": "arn:aws:kinesisvideo:us-east-1:012345678910:stream/VideoStream/12345",
                "currentVersion": "keqrjeqkj9"
             },
            "responseElements": null,
            "requestID": "f0944d86-c757-11e3-b4ae-25654b1d3136",
            "eventID": "0b2f1396-88af-4561-b16f-398f8eaea596"
        },
        {
            "eventVersion": "1.05",
            "userIdentity": {
                "type": "IAMUser",
                "principalId": "EX_PRINCIPAL_ID",
                "arn": "arn:aws:iam::123456789012:user/Alice",
                "accountId": "123456789012",
                "accessKeyId": "EXAMPLE_KEY_ID",
                "userName": "Alice"
            },
            "eventTime": "2014-04-19T00:15:02Z",
            "eventSource": " kinesisvideo.amazonaws.com",
            "eventName": "DescribeStream",
            "awsRegion": "us-east-1",
            "sourceIPAddress": "127.0.0.1",
            "userAgent": "aws-sdk-java/unknown-version Linux/x.xx",
            "requestParameters": {
                "streamName": "VideoStream"
             },
            "responseElements": null,
            "requestID": "a68541ca-c757-11e3-901b-cbcfe5b3677a",
            "eventID": "22a5fb8f-4e61-4bee-a8ad-3b72046b4c4d"
        },
        {
            "eventVersion": "1.05",
            "userIdentity": {
                "type": "IAMUser",
                "principalId": "EX_PRINCIPAL_ID",
                "arn": "arn:aws:iam::123456789012:user/Alice",
                "accountId": "123456789012",
                "accessKeyId": "EXAMPLE_KEY_ID",
                "userName": "Alice"
            },
            "eventTime": "2014-04-19T00:15:03Z",
            "eventSource": "kinesisvideo.amazonaws.com",
            "eventName": "GetDataEndpoint",
            "awsRegion": "us-east-1",
            "sourceIPAddress": "127.0.0.1",
            "userAgent": "aws-sdk-java/unknown-version Linux/x.xx",
            "requestParameters": {
                "streamName": "VideoStream",
                "aPIName": "LIST_FRAGMENTS"
"
            },
            "responseElements": null,
            "requestID": "a6e6e9cd-c757-11e3-901b-cbcfe5b3677a",
            "eventID": "dcd2126f-c8d2-4186-b32a-192dd48d7e33"
        },
        {
            "eventVersion": "1.05",
            "userIdentity": {
                "type": "IAMUser",
                "principalId": "EX_PRINCIPAL_ID",
                "arn": "arn:aws:iam::123456789012:user/Alice",
                "accountId": "123456789012",
                "accessKeyId": "EXAMPLE_KEY_ID",
                "userName": "Alice"
            },
            "eventTime": "2018-05-25T00:16:56Z",
            "eventSource": "kinesisvideo.amazonaws.com",
            "eventName": "ListStreams",
            "awsRegion": "us-east-1",
            "sourceIPAddress": "127.0.0.1",
            "userAgent": "aws-sdk-java/unknown-version Linux/x.xx",
            "requestParameters": {
                "maxResults": 100, 
                "streamNameCondition": {"comparisonValue":"MyVideoStream" comparisonOperator":"BEGINS_WITH"}}
            }, 
            "responseElements": null,
            "requestID": "e9f9c8eb-c757-11e3-bf1d-6948db3cd570",
            "eventID": "77cf0d06-ce90-42da-9576-71986fec411f"
        }
    ]
}
```