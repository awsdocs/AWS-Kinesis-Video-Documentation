# Step 1: Download and Configure the C\+\+ Producer Library Code<a name="producersdk-cpp-download"></a>

In this section, you download the low\-level libraries and configure the application to use your AWS credentials\. 

For prerequisites and other details about this example, see [Using the C\+\+ Producer Library](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producer-sdk-cpp.html)\.

1. Create a directory, and then clone the example source code from the GitHub repository\. 

   ```
   $ git clone https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp 
   ```

1. Open the code in the integrated development environment \(IDE\) of your choice \(for example, [Eclipse](http://www.eclipse.org/)\)\.

1. At the command line, set the `ACCESS_KEY_ENV_VAR` and `SECRET_KEY_ENV_VAR` environment variables to your AWS credentials\. Alternatively, you can hardcode your AWS credentials in the following lines of `ProducerTestFixture.h`: 

   ```
           if (nullptr == (accessKey = getenv(ACCESS_KEY_ENV_VAR))) {
               accessKey = "AccessKey";
           }
   
           if (nullptr == (secretKey = getenv(SECRET_KEY_ENV_VAR))) {
               secretKey = "SecretKey";
           }
   ```

1. In `tst/ProducerTestFixture.h`, find the call to `CreateStream`\. Change the name of the stream definition from `ScaryTestStream2` to a unique name:

   ```
   shared_ptr<KinesisVideoStream> CreateTestStream(int index) {
           char stream_name[MAX_STREAM_NAME_LEN];
           sprintf(stream_name, "ScaryTestStream_%d", index);
   ```

## Next Step<a name="producersdk-cpp-download-next"></a>

[Step 2: Write and Examine the Code](producersdk-cpp-write.md)