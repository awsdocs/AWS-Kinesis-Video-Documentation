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

| Code | Message | 
| --- | --- | 
| 0x52000001 | STATUS\_MAX\_STREAM\_COUNT  | 
| 0x52000002 | STATUS\_MIN\_STREAM\_COUNT  | 
| 0x52000003 | STATUS\_INVALID\_DEVICE\_NAME\_LENGTH  | 
| 0x52000004 | STATUS\_INVALID\_DEVICE\_INFO\_VERSION  | 
| 0x52000005 | STATUS\_MAX\_TAG\_COUNT  | 
| 0x52000006 | STATUS\_DEVICE\_FINGERPRINT\_LENGTH  | 
| 0x52000007 | STATUS\_INVALID\_CALLBACKS\_VERSION  | 
| 0x52000008 | STATUS\_INVALID\_STREAM\_INFO\_VERSION  | 
| 0x52000009 | STATUS\_INVALID\_STREAM\_NAME\_LENGTH  | 
| 0x5200000a | STATUS\_INVALID\_STORAGE\_SIZE  | 
| 0x5200000b | STATUS\_INVALID\_ROOT\_DIRECTORY\_LENGTH  | 
| 0x5200000c | STATUS\_INVALID\_SPILL\_RATIO  | 
| 0x5200000d | STATUS\_INVALID\_STORAGE\_INFO\_VERSION  | 
| 0x5200000e | STATUS\_INVALID\_STREAM\_STATE  | 
| 0x5200000f | STATUS\_SERVICE\_CALL\_CALLBACKS\_MISSING  | 
| 0x52000010 | STATUS\_SERVICE\_CALL\_NOT\_AUTHORIZED\_ERROR  | 
| 0x52000011 | STATUS\_DESCRIBE\_STREAM\_CALL\_FAILED  | 
| 0x52000012 | STATUS\_INVALID\_DESCRIBE\_STREAM\_RESPONSE  | 
| 0x52000013 | STATUS\_STREAM\_IS\_BEING\_DELETED\_ERROR  | 
| 0x52000014 | STATUS\_SERVICE\_CALL\_INVALID\_ARG\_ERROR  | 
| 0x52000015 | STATUS\_SERVICE\_CALL\_DEVICE\_NOT\_FOND\_ERROR  | 
| 0x52000016 | STATUS\_SERVICE\_CALL\_DEVICE\_NOT\_PROVISIONED\_ERROR  | 
| 0x52000017 | STATUS\_SERVICE\_CALL\_RESOURCE\_NOT\_FOUND\_ERROR  | 
| 0x52000018 | STATUS\_INVALID\_AUTH\_LEN  | 
| 0x52000019 | STATUS\_CREATE\_STREAM\_CALL\_FAILED  | 
| 0x5200002a | STATUS\_GET\_STREAMING\_TOKEN\_CALL\_FAILED  | 
| 0x5200002b | STATUS\_GET\_STREAMING\_ENDPOINT\_CALL\_FAILED  | 
| 0x5200002c | STATUS\_INVALID\_URI\_LEN  | 
| 0x5200002d | STATUS\_PUT\_STREAM\_CALL\_FAILED  | 
| 0x5200002e | STATUS\_STORE\_OUT\_OF\_MEMORY  | 
| 0x5200002f | STATUS\_NO\_MORE\_DATA\_AVAILABLE  | 
| 0x52000030 | STATUS\_INVALID\_TAG\_VERSION  | 
| 0x52000031 | STATUS\_SERVICE\_CALL\_UNKOWN\_ERROR  | 
| 0x52000032 | STATUS\_SERVICE\_CALL\_RESOURCE\_IN\_USE\_ERROR  | 
| 0x52000033 | STATUS\_SERVICE\_CALL\_CLIENT\_LIMIT\_ERROR  | 
| 0x52000034 | STATUS\_SERVICE\_CALL\_DEVICE\_LIMIT\_ERROR  | 
| 0x52000035 | STATUS\_SERVICE\_CALL\_STREAM\_LIMIT\_ERROR  | 
| 0x52000036 | STATUS\_SERVICE\_CALL\_RESOURCE\_DELETED\_ERROR  | 
| 0x52000037 | STATUS\_SERVICE\_CALL\_TIMEOUT\_ERROR  | 
| 0x52000038 | STATUS\_STREAM\_READY\_CALLBACK\_FAILED  | 
| 0x52000039 | STATUS\_DEVICE\_TAGS\_COUNT\_NON\_ZERO\_TAGS\_NULL  | 
| 0x5200003a | STATUS\_INVALID\_STREAM\_DESCRIPTION\_VERSION  | 
| 0x5200003b | STATUS\_INVALID\_TAG\_NAME\_LEN  | 
| 0x5200003c | STATUS\_INVALID\_TAG\_VALUE\_LEN  | 
| 0x5200003d | STATUS\_TAG\_STREAM\_CALL\_FAILED  | 
| 0x5200003e | STATUS\_INVALID\_CUSTOM\_DATA  | 
| 0x5200003f | STATUS\_INVALID\_CREATE\_STREAM\_RESPONSE  | 
| 0x52000040 | STATUS\_CLIENT\_AUTH\_CALL\_FAILED  | 
| 0x52000041 | STATUS\_GET\_CLIENT\_TOKEN\_CALL\_FAILED  | 
| 0x52000042 | STATUS\_CLIENT\_PROVISION\_CALL\_FAILED  | 
| 0x52000043 | STATUS\_CREATE\_CLIENT\_CALL\_FAILED  | 
| 0x52000044 | STATUS\_CLIENT\_READY\_CALLBACK\_FAILED  | 
| 0x52000045 | STATUS\_TAG\_CLIENT\_CALL\_FAILED  | 
| 0x52000046 | STATUS\_INVALID\_CREATE\_DEVICE\_RESPONSE  | 
| 0x52000047 | STATUS\_ACK\_TIMESTAMP\_NOT\_IN\_VIEW\_WINDOW  | 
| 0x52000048 | STATUS\_INVALID\_FRAGMENT\_ACK\_VERSION  | 
| 0x52000049 | STATUS\_INVALID\_TOKEN\_EXPIRATION  | 
| 0x5200004a | STATUS\_END\_OF\_STREAM  | 
| 0x5200004b | STATUS\_DUPLICATE\_STREAM\_NAME  | 
| 0x5200004c | STATUS\_INVALID\_RETENTION\_PERIOD  | 
| 0x5200004d | STATUS\_INVALID\_ACK\_KEY\_START  | 
| 0x5200004e | STATUS\_INVALID\_ACK\_DUPLICATE\_KEY\_NAME  | 
| 0x5200004f | STATUS\_INVALID\_ACK\_INVALID\_VALUE\_START  | 
| 0x52000050 | STATUS\_INVALID\_ACK\_INVALID\_VALUE\_END  | 
| 0x52000051 | STATUS\_INVALID\_PARSED\_ACK\_TYPE  | 
| 0x52000052 | STATUS\_STREAM\_HAS\_BEEN\_STOPPED  | 

### Error and Status Codes Returned by the Duration Library<a name="producer-sdk-errors-duration"></a>

The following table contains error and status information returned by methods in the `Duration` library\.


****  

| Code | Message | 
| --- | --- | 
| 0xFFFFFFFFFFFFFFFF | INVALID\_DURATION\_VALUE  | 

### Error and Status Codes Returned by the Common Library<a name="producer-sdk-errors-common"></a>

The following table contains error and status information returned by methods in the `Common` library\.


****  

| Code | Message | 
| --- | --- | 
| 0x00000001 | STATUS\_NULL\_ARG  | 
| 0x00000002 | STATUS\_INVALID\_ARG  | 
| 0x00000003 | STATUS\_INVALID\_ARG\_LEN  | 
| 0x00000004 | STATUS\_OUT\_OF\_MEMORY  | 
| 0x00000005 | STATUS\_BUFFER\_TOO\_SMALL  | 
| 0x00000006 | STATUS\_UNEXPECTED\_EOF  | 
| 0x00000007 | STATUS\_FORMAT\_ERROR  | 
| 0x00000008 | STATUS\_INVALID\_HANDLE\_ERROR  | 
| 0x00000009 | STATUS\_OPEN\_FILE\_FAILED  | 
| 0x0000000a | STATUS\_READ\_FILE\_FAILED  | 
| 0x0000000b | STATUS\_WRITE\_TO\_FILE\_FAILED  | 
| 0x0000000c | STATUS\_INTERNAL\_ERROR  | 
| 0x0000000d | STATUS\_INVALID\_OPERATION  | 
| 0x0000000e | STATUS\_NOT\_IMPLEMENTED  | 

### Error and Status Codes Returned by the Heap Library<a name="producer-sdk-errors-heap"></a>

The following table contains error and status information returned by methods in the `Heap` library\.


****  

| Code | Message | 
| --- | --- | 
| 0x01000001 | STATUS\_HEAP\_FLAGS\_ERROR  | 
| 0x01000002 | STATUS\_HEAP\_NOT\_INITIALIZED  | 
| 0x01000003 | STATUS\_HEAP\_CORRUPTED  | 
| 0x01000004 | STATUS\_HEAP\_VRAM\_LIB\_MISSING  | 
| 0x01000005 | STATUS\_HEAP\_VRAM\_LIB\_REOPEN  | 
| 0x01000006 | STATUS\_HEAP\_VRAM\_INIT\_FUNC\_SYMBOL  | 
| 0x01000007 | STATUS\_HEAP\_VRAM\_ALLOC\_FUNC\_SYMBOL  | 
| 0x01000008 | STATUS\_HEAP\_VRAM\_FREE\_FUNC\_SYMBOL  | 
| 0x01000009 | STATUS\_HEAP\_VRAM\_LOCK\_FUNC\_SYMBOL  | 
| 0x0100000a | STATUS\_HEAP\_VRAM\_UNLOCK\_FUNC\_SYMBOL  | 
| 0x0100000b | STATUS\_HEAP\_VRAM\_UNINIT\_FUNC\_SYMBOL  | 
| 0x0100000c | STATUS\_HEAP\_VRAM\_GETMAX\_FUNC\_SYMBOL  | 
| 0x0100000d | STATUS\_HEAP\_DIRECT\_MEM\_INIT  | 
| 0x0100000e | STATUS\_HEAP\_VRAM\_INIT\_FAILED  | 
| 0x0100000f | STATUS\_HEAP\_LIBRARY\_FREE\_FAILED  | 
| 0x01000010 | STATUS\_HEAP\_VRAM\_ALLOC\_FAILED  | 
| 0x01000011 | STATUS\_HEAP\_VRAM\_FREE\_FAILED  | 
| 0x01000012 | STATUS\_HEAP\_VRAM\_MAP\_FAILED  | 
| 0x01000013 | STATUS\_HEAP\_VRAM\_UNMAP\_FAILED  | 
| 0x01000014 | STATUS\_HEAP\_VRAM\_UNINIT\_FAILED  | 

### Error and Status Codes Returned by the MKVGen Library<a name="producer-sdk-errors-mkvgen"></a>

The following table contains error and status information returned by methods in the `MKVGen` library\.


****  

| Code | Message | 
| --- | --- | 
| 0x32000001 | STATUS\_MKV\_INVALID\_FRAME\_DATA  | 
| 0x32000002 | STATUS\_MKV\_INVALID\_FRAME\_TIMESTAMP  | 
| 0x32000003 | STATUS\_MKV\_INVALID\_CLUSTER\_DURATION  | 
| 0x32000004 | STATUS\_MKV\_INVALID\_CONTENT\_TYPE\_LENGTH  | 
| 0x32000005 | STATUS\_MKV\_NUMBER\_TOO\_BIG  | 
| 0x32000006 | STATUS\_MKV\_INVALID\_CODEC\_ID\_LENGTH  | 
| 0x32000007 | STATUS\_MKV\_INVALID\_TRACK\_NAME\_LENGTH  | 
| 0x32000008 | STATUS\_MKV\_INVALID\_CODEC\_PRIVATE\_LENGTH  | 
| 0x32000009 | STATUS\_MKV\_CODEC\_PRIVATE\_NULL  | 
| 0x3200000a | STATUS\_MKV\_INVALID\_TIMECODE\_SCALE  | 
| 0x3200000b | STATUS\_MKV\_MAX\_FRAME\_TIMECODE  | 
| 0x3200000c | STATUS\_MKV\_LARGE\_FRAME\_TIMECODE  | 
| 0x3200000d | STATUS\_MKV\_INVALID\_ANNEXB\_NALU\_IN\_FRAME\_DATA  | 
| 0x3200000e | STATUS\_MKV\_INVALID\_AVCC\_NALU\_IN\_FRAME\_DATA  | 
| 0x3200000f | STATUS\_MKV\_BOTH\_ANNEXB\_AND\_AVCC\_SPECIFIED  | 
| 0x32000010 | STATUS\_MKV\_INVALID\_ANNEXB\_NALU\_IN\_CPD  | 
| 0x32000011 | STATUS\_MKV\_PTS\_DTS\_ARE\_NOT\_SAME  | 
| 0x32000012 | STATUS\_MKV\_INVALID\_H264\_H265\_CPD  | 
| 0x32000013 | STATUS\_MKV\_INVALID\_H264\_H265\_SPS\_WIDTH  | 
| 0x32000014 | STATUS\_MKV\_INVALID\_H264\_H265\_SPS\_HEIGHT  | 
| 0x32000015 | STATUS\_MKV\_INVALID\_H264\_H265\_SPS\_NALU  | 
| 0x32000016 | STATUS\_MKV\_INVALID\_BIH\_CPD  | 

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

| Code | Message | 
| --- | --- | 
| 0x30000001 | STATUS\_MIN\_CONTENT\_VIEW\_ITEMS  | 
| 0x30000002 | STATUS\_INVALID\_CONTENT\_VIEW\_DURATION  | 
| 0x30000003 | STATUS\_CONTENT\_VIEW\_NO\_MORE\_ITEMS  | 
| 0x30000004 | STATUS\_CONTENT\_VIEW\_INVALID\_INDEX  | 
| 0x30000005 | STATUS\_CONTENT\_VIEW\_INVALID\_TIMESTAMP  | 
| 0x30000006 | STATUS\_INVALID\_CONTENT\_VIEW\_LENGTH  | 