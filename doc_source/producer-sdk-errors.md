# Error Code Reference<a name="producer-sdk-errors"></a>

This section contains error and status code information for the [Producer Libraries](producer-sdk.md)\.

For information about solutions to common issues, see [Troubleshooting Kinesis Video Streams](troubleshooting.md)\.

**Topics**
+ [Errors and Status Codes Returned by PutFrame Callbacks \- Platform Independent Code \(PIT\)](#producer-sdk-errors-putframe)
+ [Errors and Status Codes Returned by PutFrame Callbacks \- C Producer Library](#producer-sdk-errors-putframe-c)

## Errors and Status Codes Returned by PutFrame Callbacks \- Platform Independent Code \(PIT\)<a name="producer-sdk-errors-putframe"></a>

The following sections contain error and status information that is returned by callbacks for the `PutFrame` operation within the Platform Independent Codebase \(PIT\)\.

**Topics**
+ [Error and Status Codes Returned by the Client Library](#producer-sdk-errors-client)
+ [Error and Status Codes Returned by the Duration Library](#producer-sdk-errors-duration)
+ [Error and Status Codes Returned by the Common Library](#producer-sdk-errors-common)
+ [Error and Status Codes Returned by the Heap Library](#producer-sdk-errors-heap)
+ [Error and Status Codes Returned by the MKVGen Library](#producer-sdk-errors-mkvgen)
+ [Error and Status Codes Returned by the Trace Library](#producer-sdk-errors-trace)
+ [Error and Status Codes Returned by the Utils Library](#producer-sdk-errors-utils)
+ [Error and Status Codes Returned by the View Library](#producer-sdk-errors-view)

### Error and Status Codes Returned by the Client Library<a name="producer-sdk-errors-client"></a>

The following table contains error and status information that is returned by methods in the Kinesis Video Streams `Client` library\.


****  

| Code | Message | Description | Recommended Action | 
| --- | --- | --- | --- | 
| 0x52000001 | STATUS\_MAX\_STREAM\_COUNT | The maximum stream count was reached\. | Specify a larger max stream count in DeviceInfo as specified in [Producer SDK Limits](producer-sdk-limits.md)\. | 
| 0x52000002 | STATUS\_MIN\_STREAM\_COUNT | Minimum stream count error\. | Specify the max number of streams greater than 0 in DeviceInfo\. | 
| 0x52000003 | STATUS\_INVALID\_DEVICE\_NAME\_LENGTH | Invalid device name length\. | Refer to the max device name length in characters that is specified in [Producer SDK Limits](producer-sdk-limits.md)\. | 
| 0x52000004 | STATUS\_INVALID\_DEVICE\_INFO\_VERSION | Invalid DeviceInfo structure version\. | Specify the correct current version of the structure\. | 
| 0x52000005 | STATUS\_MAX\_TAG\_COUNT | The maximum tag count was reached\. | Refer to the current max tag count that is specified in [Producer SDK Limits](producer-sdk-limits.md)\. | 
| 0x52000006 | STATUS\_DEVICE\_FINGERPRINT\_LENGTH | 
| 0x52000007 | STATUS\_INVALID\_CALLBACKS\_VERSION | Invalid Callbacks structure version\. | Specify the correct current version of the structure\. | 
| 0x52000008 | STATUS\_INVALID\_STREAM\_INFO\_VERSION | Invalid StreamInfo structure version\. | Specify the correct current version of the structure\. | 
| 0x52000009 | STATUS\_INVALID\_STREAM\_NAME\_LENGTH | Invalid stream name length\. | Refer to the max stream name length in characters that is specified in [Producer SDK Limits](producer-sdk-limits.md)\. | 
| 0x5200000a | STATUS\_INVALID\_STORAGE\_SIZE | An invalid storage size was specified\. | The storage size in bytes must be within the limits specified in [Producer SDK Limits](producer-sdk-limits.md)\. | 
| 0x5200000b | STATUS\_INVALID\_ROOT\_DIRECTORY\_LENGTH | Invalid root directory string length\. | Refer to the max root directory path length that is specified in [Producer SDK Limits](producer-sdk-limits.md)\. | 
| 0x5200000c | STATUS\_INVALID\_SPILL\_RATIO | Invalid spill ratio\. | Express the spill ratio as a percentage from 0 to 100\. | 
| 0x5200000d | STATUS\_INVALID\_STORAGE\_INFO\_VERSION | Invalid StorageInfo structure version\. | Specify the correct current version of the structure\. | 
| 0x5200000e | STATUS\_INVALID\_STREAM\_STATE | The stream is in a state that doesn't permit the current operation\. | Most commonly, this error occurs when the SDK fails to reach the state that it needs to perform the requested operation\. For example, it occurs if the GetStreamingEndpoint API call fails, and the client application ignores it and continues putting frames into the stream\.  | 
| 0x5200000f | STATUS\_SERVICE\_CALL\_CALLBACKS\_MISSING | The Callbacks structure has missing function entry points for some mandatory functions\. | Ensure that the mandatory callbacks are implemented in the client application\. This error is exposed only to PIC \(Platform Independent Code\) clients\. C\+\+ and other higher\-level wrappers satisfy these calls\. | 
| 0x52000010 | STATUS\_SERVICE\_CALL\_NOT\_AUTHORIZED\_ERROR | Not authorized\. | Verify the security token/certificate/security token integration/expiration\. Ensure that the token has the correct associated rights with it\. For the Kinesis Video Streams sample applications, ensure that the environment variable is set correctly\. | 
| 0x52000011 | STATUS\_DESCRIBE\_STREAM\_CALL\_FAILED | DescribeStream API failure\. | This error is returned after the DescribeStream API retry failure\. The PIC client returns this error after it gives up retrying\.  | 
| 0x52000012 | STATUS\_INVALID\_DESCRIBE\_STREAM\_RESPONSE | Invalid DescribeStreamResponse structure\.  | The structure that was passed to the DescribeStreamResultEvent is either null or contains invalid items like a null Amazon Resource Name \(ARN\)\. | 
| 0x52000013 | STATUS\_STREAM\_IS\_BEING\_DELETED\_ERROR | The stream is being deleted\. | An API failure was caused by the stream being deleted\. Ensure that no other processes are trying to delete the stream while the stream is in use\.  | 
| 0x52000014 | STATUS\_SERVICE\_CALL\_INVALID\_ARG\_ERROR | Invalid arguments were specified for the service call\.  | The backend returns this error when a service call argument is not valid or when the SDK encounters an error that it can't interpret\. | 
| 0x52000015 | STATUS\_SERVICE\_CALL\_DEVICE\_NOT\_FOUND\_ERROR | The device was not found\.  | Ensure that the device is not deleted while in use\.  | 
| 0x52000016 | STATUS\_SERVICE\_CALL\_DEVICE\_NOT\_PROVISIONED\_ERROR | The device was not provisioned\.  | Ensure that the device has been provisioned\.  | 
| 0x52000017 | STATUS\_SERVICE\_CALL\_RESOURCE\_NOT\_FOUND\_ERROR | Generic resource not found returned from the service\.  | This error occurs when the service can't locate the resource \(for example, a stream\)\. It might mean different things in different contexts, but the likely cause is the usage of APIs before the stream is created\. Using the SDK ensures that the stream is created first\.  | 
| 0x52000018 | STATUS\_INVALID\_AUTH\_LEN | Invalid auth info length\.  | Refer to the current values that are specified in [Producer SDK Limits](producer-sdk-limits.md)\. | 
| 0x52000019 | STATUS\_CREATE\_STREAM\_CALL\_FAILED | The CreateStream API call failed\.  | Refer to the error string for more detailed information about why the operation failed\. | 
| 0x5200002a | STATUS\_GET\_STREAMING\_TOKEN\_CALL\_FAILED | The GetStreamingToken call failed\.  | Refer to the error string for more detailed information about why the operation failed\.  | 
| 0x5200002b | STATUS\_GET\_STREAMING\_ENDPOINT\_CALL\_FAILED | The GetStreamingEndpoint API call failed\.  | Refer to the error string for more detailed information about why the operation failed\. | 
| 0x5200002c | STATUS\_INVALID\_URI\_LEN | An invalid URI string length was returned from the GetStreamingEndpoint API\.  |  Refer to the current maximum values that are specified in [Producer SDK Limits](producer-sdk-limits.md)\. | 
| 0x5200002d | STATUS\_PUT\_STREAM\_CALL\_FAILED | The PutMedia API call failed\.  | Refer to the error string for more detailed information about why the operation failed\.  | 
| 0x5200002e | STATUS\_STORE\_OUT\_OF\_MEMORY | The content store is out of memory\. | The content store is shared between the streams and should have enough capacity to store the maximum durations for all the streams \+ \~20% \(accounting for the defragmentation\)\. It's important to not overflow the storage\. Choose values for the maximum duration per stream that correspond to the cumulative storage size and the latency tolerances\. It's better to drop the frames as they fall out of the content view window versus just being put \(content store memory pressure\)\. This is because dropping the frames triggers the stream pressure notification callbacks\. Then the application can adjust the upstream media components \(like the encoder\) to thin the bitrate, drop frames, or act accordingly\.  | 
| 0x5200002f | STATUS\_NO\_MORE\_DATA\_AVAILABLE | No more data is available currently for a stream\.  | This is a potential valid result when the media pipeline produces more slowly than the networking thread consumes the frames to be sent to the service\. Higher\-level clients \(for example, C\+\+, Java, or Android\) do not see this warning because it's handled internally\. | 
| 0x52000030 | STATUS\_INVALID\_TAG\_VERSION | Invalid Tag structure version\.  | Specify the correct current version of the structure\.  | 
| 0x52000031 | STATUS\_SERVICE\_CALL\_UNKNOWN\_ERROR | An unknown or generic error was returned from the networking stack\.  | See the logs for more detailed information\.  | 
| 0x52000032 | STATUS\_SERVICE\_CALL\_RESOURCE\_IN\_USE\_ERROR | Resource in use\.  | Returned from the service\. For more information, see the Kinesis Video Streams API Reference\.  | 
| 0x52000033 | STATUS\_SERVICE\_CALL\_CLIENT\_LIMIT\_ERROR | Client limit\.  | Returned from the service\. For more information, see the Kinesis Video Streams API Reference\. | 
| 0x52000034 | STATUS\_SERVICE\_CALL\_DEVICE\_LIMIT\_ERROR | Device limit\.  | Returned from the service\. For more information, see the Kinesis Video Streams API Reference\. | 
| 0x52000035 | STATUS\_SERVICE\_CALL\_STREAM\_LIMIT\_ERROR | Stream limit\.  | Returned from the service\. For more information, see the Kinesis Video Streams API Reference\. | 
| 0x52000036 | STATUS\_SERVICE\_CALL\_RESOURCE\_DELETED\_ERROR | The resource was deleted or is being deleted\.  | Returned from the service\. For more information, see the Kinesis Video Streams API Reference\. | 
| 0x52000037 | STATUS\_SERVICE\_CALL\_TIMEOUT\_ERROR | The service call timed out\.  | Calling a particular service API resulted in a timeout\. Ensure that you have a valid network connection\. The PIC will retry the operation automatically\.  | 
| 0x52000038 | STATUS\_STREAM\_READY\_CALLBACK\_FAILED | Stream ready notification\.  | This notification is sent from the PIC to the client indicating that the async stream has been created\.  | 
| 0x52000039 | STATUS\_DEVICE\_TAGS\_COUNT\_NON\_ZERO\_TAGS\_NULL | Invalid tags were specified\.  | The tag count is not zero, but the tags are empty\. Ensure that the tags are specified or the count is zero\.  | 
| 0x5200003a | STATUS\_INVALID\_STREAM\_DESCRIPTION\_VERSION | Invalid StreamDescription structure version\.  | Specify the correct current version of the structure\. | 
| 0x5200003b | STATUS\_INVALID\_TAG\_NAME\_LEN | Invalid tag name length\.  | Refer to the limits for the tag name that are specified in [Producer SDK Limits](producer-sdk-limits.md)\. | 
| 0x5200003c | STATUS\_INVALID\_TAG\_VALUE\_LEN | Invalid tag value length\.  | Refer to the limits for the tag value that are specified in [Producer SDK Limits](producer-sdk-limits.md)\. | 
| 0x5200003d | STATUS\_TAG\_STREAM\_CALL\_FAILED | The TagResource API failed\. | The TagResource API call failed\. Check for a valid network connection\. See the logs for more information about the failure\.  | 
| 0x5200003e | STATUS\_INVALID\_CUSTOM\_DATA | Invalid custom data calling PIC APIs\.  | Invalid custom data has been specified in a call to the PIC APIs\. This can occur only in the clients that directly use PIC\.  | 
| 0x5200003f | STATUS\_INVALID\_CREATE\_STREAM\_RESPONSE | Invalid CreateStreamResponse structure\.  | The structure or its member fields are invalid \(that is, the ARN is null or larger than what's specified in [Producer SDK Limits](producer-sdk-limits.md)\)\.  | 
| 0x52000040 | STATUS\_CLIENT\_AUTH\_CALL\_FAILED  | Client auth failed\.  | The PIC failed to get proper auth information \(that is, AccessKeyId or SecretAccessKey\) after a number of retries\. Check the authentication integration\. The sample applications use environment variables to pass in credential information to the C\+\+ Producer Library\.  | 
| 0x52000041 | STATUS\_GET\_CLIENT\_TOKEN\_CALL\_FAILED | Getting the security token call failed\.  | This situation can occur for clients that use PIC directly\. After a number of retries, the call fails with this error\. | 
| 0x52000042 | STATUS\_CLIENT\_PROVISION\_CALL\_FAILED | Provisioning error\.  | Provisioning is not implemented\.  | 
| 0x52000043 | STATUS\_CREATE\_CLIENT\_CALL\_FAILED | Failed to create the producer client\.  | A generic error returned by the PIC after a number of retries when the client creation fails\.  | 
| 0x52000044 | STATUS\_CLIENT\_READY\_CALLBACK\_FAILED | Failed to get the producer client to a READY state\.  | Returned by the PIC state machine if the PIC fails to move to the READY state\. See the logs for more information about the root cause\.  | 
| 0x52000045 | STATUS\_TAG\_CLIENT\_CALL\_FAILED | The TagResource for the producer client failed\.  | The TagResource API call failed for the producer client\. See the logs for more information about the root cause\.  | 
| 0x52000046 | STATUS\_INVALID\_CREATE\_DEVICE\_RESPONSE | Device/Producer creation failed\.  | The higher\-level SDKs \(for example, C\+\+ or Java\) don't implement the device/producer creation API yet\. Clients that use PIC directly can indicate a failure using the result notification\.  | 
| 0x52000047 | STATUS\_ACK\_TIMESTAMP\_NOT\_IN\_VIEW\_WINDOW | The timestamp of the received ACK is not in the view\.  | This error occurs if the frame corresponding to the received ACK falls out of the content view window\. Generally, this occurs if the ACK delivery is slow\. It can be interpreted as a warning and an indication that the downlink is slow\.  | 
| 0x52000048 | STATUS\_INVALID\_FRAGMENT\_ACK\_VERSION | Invalid FragmentAck structure version\.  | Specify the correct current version of the FragmentAck structure\. | 
| 0x52000049 | STATUS\_INVALID\_TOKEN\_EXPIRATION | Invalid security token expiration\.  |  The security token expiration should have an absolute timestamp in the future that is greater than the current timestamp, with a grace period\. For the limits for the grace period, see the [Producer SDK Limits](producer-sdk-limits.md)\.  | 
| 0x5200004a | STATUS\_END\_OF\_STREAM | End of stream \(EOS\) indicator\.  | In the GetStreamData API call, indicates that the current upload handle session has ended\. This occurs if the session ends or errors, or if the session token has expired and the session is being rotated\.  | 
| 0x5200004b | STATUS\_DUPLICATE\_STREAM\_NAME | Duplicate stream name\.  | Multiple streams can't have the same stream name\. Choose a unique name for the stream\.  | 
| 0x5200004c | STATUS\_INVALID\_RETENTION\_PERIOD | Invalid retention period\.  | An invalid retention period is specified in the StreamInfo structure\. For information about the valid range of values for the retention period, see [Producer SDK Limits](producer-sdk-limits.md)\.  | 
| 0x5200004d | STATUS\_INVALID\_ACK\_KEY\_START | Invalid FragmentAck\.  | Failed to parse the fragment ACK string\. Invalid key start indicator\. The fragment ACK string might be damaged\. It can self\-correct and this error can be treated as a warning\.  | 
| 0x5200004e | STATUS\_INVALID\_ACK\_DUPLICATE\_KEY\_NAME | Invalid FragmentAck\.  | Failed to parse the fragment ACK string\. Multiple keys have the same name\. The fragment ACK string might be damaged\. It can self\-correct and this error can be treated as a warning\.  | 
| 0x5200004f | STATUS\_INVALID\_ACK\_INVALID\_VALUE\_START | Invalid FragmentAck\.  | Failed to parse the fragment ACK string because of an invalid key value start indicator\. The fragment ACK string might be damaged\. It can self\-correct, and this error can be treated as a warning\.  | 
| 0x52000050 | STATUS\_INVALID\_ACK\_INVALID\_VALUE\_END | Invalid FragmentAck\.  | Failed to parse the fragment ACK string because of an invalid key value end indicator\. The fragment ACK string might be damaged\. It can self\-correct and this error can be treated as a warning\.  | 
| 0x52000051 | STATUS\_INVALID\_PARSED\_ACK\_TYPE | Invalid FragmentAck\.  | Failed to parse the fragment ACK string because an invalid ACK type was specified\.  | 
| 0x52000052 | STATUS\_STREAM\_HAS\_BEEN\_STOPPED | Stream was stopped\.  | The stream has been stopped, but a frame is still being put into the stream\. | 
|  0x52000053  | STATUS\_INVALID\_STREAM\_METRICS\_VERSION | Invalid StreamMetrics structure version\.  | Specify the correct current version of the StreamMetrics structure\.  | 
|  0x52000054  | STATUS\_INVALID\_CLIENT\_METRICS\_VERSION | Invalid ClientMetrics structure version\.  | Specify the correct current version of the ClientMetrics structure\.  | 
|  0x52000055  | STATUS\_INVALID\_CLIENT\_READY\_STATE | Producer initialization failed to reach a READY state\.  | Failed to reach the READY state during the producer client initialization\. See the logs for more information\.  | 
|  0x52000056  | STATUS\_STATE\_MACHINE\_STATE\_NOT\_FOUND | Internal state machine error\.  | Not a publicly visible error\.  | 
|  0x52000057  | STATUS\_INVALID\_FRAGMENT\_ACK\_TYPE | Invalid ACK type is specified in the FragmentAck structure\.  | The FragmentAck structure should contain ACK types defined in the public header\.  | 
|  0x52000058  | STATUS\_INVALID\_STREAM\_READY\_STATE | Internal state machine transition error\.  | Not a publicly visible error\.  | 
|  0x52000059  | STATUS\_CLIENT\_FREED\_BEFORE\_STREAM | The stream object was freed after the producer was freed\.  | There was an attempt to free a stream object after the producer object was freed\. This can only occur in clients that directly use PIC\.  | 
|  0x5200005a  | STATUS\_ALLOCATION\_SIZE\_SMALLER\_THAN\_REQUESTED | Internal storage error\.  | An internal error indicating that the actual allocation size from the content store is smaller than the size of the packaged frame/fragment\.  | 
|  0x5200005b  | STATUS\_VIEW\_ITEM\_SIZE\_GREATER\_THAN\_ALLOCATION | Internal storage error\.  | The stored size of the allocation in the content view is greater than the allocation size in the content store\.  | 
|  0x5200005c  | STATUS\_ACK\_ERR\_STREAM\_READ\_ERROR | Stream read error ACK\.  | An error that the ACK returned from the backend indicating a stream read/parsing error\. This generally occurs when the backend fails to retrieve the stream\. Auto\-restreaming can usually correct this error\.  | 
|  0x5200005d  | STATUS\_ACK\_ERR\_FRAGMENT\_SIZE\_REACHED | The maximum fragment size was reached\. | The max fragment size in bytes is defined in [Producer SDK Limits](producer-sdk-limits.md)\. This error indicates that there are either very large frames, or there are no key frames to create manageable size fragments\. Check the encoder settings and ensure that key frames are being produced properly\. For streams that have very high density, configure the encoder to produce fragments at smaller durations to manage the maximum size\.  | 
|  0x5200005e  | STATUS\_ACK\_ERR\_FRAGMENT\_DURATION\_REACHED | The maximum fragment duration was reached\.  | The max fragment duration is defined in [Producer SDK Limits](producer-sdk-limits.md)\. This error indicates that there are either very low frames per second or there are no key frames to create manageable duration fragments\. Check the encoder settings and ensure that key frames are being produced properly at the regular intervals\.  | 
|  0x5200005f  | STATUS\_ACK\_ERR\_CONNECTION\_DURATION\_REACHED | The maximum connection duration was reached\.  | Kinesis Video Streams enforces the max connection duration as specified in the [Producer SDK Limits](producer-sdk-limits.md)\. The Producer SDK automatically rotates the stream/token before the maximum is reached, and so clients using the SDK should not receive this error\. | 
|  0x52000060  | STATUS\_ACK\_ERR\_FRAGMENT\_TIMECODE\_NOT\_MONOTONIC | Timecodes are not monotonically increasing\.  | The Producer SDK enforces timestamps, so clients using the SDK should not receive this error\. | 
|  0x52000061  | STATUS\_ACK\_ERR\_MULTI\_TRACK\_MKV | Multiple tracks in the MKV\.  | The Producer SDK enforces single track streams, so clients using the SDK should not receive this error\.  | 
|  0x52000062  | STATUS\_ACK\_ERR\_INVALID\_MKV\_DATA | Invalid MKV data\.  | The backend MKV parser encountered an error parsing the stream\. Clients using the SDK might encounter this error if the stream is corrupted in the transition or if the buffer pressures force the SDK to drop tail frames that are partially transmitted\. In the latter case, we recommend that you either reduce the FPS/resolution, increase the compression ratio, or \(in the case of a "bursty" network\) allow for larger content store and buffer duration to accommodate for the temporary pressures\.  | 
|  0x52000063  | STATUS\_ACK\_ERR\_INVALID\_PRODUCER\_TIMESTAMP | Invalid producer timestamp\.  | The service returns this error ACK if the producer clock has a large drift into the future\. Higher\-level SDKs \(for example, Java or C\+\+\) use some version of the system clock to satisfy the current time callback from PIC\. Ensure that the system clock is set properly\. Clients using the PIC directly should ensure that their callback functions return the correct timestamp\.  | 
|  0x52000064  | STATUS\_ACK\_ERR\_STREAM\_NOT\_ACTIVE | Inactive stream\.  | A call to a backend API was made while the stream was not in an "Active" state\. This occurs when the client creates the stream and immediately continues to push frames into it\. The SDK handles this scenario through the state machine and recovery mechanism\.  | 
|  0x52000065  | STATUS\_ACK\_ERR\_KMS\_KEY\_ACCESS\_DENIED | AWS KMS access denied error\.  | Returned when the account has no access to the specified key\.  | 
|  0x52000066  | STATUS\_ACK\_ERR\_KMS\_KEY\_DISABLED | AWS KMS key is disabled  | The specified key has been disabled\.  | 
|  0x52000067  | STATUS\_ACK\_ERR\_KMS\_KEY\_VALIDATION\_ERROR  | AWS KMS key validation error\.  | Generic validation error\. For more information, see the [AWS Key Management Service API Reference](https://docs.aws.amazon.com/kms/latest/APIReference/)\.  | 
|  0x52000068  | STATUS\_ACK\_ERR\_KMS\_KEY\_UNAVAILABLE | AWS KMS key unavailable\.  | The key is unavailable\. For more information, see the [AWS Key Management Service API Reference](https://docs.aws.amazon.com/kms/latest/APIReference/)\.  | 
|  0x52000069  | STATUS\_ACK\_ERR\_KMS\_KEY\_INVALID\_USAGE | Invalid use of AWS KMS key\. | The AWS KMS key is not configured to be used in this context\. For more information, see the [AWS Key Management Service API Reference](https://docs.aws.amazon.com/kms/latest/APIReference/)\.  | 
|  0x5200006a  | STATUS\_ACK\_ERR\_KMS\_KEY\_INVALID\_STATE | AWS KMS invalid state\.  | For more information, see the [AWS Key Management Service API Reference](https://docs.aws.amazon.com/kms/latest/APIReference/)\. | 
|  0x5200006b  | STATUS\_ACK\_ERR\_KMS\_KEY\_NOT\_FOUND | AWS KMS key not found\.  | The key was not found\. For more information, see the [AWS Key Management Service API Reference](https://docs.aws.amazon.com/kms/latest/APIReference/)\.  | 
|  0x5200006c  | STATUS\_ACK\_ERR\_STREAM\_DELETED | The stream has been or is being deleted\.  | The stream is being deleted by another application or through the AWS Management Console\. | 
|  0x5200006d  | STATUS\_ACK\_ERR\_ACK\_INTERNAL\_ERROR | Internal error\.  | Generic service internal error\.  | 
|  0x5200006e  | STATUS\_ACK\_ERR\_FRAGMENT\_ARCHIVAL\_ERROR | Fragment archival error\.  | Returned when the service fails to durably persist and index the fragment\. Although it's rare, it can occur for various reasons\. By default, the SDK retries sending the fragment\.  | 
|  0x5200006f  | STATUS\_ACK\_ERR\_UNKNOWN\_ACK\_ERROR | Unknown error\.  | The service returned an unknown error\.  | 
|  0x52000070  | STATUS\_MISSING\_ERR\_ACK\_ID | Missing ACK information\.  | The ACK parser completed parsing, but the FragmentAck information is missing\.  | 
|  0x52000071  | STATUS\_INVALID\_ACK\_SEGMENT\_LEN | Invalid ACK segment length\.  | An ACK segment string with an invalid length was specified to the ACK parser\. For more information, see [Producer SDK Limits](producer-sdk-limits.md)\.  | 
|  0x52000074  | STATUS\_MAX\_FRAGMENT\_METADATA\_COUNT | The maximum number of metadata items has been added to a fragment\. | A Kinesis video stream can add up to 10 metadata items to a fragment, either by adding a nonpersistent item to a fragment, or by adding a persistent item to the metadata queue\. For more information, see [Using Streaming Metadata with Kinesis Video Streams](how-meta.md)\.  | 
|  0x52000075  | STATUS\_ACK\_ERR\_FRAGMENT\_METADATA\_LIMIT\_REACHED | A limit \(maximum metadata count, metadata name length, or metadata value length\) has been reached\. | The Producer SDK limits the number and size of metadata items\. This error does not occur unless the limits in the Producer SDK code are changed\. For more information, see [Using Streaming Metadata with Kinesis Video Streams](how-meta.md)\.  | 
|  0x52000076  | STATUS\_BLOCKING\_PUT\_INTERRUPTED\_STREAM\_TERMINATED | Not implemented\. |  | 
|  0x52000077  | STATUS\_INVALID\_METADATA\_NAME | The metadata name is not valid\. | The metadata name cannot start with the string "AWS"\. If this error occurs, the metadata item is not added to the fragment or metadata queue\. For more information, see [Using Streaming Metadata with Kinesis Video Streams](how-meta.md)\. | 
|  0x52000078  | STATUS\_END\_OF\_FRAGMENT\_FRAME\_INVALID\_STATE | The end of a fragment frame is in an invalid state\. | The end of fragment should not be sent in a non\-key\-frame fragmented stream\. | 
|  0x52000079  | STATUS\_TRACK\_INFO\_MISSING | Track information is missing\. | The track number must be greater than 0 and it must match the track id\. | 
|  0x5200007a  | STATUS\_MAX\_TRACK\_COUNT\_EXCEEDED | Maximum track count is exceeded\. | You can have a maximum of 3 tracks per stream\. | 
|  0x5200007b  | STATUS\_OFFLINE\_MODE\_WITH\_ZERO\_RETENTION | The offline streaming mode retention time is set to zero\. | The offline streaming mode retention time should not be set to zero\. | 
|  0x5200007c  | STATUS\_ACK\_ERR\_TRACK\_NUMBER\_MISMATCH | The track number of the error ACK is mismatched\. |  | 
|  0x5200007d  | STATUS\_ACK\_ERR\_FRAMES\_MISSING\_FOR\_TRACK | Frames missing for a track\.  |  | 
|  0x5200007e  | STATUS\_ACK\_ERR\_MORE\_THAN\_ALLOWED\_TRACKS\_FOUND | Maximum allowed number of tracks is exceeded\. |  | 
|  0x5200007f  | STATUS\_UPLOAD\_HANDLE\_ABORTED | Upload handle is aborted\. |  | 
|  0x52000080  | STATUS\_INVALID\_CERT\_PATH\_LENGTH | Invalid certificate path length\. |  | 
|  0x52000081  | STATUS\_DUPLICATE\_TRACK\_ID\_FOUND | Duplicate track ID found\. |  | 

### Error and Status Codes Returned by the Duration Library<a name="producer-sdk-errors-duration"></a>

The following table contains error and status information that is returned by methods in the `Duration` library\.


****  

| Code | Message | 
| --- | --- | 
| 0xFFFFFFFFFFFFFFFF | INVALID\_DURATION\_VALUE | 

### Error and Status Codes Returned by the Common Library<a name="producer-sdk-errors-common"></a>

The following table contains error and status information that is returned by methods in the `Common` library\.

**Note**  
These error and status information codes are common to many APIs\.


****  

| Code | Message | Description | 
| --- | --- | --- | 
|  0x00000001  | STATUS\_NULL\_ARG  | NULL was passed for a mandatory argument\.  | 
|  0x00000002  | STATUS\_INVALID\_ARG  | An invalid value was specified for an argument\.  | 
|  0x00000003  | STATUS\_INVALID\_ARG\_LEN  | An invalid argument length was specified\. | 
|  0x00000004  | STATUS\_NOT\_ENOUGH\_MEMORY  | Could not allocate enough memory\.  | 
|  0x00000005  | STATUS\_BUFFER\_TOO\_SMALL  | The specified buffer size is too small\.  | 
|  0x00000006  | STATUS\_UNEXPECTED\_EOF  | An unexpected end of file was reached\.  | 
|  0x00000007  | STATUS\_FORMAT\_ERROR  | An invalid format was encountered\.  | 
|  0x00000008  | STATUS\_INVALID\_HANDLE\_ERROR  | Invalid handle value\.  | 
|  0x00000009  | STATUS\_OPEN\_FILE\_FAILED  | Failed to open a file\.  | 
|  0x0000000a  | STATUS\_READ\_FILE\_FAILED | Failed to read from a file\.  | 
|  0x0000000b  | STATUS\_WRITE\_TO\_FILE\_FAILED  | Failed to write to a file\.  | 
|  0x0000000c  | STATUS\_INTERNAL\_ERROR  | An internal error that normally doesn't occur and might indicate an SDK or service API bug\.  | 
|  0x0000000d  | STATUS\_INVALID\_OPERATION  | There was an invalid operation, or the operation is not permitted\.  | 
|  0x0000000e  | STATUS\_NOT\_IMPLEMENTED  | The feature is not implemented\.  | 
|  0x0000000f  | STATUS\_OPERATION\_TIMED\_OUT  | The operation timed out\.  | 
|  0x00000010  | STATUS\_NOT\_FOUND  | A required resource was not found\.  | 

### Error and Status Codes Returned by the Heap Library<a name="producer-sdk-errors-heap"></a>

The following table contains error and status information that is returned by methods in the `Heap` library\.


****  

| Code | Message | Description | 
| --- | --- | --- | 
|  0x01000001  | STATUS\_HEAP\_FLAGS\_ERROR  | An invalid combination of flags was specified\.  | 
|  0x01000002  | STATUS\_HEAP\_NOT\_INITIALIZED  | An operation was attempted before the heap was initialized\.  | 
|  0x01000003  | STATUS\_HEAP\_CORRUPTED  | The heap was corrupted or the guard band \(in debug mode\) was overwritten\. A buffer overflow in the client code might lead to a heap corruption\.  | 
|  0x01000004  | STATUS\_HEAP\_VRAM\_LIB\_MISSING  | The VRAM \(video RAM\) user or kernel mode library cannot be loaded or is missing\. Check if the underlying platform supports VRAM allocations\.  | 
|  0x01000005  | STATUS\_HEAP\_VRAM\_LIB\_REOPEN  | Failed to open the VRAM library\.  | 
|  0x01000006  | STATUS\_HEAP\_VRAM\_INIT\_FUNC\_SYMBOL  | Failed to load the INIT function export\.  | 
|  0x01000007  | STATUS\_HEAP\_VRAM\_ALLOC\_FUNC\_SYMBOL  | Failed to load the ALLOC function export\.  | 
|  0x01000008  | STATUS\_HEAP\_VRAM\_FREE\_FUNC\_SYMBOL  | Failed to load the FREE function export\.  | 
|  0x01000009  | STATUS\_HEAP\_VRAM\_LOCK\_FUNC\_SYMBOL  | Failed to load the LOCK function export\.  | 
|  0x0100000a  | STATUS\_HEAP\_VRAM\_UNLOCK\_FUNC\_SYMBOL  | Failed to load the UNLOCK function export\.  | 
|  0x0100000b  | STATUS\_HEAP\_VRAM\_UNINIT\_FUNC\_SYMBOL  | Failed to load the UNINIT function export\.  | 
|  0x0100000c  | STATUS\_HEAP\_VRAM\_GETMAX\_FUNC\_SYMBOL  | Failed to load the GETMAX function export\.  | 
|  0x0100000d  | STATUS\_HEAP\_DIRECT\_MEM\_INIT  | Failed to initialize the main heap pool in the hybrid heap\.  | 
|  0x0100000e  | STATUS\_HEAP\_VRAM\_INIT\_FAILED  | The VRAM dynamic initialization failed\.  | 
|  0x0100000f  | STATUS\_HEAP\_LIBRARY\_FREE\_FAILED  | Failed to de\-allocate and free the VRAM library\.  | 
|  0x01000010  | STATUS\_HEAP\_VRAM\_ALLOC\_FAILED  | The VRAM allocation failed\.  | 
|  0x01000011  | STATUS\_HEAP\_VRAM\_FREE\_FAILED  | The VRAM free failed\.  | 
|  0x01000012  | STATUS\_HEAP\_VRAM\_MAP\_FAILED  | The VRAM map failed\.  | 
|  0x01000013  | STATUS\_HEAP\_VRAM\_UNMAP\_FAILED  | The VRAM unmap failed\.  | 
|  0x01000014  | STATUS\_HEAP\_VRAM\_UNINIT\_FAILED  | The VRAM deinitialization failed\.  | 

### Error and Status Codes Returned by the MKVGen Library<a name="producer-sdk-errors-mkvgen"></a>

The following table contains error and status information that is returned by methods in the `MKVGen` library\.


****  

| Code | Message | Description / Recommended Action | 
| --- | --- | --- | 
|  0x32000001  | STATUS\_MKV\_INVALID\_FRAME\_DATA  | Invalid members of the Frame data structure\. Ensure that the duration, size, and frame data are valid and are within the limits specified in [Producer SDK Limits](producer-sdk-limits.md)\.  | 
|  0x32000002  | STATUS\_MKV\_INVALID\_FRAME\_TIMESTAMP  | Invalid frame timestamp\. The calculated PTS \(presentation timestamp\) and DTS \(decoding timestamp\) are greater or equal to the timestamp of the start frame of the fragment\. This is an indication of a potential media pipeline restart or an encoder stability issue\. For troubleshooting information, see [Error: "Failed to submit frame to Kinesis Video client"](troubleshooting.md#troubleshooting-producer-failed-frame-client) | 
|  0x32000003  | STATUS\_MKV\_INVALID\_CLUSTER\_DURATION  | An invalid fragment duration was specified\. For more information, see [Producer SDK Limits](producer-sdk-limits.md)\.  | 
|  0x32000004  | STATUS\_MKV\_INVALID\_CONTENT\_TYPE\_LENGTH  | Invalid content type string length\. For more information, see [Producer SDK Limits](producer-sdk-limits.md)\.  | 
|  0x32000005  | STATUS\_MKV\_NUMBER\_TOO\_BIG  | There was an attempt to encode a number that's too large to be represented in EBML \(Extensible Binary Meta Language\) format\. This should not be exposed to the SDK clients\. | 
|  0x32000006  | STATUS\_MKV\_INVALID\_CODEC\_ID\_LENGTH  | Invalid codec ID string length\. For more information, see [Producer SDK Limits](producer-sdk-limits.md)\.  | 
|  0x32000007  | STATUS\_MKV\_INVALID\_TRACK\_NAME\_LENGTH  | Invalid track name string length\. For more information, see [Producer SDK Limits](producer-sdk-limits.md)\.  | 
|  0x32000008  | STATUS\_MKV\_INVALID\_CODEC\_PRIVATE\_LENGTH  | Invalid codec private data length\. For more information, see [Producer SDK Limits](producer-sdk-limits.md)\.  | 
|  0x32000009  | STATUS\_MKV\_CODEC\_PRIVATE\_NULL  | The codec private data \(CPD\) is NULL, whereas the CPD size is greater than 0\.  | 
|  0x3200000a  | STATUS\_MKV\_INVALID\_TIMECODE\_SCALE  | Invalid timecode scale value\. For more information, see [Producer SDK Limits](producer-sdk-limits.md)\.  | 
|  0x3200000b  | STATUS\_MKV\_MAX\_FRAME\_TIMECODE  | The frame timecode is greater than the maximum\. For more information, see [Producer SDK Limits](producer-sdk-limits.md)\.  | 
|  0x3200000c  | STATUS\_MKV\_LARGE\_FRAME\_TIMECODE  | The max frame timecode was reached\. The MKV format uses signed 16 bits to represent the relative timecode of the frame to the beginning of the cluster\. The error is generated if the frame timecode cannot be represented\. This error indicates either a bad timecode scale selection or the cluster duration is too long, so representing the frame timecode overflows the signed 16\-bit space\.  | 
|  0x3200000d  | STATUS\_MKV\_INVALID\_ANNEXB\_NALU\_IN\_FRAME\_DATA  | An invalid Annex\-B start code was encountered\. For example, the Annex\-B adaptation flag was specified and the code encounters an invalid start sequence of more than three zeroes\. A valid Annex\-B format should have an "emulation prevention" sequence to escape a sequence of three or more zeroes in the bytestream\. For more information, see the MPEG specification\. For information about this error on Android, see [STATUS\_MKV\_INVALID\_ANNEXB\_NALU\_IN\_FRAME\_DATA \(0x3200000d\) error on Android](troubleshooting.md#troubleshooting-producer-android-invalid-annexb)\. | 
|  0x3200000e  | STATUS\_MKV\_INVALID\_AVCC\_NALU\_IN\_FRAME\_DATA  | Invalid AVCC NALu packaging when the adapting AVCC flag is specified\. Ensure that the bytestream is in a valid AVCC format\. For more information, see the MPEG specification\.  | 
|  0x3200000f  | STATUS\_MKV\_BOTH\_ANNEXB\_AND\_AVCC\_SPECIFIED  | Both adapting AVCC and Annex\-B NALs were specified\. Specify either one, or specify none\.  | 
|  0x32000010  | STATUS\_MKV\_INVALID\_ANNEXB\_NALU\_IN\_CPD  | Invalid Annex\-B format of CPD when the adapting Annex\-B flag is specified\. Ensure that the CPD is in valid Annex\-B format\. If it is not, then remove the CPD Annex\-B adaptation flag\.  | 
|  0x32000011  | STATUS\_MKV\_PTS\_DTS\_ARE\_NOT\_SAME  | Kinesis Video Streams enforces the PTS \(presentation timestamp\) and DTS \(decoding timestamp\) to be the same for the fragment start frames\. These are the key frames that start the fragment\.  | 
|  0x32000012  | STATUS\_MKV\_INVALID\_H264\_H265\_CPD  | Failed to parse H264/H265 codec private data\.  | 
|  0x32000013  | STATUS\_MKV\_INVALID\_H264\_H265\_SPS\_WIDTH  | Failed to extract the width from the codec private data\.  | 
|  0x32000014  | STATUS\_MKV\_INVALID\_H264\_H265\_SPS\_HEIGHT  | Failed to extract the height from codec private data\.  | 
|  0x32000015  | STATUS\_MKV\_INVALID\_H264\_H265\_SPS\_NALU  | Invalid H264/H265 SPS NALu\. | 
|  0x32000016  | STATUS\_MKV\_INVALID\_BIH\_CPD  | Invalid bitmap info header format in the codec private data\.  | 
|  0x32000017  | STATUS\_MKV\_INVALID\_HEVC\_NALU\_COUNT  | Invalid High Efficiency Video Coding \(HEVC\) Network Abstraction Layer units \(NALU\) count\.  | 
|  0x32000018  | STATUS\_MKV\_INVALID\_HEVC\_FORMAT  | Invalid HEVC format\. | 
|  0x32000019  | STATUS\_MKV\_HEVC\_SPS\_NALU\_MISSING  | Missing HEVC NALUs in the Sequence Parameter Set \(SPS\)\. | 
|  0x3200001a  | STATUS\_MKV\_INVALID\_HEVC\_SPS\_NALU\_SIZE   | Invalid HEVC SPS NALU size\. | 
|  0x3200001b  | STATUS\_MKV\_INVALID\_HEVC\_SPS\_CHROMA\_FORMAT\_IDC   | Invalid Chroma format IDC\. | 
|  0x3200001c  | STATUS\_MKV\_INVALID\_HEVC\_SPS\_RESERVED   | Invalid HEVC reserved SPS\. | 
|  0x3200001d  | STATUS\_MKV\_MIN\_ANNEX\_B\_CPD\_SIZE   | Minimum Annex\-b codec private beta value size\. For H264, this value must be equal to or greater that 11\. For H265, this value must be equal to or greater than 15\. | 
|  0x3200001e  | STATUS\_MKV\_ANNEXB\_CPD\_MISSING\_NALUS  | Missing codec private data in Annex\-B NALUs\. | 
|  0x3200001f  | STATUS\_MKV\_INVALID\_ANNEXB\_CPD\_NALUS  | Invalid codec private beta in Annex\-B NALUs\. | 
|  0x32000020  | STATUS\_MKV\_INVALID\_TAG\_NAME\_LENGTH   | Invalid tag name length\. Valid value is greater than zero and less than 128\. | 
|  0x32000021  | STATUS\_MKV\_INVALID\_TAG\_VALUE\_LENGTH   | Invalid tag value length\. valid value is greater than zero and less than 256\. | 
|  0x32000022  | STATUS\_MKV\_INVALID\_GENERATOR\_STATE\_TAGS   | Invalid generator state tags\. | 
|  0x32000023  | STATUS\_MKV\_INVALID\_AAC\_CPD\_SAMPLING\_FREQUENCY\_INDEX   | Invalid AAC codec private data sampling frequency index\. | 
|  0x32000024  | STATUS\_MKV\_INVALID\_AAC\_CPD\_CHANNEL\_CONFIG   | Invalid AAC codec private data channel configuration\. | 
|  0x32000025  | STATUS\_MKV\_INVALID\_AAC\_CPD   | Invalid AAC codec private data\. | 
|  0x32000026  | STATUS\_MKV\_TRACK\_INFO\_NOT\_FOUND   | Track information not found\. | 
|  0x32000027  | STATUS\_MKV\_INVALID\_SEGMENT\_UUID   | Invalid segment UUID\. | 
|  0x32000028  | STATUS\_MKV\_INVALID\_TRACK\_UID   | Invalid track UID\. | 

### Error and Status Codes Returned by the Trace Library<a name="producer-sdk-errors-trace"></a>

The following table contains error and status information that is returned by methods in the `Trace` library\.


****  

| Code | Message | 
| --- | --- | 
| 0x10100001 | STATUS\_MIN\_PROFILER\_BUFFER  | 

### Error and Status Codes Returned by the Utils Library<a name="producer-sdk-errors-utils"></a>

The following table contains error and status information that is returned by methods in the `Utils` library\.


****  

| Code | Message | 
| --- | --- | 
| 0x40000001 | STATUS\_INVALID\_BASE64\_ENCODE  | 
| 0x40000002 | STATUS\_INVALID\_BASE  | 
| 0x40000003 | STATUS\_INVALID\_DIGIT  | 
| 0x40000004 | STATUS\_INT\_OVERFLOW  | 
| 0x40000005 | STATUS\_EMPTY\_STRING  | 
| 0x40000006 | STATUS\_DIRECTORY\_OPEN\_FAILED  | 
| 0x40000007 | STATUS\_PATH\_TOO\_LONG  | 
| 0x40000008 | STATUS\_UNKNOWN\_DIR\_ENTRY\_TYPE  | 
| 0x40000009 | STATUS\_REMOVE\_DIRECTORY\_FAILED  | 
| 0x4000000a | STATUS\_REMOVE\_FILE\_FAILED  | 
| 0x4000000b | STATUS\_REMOVE\_LINK\_FAILED  | 
| 0x4000000c | STATUS\_DIRECTORY\_ACCESS\_DENIED  | 
| 0x4000000d | STATUS\_DIRECTORY\_MISSING\_PATH  | 
| 0x4000000e | STATUS\_DIRECTORY\_ENTRY\_STAT\_ERROR  | 

### Error and Status Codes Returned by the View Library<a name="producer-sdk-errors-view"></a>

The following table contains error and status information that is returned by methods in the `View` library\.


****  

| Code | Message | Description | 
| --- | --- | --- | 
|  0x30000001  | STATUS\_MIN\_CONTENT\_VIEW\_ITEMS  | An invalid content view item count was specified\. For more information, see [Producer SDK Limits](producer-sdk-limits.md)\.  | 
|  0x30000002  | STATUS\_INVALID\_CONTENT\_VIEW\_DURATION  | An invalid content view duration was specified\. For more information, see [Producer SDK Limits](producer-sdk-limits.md)\.  | 
|  0x30000003  | STATUS\_CONTENT\_VIEW\_NO\_MORE\_ITEMS  | An attempt was made to get past the head position\. | 
|  0x30000004  | STATUS\_CONTENT\_VIEW\_INVALID\_INDEX  | An invalid index is specified\. | 
|  0x30000005  | STATUS\_CONTENT\_VIEW\_INVALID\_TIMESTAMP  | There was an invalid timestamp or a timestamp overlap\. The frame decoding timestamp should be greater or equal to the previous frame timestamp, plus the previous frame duration: `DTS\(n\) >= DTS\(n\-1\) \+ Duration\(n\-1\)`\. This error often indicates an "unstable" encoder\. The encoder produces a burst of encoded frames, and their timestamps are smaller than the intra\-frame durations\. Or the stream is configured to use SDK timestamps, and the frames are sent faster than the frame durations\. To help with some "jitter" in the encoder, specify a smaller frame duration in the StreamInfo\.StreamCaps structure\. For example, if the stream is 25FPS, each frame's duration is 40 ms\. However, to handle the encoder jitter, we recommend that you use half of that frame duration \(20 ms\)\. Some streams require more precise control over the timing for error detection\.  | 
|  0x30000006  |  STATUS\_INVALID\_CONTENT\_VIEW\_LENGTH  | An invalid content view item data length was specified\.  | 

## Errors and Status Codes Returned by PutFrame Callbacks \- C Producer Library<a name="producer-sdk-errors-putframe-c"></a>

The following section contains error and status information that is returned by callbacks for the `PutFrame` operation within the C producer library\.


****  

| Code | Message | Description | Recommended Action | 
| --- | --- | --- | --- | 
| 0x15000001 | STATUS\_STOP\_CALLBACK\_CHAIN | The callback chain has stopped\. |  | 
| 0x15000002 | STATUS\_MAX\_CALLBACK\_CHAIN | The maximum callback chain was reached\. |  | 
| 0x15000003 | STATUS\_INVALID\_PLATFORM\_CALLBACKS\_VERSION | Invalid PlatformCallbacks structure version\. | Specify the correct current version of the structure\. | 
| 0x15000004 | STATUS\_INVALID\_PRODUCER\_CALLBACKS\_VERSION | Invalid ProducerCallbacks structure version\. | Specify the correct current version of the structure\. | 
| 0x15000005 | STATUS\_INVALID\_STREAM\_CALLBACKS\_VERSION | Invalid StreamCallbacks structure version\. | Specify the correct current version of the structure\. | 
| 0x15000006 | STATUS\_INVALID\_AUTH\_CALLBACKS\_VERSION | Invalid AuthCallbacks structure version\. | Specify the correct current version of the structure\. | 
| 0x15000007 | STATUS\_INVALID\_API\_CALLBACKS\_VERSION | Invalid ApiCallbacks structure version\. | Specify the correct current version of the structure\. | 
| 0x15000008 | STATUS\_INVALID\_AWS\_CREDENTIALS\_VERSION | Invalid AwsCredentials structure version\. | Specify the correct current version of the structure\. | 
| 0x15000009 | STATUS\_MAX\_REQUEST\_HEADER\_COUNT | The maximum request header count was reached\. |  | 
| 0x1500000a | STATUS\_MAX\_REQUEST\_HEADER\_NAME\_LEN | The maximum request header name length was reached\. |  | 
| 0x1500000b | STATUS\_MAX\_REQUEST\_HEADER\_VALUE\_LEN | The maximum request header value length was reached\. |  | 
| 0x1500000c | STATUS\_INVALID\_API\_CALL\_RETURN\_JSON | Invalid return JSON for an API call\. |  | 
| 0x1500000d | STATUS\_CURL\_INIT\_FAILED | Curl initialization failed\. |  | 
| 0x1500000e | STATUS\_CURL\_LIBRARY\_INIT\_FAILED | Curl lib initialization failed\. |  | 
| 0x1500000f | STATUS\_INVALID\_DESCRIBE\_STREAM\_RETURN\_JSON | Invalid return JSON for DescribeStream\. |  | 
| 0x150000010 | STATUS\_HMAC\_GENERATION\_ERROR | HMAC generation error\. |  | 
| 0x150000011 | STATUS\_IOT\_FAILED | IOT authorization failed\. |  | 
| 0x150000012 | STATUS\_MAX\_ROLE\_ALIAS\_LEN\_EXCEEDED | The maximum role alias length was reached\. | Specify a shorter alias length\. | 
| 0x150000013 | STATUS\_MAX\_USER\_AGENT\_NAME\_POSTFIX\_LEN\_EXCEEDED | The maximum agent name postfix length was reached\. |  | 
| 0x150000014 | STATUS\_MAX\_CUSTOM\_USER\_AGENT\_LEN\_EXCEEDED | The maximum customer user agent length was reached\. |  | 
| 0x150000015 | STATUS\_INVALID\_USER\_AGENT\_LENGTH | Invalid user agent length\.  |  | 
| 0x150000016 | STATUS\_INVALID\_ENDPOINT\_CACHING\_PERIOD | Invalid endpoint caching period\. | Specify a caching period that is less than 24 hours\. | 
| 0x150000017 | STATUS\_IOT\_EXPIRATION\_OCCURS\_IN\_PAST | IOT expiration timestamp occurs in the past\. |  | 
| 0x150000018 | STATUS\_IOT\_EXPIRATION\_PARSING\_FAILED | The IOT expiration parsing has failed\. |  | 