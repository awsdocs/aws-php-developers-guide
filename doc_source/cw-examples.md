# Amazon CloudWatch Examples Using the AWS SDK for PHP Version 3<a name="cw-examples"></a>

Amazon CloudWatch \(CloudWatch\) is a web service that monitors your Amazon Web Services resources and the applications you run on AWS in real time\. You can use CloudWatch to collect and track metrics, which are variables you can measure for your resources and applications\. CloudWatch alarms send notifications or automatically make changes to the resources you are monitoring based on rules that you define\.

![\[Diagram that provides an overview of how AWS SDK for PHP connects to Amazon CloudWatch\]](http://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/images/code-samples-cloudwatch.png)

All the example code for the AWS SDK for PHP Version 3 is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage Patterns of the AWS SDK for PHP Version 3](getting-started_basic-usage.md)\.

**Topics**
+ [Credentials](#credentials)
+ [Working with Amazon CloudWatch Alarms](cw-examples-work-with-alarms.md)
+ [Getting Metrics from Amazon CloudWatch](cw-examples-getting-metrics.md)
+ [Publishing Custom Metrics in Amazon CloudWatch](cw-examples-publishing-custom-metrics.md)
+ [Sending Events to Amazon CloudWatch Events](cw-examples-sending-events.md)
+ [Using Alarm Actions with Amazon CloudWatch Alarms](cw-examples-using-alarm-actions.md)