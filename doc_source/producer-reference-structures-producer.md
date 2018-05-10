# Producer SDK Structures<a name="producer-reference-structures-producer"></a>

This section includes information on structures used to provide data to the Producer SDK\.

**Topics**
+ [DeviceInfo/ DefaultDeviceInfoProvider](#producer-reference-structures-producer-deviceinfo)
+ [StorageInfo](#producer-reference-structures-producer-storageinfo)

## DeviceInfo/ DefaultDeviceInfoProvider<a name="producer-reference-structures-producer-deviceinfo"></a>

The **DeviceInfo** and **DevaultDeviceInfoProvider** objects control the behavior of the Kinesis Video Streams Producer object\.

### Member Fields<a name="producer-reference-structures-producer-deviceinfo-fields"></a>
+ **version**: An integer value used to ensure the correct version of the structure is used with the current version of the codebase\. The current version is specified with the `DEVICE_INFO_CURRENT_VERSION` macro\.
+ **name**: The human\-readable name for the device\.
+ **tagCount/ tags**: Not currently used\.
+ **streamCount**: The maximum number of streams the device can handle\. This pre\-allocates the storage for pointers to the stream objects initially, but the actual stream objects will be created later\. The default is 16 streams, but this can be changed in the `DefaultDeviceInfoProvider.cpp` file\.
+ **storageInfo**: An object describing the main storage configurtaion\. See [StorageInfo](#producer-reference-structures-producer-storageinfo)\.

## StorageInfo<a name="producer-reference-structures-producer-storageinfo"></a>

The `StorageInfo` object specifies the configuration of the main storage for Kinesis Video Streams\.

The default implementation is based on a low\-fragmentation fast heap implementation, which is optimized for streaming\. It uses the `MEMALLOC` allocator, which can be overwritten on a given platform\. Some platforms have virtual memory allocation without backing the allocation with physical pages\. As the memory is used, the virtual pages will be backed by the physical pages\. This will result in low\-memory pressure on the overall system when storage is underutilized\.

The default storage size should be calculated based on the following formula:

```
Size = NumberOfStreams * AverageFrameSize * FramesPerSecond * BufferDurationInSeconds * DefragmentationFactor
```

In the preceding formula, the `DefragmentationFactor` should be set to 1\.2 \(20 percent\)\.

In the following example, a device has Audio and Video streams\. The Audio stream has 512 samples per second, with an average sample of 100 bytes\. The Video stream has 25 frames per second, with an average of 10,000 bytes, and each stream has 3 minutes of buffer duration:

```
Size = (512 * 100 * (3 * 60) + 25 * 10000 * (3 * 60)) * 1.2 = (9216000 + 45000000) * 1.2 = 65059200 = ~ 66MB.
```

If the device has more available memory, it is prudent to add more memory to storage, to avoid severe fragmentation\. 

Care should be taken to ensure that the storage size is adequate to accommodate the full buffers for all streams at high encoding complexity \(when the frame size is larger due to high motion\) or during times when the bandwidth is low\. If the producer is hitting memory pressure, it will emit storage overflow pressure callbacks \(`StorageOverflowPressureFunc`\), but when there is no memory available in the content store, it will drop the frame that’s being pushed into KVS with an error \(`STATUS_STORE_OUT_OF_MEMORY = 0x5200002e`, see [Error and Status Codes Returned by the Client Library](producer-sdk-errors.md#producer-sdk-errors-client) for more details\.\) This can also happen if the application ACKs are not available or the Persisted ACKs are delayed; in this case the buffers will fill to the “buffer duration” capacity before the older frames start dropping out\.

### Member Fields<a name="producer-reference-structures-producer-storageinfo-fields"></a>
+ **version**: An integer value used to ensure the correct version of the structure is used with the current version of the codebase\.
+ **storageType**: A `DEVICE_STORAGE_TYPE` enumeration that specifies the underlying backing/implementation of the storage\. Currently the only supported value is `DEVICE_STORAGE_TYPE_IN_MEM`\. `DEVICE_STORAGE_TYPE_HYBRID_FILE` will be supported in a future implmenetation, indicating that storage falls back to the file\-backed content store\.
+ **storageSize**: The storage size in bytes to pre\-allocate\. The minimum allocation is 10MB, and the maximum allocation is 10GB \(This will change with the future implementation of the file backed content store\)\.
+ **spillRatio**: An integer value representing the percentage of the storage to be allocated from the direct memory storage type \(RAM\), as opposed to the secondary overflow storage \(File storage\)\. Not currently used\.
+ **rootDirectory**: The path to the directory where the file\-backed content store is stored\. Not currently used\.