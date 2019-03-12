# Step 1: Download and Configure the Code<a name="parser-library-download"></a>

In this section, you download the Java library and test code, and import the project into your Java IDE\.

For prerequisites and other details about this procedure, see [Kinesis Video Stream Parser Library](parser-library.md)\.

1. Create a directory and clone the library source code from the GitHub repository \([https://github\.com/aws/amazon\-kinesis\-video\-streams\-parser\-library](https://github.com/aws/amazon-kinesis-video-streams-parser-library)\)\. 

   ```
   $ git clone https://github.com/aws/amazon-kinesis-video-streams-parser-library
   ```

1. Open the Java IDE that you are using \(for example, [Eclipse](http://www.eclipse.org/) or [IntelliJ IDEA](https://www.jetbrains.com/idea/)\) and import the Apache Maven project that you downloaded: 
   + **In Eclipse:** Choose **File**, **Import**, **Maven**, **Existing Maven Projects**, and navigate to the `kinesis-video-streams-parser-lib` folder\.
   + **In IntelliJ Idea: ** Choose **Import**\. Navigate to the **pom\.xml** file in the root of the downloaded package\.

    For more information, see the related IDE documentation\.

## Next Step<a name="parser-library-download-next"></a>

[Step 2: Write and Examine the Code](parser-library-write.md)