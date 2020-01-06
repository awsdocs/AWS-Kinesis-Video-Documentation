# Data Protection in Kinesis Video Streams<a name="how-kms"></a>

Server\-side encryption using AWS Key Management Service \(AWS KMS\) keys makes it easier for you to meet strict data management requirements by encrypting your data at rest in Amazon Kinesis Video Streams\.

**Topics**
+ [What Is Server\-Side Encryption for Kinesis Video Streams?](#what-is-sse-akvs)
+ [Costs, Regions, and Performance Considerations](#costs-performance-akvs)
+ [How Do I Get Started with Server\-Side Encryption?](#getting-started-with-sse-akvs)
+ [Creating and Using User\-Generated AWS KMS Master Keys](#creating-using-sse-master-keys-akvs)
+ [Permissions to Use User\-Generated AWS KMS Master Keys](#permissions-user-key-KMS-akvs)

## What Is Server\-Side Encryption for Kinesis Video Streams?<a name="what-is-sse-akvs"></a>

Server\-side encryption is a feature in Kinesis Video Streams that automatically encrypts data before it's at rest by using an AWS KMS customer master key \(CMK\) that you specify\. Data is encrypted before it is written to the Kinesis Video Streams stream storage layer, and it is decrypted after it is retrieved from storage\. As a result, your data is always encrypted at rest within the Kinesis Video Streams service\.

With server\-side encryption, your Kinesis video stream producers and consumers don't need to manage master keys or cryptographic operations\. If data retention is enabled, your data is automatically encrypted as it enters and leaves Kinesis Video Streams, so your data at rest is encrypted\. AWS KMS provides all the master keys that are used by the server\-side encryption feature\. AWS KMS makes it easier to use a CMK for Kinesis Video Streams that is managed by AWS, a user\-specified AWS KMS CMK, or a master key imported into the AWS KMS service\.

## Costs, Regions, and Performance Considerations<a name="costs-performance-akvs"></a>

When you apply server\-side encryption, you are subject to AWS KMS API usage and key costs\. Unlike custom AWS KMS master keys, the `(Default) aws/kinesis-video` customer master key \(CMK\) is offered free of charge\. However, you still must pay for the API usage costs that Kinesis Video Streams incurs on your behalf\.

API usage costs apply for every CMK, including custom ones\. The KMS costs scale with the number of user credentials that you use on your data producers and consumers because each user credential requires a unique API call to AWS KMS\. 

The following describes the costs by resource:

**Keys**
+ The CMK for Kinesis Video Streams that's managed by AWS \(alias = `aws/kinesis-video`\) is free\.
+ User\-generated AWS KMS keys are subject to AWS KMS key costs\. For more information, see [AWS Key Management Service Pricing](https://aws.amazon.com/kms/pricing/#Keys)\.

### AWS KMS API Usage<a name="api-usage"></a>

API requests to generate new data encryption keys or to retrieve existing encryption keys increase as traffic increases, and are subject to AWS KMS usage costs\. For more information, see [AWS Key Management Service Pricing: Usage](https://aws.amazon.com/kms/pricing/#Usage)\.

Kinesis Video Streams generates key requests even when retention is set to 0 \(no retention\)\.

### Availability of Server\-Side Encryption by Region<a name="sse-regions-akvs"></a>

Server\-side encryption of Kinesis video streams is available in all the AWS Regions where Kinesis Video Streams is available\.

## How Do I Get Started with Server\-Side Encryption?<a name="getting-started-with-sse-akvs"></a>

Server\-side encryption is always enabled on Kinesis video streams\. If a user\-provided key is not specified when the stream is created, the default key \(provided by Kinesis Video Streams\) is used\.

A user\-provided AWS KMS master key must be assigned to a Kinesis video stream when it is created\. You can't later assign a different key to a stream using the [UpdateStream](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_UpdateStream.html) API\.

You can assign a user\-provided AWS KMS master key to a Kinesis video stream in two ways:
+ When creating a Kinesis video stream in the AWS Management Console, specify the AWS KMS master key in the **Encryption** tab on the **Create a new video stream** page\.
+ When creating a Kinesis video stream using the [CreateStream](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_CreateStream.html) API, specify the key ID in the `KmsKeyId` parameter\.

## Creating and Using User\-Generated AWS KMS Master Keys<a name="creating-using-sse-master-keys-akvs"></a>

This section describes how to create and use your own AWS KMS master keys instead of using the master key administered by Amazon Kinesis Video Streams\.

### Creating User\-Generated AWS KMS Master Keys<a name="creating-sse-master-keys-akvs"></a>

For information about how to create your own master keys, see [Creating Keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service Developer Guide*\. After you create keys for your account, the Kinesis Video Streams service returns these keys in the **KMS master key** list\.

### Using User\-Generated AWS KMS Master Keys<a name="using-sse-master-keys-akvs"></a>

After the correct permissions are applied to your consumers, producers, and administrators, you can use custom AWS KMS master keys in your own AWS account or another AWS account\. All AWS KMS master keys in your account appear in the **KMS Master Key** list on the console\.

To use custom AWS KMS master keys that are located in another account, you must have permissions to use those keys\. You must also create the stream using the `CreateStream` API\. You can't use AWS KMS master keys from different accounts in streams created in the console\.

**Note**  
The AWS KMS key is not accessed until the `PutMedia` or `GetMedia` operation is executed\. This has the following results:  
If the key you specify doesn't exist, the `CreateStream` operation succeeds, but `PutMedia` and `GetMedia` operations on the stream fail\.
If you use the provided master key \(`aws/kinesis-video`\), the key is not present in your account until the first `PutMedia` or `GetMedia` operation is performed\.

## Permissions to Use User\-Generated AWS KMS Master Keys<a name="permissions-user-key-KMS-akvs"></a>

Before you can use server\-side encryption with a user\-generated AWS KMS master key, you must configure AWS KMS key policies to allow encryption of streams and encryption and decryption of stream records\. For examples and more information about AWS KMS permissions, see [AWS KMS API Permissions: Actions and Resources Reference](https://docs.aws.amazon.com/kms/latest/developerguide/kms-api-permissions-reference.html)\. 

**Note**  
The use of the default service key for encryption does not require application of custom IAM permissions\.

Before you use user\-generated AWS KMS master keys, ensure that your Kinesis video stream producers and consumers \(IAM principals\) are users in the AWS KMS master key policy\. Otherwise, writes and reads from a stream will fail, which could ultimately result in data loss, delayed processing, or hung applications\. You can manage permissions for AWS KMS keys using IAM policies\. For more information, see [Using IAM Policies with AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/iam-policies.html)\.

### Example Producer Permissions<a name="example-producer-permissions-akvs"></a>

Your Kinesis video stream producers must have the `kms:GenerateDataKey` permission:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
        "Effect": "Allow",
        "Action": [
            "kms:GenerateDataKey"
        ],
        "Resource": "arn:aws:kms:us-west-2:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab"
    }, 
    {
        "Effect": "Allow",
        "Action": [
            "kinesis-video:PutMedia",
        ],
        "Resource": "arn:aws:kinesis-video:*:123456789012:MyStream"
    }
  ]
}
```

### Example Consumer Permissions<a name="example-consumer-permissions-akvs"></a>

Your Kinesis video stream consumers must have the `kms:Decrypt` permission:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
        "Effect": "Allow",
        "Action": [
            "kms:Decrypt"
        ],
        "Resource": "arn:aws:kms:us-west-2:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab"
    }, 
    {
        "Effect": "Allow",
        "Action": [
            "kinesis-video:GetMedia",
        ],
        "Resource": "arn:aws:kinesis-video:*:123456789012:MyStream"
    }
  ]
}
```