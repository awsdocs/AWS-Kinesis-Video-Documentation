# What Is Amazon Kinesis Video Streams?<a name="what-is-kinesis-video"></a>

Amazon Kinesis Video Streams is a fully managed AWS service that you can use to stream live video from devices to the AWS Cloud, or build applications for real\-time video processing or batch\-oriented video analytics\.

Kinesis Video Streams isn't just storage for video data\. You can use it to watch your video streams in real time as they are received in the cloud\. You can either monitor your live streams in the AWS Management Console, or develop your own monitoring application that uses the Kinesis Video Streams API library to display live video\.

You can use Kinesis Video Streams to capture massive amounts of live video data from millions of sources, including smartphones, security cameras, webcams, cameras embedded in cars, drones, and other sources\. You can also send non\-video time\-serialized data such as audio data, thermal imagery, depth data, RADAR data, and more\. As live video streams from these sources into a Kinesis video stream, you can build applications that can access the data, frame\-by\-frame, in real time for low\-latency processing\. Kinesis Video Streams is source\-agnostic; you can stream video from a computer's webcam using the [GStreamer](examples-gstreamer-plugin.md) library, or from a camera on your network using RTSP\.

You can also configure your Kinesis video stream to durably store media data for the specified retention period\. Kinesis Video Streams automatically stores this data and encrypts it at rest\. Additionally, Kinesis Video Streams time\-indexes stored data based on both the producer time stamps and ingestion time stamps\. You can build applications that periodically batch\-process the video data, or you can create applications that require ad hoc access to historical data for different use cases\.

Your custom applications, real\-time or batch\-oriented, can run on Amazon EC2 instances\. These applications might process data using open source deep\-learning algorithms, or use third\-party applications that integrate with Kinesis Video Streams\.

Benefits of using Kinesis Video Streams include the following:
+ **Connect and stream from millions of devices ** – Kinesis Video Streams enables you to connect and stream video, audio, and other data from millions of devices ranging from consumer smartphones, drones, dash cams, and more\. You can use the Kinesis Video Streams producer libraries to configure your devices and reliably stream in real time, or as after\-the\-fact media uploads\. 
+ **Durably store, encrypt, and index data **– You can configure your Kinesis video stream to durably store media data for custom retention periods\. Kinesis Video Streams also generates an index over the stored data based on producer\-generated or service\-side time stamps\. Your applications can easily retrieve specified data in a stream using the time\-index\. 
+ **Focus on managing applications instead of infrastructure** – Kinesis Video Streams is serverless, so there is no infrastructure to set up or manage\. You don't need to worry about the deployment, configuration, or elastic scaling of the underlying infrastructure as your data streams and number of consuming applications grow and shrink\. Kinesis Video Streams automatically does all the administration and maintenance required to manage streams, so you can focus on the applications, not the infrastructure\. 
+ **Build real\-time and batch applications on data streams** – You can use Kinesis Video Streams to build custom real\-time applications that operate on live data streams, and create batch or ad hoc applications that operate on durably persisted data without strict latency requirements\. You can build, deploy, and manage custom applications: open source \(Apache MXNet, OpenCV\), homegrown, or third\-party solutions via the AWS Marketplace to process and analyze your streams\. Kinesis Video Streams `Get` APIs enable you to build multiple concurrent applications processing data in a real\-time or batch\-oriented basis\. 
+ **Stream data more securely ** – Kinesis Video Streams encrypts all data as it flows through the service and when it persists the data\. Kinesis Video Streams enforces Transport Layer Security \(TLS\)\-based encryption on data streaming from devices, and encrypts all data at rest using AWS Key Management Service \(AWS KMS\)\. Additionally, you can manage access to your data using AWS Identity and Access Management \(IAM\)\.
+ **Pay as you go ** – For more information, see [AWS Pricing](https://aws.amazon.com/pricing/)\.

## Are You a First\-Time User of Kinesis Video Streams?<a name="first-time-user"></a>

If you're a first\-time user of Kinesis Video Streams, we recommend that you read the following sections in order:

1. **[Amazon Kinesis Video Streams: How It Works](how-it-works.md)** – To learn about Kinesis Video Streams concepts\.

1. **[Getting Started with Kinesis Video Streams](getting-started.md)** – To set up your account and test Kinesis Video Streams\.

1. **[Kinesis Video Streams Producer Libraries](producer-sdk.md)** – To learn about creating a Kinesis Video Streams producer application\.

1. **[Kinesis Video Stream Parser Library](parser-library.md)** – To learn about processing incoming data frames in a Kinesis Video Streams consumer application\.

1. **[Amazon Kinesis Video Streams Examples](examples.md)** – To see more examples of what you can do with Kinesis Video Streams\.