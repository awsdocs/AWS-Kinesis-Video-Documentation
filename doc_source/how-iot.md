# Controlling Access to Kinesis Video Streams Resources Using AWS IoT<a name="how-iot"></a>

This section describes how to enable a device \(for example, a camera\) to send audio/video data to one particular Kinesis video stream only\. You can do this by using the AWS IoT credentials provider and an AWS Identity and Access Management \(IAM\) role\.

Devices can use X\.509 certificates to connect to AWS IoT using TLS mutual authentication protocols\. Other AWS services \(for example, Kinesis Video Streams\) do not support certificate\-based authentication, but they can be called using AWS credentials in AWS Signature Version 4 format\. The Signature Version 4 algorithm normally requires the caller to have an access key ID and a secret access key\. AWS IoT has a credentials provider that allows you to use the built\-in X\.509 certificate as the unique device identity to authenticate AWS requests \(for example, requests to Kinesis Video Streams\)\. This eliminates the need to store an access key ID and a secret access key on your device\. 

The credentials provider authenticates a client \(in this case, a Kinesis Video Streams SDK that is running on the camera from which you want to send data to a video stream\) using an X\.509 certificate and issues a temporary, limited\-privilege security token\. The token can be used to sign and authenticate any AWS request \(in this case, a call to the Kinesis Video Streams\)\. For more information, see [Authorizing Direct Calls to AWS Services](https://docs.aws.amazon.com/iot/latest/developerguide/authorizing-direct-aws.html)\.

This way of authenticating your camera's requests to Kinesis Video Streams requires you to create and configure an IAM role and attach appropriate IAM policies to the role so that the IoT credentials provider can assume the role on your behalf\. 

For more information about AWS IoT, see [AWS IoT Core Documentation](https://docs.aws.amazon.com/iot/?id=docs_gateway)\. For more information about IAM, see [AWS Identity and Access Management \(IAM\)](https://aws.amazon.com/iam/)\.

**Topics**
+ [IoT ThingName as Stream Name](#how-iot-thingnamestreamname)
+ [IoT CertificateId as Stream Name](#how-iot-iotcertstreamname)

## IoT ThingName as Stream Name<a name="how-iot-thingnamestreamname"></a>

**Topics**
+ [Step 1: Create an IoT Thing Type and an IoT Thing](#how-iot-create-thing-type)
+ [Step 2: Create an IAM Role to be Assumed by IoT](#how-iot-add-iot-policy)
+ [Step 3: Create and Configure the X\.509 Certificate](#how-iot-create-cert-keys)
+ [Step 4: Test the IoT Credentials with Your Kinesis Video Stream](#how-iot-test-it)
+ [Step 5: Deploying IoT Certificates and Credentials on Your Camera's File System and Streaming Data to Your Video Stream](#how-iot-test-it)

### Step 1: Create an IoT Thing Type and an IoT Thing<a name="how-iot-create-thing-type"></a>

In IoT, a thing is a representation of a specific device or logical entity\. In this case, an IoT thing represents your Kinesis video stream for which you want to configure resource\-level access control\. In order to create a thing, first, you must create an IoT thing type\. IoT thing types enable you to store description and configuration information that is common to all things associated with the same thing type\.

1. The following example command creates a thing type `kvs_example_camera`:

   ```
   aws --profile default  iot create-thing-type --thing-type-name kvs_example_camera > iot-thing-type.json
   ```

1. And this example command creates the `kvs_example_camera_stream` thing of the `kvs_example_camera` thing type:

   ```
   aws --profile default  iot create-thing --thing-name kvs_example_camera_stream --thing-type-name kvs_example_camera > iot-thing.json
   ```

### Step 2: Create an IAM Role to be Assumed by IoT<a name="how-iot-add-iot-policy"></a>

IAM roles are similar to IAM users, in that a role is an AWS identity with permission policies that determine what the identity can and cannot do in AWS\. A role can be assumed by anyone who needs it\. When you assume a role, it provides you with temporary security credentials for your role session\.

The role that you create in this step can be assumed by IoT in order to obtain temporary credentials from the security token service \(STS\) when performing credential authorization requests from the client\. In this case, the client is the Kinesis Video Streams SDK that is running on your camera\. 

Perform the following steps to create and configure this IAM role:

1. Create an IAM role\.

   The following example command creates an IAM role called `KVSCameraCertificateBasedIAMRole`:

   ```
    
   aws --profile default  iam create-role --role-name KVSCameraCertificateBasedIAMRole --assume-role-policy-document 'file://iam-policy-document.json' > iam-role.json
   ```

   You can use the following trust policy JSON for the `iam-policy-document.json`:

   ```
   {
      "Version":"2012-10-17",
      "Statement":[
         {
   	 "Effect":"Allow",
   	 "Principal":{
   	    "Service":"credentials.iot.amazonaws.com"
       },
   	 "Action":"sts:AssumeRole"
    }
      ]
   }
   ```

1. Next, you must attach a permissions policy to the IAM role you created above\. This permissions policy allows selective access control \(a subset of supported operations\) for an AWS resource\. In this case, the AWS resource is the video stream to which you want your camera to send data\. In other words, once all the configuration steps are complete, this camera will be able to send data only to this video stream\.

   ```
   aws --profile default iam put-role-policy --role-name KVSCameraCertificateBasedIAMRole --policy-name KVSCameraIAMPolicy --policy-document 'file://iam-permission-document.json' 
   ```

   You can use the following IAM policy JSON for the iam\-permission\-document\.json:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "kinesisvideo:DescribeStream",
                   "kinesisvideo:PutMedia",
                   "kinesisvideo:TagStream",
                   "kinesisvideo:GetDataEndpoint"
               ],
               "Resource": "arn:aws:kinesisvideo:*:*:stream/${credentials-iot:ThingName}/*"
           }
       ]
   }
   ```

   Note that this policy authorizes the specified actions only on a video stream \(AWS resource\) that is specified by the placeholder \($\{credentials\-iot:ThingName\}\)\. This placeholder takes on the value of the IoT thing attribute `ThingName` when the IoT credentials provider sends the video stream name in the request\. 

1. Next, create a Role Alias for your IAM Role\. Role alias is an alternate data model that points to the IAM role\. An IoT credendials provider request must include a role\-alias to indicate which IAM role to assume in order to obtain the temporary credentials from the STS\.

   The following sample command creates a role alias called `KvsCameraIoTRoleAlias`,

   ```
   aws --profile default  iot create-role-alias --role-alias KvsCameraIoTRoleAlias --role-arn $(jq --raw-output '.Role.Arn' iam-role.json) --credential-duration-seconds 3600 > iot-role-alias.json
   ```

1. Now you can create the policy that will enable IoT to assume role with the certificate \(once it is attached\) using the role alias\. 

   The following sample command creates a policy for IoT called `KvsCameraIoTPolicy`\.

   ```
   aws --profile default iot create-policy --policy-name KvsCameraIoTPolicy --policy-document 'file://iot-policy-document.json' 
   ```

   You can use the following command to create the iot\-policy\-document\.json document JSON:

   ```
   cat > iot-policy-document.json <<EOF
   {
      "Version":"2012-10-17",
      "Statement":[
         {
   	 "Effect":"Allow",
   	 "Action":[
   	    "iot:Connect"
   	 ],
   	 "Resource":"$(jq --raw-output '.roleAliasArn' iot-role-alias.json)"
    },
         {
   	 "Effect":"Allow",
   	 "Action":[
   	    "iot:AssumeRoleWithCertificate"
   	 ],
   	 "Resource":"$(jq --raw-output '.roleAliasArn' iot-role-alias.json)"
    }
      ]
   }
   EOF
   ```

### Step 3: Create and Configure the X\.509 Certificate<a name="how-iot-create-cert-keys"></a>

Communication between a device \(your video stream\) and AWS IoT is protected through the use of X\.509 certificates\. 

1. Create the certificate to which you must attach the policy for IoT that you created above\.

   ```
   aws --profile default  iot create-keys-and-certificate --set-as-active --certificate-pem-outfile certificate.pem --public-key-outfile public.pem.key --private-key-outfile private.pem.key > certificate
   ```

1. Attach the policy for IoT \(`KvsCameraIoTPolicy` created above\) to this certificate\.

   ```
   aws --profile default  iot attach-policy --policy-name KvsCameraIoTPolicy --target $(jq --raw-output '.certificateArn' certificate)
   ```

1. Attach your IoT thing \(`kvs_example_camera_stream`\) to the certificate you just created: 

   ```
   aws --profile default  iot attach-thing-principal --thing-name kvs_example_camera_stream --principal $(jq --raw-output '.certificateArn' certificate)
   ```

1. In order to authorize requests through the IoT credentials provider, you need the IoT credentials endpoint which is unique to your AWS account ID\. You can use the following command to get the IoT credentials endpoint\.

   ```
   aws --profile default  iot describe-endpoint --endpoint-type iot:CredentialProvider --output text > iot-credential-provider.txt
   ```

1. In addition to the X\.509 cerficiate created above, you must also have a CA certificate to establish trust with the back\-end service through TLS\. You can get the CA certificate using the following command:

   ```
   curl --silent 'https://www.amazontrust.com/repository/SFSRootCAG2.pem' --output cacert.pem
   ```

### Step 4: Test the IoT Credentials with Your Kinesis Video Stream<a name="how-iot-test-it"></a>

Now you can test the IoT credentials that you've set up so far\. 

1. First, create a Kinesis video stream that you want to test this configuration with\.
**Important**  
Create a video stream with a name that is identical to the IoT thing name that you created in the previous step \(`kvs_example_camera_stream`\)\.

   ```
   aws kinesisvideo create-stream  --data-retention-in-hours 24 --stream-name kvs_example_camera_stream
   ```

1. Next, call the IoT credentials provider to get the temporary credentials:

   ```
   curl --silent -H "x-amzn-iot-thingname:kvs_example_camera_stream" --cert certificate.pem --key private.pem.key https://IOT_GET_CREDENTIAL_ENDPOINT/role-aliases/KvsCameraIoTRoleAlias/credentials --cacert ./cacert.pem > token.json
   ```
**Note**  
You can use the following command to get the IOT\_GET\_CRENDETIAL\_ENDPOINT:  

   ```
   IOT_GET_CREDENTIAL_ENDPOINT=`cat iot-credential-provider.txt`
   ```

   The output JSON contains the accessKey, secretKey, and the sessionToken which you can use it for accessing the Kinesis Video Streams\.

1. Now, for your test, you can use these credentials to invoke the Kinesis Video Streams DescribeStream API for the sample `kvs_example_camera_stream` video stream\.

   ```
   AWS_ACCESS_KEY_ID=$(jq --raw-output '.credentials.accessKeyId' token.json) AWS_SECRET_ACCESS_KEY=$(jq --raw-output '.credentials.secretAccessKey' token.json) AWS_SESSION_TOKEN=$(jq --raw-output '.credentials.sessionToken' token.json) aws kinesisvideo describe-stream --stream-name kvs_example_camera_stream
   ```

### Step 5: Deploying IoT Certificates and Credentials on Your Camera's File System and Streaming Data to Your Video Stream<a name="how-iot-test-it"></a>

**Note**  
The steps in this section describe sending media to a Kinesis video stream from a camera that is using the [C\+\+ Producer Library](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producer-sdk-cpp.html)\.

1. Copy the X\.509 certificate, the private key, and the CA certificate generated in the steps above to your camera's file system\. You need to specify the paths for where these files are stored, the role alias name, and the IOT credentials endpoint for running the gst\-launch\-1\.0 command or your sample application\. 

1. The following sample command uses IoT certificate authorization to send video to Kinesis Video Streams: 

   ```
   $ gst-launch-1.0 rtspsrc location=rtsp://YourCameraRtspUrl short-header=TRUE ! rtph264depay ! video/x-h264, format=avc,alignment=au !
    h264parse ! kvssink stream-name="kvs_example_camera_stream" iot-certificate="iot-certificate,endpoint=iot-credential-endpoint-host-name,cert-path=/path/to/certificate.pem,key-path=/path/to/private.pem.key,ca-path=/path/to/cacert.pem,role-aliases=KvsCameraIoTRoleAlias"
   ```

## IoT CertificateId as Stream Name<a name="how-iot-iotcertstreamname"></a>

If you want to represent your device \(for example, your camera\) through an IoT thing but authorize a different stream name, then you can use the IoT `certifiacateId` attribute as your stream name and provide Kinesis Video Streams permissions on the stream using IoT\. The steps for accomplishing this are similar to the ones outlined above, with a few changes\.
+ Modify the permissions policy to your IAM role \(iam\-permission\-document\.json\) as follows:

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": [
                  "kinesisvideo:DescribeStream",
                  "kinesisvideo:PutMedia",
                  "kinesisvideo:TagStream",
                  "kinesisvideo:GetDataEndpoint"
              ],
              "Resource": "arn:aws:kinesisvideo:*:*:stream/${credentials-iot:AwsCertificateId}/*" 
          }
      ]
  }
  ```
**Note**  
The resource ARN uses certificate ID as the placeholder for the stream name\. The IAM permission will work when you use the certificate id as the stream name\. Get the certificate ID from the certificate so that we can use that as stream name in the following describe stream api call\.  

  ```
  export CERTIFICATE_ID=`cat certificate | jq --raw-output '.certificateId'jq --raw-output '.certificateId'`
  ```
+ Verify this change using the Kinesis Video Streams describe\-stream CLI command:

  ```
  AWS_ACCESS_KEY_ID=$(jq --raw-output '.credentials.accessKeyId' token.json) AWS_SECRET_ACCESS_KEY=$(jq --raw-output '.credentials.secretAccessKey' token.json) AWS_SESSION_TOKEN=$(jq --raw-output '.credentials.sessionToken' token.json) aws kinesisvideo describe-stream --stream-name ${CERTIFICATE_ID}
  ```
+ Pass the certificatId to the IoT credentials provider in the [sample application](https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp/blob/master/samples/kinesis_video_gstreamer_sample_app.cpp) in the Kinesis Video Streams C\+\+ SDK: 

  ```
  credential_provider = make_unique<IotCertCredentialProvider>(iot_get_credential_endpoint,
          cert_path,
          private_key_path,
          role_alias,
          ca_cert_path,
          certificateId);
  ```
**Note**  
Note that you are passing the thingname to the IoT credentials provider\. You can use getenv to pass the thingname to the demo application similar to passing the other IoT attributes\. Use the certificate ID as the stream name in the command line parameters when you are running the sample application\.