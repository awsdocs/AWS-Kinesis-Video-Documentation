# Monitoring Kinesis Video Streams<a name="monitoring"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of Kinesis Video Streams and your AWS solutions\. You should collect monitoring data from all of the parts of your AWS solution so that you can more easily debug a multi\-point failure if one occurs\. Before you start monitoring Kinesis Video Streams, however, you should create a monitoring plan that includes answers to the following questions:
+ What are your monitoring goals?
+ What resources will you monitor?
+ How often will you monitor these resources?
+ What monitoring tools will you use?
+ Who will perform the monitoring tasks?
+ Who should be notified when something goes wrong?

After you have defined your monitoring goals and have created your monitoring plan, the next step is to establish a baseline for normal Kinesis Video Streams performance in your environment\. You should measure Kinesis Video Streams performance at various times and under different load conditions\. As you monitor Kinesis Video Streams, you should store a history of monitoring data that you've collected\. You can compare current Kinesis Video Streams performance to this historical data to help you to identify normal performance patterns and performance anomalies, and devise methods to address issues that may arise\. 

**Topics**
+ [Monitoring Kinesis Video Streams Metrics with CloudWatch](monitoring-cloudwatch.md)
+ [Logging Kinesis Video Streams API Calls with AWS CloudTrail](monitoring-cloudtrail.md)