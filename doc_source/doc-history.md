# Document History for Amazon Kinesis Data Analytics<a name="doc-history"></a>

The following table describes the important changes to the documentation since the last release of Amazon Kinesis Data Analytics\.
+ **API version: 2015\-08\-14** 
+ **Latest documentation update:** July 18, 2018


****  

| Change | Description | Date | 
| --- | --- | --- | 
| Kinesis Data Analytics available in Frankfurt region | Kinesis Analytics is now available in the EU \(Frankfurt\) Region region\. For more information, see [AWS Regions and Endpoints: Kinesis Data Analytics](http://docs.aws.amazon.com/general/latest/gr/rande.html#ka_region)\. | July 18, 2018 | 
| Use reference data in the console | You can now work with application reference data in the console\. For more information, see [Example: Adding Reference Data to a Kinesis Data Analytics Application](app-add-reference-data.md) \. | July 13, 2018 | 
| Windowed query examples | Example applications for windows and aggregation\. For more information, see [Examples: Windows and Aggregation](examples-window.md) \. | July 9, 2018 | 
| Testing applications | Guidance on testing changes to application schema and code\. For more information, see [Testing Applications](best-practices.md#bp-testing) \. | July 3, 2018 | 
| Example applications for preprocessing data | Additional code samples for REGEX\_LOG\_PARSE, REGEX\_REPLACE, and DateTime operators\. For more information, see [Examples: Transforming Data](examples-transforming.md) \. | May 18, 2018 | 
| Increase in size of returned rows and SQL code | The limit for the size for a returned row is increased to 512 KB, and the limit for the size of the SQL code in an application is increased to 100 KB\. For more information, see [Limits](limits.md)\. | May 2, 2018 | 
| AWS Lambda function examples in Java and \.NET | Code samples for creating Lambda functions for preprocessing records and for application destinations\. For more information, see [Creating Lambda Functions for Preprocessing](lambda-preprocessing-functions.md) and [Creating Lambda Functions for Application Destinations](how-it-works-output-lambda-functions.md)\. | March 22, 2018 | 
| New HOTSPOTS function | Locate and return information about relatively dense regions in your data\. For more information, see [Example: Detecting Hotspots on a Stream \(HOTSPOTS Function\)](app-hotspots-detection.md)\. | March 19, 2018 | 
| Lambda function as a destination | Send analytics results to a Lambda function as a destination\. For more information, see [Using a Lambda Function as Output](how-it-works-output-lambda.md)\. | December 20, 2017 | 
| New RANDOM\_CUT\_FOREST\_WITH\_EXPLANATION function | Get an explanation of what fields contribute to an anomaly score in a data stream\. For more information, see [Example: Detecting Data Anomalies and Getting an Explanation \(RANDOM\_CUT\_FOREST\_WITH\_EXPLANATION Function\)](app-anomaly-detection-with-explanation.md)\. | November 2, 2017 | 
| Schema discovery on static data | Run schema discovery on static data stored in an Amazon S3 bucket\. For more information, see [Using the Schema Discovery Feature on Static Data](sch-dis-ref.md)\. | October 6, 2017 | 
| Lambda preprocessing feature | Preprocess records in an input stream with AWS Lambda before analysis\. For more information, see [Preprocessing Data Using a Lambda Function](lambda-preprocessing.md)\. | October 6, 2017 | 
| Auto scaling applications | Automatically increase the data throughput of your application with auto scaling\. For more information, see [Automatically Scaling Applications to Increase Throughput](how-it-works-autoscaling.md)\. | September 13, 2017 | 
| Multiple in\-application input streams | Increase application throughput with multiple in\-application streams\. For more information, see [Parallelizing Input Streams for Increased Throughput](input-parallelism.md)\. | June 29, 2017 | 
| Guide to using the AWS Management Console for Kinesis Data Analytics | Edit an inferred schema and SQL code using the schema editor and SQL editor in the Kinesis Data Analytics console\. For more information, see [Step 4 \(Optional\) Edit the Schema and SQL Code Using the Console](console-feature-summary.md)\. | April 7, 2017 | 
| Public release | Public release of the Amazon Kinesis Data Analytics Developer Guide\. | August 11, 2016 | 
| Preview release | Preview release of the Amazon Kinesis Data Analytics Developer Guide\. | January 29, 2016 | 