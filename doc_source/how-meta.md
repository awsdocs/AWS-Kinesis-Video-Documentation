# Using Streaming Metadata with Kinesis Video Streams<a name="how-meta"></a>

You can use the Amazon Kinesis Video Streams Producer SDK to embed metadata at the individual fragment level in a Kinesis video stream\. Metadata in Kinesis Video Streams is a mutable key\-value pair\. You can use it to describe the content of the fragment, embed associated sensor readings that need to be transferred along with the actual fragment, or meet other custom needs\. The metadata is made available as part of the [GetMedia](API_dataplane_GetMedia.md) or [GetMediaForFragmentList](API_reader_GetMediaForFragmentList.md) API operations\. It is stored along with the fragments for the entire duration of the stream's retention period\. Your consuming applications can read, process, and take action based on the metadata using the [Kinesis Video Stream Parser Library](parser-library.md)\. 

There are two modes in which the metadata can be embedded with fragments in a stream: 
+ **Nonpersistent:** You can affix metadata on an ad hoc basis to fragments in a stream, based on business\-specific criteria that have occurred\. An example is a smart camera that detects motion and adds metadata to the corresponding fragments that contain the motion before sending the fragments to its Kinesis video stream\. You might apply metadata to the fragment in the following format: `Motion = true`\.
+ **Persistent:** You can affix metadata to successive, consecutive fragments in a stream based on a continuing need\. An example is a smart camera that sends the current latitude and longitude coordinates associated with all fragments that it sends to its Kinesis video stream\. You might apply metadata to all the fragments in the following format: `Lat = 47.608013N , Long = -122.335167W`

You can affix metadata in both of these modes to the same fragment simultaneously, based on your application's needs\. The embedded metadata might include objects detected, activity tracked, GPS coordinates, or any other custom data that you want to associate with the fragments in the stream\. Metadata is encoded as key\-value string pairs\.

**Topics**
+ [Adding Metadata to a Kinesis Video Stream](#how-meta-add)
+ [Consuming Metadata Embedded in a Kinesis Video Stream](#how-meta-consume)
+ [Streaming Metadata Limitations](#how-meta-limits)

## Adding Metadata to a Kinesis Video Stream<a name="how-meta-add"></a>

Metadata that you add to a Kinesis video stream is modeled as MKV tags, which are implemented as key\-value pairs\. 

Metadata can either be *transient*, such as to mark an event within the stream, or *persistent*, such as to identify fragments where a given event is taking place\. A persistent metadata item remains, and is applied to each consecutive fragment, until it is canceled\.

**Note**  
The metadata items added using the [Producer Libraries](producer-sdk.md) are distinct from the stream\-level tagging APIs implemented with [TagStream](API_TagStream.md), [UntagStream](API_UntagStream.md), and [ListTagsForStream](API_ListTagsForStream.md)\.

### Streaming Metadata API<a name="how-meta-api"></a>

You can use the following operations in the Producer SDK to implement streaming metadata\.

#### PIC<a name="how-meta-api-pic"></a>

```
PUBLIC_API STATUS putKinesisVideoFragmentMetadata(STREAM_HANDLE streamHandle, 
    PCHAR name, 
    PCHAR value, 
    BOOL persistent);
```

#### C\+\+ Producer SDK<a name="how-meta-api-cpp"></a>

```
/**
 * Appends a "tag" or metadata - a key/value string pair into the stream.
 */
bool putFragmentMetadata(const std::string& name, const std::string& value, bool persistent = true);
```

#### Java Producer SDK<a name="how-meta-api-java"></a>

Using the Java Producer SDK, you add metadata to a `MediaSource` using `MediaSourceSink.onCodecPrivateData`:

```
void onFragmentMetadata(final @Nonnull String metadataName, final @Nonnull String metadataValue, final boolean persistent)
throws KinesisVideoException;
```

#### Persistent and Nonpersistent Metadata<a name="how-meta-api-persistence"></a>

For nonpersistent metadata, you can add multiple metadata items with the same *name*\. The Producer SDK collects the metadata items in the metadata queue until they are prepended to the next fragment\. The metadata queue is cleared as the metadata items are applied to the stream\. To repeat the metadata, call `putKinesisVideoFragmentMetadata` or `putFragmentMetadata` again\. 

For persistent metadata, the Producer SDK collects the metadata items in the metadata queue in the same way as for nonpersistent metadata\. However, the metadata items are not removed from the queue when they are prepended to the next fragment\.

Calling `putKinesisVideoFragmentMetadata` or `putFragmentMetadata` with `persistent` set to `true` has the following behavior: 
+ Calling the API puts the metadata item in the queue\. The metadata is added as an MKV tag to every fragment while the item is in the queue\.
+ Calling the API with the same *name* and a different *value* as a previously added metadata item overwrites the item\.
+ Calling the API with an empty *value* removes \(cancels\) the metadata item from the metadata queue\.

## Consuming Metadata Embedded in a Kinesis Video Stream<a name="how-meta-consume"></a>

To consume the metadata in a Kinesis video stream, use an implementation of `MkvTagProcessor`:

```
public interface MkvTagProcessor {
        default void process(MkvTag mkvTag, Optional<FragmentMetadata> currentFragmentMetadata) {
            throw new NotImplementedException("Default FragmentMetadataVisitor.MkvTagProcessor");
        }
        default void clear() {
            throw new NotImplementedException("Default FragmentMetadataVisitor.MkvTagProcessor");
	    }
    }
}
```

This interface is found in the [FragmentMetadataVisitor](parser-library-write.md#parser-library-write-FMV) class in the [Kinesis Video Stream Parser Library](parser-library.md)\.

The `FragmentMetadataVisitor` class contains an implementation of `MkvTagProcessor`:

```
public static final class BasicMkvTagProcessor implements FragmentMetadataVisitor.MkvTagProcessor {
    @Getter
    private List<MkvTag> tags = new ArrayList<>();

    @Override
    public void process(MkvTag mkvTag, Optional<FragmentMetadata> currentFragmentMetadata) {
        tags.add(mkvTag);
    }
    
    @Override
    public void clear() {
        tags.clear();
	}
}
```

The `KinesisVideoRendererExample` class contains an example of how to use a `BasicMkvTagProcessor`\. In the following example, a `BasicMkvTagProcessor` is added to the `MediaProcessingArguments` of an application:

```
if (renderFragmentMetadata) {
    getMediaProcessingArguments = KinesisVideoRendererExample.GetMediaProcessingArguments.create(
        Optional.of(new FragmentMetadataVisitor.BasicMkvTagProcessor()));
```

The `BasicMkvTagProcessor.process` method is called when fragment metadata arrives\. You can retrieve the accumulated metadata with `GetTags`\. If you want to retrieve a single metadata item, first call `clear` to clear the collected metadata, and then retrieve the metadata items again\.

## Streaming Metadata Limitations<a name="how-meta-limits"></a>

The following limitations apply to adding streaming metadata to a Kinesis video stream:
+ You can prepend up to 10 metadata items to a fragment\.
+ A fragment metadata *name* can be up to 128 bytes in length\.
+ A fragment metadata *value* can be up to 256 bytes in length\.
+ A fragment metadata *name* cannot begin with the string "`AWS`"\. If such a metadata item is added, the `putFragmentMetadata` method in the PIC returns a `STATUS_INVALID_METADATA_NAME` error \(error code `0x52000077`\)\. Your application can then either ignore the error \(the PIC doesn't add the metadata item\), or respond to the error\.