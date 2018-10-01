# Step 3: Run and Verify the Code<a name="producersdk-javaapi-reviewcode"></a>

To run the Java test harness for the [Java Producer library](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producer-sdk-javaapi.html), do the following\.

1. Choose **DemoAppMain**\.

1. Choose **Run**, **Run 'DemoAppMain'**\.

1. Add your credentials to the JVM arguments for the application:
   + **For non\-temporary AWS credentials:** `"-Daws.accessKeyId={YourAwsAccessKey} -Daws.secretKey={YourAwsSecretKey} -Djava.library.path={NativeLibraryPath}"` 
   + **For temporary AWS credentials:** `"-Daws.accessKeyId={YourAwsAccessKey} -Daws.secretKey={YourAwsSecretKey} -Daws.sessionToken={YourAwsSessionToken} -Djava.library.path={NativeLibraryPath}" ` 

1. Sign in to the AWS Management Console and open the Kinesis Video Streams console\. 

   On the **Manage Streams** page, choose your stream\.

1. The sample video will play in the embedded player\. You might need to wait a short time \(up to ten seconds under typical bandwidth and processor conditions\) while the frames accumulate before the video appears\.

The code example creates a stream\. As the `MediaSource` in the code starts, it begins sending sample frames to the `KinesisVideoClient`\. The client then sends the data to your Kinesis video stream\. 