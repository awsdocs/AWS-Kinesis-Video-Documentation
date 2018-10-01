# Using the Android Producer Library<a name="producer-sdk-android"></a>

Amazon Kinesis Video Streams provides the Android Producer Library, which you can use to write application code, with minimal configuration, to send media data from an Android device to a Kinesis video stream\. 

You must perform the following steps to integrate your code with Kinesis Video Streams so that your application can start streaming data to your Kinesis video stream:

1. Create an instance of the `KinesisVideoClient` object\. 

1. Create a `MediaSource` object by providing media source information\. For example, when creating a camera media source, you provide information such as identifying the camera and specifying the encoding the camera uses\.

   When you want to start streaming, you must create a custom media source\. 

## Procedure: Using the Android Producer SDK<a name="producer-sdk-android-using"></a>

This procedure demonstrates how to use the Kinesis Video Streams Android Producer Client in your Android application to send data to your Kinesis video stream\. 

The procedure includes the following steps:
+ [Download and Configure the Code](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producersdk-android-downloadcode.html)
+ [Examine the Code](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producersdk-android-writecode.html)
+ [Run and Verify the Code](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producersdk-android-reviewcode.html)

### Prerequisites<a name="producersdk-android-prerequisites"></a>
+ We recommend [Android Studio](https://developer.android.com/studio/index.html) for examining, editing, and running the application code\. We recommend at least version 3\.0\.0, released October 2017\.
+ In the sample code, you provide Amazon Cognito credentials\. Follow these procedures to set up an Amazon Cognito user pool and identity pool:

**To set up a user pool**

  1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\.

  1. Choose **Manage your User Pools**\.

  1. Choose **Create a user pool**\.

  1. Type a value for **Pool name**; for example, **<username>\_android\_user\_pool**\.

  1. Choose **Review defaults**\.

  1. Choose **Create pool**\.

  1. Copy and save the **Pool Id** value\. You will need this value when you configure the example application\.

  1. On the page for your pool, choose **App clients**\.

  1. Choose **Add an app client**\.

  1. Type a value for **App client name**; for example, **<username>\_android\_app\_client**\.

  1. Choose **Create app client**\.

  1. Choose **Show Details**, and copy and save the **App client ID** and **App client secret**\. You will need these values when you configure the example application\.

**To set up an identity pool**

  1. Open the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\.

  1. Choose **Manage Identity Pools**\.

  1. Choose **Create new identity pool**\.

  1. Type a value for **Identity pool name**; for example, **<username>\_android\_identity\_pool**\.

  1. Expand the **Authentication providers** section\. On the **Cognito** tab, add the values for the **User Pool ID** and **App client ID** from the previous procedure\.

  1. Choose **Create pool**\.

  1. On the next page, expand the **Show Details** section\. 

  1. In the section that has a value for **Role name** that ends in **Auth\_Role**, choose **View Policy Document**\.

  1. Choose **Edit**, and confirm the **Edit Policy** dialog box that appears\. Then copy the following JSON and paste it into the editor: 

     ```
     {
         "Version": "2012-10-17",
         "Statement": [
         {
           "Effect": "Allow",
           "Action": [
             "cognito-identity:*",
             "kinesisvideo:*"
           ],
           "Resource": [
             "*"
           ]
         }
       ]
     }
     ```

  1. Choose **Allow**\.

  1. On the next page, copy and save the **Identity pool ID** value from the **Get AWS Credentials** code snippet\. You will need this value when you configure the example application\.