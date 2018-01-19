# Kinesis Video Streams Limits<a name="limits"></a>

Kinesis Video Streams has the following limits:

The limits below are either soft \[s\], which can be upgraded by submitting a support ticket, or hard \[h\], which cannot be increased\.

## Control Plane API limits<a name="limits-akv-control"></a>

The following section describes limits for Control Plane APIs\.

When an account\-level Request limit is reached, a `ClientLimitExceededException` is thrown\.

When an account\-level Streams limit is reached, or a stream\-level limit is reached, a `StreamLimitExceededException` is thrown\.


**Control Plane API limits**  

| API | Account Limit: Request | Account Limit: Streams | Stream\-level limit | Relevant Exceptions and Notes | 
| --- | --- | --- | --- | --- | 
| CreateStream | 50 TPS \[s\] | 100 streams per account \[s\] | 5 TPS \[h\] | Devices, CLIs, SDK\-driven access and the console can all invoke this API\. Only one API call succeeds if the stream doesn’t already exist\. | 
| DescribeStream | 300 TPS \[h\] | N/A | 5 TPS \[h\] |  | 
| UpdateStream | 50 TPS \[h\] | N/A | 5 TPS \[h\] |  | 
| ListStreams | 300 TPS \[h\] | N/A | 5 TPS \[h\] |  | 
| DeleteStream | 50 TPS \[h\] | N/A | 5 TPS \[h\] |  | 
| GetDataEndpoint | 300 TPS \[h\] | N/A | 5 TPS \[h\] | When combined with account limit, this implies a maximum of 60 streams can be Put to and Read from \(with 4 consumers\)\. | 

## Media and Archived Media API limits<a name="limits-akv-data"></a>

The following section describes limits for Media and Archived Media APIs\.

When a stream\-level limit is exceeded, a `StreamLimitExceededException` is thrown\.

When a connection\-level limit is reached, a `ConnectionLimitExceededException` is thrown\.

The following errors or acks are thrown when a fragment\-level limit is reached:

+ A `MIN_FRAGMENT_DURATION_REACHED` ack is returned for a fragment below the minumum duration\.

+ A `MAX_FRAGMENT_DURATION_REACHED` ack is returned for a fragment above the maximum duration\.

+ A `MAX_FRAGMENT_SIZE` ack is returned for a fragment above the maximum data size\.

+ A `FragmentLimitExceeded` exception is thrown if a fragment limit is reached in a `GetMediaForFragmentList` operation\.


**Data Plane API limits**  

| API | Stream\-level limit | Connection\-level limit | Bandwidth limit | Fragment\-level limit | Relevant Exceptions and Notes | 
| --- | --- | --- | --- | --- | --- | 
| PutMedia | 5 TPS \[h\] | 1 \(5 in the config, to allow for streaming token rotation, retries, etc\.\) | 12\.5 MB/second, or 100 Mbps | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/limits.html) | A typical PutMedia request will contain data for several seconds, resulting in a lower TPS per stream\. In the case of multiple concurrent connections that exceed limits, the last connection is accepted\. | 
| GetMedia | 5 TPS \[h\] | 3 | 25 MB/s or 200 Mbps | N/A | Only three clients can concurrently receive content from the media stream at any moment of time\. Further client connections are rejected\. A unique consuming client shouldn’t need more than 2 or 3 TPS, since once the connection is established, we anticipate that the application will read continuously\.  If a typical fragment is approximately 5 MB, this limit will mean \~75 MB/ sec per Kinesis video stream\. Such a stream would have an outgoing bit rate of 2x the streams' maximum incoming bit rate\. | 
| ListFragments | 5 TPS \[h\] | 5 | N/A | N/A | Five fragment\-based consuming applications can concurrently list fragments based on processing requirements\. | 
| GetMediaForFragmentList | 5 TPS \[h\] | 5 | 25 MB/s or 200 MbpsA | Maximum number of fragments: 1000 | Five fragment\-based consuming applications can concurrently get media\. Further connections are rejected\. | 