# Step 1: Download and Configure the C\+\+ Producer Library Code<a name="producersdk-cpp-download"></a>

In this section, you download the low\-level libraries and configure the application to use your AWS credentials\. 

For prerequisites and other details about this example, see [Using the C\+\+ Producer Library](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producer-sdk-cpp.html)\.

1. Create a directory, and then clone the example source code from the GitHub repository\. 

   ```
   git clone --recursive https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp.git
   ```
**Note**  
If you don't run git clone with `--recursive`, run `git submodule update --init` in the `amazon-kinesis-video-streams-producer-sdk-cpp/open-source` directory\. You must also install pkg\-config, CMake, and a build enviroment\. If you want to build the GStreamer plugin, you also have to install it locally\.  
For more information, see the README\.md in [https://github\.com/awslabs/amazon\-kinesis\-video\-streams\-producer\-sdk\-cpp](https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp)\.

1. Open the code in the integrated development environment \(IDE\) of your choice \(for example, [Eclipse](http://www.eclipse.org/)\)\.

## Next Step<a name="producersdk-cpp-download-next"></a>

[Step 2: Write and Examine the Code](producersdk-cpp-write.md)