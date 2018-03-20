# Step 2: Write and Examine the Code<a name="parser-library-write"></a>

In this section, you examine the Java library and test code, and learn how to use the tools from the library in your own code\.

The Kinesis Video Stream Parser Library contains the following tools:
+ [StreamingMkvReader](#parser-library-write-SMSR)
+ [FragmentMetadataVisitor](#parser-library-write-FMV)
+ [OutputSegmentMerger](#parser-library-write-OSM)
+ [MergeGetMediaOutput](#parser-library-write-MGMO)
+ [KinesisVideoExample](#parser-library-write-example)

## StreamingMkvReader<a name="parser-library-write-SMSR"></a>

This class reads specified MKV elements from a stream in a non\-blocking way\.

The following code example \(from `FragmentMetadataVisitorTest`\) shows how to create and use a `Streaming MkvReader` to retrieve `MkvElement` objects from an input stream called `in`:

```
StreamingMkvReader mkvStreamReader =
                StreamingMkvReader.createDefault(new InputStreamParserByteSource(in));
        while (mkvStreamReader.mightHaveNext()) {
            Optional<MkvElement> mkvElement = mkvStreamReader.nextIfAvailable();
            if (mkvElement.isPresent()) {
                mkvElement.get().accept(compositeVisitor);
                ...
                }
            }
        }
```

## FragmentMetadataVisitor<a name="parser-library-write-FMV"></a>

This class retrieves metadata for fragments \(media elements\) and tracks \(individual data streams containing media information, such as codec private data, pixel width, or pixel height\)\. 

The following code example \(from the `FragmentMetadataVisitorTest` file\) shows how to use `FragmentMetadataVisitor` to retrieve data from a `MkvElement` object:

```
FragmentMetadataVisitor fragmentVisitor = FragmentMetadataVisitor.create();
        StreamingMkvReader mkvStreamReader =
                StreamingMkvReader.createDefault(new InputStreamParserByteSource(in));
        int segmentCount = 0;
        while(mkvStreamReader.mightHaveNext()) {
            Optional<MkvElement> mkvElement = mkvStreamReader.nextIfAvailable();
            if (mkvElement.isPresent()) {
                mkvElement.get().accept(fragmentVisitor);
                if (MkvTypeInfos.SIMPLEBLOCK.equals(mkvElement.get().getElementMetaData().getTypeInfo())) {
                    MkvDataElement dataElement = (MkvDataElement) mkvElement.get();
                    Frame frame = ((MkvValue<Frame>)dataElement.getValueCopy()).getVal();
                    MkvTrackMetadata trackMetadata = fragmentVisitor.getMkvTrackMetadata(frame.getTrackNumber());
                    assertTrackAndFragmentInfo(fragmentVisitor, frame, trackMetadata);
                }
                if (MkvTypeInfos.SEGMENT.equals(mkvElement.get().getElementMetaData().getTypeInfo())) {
                    if (mkvElement.get() instanceof MkvEndMasterElement) {
                        if (segmentCount < continuationTokens.size()) {
                            Optional<String> continuationToken = fragmentVisitor.getContinuationToken();
                            Assert.assertTrue(continuationToken.isPresent());
                            Assert.assertEquals(continuationTokens.get(segmentCount), continuationToken.get());
                        }
                        segmentCount++;
                    }
                }
            }

        }
```

The preceding example shows the following coding pattern:
+ Create a `FragmentMetadataVisitor` to parse the data, and a [StreamingMkvReader](#parser-library-write-SMSR) to provide the data\.
+ For each `MkvElement` in the stream, test if its metadata is of type `SIMPLEBLOCK`\.
+ If it is, retrieve the `MkvDataElement` from the `MkvElement`\.
+ Retrieve the `Frame` \(media data\) from the `MkvDataElement`\.
+ Retrieve the `MkvTrackMetadata` for the `Frame` from the `FragmentMetadataVisitor`\.
+ Retrieve and verify the following data from the `Frame` and `MkvTrackMetadata` objects:
  + The track number\.
  + The frame's pixel height\.
  + The frame's pixel width\.
  + The codec ID for the codec used to encode the frame\.
  + That this frame arrived in order\. That is, verify that the track number of the previous frame, if present, is less than that of the current frame\.

To use `FragmentMetadataVisitor` in your project, pass `MkvElement` objects to the visitor using their `accept` method:

```
mkvElement.get().accept(FragmentMetadataVisitor);
```

## OutputSegmentMerger<a name="parser-library-write-OSM"></a>

This class merges metadata from different tracks in the stream into a stream with a single segment\.

The following code example \(from the `OutputSegmentMergerTest` file\) shows how to use `OutputSegmentMerger` to merge track metadata from a byte array called `inputBytes`:

```
ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
        ByteArrayInputStream in = new ByteArrayInputStream(inputBytes);
        OutputSegmentMerger merger =
                new OutputSegmentMerger(outputStream, new ArrayList<>(), getCountVisitor(), false);

        StreamingMkvReader mkvStreamReader =
                StreamingMkvReader.createDefault(new InputStreamParserByteSource(in));
        while(mkvStreamReader.mightHaveNext()) {
            Optional<MkvElement> mkvElement = mkvStreamReader.nextIfAvailable();
            if (mkvElement.isPresent()) {
                mkvElement.get().accept(merger);
            }
        }
```

The preceding example shows the following coding pattern:
+ Create an output stream to receive the merged metadata\.
+ Create a `ByteArrayInputStream` from the provided byte array\.
+ Create an `OutputSegmentMerger`, passing in the following parameters:
  + The `ByteArrayOutputStream`\.
  + An empty list\. This list would contain the types of the master elements \(`MkvStartMasterElement` or `MkvEndMasterElement`\) in the stream\. Because the test input stream doesn't have any master elements, the test passes in an empty list\. 
  + A count visitor containing a list of MKV information types to count\.
  + `False`, indicating that the merger will continue merging when a non\-matching type is encountered\.
+ Merge each element in the input data into the output stream\.

## MergeGetMediaOutput<a name="parser-library-write-MGMO"></a>

This class uses the AWS SDK for Java to execute a [GetMedia](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_dataplane_GetMedia.html) operation, and then parses the result into an MKV stream\.

The following code examples are from the `MergeGetMediaOutput` file\.

The following example demonstrates how to execute a `GetMedia` operation:

```
 public static void main (String [] args) throws IOException {
        AmazonKinesisVideo client = AmazonKinesisVideoClient.builder().standard()
                .withCredentials(new ProfileCredentialsProvider())
                .withEndpointConfiguration(new AwsClientBuilder.EndpointConfiguration(
                        "https://kinesisvideo.us-east-1.amazonaws.com",
                        REGION.getName()))
                .build();

        String getMediaEndpoint = client.getDataEndpoint(new GetDataEndpointRequest().withStreamName(STREAM_NAME)
                .withAPIName(APIName.GET_MEDIA)).getDataEndpoint();
        System.out.println("GetMedia endpoint " + getMediaEndpoint);

        AmazonKinesisVideoMedia mediaClient = AmazonKinesisVideoMediaClient.builder()
                .withCredentials(new ProfileCredentialsProvider())
                .withEndpointConfiguration(new AwsClientBuilder.EndpointConfiguration(getMediaEndpoint,
                        REGION.getName()))
                .build();
        GetMediaResult result = mediaClient.getMedia(getMediaRequest());
        System.out.println("GetMedia response started: Request Id: " + result.getSdkResponseMetadata().getRequestId());
        System.out.println("GetMedia response started  " + result.getContentType());

        mergeGetMedia(result);
    }
```

The previous code example shows the following coding pattern:
+ Create an `AmazonKinesisVideo` client\. The client is used to make calls to Kinesis Video Streams\.
+ Get an endpoint for the Kinesis Video Streams service\.
+ Create an `AmazonKinesisVideoMedia` client\.
+ Execute a `GetMedia` operation\.
+ Pass the result of the operation to the private `mergeGetMedia` method\.

The following example from the `mergeGetMedia` function uses a [StreamingMkvReader](#parser-library-write-SMSR) and a [OutputSegmentMerger](#parser-library-write-OSM) to parse the result of the `GetMedia` operation:

```
private static void mergeGetMedia(GetMediaResult result) throws IOException {
        Path outputPath = Paths.get("merged_output2.mkv");
        System.out.println("Writing merged file to " +outputPath.toAbsolutePath().toString());
        OutputStream fileOutputStream = Files.newOutputStream(outputPath,
                StandardOpenOption.WRITE, StandardOpenOption.CREATE);
        try (BufferedOutputStream outputStream = new BufferedOutputStream(fileOutputStream)) {
            outputStream.flush();

            StreamingMkvReader mkvStreamReader = StreamingMkvReader.createDefault(new InputStreamParserByteSource(result.getPayload()));
            OutputSegmentMerger outputSegmentMerger = OutputSegmentMerger.createDefault(fileOutputStream);

            long prevFragmentDoneTime = System.currentTimeMillis();
            //Apply the OutputSegmentMerger to the mkv elements from the mkv stream reader.
            try {
                while (mkvStreamReader.mightHaveNext()) {
                    Optional<MkvElement> mkvElementOptional = mkvStreamReader.nextIfAvailable();
                    if (mkvElementOptional.isPresent()) {
                        MkvElement mkvElement = mkvElementOptional.get();
                        //Apply the segment merger to this element.
                        mkvElement.accept(outputSegmentMerger);

                        //count the number of fragments merged.
                        if (MkvTypeInfos.SEGMENT.equals(mkvElement.getElementMetaData().getTypeInfo())) {
                            if (mkvElement instanceof MkvEndMasterElement) {
                                System.out.println("Fragment numbers merged " + outputSegmentMerger.getClustersCount() + " took  "
                                        + Long.toString(System.currentTimeMillis() - prevFragmentDoneTime) + " ms.");
                                prevFragmentDoneTime = System.currentTimeMillis();
                                outputStream.flush();
                            }
                        }
                    }
                }
            } catch (MkvElementVisitException e) {
                System.err.println("Exception from visitor to MkvElement "+e);
            }
        }
    }
```

The previous code example shows the following coding pattern:
+ Create an output stream\. The output stream writes the MKV data to a file\.
+ Create a `StreamingMkvReader`, and a `OutputSegmentMerger`\.
+ Iterate on the items in the `StreamingMkvReader` and write them to the `OutputSegmentMerger`\.
+ When a `MkvEndMasterElement` is reached, flush the contents of the output stream to the file\.

## KinesisVideoExample<a name="parser-library-write-example"></a>

This is a sample application that shows how to use the Kinesis Video Stream Parser Library\.

This class performs the following operations:
+ Creates a Kinesis video stream\. If a stream with the given name already exists, the stream is deleted and recreated\.
+ Calls [PutMedia](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_dataplane_PutMedia.html) to stream video fragments to the Kinesis video stream\.
+ Calls [GetMedia](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_dataplane_GetMedia.html) to stream video fragments out of the Kinesis video stream\.
+ Uses a [StreamingMkvReader](#parser-library-write-SMSR) to parse the returned fragments on the stream, and uses a [FragmentMetadataVisitor](#parser-library-write-FMV) to log the fragments\.

### Delete and recreate the stream<a name="parser-library-write-example-create"></a>

The following code example \(from the `KinesisVideoExample.java` file\) deletes a given Kinesis video stream:

```
//Delete the stream
amazonKinesisVideo.deleteStream(new DeleteStreamRequest().withStreamARN(streamInfo.get().getStreamARN()));
```

The following code example \(from the `KinesisVideoExample.java` file\) creates a Kinesis video stream with the specified name:

```
amazonKinesisVideo.createStream(new CreateStreamRequest().withStreamName(streamName)
.withDataRetentionInHours(DATA_RETENTION_IN_HOURS)
.withMediaType("video/h264"));
```

### Call PutMedia<a name="parser-library-write-example-putmedia"></a>

The following code example \(from the `PutMediaWorker.java` file\) calls [PutMedia](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_dataplane_PutMedia.html) on the stream:

```
 putMedia.putMedia(new PutMediaRequest().withStreamName(streamName)
.withFragmentTimecodeType(FragmentTimecodeType.RELATIVE)
.withProducerStartTimestamp(new Date())
.withPayload(inputStream), new PutMediaAckResponseHandler() {
...
});
```

### Call GetMedia<a name="parser-library-write-example-getmedia"></a>

The following code example \(from the `GetMediaWorker.java` file\) calls [GetMedia](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_dataplane_GetMedia.html) on the stream:

```
GetMediaResult result = videoMedia.getMedia(new GetMediaRequest().withStreamName(streamName).withStartSelector(startSelector));
```

### Parse the GetMedia result<a name="parser-library-write-example-parse"></a>

This section describes how to use [StreamingMkvReader](#parser-library-write-SMSR), [FragmentMetadataVisitor](#parser-library-write-FMV) and `CompositeMkvElementVisitor` to parse, save to file, and log the data returned from `GetMedia`\.

#### Read the output of GetMedia with StreamingMkvReader<a name="parser-library-write-example-parse-smr"></a>

The following code example \(from the `GetMediaWorker.java` file\) creates a [StreamingMkvReader](#parser-library-write-SMSR) and uses it to parse the result from the [GetMedia](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_dataplane_GetMedia.html) operation:

```
StreamingMkvReader mkvStreamReader = StreamingMkvReader.createDefault(new InputStreamParserByteSource(result.getPayload()));
log.info("StreamingMkvReader created for stream {} ", streamName);
try {
    while (mkvStreamReader.mightHaveNext()) {
        Optional<MkvElement> mkvElementOptional = mkvStreamReader.nextIfAvailable();
```

In the preceding code example, the [StreamingMkvReader](#parser-library-write-SMSR) retrieves `MKVElement` objects from the payload of the `GetMedia` result\. In the next section, the elements are passed to a [FragmentMetadataVisitor](#parser-library-write-FMV)\.

#### Retrieve Fragments with FragmentMetadataVisitor<a name="parser-library-write-example-parse-fmv"></a>

The following code examples \(from the `KinesisVideoExample.java` and `GetMediaWorker.java` files\) create a [FragmentMetadataVisitor](#parser-library-write-FMV)\. The `MkvElement` objects iterated by the [StreamingMkvReader](#parser-library-write-SMSR) are then passed to the visitor using the `accept` method\. 

*from `KinesisVideoExample.java`:*

```
FragmentMetadataVisitor fragmentMetadataVisitor = FragmentMetadataVisitor.create();
```

*from `GetMediaWorker.java`:*

```
if (mkvElementOptional.isPresent()) {
    //Apply the MkvElement to the visitor
    mkvElementOptional.get().accept(elementVisitor);
        }
```

#### Log the elements and write them to a file<a name="parser-library-write-example-parse-cmev"></a>

The following code example \(from the `KinesisVideoExample.java` file\) creates the following objects and returns them as part of the return value of the `GetMediaProcessingArguments` function:
+ A `LogVisitor` \(an extension of `MkvElementVisitor`\) that writes to the system log\.
+ An `OutputStream` that writes the incoming data to an MKV file\.
+ A `BufferedOutputStream` that buffers data bound for the `OutputStream`\.
+ An [OutputSegmentMerger](#parser-library-write-OSM) that merges consecutive elements in the `GetMedia` result with the same track and EBML data\.
+ A `CompositeMkvElementVisitor` that composes the [FragmentMetadataVisitor](#parser-library-write-FMV), [OutputSegmentMerger](#parser-library-write-OSM), and `LogVisitor` into a single element visitor

```
//A visitor used to log as the GetMedia stream is processed.
    LogVisitor logVisitor = new LogVisitor(fragmentMetadataVisitor);

    //An OutputSegmentMerger to combine multiple segments that share track and ebml metadata into one
    //mkv segment.
    OutputStream fileOutputStream = Files.newOutputStream(Paths.get("kinesis_video_example_merged_output2.mkv"),
            StandardOpenOption.WRITE, StandardOpenOption.CREATE);
    BufferedOutputStream outputStream = new BufferedOutputStream(fileOutputStream);
    OutputSegmentMerger outputSegmentMerger = OutputSegmentMerger.createDefault(outputStream);

    //A composite visitor to encapsulate the three visitors.
    CompositeMkvElementVisitor mkvElementVisitor =
            new CompositeMkvElementVisitor(fragmentMetadataVisitor, outputSegmentMerger, logVisitor);

    return new GetMediaProcessingArguments(outputStream, logVisitor, mkvElementVisitor);
```

The media processing arguments are then passed into the `GetMediaWorker`, which is in turn passed to the `ExecutorService` which executes the worker on a separate thread:

```
GetMediaWorker getMediaWorker = GetMediaWorker.create(getRegion(),
        getCredentialsProvider(),
        getStreamName(),
        new StartSelector().withStartSelectorType(StartSelectorType.EARLIEST),
        amazonKinesisVideo,
        getMediaProcessingArgumentsLocal.getMkvElementVisitor());
executorService.submit(getMediaWorker);
```

## Next Step<a name="parser-library-write-next"></a>

[Step 3: Run and Verify the Code](parser-library-run.md)