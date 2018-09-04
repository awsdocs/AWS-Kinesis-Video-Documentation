# Producer SDK Structures<a name="producer-reference-structures-producer"></a>

This section includes information about structures that you can use to provide data to the Kinesis Video Streams Producer object\.

**Topics**
+ [DeviceInfo/DefaultDeviceInfoProvider](#producer-reference-structures-producer-deviceinfo)
+ [StorageInfo](#producer-reference-structures-producer-storageinfo)

## DeviceInfo/DefaultDeviceInfoProvider<a name="producer-reference-structures-producer-deviceinfo"></a>

The **DeviceInfo** and **DefaultDeviceInfoProvider** objects control the behavior of the Kinesis Video Streams Producer object\.

### Member Fields<a name="producer-reference-structures-producer-deviceinfo-fields"></a>
+ **version**: An integer value used to ensure that the correct version of the structure is used with the current version of the code base\. The current version is specified using the `DEVICE_INFO_CURRENT_VERSION` macro\.
+ **name**: The human\-readable name for the device\.
+ **tagCount/tags**: Not currently used\.
+ **streamCount**: The maximum number of streams that the device can handle\. This pre\-allocates the storage for pointers to the stream objects initially, but the actual stream objects are created later\. The default is 16 streams, but you can change this number in the `DefaultDeviceInfoProvider.cpp` file\.
+ **storageInfo**: An object that describes the main storage configuration\. For more information, see [StorageInfo](#producer-reference-structures-producer-storageinfo)\.

## StorageInfo<a name="producer-reference-structures-producer-storageinfo"></a>

Specifies the configuration of the main storage for Kinesis Video Streams\.

The default implementation is based on a low\-fragmentation fast heap implementation, which is optimized for streaming\. It uses the `MEMALLOC` allocator, which can be overwritten on a given platform\. Some platforms have virtual memory allocation without backing the allocation with physical pages\. As the memory is used, the virtual pages are backed by the physical pages\. This results in low\-memory pressure on the overall system when storage is underused\.

Calculate the default storage size based on the following formula\. The `DefragmentationFactor` should be set to 1\.2 \(20 percent\)\.

```
Size = NumberOfStreams * AverageFrameSize * FramesPerSecond * BufferDurationInSeconds * DefragmentationFactor
```

In the following example, a device has audio and video streams\. The audio stream has 512 samples per second, with an average sample of 100 bytes\. The video stream has 25 frames per second, with an average of 10,000 bytes\. Each stream has 3 minutes of buffer duration\.

```
Size = (512 * 100 * (3 * 60) + 25 * 10000 * (3 * 60)) * 1.2 = (9216000 + 45000000) * 1.2 = 65059200 = ~ 66MB.
```

If the device has more available memory, it is recommended that you add more memory to storage to avoid severe fragmentation\. 

Ensure that the storage size is adequate to accommodate the full buffers for all streams at high encoding complexity \(when the frame size is larger due to high motion\) or when the bandwidth is low\. If the producer hits memory pressure, it emits storage overflow pressure callbacks \(`StorageOverflowPressureFunc`\)\. However, when no memory is available in the content store, it drops the frame that’s being pushed into Kinesis Video Streams with an error \(`STATUS_STORE_OUT_OF_MEMORY = 0x5200002e`\)\. For more information, see [Error and Status Codes Returned by the Client Library](producer-sdk-errors.md#producer-sdk-errors-client)\. This can also happen if the application acknowledgements \(ACKs\) are not available, or the persisted ACKs are delayed\. In this case, the buffers fill to the "buffer duration" capacity before the older frames start dropping out\.

### Member Fields<a name="producer-reference-structures-producer-storageinfo-fields"></a>
+ **version**: An integer value used to ensure that the correct version of the structure is used with the current version of the code base\.
+ **storageType**: A `DEVICE_STORAGE_TYPE` enumeration that specifies the underlying backing/implementation of the storage\. Currently the only supported value is `DEVICE_STORAGE_TYPE_IN_MEM`\. A future implementation will support `DEVICE_STORAGE_TYPE_HYBRID_FILE`, indicating that storage falls back to the file\-backed content store\.
+ **storageSize**: The storage size in bytes to preallocate\. The minimum allocation is 10 MB, and the maximum allocation is 10 GB\. \(This will change with the future implementation of the file\-backed content store\.\)
+ **spillRatio**: An integer value that represents the percentage of the storage to be allocated from the direct memory storage type \(RAM\), as opposed to the secondary overflow storage \(file storage\)\. Not currently used\.
+ **rootDirectory**: The path to the directory where the file\-backed content store is located\. Not currently used\.