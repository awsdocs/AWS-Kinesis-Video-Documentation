# Example: Sending Data to Kinesis Video Streams Using the PutMedia API<a name="examples-putmedia"></a>

This example demonstrates how to use the [PutMedia](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_dataplane_PutMedia.html) API\. It shows how to send data that is already in a container format \(MKV\)\. If your data needs to be assembled into a container format before sending \(for example, if you are assembling camera video data into frames\), see [Kinesis Video Streams Producer Libraries](producer-sdk.md)\.

**Note**  
The `PutMedia` operation is available only in the C\+\+ and Java SDKs, due to the full\-duplex management of connections, data flow, and acknowledgements\. It is not supported in other languages\.

**Topics**
+ [Step 1: Download and Configure the Code](#examples-putmedia-download)
+ [Step 2: Write and Examine the Code](#examples-putmedia-write)
+ [Step 3: Run and Verify the Code](#examples-putmedia-run)

## Step 1: Download and Configure the Code<a name="examples-putmedia-download"></a>

In this section, you download the Java example code, import the project into your Java IDE, configure the library locations, and configure the code to use your AWS credentials\.

1. Create a directory and clone the example source code from the GitHub repository\. The `PutMedia` example is part of the [Java Producer Library](producer-sdk-javaapi.md)\.

   ```
   $ git clone https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-java
   ```

1. Open the Java IDE that you are using \(for example, [Eclipse](http://www.eclipse.org/) or [IntelliJ IDEA](https://www.jetbrains.com/idea/)\), and import the Apache Maven project that you downloaded: 
   + **In Eclipse:** Choose **File**, **Import**, **Maven**, **Existing Maven Projects**, and navigate to the root of the downloaded package\. Select the `pom.xml` file\.
   + **In IntelliJ Idea: ** Choose **Import**\. Navigate to the `pom.xml` file in the root of the downloaded package\.

    For more information, see the related IDE documentation\.

1. Update the project so that the IDE can find the libraries that you imported\.
   + For IntelliJ IDEA, do the following:

     1. Open the context \(right\-click\) menu for the project's **lib** directory, and choose **Add as library**\.

     1. Choose **File**, **Project Structure**\. 

     1. Under **Project Settings**, choose **Modules**\. 

     1. In the **Sources** tab, set **Language Level** to **7** or higher\.
   + For Eclipse, do the following:

     1. Open the context \(right\-click\) menu for the project, and choose **Properties**, **Java Build Path**, **Source**\. Then do the following:

        1. On the **Source** tab, double\-click **Native library location**\.

        1. In the **Native Library Folder Configuration** wizard, choose **Workspace**\.

        1. In the **Native Library Folder** selection, choose the **lib** directory in the project\.

     1. Open the context \(right\-click\) menu for the project, and choose **Properties**\. Then do the following:

        1. On the **Libraries** tab, choose **Add Jars**\.

        1. In the **JAR selection** wizard, choose all the \.jars in the project's `lib` directory\.

## Step 2: Write and Examine the Code<a name="examples-putmedia-write"></a>

The `PutMedia` API example \(`PutMediaDemo`\) shows the following coding pattern:

**Topics**
+ [Create the PutMediaClient](#producersdk-javaapi-writecode-putmediaapi-putmediaclient)
+ [Stream Media and Pause the Thread](#producersdk-javaapi-writecode-putmediaapi-run)

The code examples in this section are from the `PutMediaDemo` class\.

### Create the PutMediaClient<a name="producersdk-javaapi-writecode-putmediaapi-putmediaclient"></a>

Creating the `PutMediaClient` object takes the following parameters:
+ The URI for the `PutMedia` endpoint\.
+ An `InputStream` pointing to the MKV file to stream\.
+ The stream name\. This example uses the stream that was created in the [Using the Java Producer Library](producer-sdk-javaapi.md) \(`my-stream`\)\. To use a different stream, change the following parameter:

  ```
  private static final String STREAM_NAME="my-stream";
  ```
**Note**  
The `PutMedia` API example does not create a stream\. You must create a stream either by using the test application for the [Using the Java Producer Library](producer-sdk-javaapi.md), by using the Kinesis Video Streams console, or by using the AWS CLI\.
+ The current timestamp\.
+ The time code type\. The example uses `RELATIVE`, indicating that the timestamp is relative to the start of the container\.
+ An `AWSKinesisVideoV4Signer` object that verifies that the received packets were sent by the authorized sender\.
+ The maximum upstream bandwidth in Kbps\.
+ An `AckConsumer` object to receive packet received acknowledgements\.

The following code creates the `PutMediaClient` object:

```
/* actually URI to send PutMedia request */
final URI uri = URI.create(KINESIS_VIDEO_DATA_ENDPOINT + PUT_MEDIA_API);

/* input stream for sample MKV file */
final InputStream inputStream = new FileInputStream(MKV_FILE_PATH);

/* use a latch for main thread to wait for response to complete */
final CountDownLatch latch = new CountDownLatch(1);

/* a consumer for PutMedia ACK events */
final AckConsumer ackConsumer = new AckConsumer(latch);

/* client configuration used for AWS SigV4 signer */
final ClientConfiguration configuration = getClientConfiguration(uri);

/* PutMedia client */
final PutMediaClient client = PutMediaClient.builder()
        .putMediaDestinationUri(uri)
        .mkvStream(inputStream)
        .streamName(STREAM_NAME)
        .timestamp(System.currentTimeMillis())
        .fragmentTimeCodeType("RELATIVE")
        .signWith(getKinesisVideoSigner(configuration))
        .upstreamKbps(MAX_BANDWIDTH_KBPS)
        .receiveAcks(ackConsumer)
        .build();
```

### Stream Media and Pause the Thread<a name="producersdk-javaapi-writecode-putmediaapi-run"></a>

After the client is created, the sample starts asynchronous streaming with `putMediaInBackground`\. The main thread is then paused with `latch.await` until the `AckConsumer` returns, at which point the client is closed\.

```
 /* start streaming video in a background thread */
            client.putMediaInBackground();

            /* wait for request/response to complete */
            latch.await();

            /* close the client */
            client.close();
```

## Step 3: Run and Verify the Code<a name="examples-putmedia-run"></a>

To run the `PutMedia` API example, do the following:

1. Create a stream named `my-stream` in the Kinesis Video Streams console or by using the AWS CLI\.

1. Change your working directory to the Java producer SDK directory:

   ```
   $ cd /<YOUR_FOLDER_PATH_WHERE_SDK_IS_DOWNLOADED>/amazon-kinesis-video-streams-producer-sdk-java/
   ```

1. Compile the Java SDK and demo application:

   ```
   mvn package
   ```

1. Create a temporary filename in the `/tmp` directory:

   ```
   $ jar_files=$(mktemp)
   ```

1. Create a classpath string of dependencies from the local repository to a file:

   ```
   $ mvn -Dmdep.outputFile=$jar_files dependency:build-classpath
   ```

1. Set the value of the `LD_LIBRARY_PATH` environment variable as follows:

   ```
   $ export LD_LIBRARY_PATH=/<YOUR_FOLDER_PATH_WHERE_SDK_IS_DOWNLOADED>/amazon-kinesis-video-streams-producer-sdk-cpp/kinesis-video-native-build/downloads/local/lib:$LD_LIBRARY_PATH
   $ classpath_values=$(cat $jar_files)
   ```

1. Run the demo from the command line as follows, providing your AWS credentials:

   ```
   $ java -classpath target/kinesisvideo-java-demo-1.0-SNAPSHOT.jar:$classpath_values -Daws.accessKeyId=${ACCESS_KEY} -Daws.secretKey=${SECRET_KEY} -Djava.library.path=/opt/amazon-kinesis-video-streams-producer-sdk-cpp/kinesis-video-native-build com.amazonaws.kinesisvideo.demoapp.DemoAppMain
   ```

1. Open the Kinesis Video Streams console at [https://console\.aws\.amazon\.com/kinesisvideo/](https://console.aws.amazon.com/kinesisvideo/), and choose your stream on the **Manage Streams** page\. The video plays in the **Video Preview** pane\.