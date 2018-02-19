# Monitoring Kinesis Video Streams Metrics with CloudWatch<a name="monitoring-cloudwatch"></a>

You can monitor a Kinesis video stream using Amazon CloudWatch, which collects and processes raw data from Kinesis Video Streams into readable, near real\-time metrics\. These statistics are recorded for a period of 15 months, so that you can access historical information and gain a better perspective on how your web application or service is performing\. 

To access the CloudWatch dashboard for a Kinesis video stream, choose **View stream metrics in CloudWatch** in the **Stream info** section of the console page for the stream\.

For a list of available metrics that Kinesis Video Streams supports, see [Kinesis Video Streams Metrics and Dimensions](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/akac-metricscollected.html)\.

## CloudWatch Metrics Guidance<a name="monitoring-cloudwatch-guidance"></a>

CloudWatch metrics can be useful for finding answers to the following questions: 


+ [Is data reaching the Kinesis Video Streams service?](#monitoring-cloudwatch-guidance-incoming)
+ [Why is data not being successfully ingested by the Kinesis Video Streams service?](#monitoring-cloudwatch-guidance-errors)
+ [Why can't the data be read from the Kinesis Video Streams service at the same rate as it's being sent from the producer?](#monitoring-cloudwatch-guidance-rate)
+ [Why is there no video in the console, or why is the video being played with a delay?](#monitoring-cloudwatch-guidance-novideo)
+ [What is the delay in reading real\-time data, and why is the client lagging behind the head of the stream?](#monitoring-cloudwatch-guidance-delay)
+ [Is the client reading data out of the Kinesis video stream, and at what rate?](#monitoring-cloudwatch-guidance-isread)
+ [Why can't the client read data out of the Kinesis video stream?](#monitoring-cloudwatch-guidance-noread)

### Is data reaching the Kinesis Video Streams service?<a name="monitoring-cloudwatch-guidance-incoming"></a>

**Relevant metrics:** 

+ `PutMedia.IncomingBytes`

+ `PutMedia.IncomingFragments`

+ `PutMedia.IncomingFrames`

**Action items:**

+ If there is a drop in these metrics, check if your application is still sending data to the service\.

+ Check the network bandwidth\. If your network bandwidth is insufficient, it could be slowing down the rate the service is receiving the data\.

### Why is data not being successfully ingested by the Kinesis Video Streams service?<a name="monitoring-cloudwatch-guidance-errors"></a>

**Relevant metrics:** 

+ `PutMedia.Requests`

+ `PutMedia.ConnectionErrors`

+ `PutMedia.Success`

+ `PutMedia.ErrorAckCount`

**Action items:**

+ If there is an increase in `PutMedia.ConnectionErrors`, look at the HTTP response/error codes received by the producer client to see what errors are occurring while establishing the connection\.

+ If there is a drop in `PutMedia.Success` or increase in `PutMedia.ErrorAckCount`, look at the ack error code in the ack responses sent by the service to see why ingestion of data is failing\. For more information, see [AckErrorCode\.Values](http://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/kinesisvideo/model/AckErrorCode.Values.html)\.

### Why can't the data be read from the Kinesis Video Streams service at the same rate as it's being sent from the producer?<a name="monitoring-cloudwatch-guidance-rate"></a>

**Relevant metrics:** 

+ `PutMedia.FragmentIngestionLatency`

+ `PutMedia.IncomingBytes`

**Action items:**

+ If there is a drop in these metrics, check the network bandwidth of your connections\. Low\-bandwidth connections could cause the data to reach the service at a lower rate\. 

### Why is there no video in the console, or why is the video being played with a delay?<a name="monitoring-cloudwatch-guidance-novideo"></a>

**Relevant metrics:** 

+ `PutMedia.FragmentIngestionLatency`

+ `PutMedia.FragmentPersistLatency`

+ `PutMedia.Success`

+ `ListFragments.Latency`

+ `PutMedia.IncomingFragments`

**Action items:**

+ If there is an increase in `PutMedia.FragmentIngestionLatency` or a drop in `PutMedia.IncomingFragments`, check the network bandwidth and whether the data is still being sent\.

+ If there is a drop in `PutMedia.Success`, check the ack error codes\. For more information, see [AckErrorCode\.Values](http://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/kinesisvideo/model/AckErrorCode.Values.html)\.

+ If there is an increase in `PutMedia.FragmentPersistLatency` or `ListFragments.Latency`, you are most likely experiencing a service issue\. If the condition persists for an extended period of time, check with your customer service contact to see if there is an issue with your service\.

### What is the delay in reading real\-time data, and why is the client lagging behind the head of the stream?<a name="monitoring-cloudwatch-guidance-delay"></a>

**Relevant metrics:** 

+ `GetMedia.MillisBehindNow`

+ `GetMedia.ConnectionErrors`

+ `GetMedia.Success`

**Action items:**

+ If there is an increase in `GetMedia.ConnectionErrors`, then the consumer might be falling behind in reading the stream, due to frequent attempts to re\-connect to the stream\. Look at the HTTP response/error codes returned for the `GetMedia` request\.

+ If there is a drop in `GetMedia.Success`, then it’s likely due to the service being unable to send the data to the consumer, which would result in dropped connection, and reconnects from consumers, which would result in the consumer lagging behind the head of the stream\.

+ If there is an increase in `GetMedia.MillisBehindNow`, look at your bandwidth limits to see if you are receiving the data at a slower rate because of lower bandwidth\.

### Is the client reading data out of the Kinesis video stream, and at what rate?<a name="monitoring-cloudwatch-guidance-isread"></a>

**Relevant metrics:** 

+ `GetMedia.OutgoingBytes`

+ `GetMedia.OutgoingFragments`

+ `GetMedia.OutgoingFrames`

+ `GetMediaForFragmentList.OutgoingBytes`

+ `GetMediaForFragmentList.OutgoingFragments`

+ `GetMediaForFragmentList.OutgoingFrames`

**Action items:**

+ These metrics indicate what rate real\-time and archived data is being read\.

### Why can't the client read data out of the Kinesis video stream?<a name="monitoring-cloudwatch-guidance-noread"></a>

**Relevant metrics:** 

+ `GetMedia.ConnectionErrors`

+ `GetMedia.Success`

+ `GetMediaForFragmentList.Success`

+ `PutMedia.IncomingBytes`

**Action items:**

+ If there is an increase in `GetMedia.ConnectionErrors`, look at the HTTP response/error codes returned by the `GetMedia` request\. For more information, see [AckErrorCode\.Values](http://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/kinesisvideo/model/AckErrorCode.Values.html)\.

+ If you are trying to read the latest/live data, check `PutMedia.IncomingBytes` to see if there is data coming into the stream for the service to send to the consumers\.

+ If there is a drop in `GetMedia.Success` or `GetMediaForFragmentList.Success`, it’s likely due to the service being unable to send the data to the consumer\. If the condition persists for an extended period of time, check with your customer service contact to see if there is an issue with your service\.