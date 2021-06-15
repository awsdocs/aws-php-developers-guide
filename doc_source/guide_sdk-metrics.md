# SDK Metrics in the AWS SDK for PHP Version 3<a name="guide_sdk-metrics"></a>

AWS SDK Metrics for Enterprise Support \(SDK Metrics\) enables enterprise customers to collect metrics from AWS SDKs on their hosts and clients shared with AWS Enterprise Support\. SDK Metrics provides information that helps speed up detection and diagnosis of issues occurring in connections to AWS services for AWS Enterprise Support customers\.

As telemetry is collected on each host, it is relayed via UDP to localhost, where the CloudWatch agent aggregates the data and sends it to the SDK Metrics service\. Therefore, to receive metrics, you must add the CloudWatch agent to your instance\.

Learn more about [SDK Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-SDK-Metrics.html) in the Amazon CloudWatch User Guide\.

Each of the following topics describes how to set up, configure, and manage SDK Metrics for the AWS SDK for PHP\.

**Topics**
+ [Authorize SDK Metrics](guide_sdk-metrics-set-permissions.md)
+ [Set Up SDK Metrics](guide_sdk-metrics-configure.md)
+ [SDK Metric Definitions](guide_sdk-metrics-definitions.md)