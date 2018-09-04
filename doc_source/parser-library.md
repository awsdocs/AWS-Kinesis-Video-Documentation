# Kinesis Video Stream Parser Library<a name="parser-library"></a>

The Kinesis Video Stream Parser Library is an easy\-to\-use set of tools you can use in Java applications to consume the MKV data in a Kinesis video stream\.

The library includes the following tools:
+ [StreamingMkvReader](parser-library-write.md#parser-library-write-SMSR): This class reads specified MKV elements from a video stream\.
+ [FragmentMetadataVisitor](parser-library-write.md#parser-library-write-FMV): This class retrieves metadata for fragments \(media elements\) and tracks \(individual data streams containing media information, such as audio or subtitles\)\.
+ [OutputSegmentMerger](parser-library-write.md#parser-library-write-OSM): This class merges consecutive fragments or chunks in a video stream\.
+ [KinesisVideoExample](parser-library-write.md#parser-library-write-example): This is a sample application that shows how to use the Kinesis Video Stream Parser Library\.

The library also includes tests that show how the tools are used\.

## Procedure: Using the Kinesis Video Stream Parser Library<a name="parser-library-procedure"></a>

This procedure includes the following steps:
+ [Step 1: Download and Configure the Code](parser-library-download.md)
+ [Step 2: Write and Examine the Code](parser-library-write.md)
+ [Step 3: Run and Verify the Code](parser-library-run.md)

## Prerequisites<a name="parser-library-prerequisites"></a>

You must have the following to examine and use the Kinesis Video Stream Parser Library:
+ An Amazon Web Services \(AWS\) account\. If you don't already have an AWS account, do the following:
  + Open [https://aws\.amazon\.com/](https://aws.amazon.com/), and then choose **Create an AWS Account**\.
**Note**  
This might be unavailable in your browser if you previously signed in to the AWS Management Console\. In that case, choose **Sign In to the Console**, and then choose **Create a new AWS account**\.
  + Follow the online instructions\.

    Part of the sign\-up procedure involves receiving a phone call and entering a PIN using the phone keypad\.

  Note your AWS account ID because you need it for configuring programmatic access to Kinesis video streams\.
+ A Java integrated development environment \(IDE\), such as [Eclipse Java Neon](https://www.eclipse.org/downloads/packages/release/neon/3/eclipse-jee-neon-3) or [JetBrains IntelliJ Idea](https://www.jetbrains.com/idea/download/)\.