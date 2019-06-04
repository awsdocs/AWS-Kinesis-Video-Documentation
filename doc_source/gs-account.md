# Step 1: Set Up an AWS Account and Create an Administrator<a name="gs-account"></a>

Before you use Kinesis Video Streams for the first time, complete the following tasks: 

1. [Sign Up for AWS](#gs-account-create) \(unless you already have an account\)

1. [Create an Administrator IAM User](#gs-account-user)

1. [Create an AWS Account Key](#gs-account-key)

## Sign Up for AWS<a name="gs-account-create"></a>

If you already have an AWS account, you can skip this step\.

When you sign up for Amazon Web Services \(AWS\), your AWS account is automatically signed up for all services in AWS, including Kinesis Video Streams\. When you use Kinesis Video Streams, you are charged based on the amount of data ingested into, stored by, and consumed from the service\. If you are a new AWS customer, you can get started with Kinesis Video Streams for free\. For more information, see [AWS Free Usage Tier](https://aws.amazon.com//free/)\.

**To create an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

Write down your AWS account ID because you need it for the next task\.

## Create an Administrator IAM User<a name="gs-account-user"></a>

When you sign up for AWS, you provide an email address and password that is associated with your AWS account\. This is your *AWS account root user*\. Its credentials provide complete access to all of your AWS resources\. 

**Note**  
For security reasons, we recommend that you use the root user only to create an *administrator*, which is an *IAM user* with full permissions to your AWS account\. You can then use this administrator to create other IAM users and roles with limited permissions\. For more information, see [IAM Best Practices](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#create-iam-users) and [Creating an Admin User and Group](http://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\. 

**To create an administrator and sign into the console**

1. Create an administrator in your AWS account\. For instructions, see [Creating Your First IAM User and Administrators Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.

1. As an administrator, you can sign in to the console using a special URL\. For more information, see [How Users Sign in to Your Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_how-users-sign-in.html) in the *IAM User Guide*\.

The administrator can create more users in the account\. IAM users by default don't have any permissions\. The administrator can create users and manage their permissions\. For more information, see [Creating Your First IAM User and Administrators Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html)\. 

For more information about IAM, see the following:
+ [AWS Identity and Access Management \(IAM\)](https://aws.amazon.com/iam/)
+ [Getting Started](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started.html)
+ [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)

## Create an AWS Account Key<a name="gs-account-key"></a>

You will need an AWS Account Key to access Kinesis Video Streams programmatically\. 

To create an AWS Account Key, do the following:

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose **Users** in the navigation bar, and choose the **Admininstrator** user\. 

1. Choose the **Security credentials** tab, and choose **Create access key**\.

1. Record the **Access key ID**\. Choose **Show** under **Secret access key**\. Record the **Secret access key**\.

## Next Step<a name="gs-next-step-2"></a>

[Step 2: Create a Kinesis Video Stream](gs-createstream.md)