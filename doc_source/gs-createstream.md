# Step 2: Create a Kinesis Video Stream<a name="gs-createstream"></a>

This section describes how to create a Kinesis video stream\.

This section contains the following procedures:
+ [Create a Video Stream Using the Console](#gs-createstream-console)
+ [Create a Video Stream Using the AWS CLI](#gs-createstream-cli)

## Create a Video Stream Using the Console<a name="gs-createstream-console"></a>

1. Sign in to the AWS Management Console and open the Kinesis console at [https://console\.aws\.amazon\.com/kinesis](https://console.aws.amazon.com/kinesis)\.

1. On the **Video streams** page, choose **Create video stream**\.

1. On the **Create a new video stream** page, type **ExampleStream** for the stream name\. Leave the **Default configuration** radio button selected\. 

1. Choose **Create video stream**\.

1. After Kinesis Video Streams creates the stream, review the details on the **ExampleStream** page\.

## Create a Video Stream Using the AWS CLI<a name="gs-createstream-cli"></a>

1. Ensure that you have the AWS CLI installed and configured\. For more information, see the [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/) documentation\.

1. Run the following `Create-Stream` command in the AWS CLI: 

   ```
   $ aws kinesisvideo create-stream --stream-name "MyKVStream" --data-retention-in-hours "24"
   ```

   The response will look similar to the following:

   ```
   {
       "StreamARN": "arn:aws:kinesisvideo:us-west-2:123456789012:stream/MyKVSStream/123456789012"
   }
   ```

## Next Step<a name="gs-next-step-3"></a>

[Step 3: Send Data to a Kinesis Video Stream](gs-send-data.md)