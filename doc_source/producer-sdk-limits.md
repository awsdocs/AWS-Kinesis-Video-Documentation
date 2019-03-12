# Producer SDK Limits<a name="producer-sdk-limits"></a>

The following table contains the current limits for values in the [Producer Libraries](producer-sdk.md)\.

**Note**  
Before setting these values, you must validate your inputs\. The SDK doesn't validate these limits, and a runtime error occurs if the limits are exceeded\.


****  

| Value | Limit | Notes | 
| --- | --- | --- | 
| Max stream count | 128 | The maximum number of streams that a producer object can create\. This is a soft limit \(you can request an increase\)\. It ensures that the producer doesn't accidentally create streams recursively\. | 
| Max device name length | 128 characters |   | 
| Max tag count | 50 per stream |   | 
| Max stream name length | 256 characters |   | 
| Min storage size | 10 MiB = 10 \* 1024 \* 1024 bytes |   | 
| Max storage size | 10 GiB = 10 \* 1024 \* 1024 \* 1024 bytes |   | 
| Max root directory path length | 4,096 characters |   | 
| Max auth info length | 10,000 bytes |   | 
| Max URI string length | 10,000 characters |   | 
| Max tag name length | 128 characters |   | 
| Max tag value length | 1,024 characters |   | 
| Min security token period | 30 seconds |   | 
| Security token grace period | 40 minutes | If the specified duration is longer, it is limited to this value\. | 
| Retention period | 0 or greater than one hour | 0 indicates no retention\. | 
| Min cluster duration | 1 second | The value is specified in 100 ns units, which is the SDK standard\. | 
| Max cluster duration | 30 seconds | The value is specified in 100 ns units, which is the SDK standard\. The backend API may enforce a shorter cluster duration\. | 
| Max fragment size | 50 MB | For more information, see [Kinesis Video Streams Limits](limits.md)\. | 
| Max fragment duration | 10 seconds | For more information, see [Kinesis Video Streams Limits](limits.md)\. | 
| Max connection duration | 45 minutes | The backend closes the connection after this time\. The SDK rotates the token and establishes a new connection within this time\. | 
| Max ACK segment length | 1,024 characters | Maximum segment length of the acknowledgement sent to the ACK parser function\. | 
| Max content type string length | 128 characters |   | 
| Max codec ID string length | 32 characters |   | 
| Max track name string length | 32 characters |   | 
| Max codec private data length | 1 MiB = 1 \* 1024 \* 1024 bytes |   | 
| Min timecode scale value length | 100 ns | The minimum timecode scale value to represent the frame timestamps in the resulting MKV cluster\. The value is specified in increments of 100 ns, which is the SDK standard\. | 
| Max timecode scale value length | 1 second | The maximum timecode scale value to represent the frame timestamps in the resulting MKV cluster\. The value is specified in increments of 100 ns, which is the SDK standard\. | 
| Min content view item count | 10 |   | 
| Min buffer duration | 20 seconds | The value is specified in increments of 100 ns, which is the SDK standard\. | 
| Max update version length | 128 characters |   | 
| Max ARN length | 1024 characters |   | 
| Max fragment sequence length | 128 characters |   | 
| Max retention period | 10 years |   | 