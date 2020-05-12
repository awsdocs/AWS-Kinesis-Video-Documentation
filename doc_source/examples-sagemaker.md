# Example: Identifying Objects in Video Streams Using Amazon SageMaker<a name="examples-sagemaker"></a>

This example demonstrates how to create a solution that uses [Amazon SageMaker](https://aws.amazon.com/sagemaker) to identify when certain objects appear in an Amazon Kinesis video stream\. Amazon SageMaker is the managed platform for developers and data scientists to build, train, and deploy machine learning models quickly and easily\.

The example consists of a [Docker](http://www.docker.com) container that includes the application functionality, and an [AWS CloudFormation](https://aws.amazon.com/cloudformation) template that automates the deployment of the application's AWS resources\.

The AWS CloudFormation template creates the following resources:
+ An [Amazon Elastic Container Service \(Amazon ECS\)](https://aws.amazon.com/ecs) cluster that uses the [AWS Fargate](https://aws.amazon.com/fargate) compute engine that runs the library software\.
+ An [Amazon DynamoDB](https://aws.amazon.com/dynamodb) table that maintains checkpoints and related state across workers that run on Fargate tasks\.
+ A [Kinesis data stream](https://aws.amazon.com/kinesis/data-streams) that captures the inference outputs generated from Amazon SageMaker\.
+ An [AWS Lambda function](https://aws.amazon.com/lambda) that parses the output from Amazon SageMaker\.
+ [AWS Identity and Access Management \(IAM\)](https://aws.amazon.com/iam) resources for providing access across services\.
+ [Amazon CloudWatch](https://aws.amazon.com/cloudwatch) resources for monitoring the application\.

The application is compatible with any Amazon SageMaker endpoint that processes data\. This example contains instructions for creating an Amazon SageMaker endpoint that uses a sample object identification algorithm template\. You can modify or replace the algorithm based on your application's use cases and requirements\.

**Topics**
+ [Prerequisites](#examples-sagemaker-prerequisites)
+ [Creating the Application](#examples-sagemaker-create)
+ [Monitoring the Application](#examples-sagemaker-monitor)
+ [Extending the Application](#examples-sagemaker-extend)
+ [Cleaning up the Application](#examples-sagemaker-cleanup)

## Prerequisites<a name="examples-sagemaker-prerequisites"></a>

**Topics**
+ [Amazon SageMaker](#examples-sagemaker-prerequisites-sm)
+ [Kinesis Video Stream](#examples-sagemaker-prerequisites-aks)
+ [Service\-Linked Role](#examples-sagemaker-prerequisites-slr)

### Amazon SageMaker<a name="examples-sagemaker-prerequisites-sm"></a>

This example requires an Amazon SageMaker notebook\. For information about creating a notebook, see [Creating a Notebook Instance](https://docs.aws.amazon.com/sagemaker/latest/dg/howitworks-create-ws.html) in the *Amazon SageMaker Developer Guide*\. Note the following when creating your notebook: 
+ Add the `object_detection_image_json_format.ipynb` example \(from the **Introduction to Amazon Algorithms** section in the **SageMaker Examples** tab of the **Jupyter** console\) to the notebook\. 
+ Create an Amazon Simple Storage Service \(Amazon S3\) bucket, and provide its name in the **Prerequisites** step when adding the example\.
+ After you create the notebook, choose **Endpoint configuration** on the Amazon SageMaker console, and make a note of the **Endpoint name**\.

### Kinesis Video Stream<a name="examples-sagemaker-prerequisites-aks"></a>

This example requires one or more Kinesis video streams that have live video data\. For information about creating a Kinesis video stream and sending data to it from a camera, see [GStreamer](examples-gstreamer-plugin.md)\. Make a note of your Kinesis video stream name\.

### Service\-Linked Role<a name="examples-sagemaker-prerequisites-slr"></a>

This example requires that your account have a service\-linked role for AWS Fargate operation\. New AWS accounts have this role enabled by default\. If you see the following error when creating the application, you must enable the service\-linked role:

```
Unable to assume the service linked role. Please verify that the ECS service linked role exists
```

To enable the service\-linked role, run the following command:

```
aws iam create-service-linked-role --aws-service-name ecs.amazonaws.com
```

## Creating the Application<a name="examples-sagemaker-create"></a>

To create the sample application, you use AWS CloudFormation and the templates that are provided\.

**To use AWS CloudFormation to create the application**

1. Sign in to the AWS Management Console and open the AWS CloudFormation console using one of the following links for your AWS Region\. The link launches the correct stack for your Region:
   +  [ Launch in Asia Pacific \(Sydney\) Region \(ap\-southeast\-2\)](https://ap-southeast-2.console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/create/review?templateURL=https:%2F%2Fs3.ap-southeast-2.amazonaws.com%2Fkvsit-ap-southeast-2%2Fcfn-template.template.yml) 
   +  [ Launch in Asia Pacific \(Tokyo\) Region \(ap\-northeast\-1\)](https://ap-northeast-1.console.aws.amazon.com/cloudformation/home?region=ap-northeast-1#/stacks/create/review?templateURL=https://s3.ap-northeast-1.amazonaws.com/kvsit-ap-northeast-1/cfn-template.template.yml) 
   +  [ Launch in Europe \(Frankfurt\) Region \(eu\-central\-1\)](https://eu-central-1.console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/create/review?templateURL=https://s3.eu-central-1.amazonaws.com/kvsit-eu-central-1/cfn-template.template.yml) 
   +  [ Launch in Europe \(Ireland\) Region \(eu\-west\-1\)](https://eu-west-1.console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/review?templateURL=https://s3.eu-west-1.amazonaws.com/kvsit-eu-west-1/cfn-template.template.yml) 
   +  [ Launch in US East \(N\. Virginia\) Region \(us\-east\-1\) ](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://s3.amazonaws.com/kvsit-us-east-1/cfn-template.template.yml)
   +  [ Launch in US West \(Oregon\) Region \(us\-west\-2\) ](https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/review?templateURL=https://s3.us-west-2.amazonaws.com/kvsit-us-west-2/cfn-template.template.yml)

1. On the **Create Stack** page, provide the following values:
   + Give the stack a unique name \(for example, *username*\-KVS\-SageMaker\)\.
   + Provide the Amazon SageMaker endpoint name \(not the endpoint ARN\) that you created in the previous section\.
   + Provide the name of your Kinesis video stream\. If you have more than one Kinesis video stream, provide the stream names in quotation marks and separated by commas\.
   + Keep the rest of the settings as they are\.

   Choose **Next**\.

1. On the **Options** page, keep the settings as they are\.

1. Select the **I acknowledge that AWS CloudFormation might create IAM resources** check box\. Choose **Next**\.

AWS CloudFormation creates the application\. 

The following table lists several parameters used by the Docker container when you create a stack using this AWS CloudFormation template\. Their values are predefined in the `SSM` resource in the template, but you can customize them as needed\. 


|  |  |  | 
| --- |--- |--- |
| Resource name  | Default value | Description | 
| inferenceInterval | 6 | The sampling ratio for video frames that are sent to the SageMaker endpoint\. Currently, we only support inferencing on I\-Frames\. The default value of 6 means that 1 out of every 6 I\-Frames is sent to the SageMaker endpoint\. | 
| sageMakerTaskQueueSize | 5000 | The size of the queue that maintains the pending requests to the SageMaker endpoint\. The size of the queue is affected by ‘inferenceInternval’ and ‘sageMakerTaskTimeoutInMilli’\. If sagemaker inference takes longer, requests are buffered in this queue\.  | 
| sageMakerTaskThreadPoolSize | 20 | Number of threads that is used to concurrently execute SageMaker requests\. | 
| sageMakerTaskTimeoutInMilli | 20000 | The maximum duration allowed for a single request \(or a retry request\) that is sent to the SageMaker endpoint\. | 
| sageMakerTaskThreadPoolName | SageMakerThreadPool\-%d | The name of the threadpool that is sending requests to the SageMaker endpoint\. | 

To customize the values of these parameters, download the AWS CloudFormation template by choosing the template URL on the **Create stack** page, and then locate these parameters in the `Params` section of the template that looks like this:

```
Params:
    Type: AWS::SSM::Parameter
    Properties: 
      Name:
        Ref: AppName
      Description: "Configuration for SageMaker app"
      Type: String
      Value: 
        Fn::Sub: |
          {"streamNames":[${StreamNames}], "tagFilters":[${TagFilters}],"sageMakerEndpoint":"${SageMakerEndpoint}",
           "endPointAcceptContentType": "${EndPointAcceptContentType}",
           "kdsStreamName":"${Kds}","inferenceInterval":6,"sageMakerTaskQueueSize":5000,
           "sageMakerTaskThreadPoolSize":20,"sageMakerTaskTimeoutInMilli":20000,
           "sageMakerTaskThreadPoolName":"SageMakerThreadPool-%d"}
```

## Monitoring the Application<a name="examples-sagemaker-monitor"></a>

The application created by the AWS CloudFormation template includes an Amazon CloudWatch dashboard and a CloudWatch log stream that you use to monitor application metrics and events\.

### Application Dashboard<a name="examples-sagemaker-monitor-dashboard"></a>

The application includes a CloudWatch dashboard for monitoring application metrics\. To view the application dashboard, open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/) and choose **Dashboards** in the left navigation bar\.

Choose the **KVS\-SageMaker\-Driver\-KvsSageMakerIntegration\-*aws\-region* ** dashboard\. The dashboard shows the following information:
+ **Frame Metrics:** Metrics for processing the video stream, sending frames to the Amazon SageMaker endpoint, and writing to the Kinesis data stream that connects the Amazon SageMaker notebook with the AWS Lambda function that processes Amazon SageMaker inference output results\.
+ **IngestToProcessLatency:** The time difference between when a video frame is ingested into the Kinesis Video Streams service and when the application receives the frame\.
+ **Current Lease Total:** The application is granted permissions to read from the Kinesis video stream using a lease\. This metric shows the number of active leases\. The application uses one lease per Kinesis video stream, and one lease for synchronization between streams\.
+ **Lease Sync Metrics:** The frequency and duration of permission lease synchronization\.
+ **LeaseCount per Worker:** The distribution of leases among the Amazon SageMaker worker threads\.
+ **Number of Workers:** The number of Amazon SageMaker workers processing streams\. Each task in an Amazon ECS cluster has one worker running\. One worker can process more than one stream\.
+ **ECS Service Utilization:** Usage metrics for the Amazon ECS cluster\.
+ **KinesisDataStream:** Usage metrics of the Kinesis data stream\.
+ **SageMaker:** Operations performed by the Amazon SageMaker notebook\.
+ **Lambda:** Number and duration of the Lambda function that processes the output from the Amazon SageMaker notebook\.

 If any of the information in these graphs indicates an operational issue \(such as a value steadily increasing rather than being stable\), see the following section about how to read the application logs to determine the issue\.

### CloudWatch Logs<a name="examples-sagemaker-monitor-log"></a>

The application includes two CloudWatch logs:

**Topics**
+ [The Application Log](#examples-sagemaker-monitor-log-app)
+ [The Lambda Function Log](#examples-sagemaker-monitor-log-lambda)

#### The Application Log<a name="examples-sagemaker-monitor-log-app"></a>

You can use the application log to monitor application events and error conditions\. This log is helpful if you need to contact product support with an issue\.

**To read the Application Log**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs](https://console.aws.amazon.com/ecs)\.

1. Choose the **KVS\-Sagemaker\-Driver** cluster\.

1. Choose the ***stack\-name*\-SageMakerDriverService** service in the **Services** tab\.

1. Choose the **Logs** tab\.

The application log shows events such as initialization, configuration, and lease activity\. 

#### The Lambda Function Log<a name="examples-sagemaker-monitor-log-lambda"></a>

You can use the Lambda function log to track successful object identifications\.

**To read the Lambda log**

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda](https://console.aws.amazon.com/lambda)\.

1. Choose the Lambda function for your application\. The Lambda function name is in the following format:

   ```
   stack-name-LambdaFunction-A1B2C3D4E5F6G
   ```

1. Choose the **Monitoring** panel\.

1. Choose **View logs in CloudWatch**\.

The CloudWatch log for the application shows successful identifications of objects in the Kinesis video stream and other application events\.

## Extending the Application<a name="examples-sagemaker-extend"></a>

You can add custom functionality to your application by modifying the values that you provide in the AWS CloudFormation template window as follows:
+ **EndPointAcceptContentType:** You can change this value if your Amazon SageMaker endpoint does not accept frames in JPG format\. The following formats are supported:
  + `image/jpeg`
  + `image/png`
  + `image/bmp`
  + `image/gif`
  + `application/x-image`
+ **LambdaFunctionBucket, LambdaFunctionKey: ** The provided settings use an AWS Lambda function that processes the Amazon SageMaker output and writes it to CloudWatch Logs\. If you want to send the Amazon SageMaker output elsewhere, you can provide your own Lambda function\.
+ **Tag Filters:** If you have streams that are tagged using the [TagStream](API_TagStream.md) action, you can specify the tags of streams that you want to process\. For example, if you have two streams that have the `Location` key with the values `Front` and `Parking`, you would filter to only use those streams using the following entry:

  ```
  {"key":"Location","values":["Front","Parking"]}
  ```

## Cleaning up the Application<a name="examples-sagemaker-cleanup"></a>

After you've finished with the application that you created for this tutorial, we recommend that you delete any resources that you don't want to keep, to avoid incurring any ongoing charges\. 

1. **Amazon SageMaker endpoint:** If you created the Amazon SageMaker endpoint for this tutorial rather than using an existing endpoint, delete the endpoint\. In the Amazon SageMaker control panel, choose **Endpoint configurations**\. Choose the endpoint you created, and choose **Actions**, **Delete**\. Confirm the deletion\.

1. **Amazon SageMaker notebook:** On the Amazon SageMaker console, choose **Notebook instances**\. Choose the notebook that you created, and choose **Actions**, **Stop**\. When the notebook shows that its **Status** is **Stopped**, choose **Actions**, **Delete**\. Confirm the deletion\.
**Note**  
For more information on cleaning up Amazon SageMaker resources, see [Clean up](https://docs.aws.amazon.com/sagemaker/latest/dg/ex1-cleanup.html) in the [Amazon SageMaker developer guide](https://docs.aws.amazon.com/sagemaker/latest/dg/)\.

1. **Amazon SageMaker execution policy:** On the IAM console, in the navigation pane, choose **Policies**\. Choose the policy that you created for this tutorial\. The name of the policy is similar to the following: `AmazonSageMaker-ExecutionPolicy-timestamp`

   Choose **Policy actions**, **Delete\.** Confirm the deletion\.

1. **Amazon SageMaker execution role:** On the IAM console, in the navigation pane, choose **Roles**\. Choose the role that you created for this tutorial\. The name of the role is similar to the following: `AmazonSageMaker-ExecutionRole-timestamp`

   Choose **Delete role**\. Confirm the deletion\.

1. **AWS CloudFormation stack:** On the AWS CloudFormation console, choose the stack that you created for this tutorial\. Choose **Actions**, **Delete Stack**\. Confirm the deletion\.

1. **Amazon S3 bucket:** On the Amazon S3 console, choose the bucket that you created to store the Amazon SageMaker assets\. Choose **Delete**\. Enter the name of the bucket and choose **Confirm** to confirm deletion\.

1. **Kinesis video stream:** On the Kinesis Video Streams console, choose the video stream that you created for the application\. Choose **Delete**\. Confirm the deletion\.