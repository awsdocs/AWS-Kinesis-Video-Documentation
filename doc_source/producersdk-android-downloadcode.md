# Step 1: Download and Configure the Android Producer Library Code<a name="producersdk-android-downloadcode"></a>

In this section of the Android Producer Library procedure, you download the Android example code and open the project in Android Studio\. 

For prerequisites and other details about this example, see [Using the Android Producer Library](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producer-sdk-android.html)\.

1. Create a directory, and then clone the AWS Android SDK from the GitHub repository\. 

   ```
   $ git clone https://github.com/awslabs/aws-sdk-android-samples
   ```

1. Open [Android Studio](https://developer.android.com/studio/index.html)\.

1. In the opening screen, choose **Open an existing Android Studio project**\.

1. Navigate to the `aws-sdk-android-samples/AmazonKinesisVideoDemoApp` directory, and choose **OK**\.

1. Open the `AmazonKinesisVideoDemoApp/src/main/res/raw/awsconfiguration.json` file\.

   In the `CredentialsProvider` node, provide the identity pool ID from the **To set up an identity pool** procedure in the [Prerequisites](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producer-sdk-android.html#producersdk-android-prerequisites) section, and provide your AWS Region \(for example, **us\-west\-2**\)\.

   In the `CognitoUserPool` node, provide the App client secret, App client ID, and Pool ID from the **To set up a user pool** procedure in the [Prerequisites](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producer-sdk-android.html#producersdk-android-prerequisites) section, and provide your AWS Region \(for example, **us\-west\-2**\)\.

1. Your `awsconfiguration.json` file will look similar to the following:

   ```
   {
     "Version": "1.0",
     "CredentialsProvider": {
       "CognitoIdentity": {
         "Default": {
           "PoolId": "us-west-2:01234567-89ab-cdef-0123-456789abcdef",
           "Region": "us-west-2"
         }
       }
     },
     "IdentityManager": {
       "Default": {}
     },
     "CognitoUserPool": {
       "Default": {
         "AppClientSecret": "abcdefghijklmnopqrstuvwxyz0123456789abcdefghijklmno",
         "AppClientId": "0123456789abcdefghijklmnop",
         "PoolId": "us-west-2_qRsTuVwXy",
         "Region": "us-west-2"
       }
     }
   }
   ```

1. Update the `AmazonKinesisVideoDemoApp/src/main/java/com/amazonaws/kinesisvideo/demoapp/KinesisVideoDemoApp.java` with your region \(in the sample below, it’s set to **US\_WEST\_2**\): 

   ```
   public class KinesisVideoDemoApp extends Application {
       public static final String TAG = KinesisVideoDemoApp.class.getSimpleName();
       public static Regions KINESIS_VIDEO_REGION = Regions.US_WEST_2;
   ```

   For AWS region constants, see [Regions](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/regions/Regions.html)\.

## Next Step<a name="producersdk-android-downloadcode-next"></a>

[Step 2: Examine the Code](producersdk-android-writecode.md)