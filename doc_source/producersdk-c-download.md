# Step 1: Download the C Producer Library Code<a name="producersdk-c-download"></a>

In this section, you download the low\-level libraries\. For prerequisites and other details about this example, see [Using the C Producer Library](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producer-sdk-cpp.html)\.

1. Create a directory, and then clone the example source code from the GitHub repository\. 

   ```
   git clone --recursive https://github.com/awslabs/amazon-kinesis-video-streams-producer-c.git
   ```
**Note**  
If you miss running git clone with `--recursive`, run `git submodule update --init` in the `amazon-kinesis-video-streams-producer-c/open-source` directory\. You must also install pkg\-config, automake, CMake, and a build enviroment\.  
For more information, see the `README.md` in [https://github\.com/awslabs/amazon\-kinesis\-video\-streams\-producer\-c\.git](https://github.com/awslabs/amazon-kinesis-video-streams-producer-c.git)\.

1. Open the code in the integrated development environment \(IDE\) of your choice \(for example, [Eclipse](http://www.eclipse.org/)\)\.

## Next Step<a name="producersdk-c-download-next"></a>

[Step 2: Write and Examine the Code](producersdk-c-write.md)