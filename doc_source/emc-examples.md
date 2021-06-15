# AWS Elemental MediaConvert Examples Using the AWS SDK for PHP Version 3<a name="emc-examples"></a>

AWS Elemental MediaConvert is a file\-based video transcoding service with broadcast\-grade features\. You can use it to create assets for broadcast and for video\-on\-demand \(VOD\) delivery across the internet\. For more information, see the [AWS Elemental MediaConvert User Guide](https://docs.aws.amazon.com/mediaconvert/latest/ug/)\.

![\[Diagram that provides an overview of how AWS SDK for PHP connects to AWS Elemental MediaConvert\]](http://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/images/code-samples-emc.png)

The PHP API for AWS Elemental MediaConvert is exposed through the *`AWS.MediaConvert`* client class\. For more information, see [https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-mediaconvert-2017-08-29.html](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-mediaconvert-2017-08-29.html) in the API reference\.

All the example code for the AWS SDK for PHP Version 3 is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage Patterns of the AWS SDK for PHP Version 3](getting-started_basic-usage.md)\.

**Topics**
+ [Credentials](#credentials)
+ [Getting Your Account\-Specific Endpoint](emc-examples-getendpoint.md)
+ [Creating and Managing Jobs](emc-examples-jobs.md)