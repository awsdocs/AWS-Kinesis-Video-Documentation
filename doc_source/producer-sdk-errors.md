# Error Code Reference<a name="producer-sdk-errors"></a>

This section contains error and status code information for the [Producer Libraries](producer-sdk.md)\.

## Errors and Status Codes Returned by PutFrame Callbacks<a name="producer-sdk-errors-putframe"></a>

The following sections contain error and status information returned by callbacks for the `PutFrame` operation\.


+ [Error and Status Codes Returned by the Client Library](#producer-sdk-errors-client)
+ [Error and Status Codes Returned by the Duration Library](#producer-sdk-errors-duration)
+ [Error and Status Codes Returned by the Common Library](#producer-sdk-errors-common)
+ [Error and Status Codes Returned by the Heap Library](#producer-sdk-errors-heap)
+ [Error and Status Codes Returned by the MKVGen Library](#producer-sdk-errors-mkvgen)
+ [Error and Status Codes Returned by the Trace Library](#producer-sdk-errors-trace)
+ [Error and Status Codes Returned by the Utils Library](#producer-sdk-errors-utils)
+ [Error and Status Codes Returned by the View Library](#producer-sdk-errors-view)

### Error and Status Codes Returned by the Client Library<a name="producer-sdk-errors-client"></a>

The following table contains error and status information returned by methods in the `Client` library\.


****  

| Code | Message | Notes | Troubleshooting |
| --- | --- | --- | --- | 
| 0x52000001 | STATUS\_MAX\_STREAM\_COUNT  | Max stream count reached | Specify a larger max stream count in the DeviceInfo structure less than specified in the [limits document](producer-sdk-limits.md) |
| 0x52000002 | STATUS\_MIN\_STREAM\_COUNT  | Min stream count error| Specify the max number of streams greater than 0 in the DeviceInfo |
| 0x52000003 | STATUS\_INVALID\_DEVICE\_NAME\_LENGTH  | Invalid device name length | Max device name length in characters is specified in the [limits document](producer-sdk-limits.md) |
| 0x52000004 | STATUS\_INVALID\_DEVICE\_INFO\_VERSION  | Invalid DeviceInfo structure version | Specify the correct current version of the structure |
| 0x52000005 | STATUS\_MAX\_TAG\_COUNT  | Max tag count reached | Current max tag count is specified in the [limits document](producer-sdk-limits.md) |
| 0x52000006 | STATUS\_DEVICE\_FINGERPRINT\_LENGTH  | 
| 0x52000007 | STATUS\_INVALID\_CALLBACKS\_VERSION  | Invalid Callbacks structure version | Specify the correct current version of the structure |
| 0x52000008 | STATUS\_INVALID\_STREAM\_INFO\_VERSION  | Invalid StreamInfo structure version | Specify the correct current version of the structure |
| 0x52000009 | STATUS\_INVALID\_STREAM\_NAME\_LENGTH  | Invalid stream name length | Max stream name length in characters is specified in the [limits document](producer-sdk-limits.md) |
| 0x5200000a | STATUS\_INVALID\_STORAGE\_SIZE  | Invalid storage size specified | Storage size in bytes should be within the limits specified in the [limits document](producer-sdk-limits.md) |
| 0x5200000b | STATUS\_INVALID\_ROOT\_DIRECTORY\_LENGTH  | Invalid root directory string length | The max root directory path length is specified in the [limits document](producer-sdk-limits.md) |
| 0x5200000c | STATUS\_INVALID\_SPILL\_RATIO  | Invalid spill ratio | The spill ratio should be expressed as a percent from 0 - 100 |
| 0x5200000d | STATUS\_INVALID\_STORAGE\_INFO\_VERSION  | Invalid StorageInfo structure version | Specify the correct current version of the structure |
| 0x5200000e | STATUS\_INVALID\_STREAM\_STATE  | Stream is in a state that the current operation is not permitted | Most commonly this error happens when the SDK fails to reach to the desired state in order to perform the requested operation. For example, if the GetStreamingEndpoing API call fails and the client application ignores it and continues on to putting frame into the stream. |
| 0x5200000f | STATUS\_SERVICE\_CALL\_CALLBACKS\_MISSING  | Callbacks structure has a missing function entry points for some mandatory functions | Ensure the mandatory callbacks are implemented in the client application. This error is only exposed to the PIC clients as the C++ and other higher-level wrappers satisfy these calls. |
| 0x52000010 | STATUS\_SERVICE\_CALL\_NOT\_AUTHORIZED\_ERROR  | Not authorized | Verify the security token/certificate/security token integration/expiration. Ensure the token has the correct associated rights with it. For the sample applications, ensure the environment variable is set correctly. |
| 0x52000011 | STATUS\_DESCRIBE\_STREAM\_CALL\_FAILED  | Describe stream API failure | This error is returned after the DescribeStream API retry failure. The PIC will return this error after it gives up retrying. |
| 0x52000012 | STATUS\_INVALID\_DESCRIBE\_STREAM\_RESPONSE  | Invalid DescribeStreamResponse structure | THe structrure passed to the DescribeStreamResultEvent is either null or contains invalid items like null ARN. |
| 0x52000013 | STATUS\_STREAM\_IS\_BEING\_DELETED\_ERROR  | Stream is being deleted | An API failure caused by the stream being deleted. Make sure there are no other processes attempting to delete the stream while the stream is in use. |
| 0x52000014 | STATUS\_SERVICE\_CALL\_INVALID\_ARG\_ERROR  | Invalid arguments have been specified for the service call | The backend will return this error if any of the service call arguments are not valid or when the SDK encounters an error that it can't interpret. |
| 0x52000015 | STATUS\_SERVICE\_CALL\_DEVICE\_NOT\_FOND\_ERROR  | Device not found | Ensure the device is not deleted while in use. |
| 0x52000016 | STATUS\_SERVICE\_CALL\_DEVICE\_NOT\_PROVISIONED\_ERROR  | Device not provisioned | Ensure the device has been provisioned. |
| 0x52000017 | STATUS\_SERVICE\_CALL\_RESOURCE\_NOT\_FOUND\_ERROR  | Generic resource not found returned from the service | This error occurs when the service can not locate the resource (for example a stream). This might mean different things in different contexts but the likely cause is the usage of APIs when the stream is not created yet. Using SDK will ensure the stream is created first. |
| 0x52000018 | STATUS\_INVALID\_AUTH\_LEN  | Invalid auth info length | Current values are specified in the [limits document](producer-sdk-limits.md) |
| 0x52000019 | STATUS\_CREATE\_STREAM\_CALL\_FAILED  | CreateStream API call failed | The error string will contain more detailed information why the operation failed |
| 0x5200002a | STATUS\_GET\_STREAMING\_TOKEN\_CALL\_FAILED  | GetStreamingToken call failed | The error string will contain more detailed information why the operation failed |
| 0x5200002b | STATUS\_GET\_STREAMING\_ENDPOINT\_CALL\_FAILED  | GetStreamingEndpoint API call failed | The error string will contain more detailed information why the operation failed |
| 0x5200002c | STATUS\_INVALID\_URI\_LEN  | Invalid URI string lenghth returned from GetStreamingEndpoint API | Current max values are specified in the [limits document](producer-sdk-limits.md) |
| 0x5200002d | STATUS\_PUT\_STREAM\_CALL\_FAILED  | PutMedia API call failed | The error string will contain more detailed information why the operation failed |
| 0x5200002e | STATUS\_STORE\_OUT\_OF\_MEMORY  | Content store OOM | Content store is out-of-memory. Content store is shared between the streams and should have adequate capacity to store the max durations for all the streams + ~20% (accounting for the defragmentation). It's important to not overflow the storage and select correct values for the max duration per stream which would correspond to the cummulative storage size and the latency tolerances. The reason is's better to drop the frames as they fall out of the content view window vs just being put (content store memory pressure) is because the former will trigger the stream pressure notification callbacks and the application can adjust the upstream media components (like the encoder) to thin the bitrate, drop frames or act accordingly. |
| 0x5200002f | STATUS\_NO\_MORE\_DATA\_AVAILABLE  | No more data avilable currently for a stream | This is one of the potential valid results in case the media pipeline produces slower than the networking thread consumes the frames to be sent to the service. Higher-level clients (C++, Java, Android, etc..) will not see this warning as it's handled internally. |
| 0x52000030 | STATUS\_INVALID\_TAG\_VERSION  | Invalid Tag structure version | Specify the correct current version of the structure |
| 0x52000031 | STATUS\_SERVICE\_CALL\_UNKOWN\_ERROR  | Unknown/Generic error returned from the networking stack | Unknown error or a generic error returned from the networking stack. The more detailed information can be found in the logs |
| 0x52000032 | STATUS\_SERVICE\_CALL\_RESOURCE\_IN\_USE\_ERROR  | Resource in use | Returned from the service. See the API reference guide for more information |
| 0x52000033 | STATUS\_SERVICE\_CALL\_CLIENT\_LIMIT\_ERROR  | Client limit | Returned from the service. See the API reference guide for more information |
| 0x52000034 | STATUS\_SERVICE\_CALL\_DEVICE\_LIMIT\_ERROR  | Device limit | Returned from the service. See the API reference guide for more information |
| 0x52000035 | STATUS\_SERVICE\_CALL\_STREAM\_LIMIT\_ERROR  | Stream limit | Returned from the service. See the API reference guide for more information |
| 0x52000036 | STATUS\_SERVICE\_CALL\_RESOURCE\_DELETED\_ERROR  | Resource deleted or is being deleted | Returned from the service. See the API reference guide for more information |
| 0x52000037 | STATUS\_SERVICE\_CALL\_TIMEOUT\_ERROR  | Service call timed out | Timeout calling a particular service API. Ensure a valid network connection. PIC will attemp to retry. |
| 0x52000038 | STATUS\_STREAM\_READY\_CALLBACK\_FAILED  | Stream ready notification | The notification is sent from PIC to the client indicating the async stream creation is finished successfully |
| 0x52000039 | STATUS\_DEVICE\_TAGS\_COUNT\_NON\_ZERO\_TAGS\_NULL  | Invalid tags specified | Tag count is not zero but the tags are empty. Please ensure the tags are specified or the count is zero |
| 0x5200003a | STATUS\_INVALID\_STREAM\_DESCRIPTION\_VERSION  | Invalid StreamDescription structure version | Specify the correct current version of the structure |
| 0x5200003b | STATUS\_INVALID\_TAG\_NAME\_LEN  | Invalid tag name length | The limits for the tag name are specified in the [limits document](producer-sdk-limits.md) |
| 0x5200003c | STATUS\_INVALID\_TAG\_VALUE\_LEN  | Invalid tag value length | The limits for the tag value are specified in the [limits document](producer-sdk-limits.md) |
| 0x5200003d | STATUS\_TAG\_STREAM\_CALL\_FAILED  | TagResource API failed | TagResource API call failed. Check for a valid network connection. Logs should contain more information about the failure |
| 0x5200003e | STATUS\_INVALID\_CUSTOM\_DATA  | Invalid custom data calling PIC APIs | Invalid custom data has been specified in a call to PIC APIs. This can only happen in the clients that directly use PIC. |
| 0x5200003f | STATUS\_INVALID\_CREATE\_STREAM\_RESPONSE  | Invalid CreateStreamResponse structure | The structure or it's member fields are invalid (i.e. ARN is null or larger than what's specified in the limits document) |
| 0x52000040 | STATUS\_CLIENT\_AUTH\_CALL\_FAILED  | Client auth failed | PIC failed to get proper auth information (i.e. AccessKeyId or SecretAccessKey) after a number of retries. Check the authentication integration. Sample applications utilize environment variables to pass in AccessKeyId/SecretAccessKey/SessionToken information to the C++ SDK. |
| 0x52000041 | STATUS\_GET\_CLIENT\_TOKEN\_CALL\_FAILED  | Getting the security token call failed | This can happen for clients that use PIC directly. After a number of retries it fails with this error. |
| 0x52000042 | STATUS\_CLIENT\_PROVISION\_CALL\_FAILED  | Provisioning error | Provisioning is not implemented |
| 0x52000043 | STATUS\_CREATE\_CLIENT\_CALL\_FAILED  | Failed to create the producer client | This is a generic error returned by the PIC after a number of retries when the client creation fails. |
| 0x52000044 | STATUS\_CLIENT\_READY\_CALLBACK\_FAILED  | Failed to get the producer client to READY state | This error is returned by the PIC state machine if the PIC fails to move to the READY state. More information can be found in the logs about the root cause. |
| 0x52000045 | STATUS\_TAG\_CLIENT\_CALL\_FAILED  | TagResource for the producer client failed | TagResource API call failed for the producer client. The logs will contain more information about the root cause. |
| 0x52000046 | STATUS\_INVALID\_CREATE\_DEVICE\_RESPONSE  | Device/Producer creation failed | Higher level SDKs (C++/Java/etc) do not implement device/producer creation API yet. Clients which utilize PIC directly can indicate a failure using the result notification. |
| 0x52000047 | STATUS\_ACK\_TIMESTAMP\_NOT\_IN\_VIEW\_WINDOW  | Timestamp of the received ACK is not in the view | This can occure if the frame corresponding to the received ACK fall out of the content view window. Generally, this can happen if the ACK delivery is slow. This can be interpreted as a warning and an indication that the downlink is slow. |
| 0x52000048 | STATUS\_INVALID\_FRAGMENT\_ACK\_VERSION  | Invalid FragmentAck structure version | Specify the correct current version of the structure |
| 0x52000049 | STATUS\_INVALID\_TOKEN\_EXPIRATION  | Invalid security token expiration | Security token expiration should have an absolute timestamp in the future greater than the current timestamp with grace period. The limits for the grace period are specified in the [limits document](producer-sdk-limits.md) |
| 0x5200004a | STATUS\_END\_OF\_STREAM  | EOS indicator | This is an indicator in the GetStreamData API call indicating that the current upload handle session has ended. This could happen if the session ends/errors or the session token has expired and the session is being rotated. |
| 0x5200004b | STATUS\_DUPLICATE\_STREAM\_NAME  | Duplicate stream name | Can not create multiple streams with the same stream name. Please select a different stream name. |
| 0x5200004c | STATUS\_INVALID\_RETENTION\_PERIOD  | Invalid retention period | Invalid retention period is specified in the StreamInfo structure. Please refer to [limits document](producer-sdk-limits.md) for more information on the valid range of values for the retention period. |
| 0x5200004d | STATUS\_INVALID\_ACK\_KEY\_START  | Invalid FragmentAck | Failed to parse the Fragment ACK string. Invalid key start indicator. Fragment ACK string can be damaged. It can be self-corrected and can be treated as a warning. |
| 0x5200004e | STATUS\_INVALID\_ACK\_DUPLICATE\_KEY\_NAME  | Invalid FragmentAck | Failed to parse the Fragment ACK string. Multiple keys with the same name. Fragment ACK string can be damaged. It can be self-corrected and can be treated as a warning. |
| 0x5200004f | STATUS\_INVALID\_ACK\_INVALID\_VALUE\_START  | Invalid FragmentAck | Failed to parse the Fragment ACK string. Invalid key value start indicator. Fragment ACK string can be damaged. It can be self-corrected and can be treated as a warning. |
| 0x52000050 | STATUS\_INVALID\_ACK\_INVALID\_VALUE\_END  | Invalid FragmentAck | Failed to parse the Fragment ACK string. Invalid key value end indicator. Fragment ACK string can be damaged. It can be self-corrected and can be treated as a warning. |
| 0x52000051 | STATUS\_INVALID\_PARSED\_ACK\_TYPE  | Invalid FragmentAck | Failed to parse the Fragment ACK string. Invalid ACK type specified. |
| 0x52000052 | STATUS\_STREAM\_HAS\_BEEN\_STOPPED  | Stream stopped | Stream has been stopped/terminated but a frame is still being put into the stream. |
| 0x52000053 | STATUS\_INVALID\_STREAM\_METRICS\_VERSION  | Invalid StreamMetrics structure version | Specify the correct current version of the structure |
| 0x52000054 | STATUS\_INVALID\_CLIENT\_METRICS\_VERSION  | Invalid ClientMetrics structure version | Specify the correct current version of the structure |
| 0x52000055 | STATUS\_INVALID\_CLIENT\_READY\_STATE  | Producer initialization failed to reach READY state | Failed to reach the READY state during the producer client initialization. Logs should contain more information. |
| 0x52000056 | STATUS\_STATE\_MACHINE\_STATE\_NOT\_FOUND  | Internal state machine error | Not a publicly visible error. |
| 0x52000057 | STATUS\_INVALID\_FRAGMENT\_ACK\_TYPE  | Invalid ACK type is specified in the FragmentAck structure | FragmentAck structure should contain ACK types defined in the public header. |
| 0x52000058 | STATUS\_INVALID\_STREAM\_READY\_STATE  | Internal state machine transition error | Not a publicly visible error. |
| 0x52000059 | STATUS\_CLIENT\_FREED\_BEFORE\_STREAM  | Stream object being freed after the producer has been freed | An attempt to free a stream object after the producer object has been freed previously. This can only happen in clients which directly use PIC. |
| 0x5200005a | STATUS\_ALLOCATION\_SIZE\_SMALLER\_THAN\_REQUESTED  | Internal storage error | An internal error indicating that the actual allocation size from the content store is smaller than the size of the packaged frame/fragment. |
| 0x5200005b | STATUS\_VIEW\_ITEM\_SIZE\_GREATER\_THAN\_ALLOCATION  | Internal storage error | Stored size of the allocation in the content view is greater than the allocation size in the content store. |
| 0x5200005c | STATUS\_ACK\_ERR\_STREAM\_READ\_ERROR  | Stream read error ACK | An error ACK returned from the backend indicating any stream read/parsing error. This generally happens when the backend fails to retrieve the stream. Auto-restreaming will generally be able to correct this error. |
| 0x5200005d | STATUS\_ACK\_ERR\_FRAGMENT\_SIZE\_REACHED  | Max fragment size reached | Max fragment size in bytes reached which is defined in [limits document](producer-sdk-limits.md). This is an indication of either a very large frames or an absense of key-frames to create manageable size fragments. Check the encoder settings and ensure key-frames are being produced properly. For streams which have very high density the encoder should be configured to produce fragments at a smaller durations to manage the max size. |
| 0x5200005e | STATUS\_ACK\_ERR\_FRAGMENT\_DURATION\_REACHED  | Max fragment duration reached | Max fragment duration reached which is defined in [limits document](producer-sdk-limits.md). This is an indication of either a very low frames per second or an absense of key-frames to create manageable duration fragments. Check the encoder settings and ensure key-frames are being produced properly at the regular intervals. |
| 0x5200005f | STATUS\_ACK\_ERR\_CONNECTION\_DURATION\_REACHED  | Max connection duration reached | KVS enforces max connection duration specified in the limits document. The Producer SDK automatically rotates the stream/token before that so clients using the SDK should not receive this error. |
| 0x52000060 | STATUS\_ACK\_ERR\_FRAGMENT\_TIMECODE\_NOT\_MONOTONIC  | Timecodes are not monotonicaly increasing | Producer SDK enfoces the timestamps so clients utilizing the SDK should not receive this error. |
| 0x52000061 | STATUS\_ACK\_ERR\_MULTI\_TRACK\_MKV  | Multiple tracks in the MKV | The producer SDK enforces single track streams. Clients utilizing the SDK should not receive this error. |
| 0x52000062 | STATUS\_ACK\_ERR\_INVALID\_MKV\_DATA  | Invalid MKV data | Backend MKV parser encountered an error parsing the stream. Clients using the SDK can encounter this error if the stream gets corrupted in the transition or when the buffer pressures force the SDK to drop tail frames which are partially transmitted. In the latter case the suggested solution would be to reduce the FPS/resolution, increase the compression ratio or in case of a "bursty" network to allow for larger content store and buffer duration to acommodate for the temporary pressures. |
| 0x52000063 | STATUS\_ACK\_ERR\_INVALID\_PRODUCER\_TIMESTAMP  | Invalid producer timestamp | The service will return this error ACK if the producer clock has a large drift into the future. Higher level SDKs (Java/C++/etc...) use some version of the system clock to satisfy the current time callback from PIC. Ensure the system clock is set properly. Clients utilizing the PIC directly should ensure their callback functions return proper timestamp. |
| 0x52000064 | STATUS\_ACK\_ERR\_STREAM\_NOT\_ACTIVE  | Inactive stream | An attempt has been made to call various backend APIs while the stream is not in an "Active" state. This could happen when the client creates the stream and immediately continues on to push frames into it. The SDK handles this scenario through the state machine and recovery mechanism. |
| 0x52000065 | STATUS\_ACK\_ERR\_KMS\_KEY\_ACCESS\_DENIED  | KMS access denied error | Returned when the account has no access to the specified key. |
| 0x52000066 | STATUS\_ACK\_ERR\_KMS\_KEY\_DISABLED  | KMS key is disabled | The specified key has been disabled. |
| 0x52000067 | STATUS\_ACK\_ERR\_KMS\_KEY\_VALIDATION\_ERROR  | KMS key validation error | Generic validation error. Check the KMS documentation for more information. |
| 0x52000068 | STATUS\_ACK\_ERR\_KMS\_KEY\_UNAVAILABLE  | KMS key unavailable | Key unavailable. Check the KMS documentation for more information. |
| 0x52000069 | STATUS\_ACK\_ERR\_KMS\_KEY\_INVALID\_USAGE  | KMS key invalid usage  | KMS key is not configured to be used in this context. Check the KMS documentation for more information. |
| 0x5200006a | STATUS\_ACK\_ERR\_KMS\_KEY\_INVALID\_STATE  | KMS invalid state | Check the KMS documentation for more information. |
| 0x5200006b | STATUS\_ACK\_ERR\_KMS\_KEY\_NOT\_FOUND  | KMS key not found | Key not found. Check the KMS documentation for more information. |
| 0x5200006c | STATUS\_ACK\_ERR\_STREAM\_DELETED  | Stream has been or being deleted | The stream is being deleted by another application or through console. |
| 0x5200006d | STATUS\_ACK\_ERR\_ACK\_INTERNAL\_ERROR  | Internal error | Generic service internal error. |
| 0x5200006e | STATUS\_ACK\_ERR\_FRAGMENT\_ARCHIVAL\_ERROR  | Fragment archival error | This error is returned when the service fails to durably persist and index the fragment. Although rare, this can happen for various reasons. The SDK will re-try sending the fragment by default. |
| 0x5200006f | STATUS\_ACK\_ERR\_UNKNOWN\_ACK\_ERROR  | Unknown error | Service returned an unknown error. |
| 0x52000070 | STATUS\_MISSING\_ERR\_ACK\_ID  | Missing ACK information | ACK parser completed parsing but the FragmentAck information is missing. |
| 0x52000071 | STATUS\_INVALID\_ACK\_SEGMENT\_LEN  | Invalid ACK segment length | Invalid length of ACK segment string has been specified to the ACK parser. Check the [limits document](producer-sdk-limits.md) for more information . |



### Error and Status Codes Returned by the Duration Library<a name="producer-sdk-errors-duration"></a>

The following table contains error and status information returned by methods in the `Duration` library\.


****  

| Code | Message | 
| --- | --- | 
| 0xFFFFFFFFFFFFFFFF | INVALID\_DURATION\_VALUE  | 

### Error and Status Codes Returned by the Common Library<a name="producer-sdk-errors-common"></a>

The following table contains error and status information returned by methods in the `Common` library. These are common to many APIs.\.


****  

| Code | Message | Notes |
| --- | --- | 
| 0x00000001 | STATUS\_NULL\_ARG  | NULL passed for a mandatory argument. |
| 0x00000002 | STATUS\_INVALID\_ARG  | An invalid value specified for an argument. |
| 0x00000003 | STATUS\_INVALID\_ARG\_LEN  Invalid argument length specified. |
| 0x00000004 | STATUS\_NOT\_ENOUGH\_MEMORY  | Could not allocate enough memory. |
| 0x00000005 | STATUS\_BUFFER\_TOO\_SMALL  | Specified buffer size is too small. |
| 0x00000006 | STATUS\_UNEXPECTED\_EOF  | Unexpected End-Of-File reached. |
| 0x00000007 | STATUS\_FORMAT\_ERROR  | Invalid format encountered. |
| 0x00000008 | STATUS\_INVALID\_HANDLE\_ERROR  | Invalid handle value. |
| 0x00000009 | STATUS\_OPEN\_FILE\_FAILED  | Failed to open a file. |
| 0x0000000a | STATUS\_READ\_FILE\_FAILED  | Failed to read from a file. |
| 0x0000000b | STATUS\_WRITE\_TO\_FILE\_FAILED  | Failed to write to a file. |
| 0x0000000c | STATUS\_INTERNAL\_ERROR  | Internal error. Normally, this should not happen. Occurence of this error indicates an SDK or the service API bug. |
| 0x0000000d | STATUS\_INVALID\_OPERATION  | Invalid operation or the operation is not permitted. |
| 0x0000000e | STATUS\_NOT\_IMPLEMENTED  | Feature is not implemented. |
| 0x0000000f | STATUS\_OPERATION\_TIMED\_OUT  | Operation has timed out. |
| 0x00000010 | STATUS\_NOT\_FOUND  | Required resource not found. |

### Error and Status Codes Returned by the Heap Library<a name="producer-sdk-errors-heap"></a>

The following table contains error and status information returned by methods in the `Heap` library\.


****  

| Code | Message | Notes |
| --- | --- | 
| 0x01000001 | STATUS\_HEAP\_FLAGS\_ERROR  | Invalid combination of flags has been specified. |
| 0x01000002 | STATUS\_HEAP\_NOT\_INITIALIZED  | An attempt to perform an operation prior the heap initialization. |
| 0x01000003 | STATUS\_HEAP\_CORRUPTED  | Heap has been corrupted and/or guard band (in debug mode) has been written over. A buffer overflow in the client code can lead to a heap corruption. |
| 0x01000004 | STATUS\_HEAP\_VRAM\_LIB\_MISSING  | VRAM user or kernel mode library can not be loaded or is missing. Check if the underlying platform supports VRAM allocations. | 
| 0x01000005 | STATUS\_HEAP\_VRAM\_LIB\_REOPEN  | Failed to open the VRAM library. |
| 0x01000006 | STATUS\_HEAP\_VRAM\_INIT\_FUNC\_SYMBOL  | Failed to load INIT function export. |
| 0x01000007 | STATUS\_HEAP\_VRAM\_ALLOC\_FUNC\_SYMBOL  | Failed to load ALLOC funciton export. |
| 0x01000008 | STATUS\_HEAP\_VRAM\_FREE\_FUNC\_SYMBOL  | Failed to load FREE funciton export. |
| 0x01000009 | STATUS\_HEAP\_VRAM\_LOCK\_FUNC\_SYMBOL  | Failed to load LOCK funciton export. |
| 0x0100000a | STATUS\_HEAP\_VRAM\_UNLOCK\_FUNC\_SYMBOL  | Failed to load UNLOCK funciton export. |
| 0x0100000b | STATUS\_HEAP\_VRAM\_UNINIT\_FUNC\_SYMBOL  | Failed to load UNINIT funciton export. |
| 0x0100000c | STATUS\_HEAP\_VRAM\_GETMAX\_FUNC\_SYMBOL  | Failed to load GETMAX funciton export. |
| 0x0100000d | STATUS\_HEAP\_DIRECT\_MEM\_INIT  | Failed to initialize the main heap pool in hybrid heap. |
| 0x0100000e | STATUS\_HEAP\_VRAM\_INIT\_FAILED  | VRAM dynamic initialization failed. |
| 0x0100000f | STATUS\_HEAP\_LIBRARY\_FREE\_FAILED  | Failed to de-allocate and free the VRAM library. |
| 0x01000010 | STATUS\_HEAP\_VRAM\_ALLOC\_FAILED  | VRAM allocation failed. |
| 0x01000011 | STATUS\_HEAP\_VRAM\_FREE\_FAILED  | VRAM free failed. |
| 0x01000012 | STATUS\_HEAP\_VRAM\_MAP\_FAILED  | VRAM map failed. |
| 0x01000013 | STATUS\_HEAP\_VRAM\_UNMAP\_FAILED  | VRAM un-map failed. |
| 0x01000014 | STATUS\_HEAP\_VRAM\_UNINIT\_FAILED  | VRAM deinitialization failed. |

### Error and Status Codes Returned by the MKVGen Library<a name="producer-sdk-errors-mkvgen"></a>

The following table contains error and status information returned by methods in the `MKVGen` library\.


****  

| Code | Message | Notes |
| --- | --- | 
| 0x32000001 | STATUS\_MKV\_INVALID\_FRAME\_DATA  | Invalid members of the Frame data structure. Ensure the duration, size, frame data are valid and within the limits specified in the [limits document](producer-sdk-limits.md). |
| 0x32000002 | STATUS\_MKV\_INVALID\_FRAME\_TIMESTAMP  | Invalid frame timestamp. The calculated PTS and DTS (Presentation timestamp and Decoding timestamp) are greater or equal to the timestamp of the start frame of the fragment. This is an indication of a potential media pipeline restart or encoder stability issue. |
| 0x32000003 | STATUS\_MKV\_INVALID\_CLUSTER\_DURATION  | Invalid fragment duration is specified. Check [limits document](producer-sdk-limits.md) for more information. |
| 0x32000004 | STATUS\_MKV\_INVALID\_CONTENT\_TYPE\_LENGTH  | Invalid content type string length. Check [limits document](producer-sdk-limits.md) for more information. |
| 0x32000005 | STATUS\_MKV\_NUMBER\_TOO\_BIG  | An attempt to encode a number that's too big to be represented in EBML format. This should not be exposed to the SDK clients. |
| 0x32000006 | STATUS\_MKV\_INVALID\_CODEC\_ID\_LENGTH  | Invalid codec ID string length. Check [limits document](producer-sdk-limits.md) for more information. |
| 0x32000007 | STATUS\_MKV\_INVALID\_TRACK\_NAME\_LENGTH  | Invalid track name string length. Check [limits document](producer-sdk-limits.md) for more information. |
| 0x32000008 | STATUS\_MKV\_INVALID\_CODEC\_PRIVATE\_LENGTH  | Invalid Codec Private Data length. Check [limits document](producer-sdk-limits.md) for more information. |
| 0x32000009 | STATUS\_MKV\_CODEC\_PRIVATE\_NULL  | Codec Private Data (CPD) is NULL whereas the CPD size is greater than 0. |
| 0x3200000a | STATUS\_MKV\_INVALID\_TIMECODE\_SCALE  | Invalid timecode scale value. Please refer to [limits document](producer-sdk-limits.md) for more information. |
| 0x3200000b | STATUS\_MKV\_MAX\_FRAME\_TIMECODE  | Frame timecode greater than max. Please refer to [limits document](producer-sdk-limits.md) for more information. |
| 0x3200000c | STATUS\_MKV\_LARGE\_FRAME\_TIMECODE  | Max frame timecode reached. MKV uses signed 16 bits to represent the relative timecode of the frame to the beginning of the cluster. The error is generated if the frame timecode can not be represented. This is an indication of either bad timecode scale selection or the cluster duration is too long so representing the frame timecode overflows the signed 16 bit space. |
| 0x3200000d | STATUS\_MKV\_INVALID\_ANNEXB\_NALU\_IN\_FRAME\_DATA  | Invalid Annex-B start code encountered. If the Annex-B adaptation flag has been specified and the code encounters invalid start sequence of more than 3 zeroes. Valid Annex-B format should have `emulation prevention` sequence to escape sequence of 3 or more zeroes in the bytestream. Refer to MPEG specification for more information. |
| 0x3200000e | STATUS\_MKV\_INVALID\_AVCC\_NALU\_IN\_FRAME\_DATA  | Invalid AVCC NALu packaging when adapting AVCC flag is specified. Ensure the bytestream is in valid AVCC format. Refer to MPEG specification for more information. |
| 0x3200000f | STATUS\_MKV\_BOTH\_ANNEXB\_AND\_AVCC\_SPECIFIED  | Both adapting AVCC and Annex-B NALs have been specified. Specify either one or none. |
| 0x32000010 | STATUS\_MKV\_INVALID\_ANNEXB\_NALU\_IN\_CPD  | Invalid Annex-B format of Codec Private Data when the adapting Annex-B flag is specified. Ensure the Codec Private Data is in valid Annex-B format and if not then remove the CPD Annex-B adaptation flag. |
| 0x32000011 | STATUS\_MKV\_PTS\_DTS\_ARE\_NOT\_SAME  | KVS enforces PTS (presentation timestamp) and DTS (decoding timestamp) to be the same for the fragment start frames. Those are the key frames which start the fragment. |
| 0x32000012 | STATUS\_MKV\_INVALID\_H264\_H265\_CPD  | Failed to parse H264/H265 Codec Private Data. |
| 0x32000013 | STATUS\_MKV\_INVALID\_H264\_H265\_SPS\_WIDTH  | Failed to extract the with from Codec Private Data. |
| 0x32000014 | STATUS\_MKV\_INVALID\_H264\_H265\_SPS\_HEIGHT  | Failed to extract the height from Codec Private Data. |
| 0x32000015 | STATUS\_MKV\_INVALID\_H264\_H265\_SPS\_NALU  | Invalid H264/H265 SPS NALu. |
| 0x32000016 | STATUS\_MKV\_INVALID\_BIH\_CPD  | Invalid Bitmap Info Header format in Codec Private Data. |

### Error and Status Codes Returned by the Trace Library<a name="producer-sdk-errors-trace"></a>

The following table contains error and status information returned by methods in the `Trace` library\.


****  

| Code | Message | 
| --- | --- | 
| 0x10100001 | STATUS\_MIN\_PROFILER\_BUFFER  | 

### Error and Status Codes Returned by the Utils Library<a name="producer-sdk-errors-utils"></a>

The following table contains error and status information returned by methods in the `Utils` library\.


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

The following table contains error and status information returned by methods in the `View` library\.


****  

| Code | Message | Notes |
| --- | --- | 
| 0x30000001 | STATUS\_MIN\_CONTENT\_VIEW\_ITEMS  | Invalid content view item count specified. Please refer to [limits document](producer-sdk-limits.md) for more information . |
| 0x30000002 | STATUS\_INVALID\_CONTENT\_VIEW\_DURATION  | Invalid content view duration is specified. Please refer to [limits document](producer-sdk-limits.md) for more information . | 
| 0x30000003 | STATUS\_CONTENT\_VIEW\_NO\_MORE\_ITEMS  | No more items error is returned when an attempt is made to get past the Head position. |
| 0x30000004 | STATUS\_CONTENT\_VIEW\_INVALID\_INDEX  | Invalid index is specified. |
| 0x30000005 | STATUS\_CONTENT\_VIEW\_INVALID\_TIMESTAMP  | Invalid timestamp or the timestamp overlap. The frame decoding timestamp should be greater or equal to the previous frame timestamp plus the previous frame duration: `DTS(n) >= DTS(n-1) + Duration(n-1)`. This error is often an indication of an "unstable" encoder where the encoder produces a burst of encoded frames and their timestamps are smaller than the intra-frame durations or the stream is configured to use SDK timestamps and the frames are sent faster than the frame durations. To help with some "jitter" in the encoder a smaller frame duration can be specified in the StreamInfo.StreamCaps structure. For example, if the stream is 25FPS then each frame's duration is 40ms. However, to handle the encoder jitter, it's recommended to use half of that frame duration = 20ms. Some streams require more precise control over the timing for error detection. |
| 0x30000006 | STATUS\_INVALID\_CONTENT\_VIEW\_LENGTH  | Invalid content view item data length has been specified. |
