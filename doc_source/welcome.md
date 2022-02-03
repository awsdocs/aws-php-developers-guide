# What Is the AWS SDK for PHP Version 3?<a name="welcome"></a>

The AWS SDK for PHP Version 3 enables PHP developers to use [Amazon Web Services](https://aws.amazon.com/) in their PHP code, and build robust applications and software using services like Amazon S3, Amazon DynamoDB, S3 Glacier, etc\. You can get started in minutes by installing the SDK through Composer — by requiring the `aws/aws-sdk-php` package — or by downloading the standalone [https://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.zip](https://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.zip) or [https://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.phar](https://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.phar) file\.

Not all services are immediately available in the SDK\. To find out which services are currently supported by the AWS SDK for PHP, see [Service Name and API Version](https://docs.aws.amazon.com/aws-sdk-php/v3/api/index.html)\. For information about the AWS SDK for PHP Version 3 on GitHub, see [Additional Resources](resources.md)\.

**Note**  
If you’re migrating your code from using Version 2 of the SDK to Version 3, be sure to read [Upgrading from Version 2 of the AWS SDK for PHP](getting-started_migration.md)\.

## Getting Started<a name="getting-started"></a>
+  [Requirements and Recommendations for the AWS SDK for PHP Version 3](getting-started_requirements.md) 
+  [Installing the AWS SDK for PHP Version 3](getting-started_installation.md) 
+  [Basic Usage Patterns of the AWS SDK for PHP Version 3](getting-started_basic-usage.md) 
+  [Upgrading from Version 2 of the AWS SDK for PHP](getting-started_migration.md) 

## SDK Guides<a name="sdk-guides"></a>
+  [Configuration for the AWS SDK for PHP](guide_configuration.md) 
+  [Credentials for the AWS SDK for PHP](guide_credentials.md) 
+  [Command Objects in the AWS SDK for PHP](guide_commands.md) 
+  [Promises in the AWS SDK for PHP](guide_promises.md) 
+  [Handlers and Middleware in the AWS SDK for PHP](guide_handlers-and-middleware.md) 
+  [Streams in the AWS SDK for PHP](guide_streams.md) 
+  [Paginators in the AWS SDK for PHP](guide_paginators.md) 
+  [Waiters in the AWS SDK for PHP](guide_waiters.md) 
+  [JMESPath Expressions in the AWS SDK for PHP](guide_jmespath.md) 

## Service\-Specific Features<a name="service-specific-features"></a>
+  [Using AWS Cloud9 with the AWS SDK for PHP](cloud9.md) 
+  [Using the DynamoDB Session Handler with AWS SDK for PHP](service_dynamodb-session-handler.md) 
+  [Amazon S3 Multi\-Region Client](s3-multiregion-client.md) 
+  [Amazon S3 Stream Wrapper](s3-stream-wrapper.md) 
+  [Amazon S3 Transfer Manager](s3-transfer.md) 
+  [Amazon S3 Client Side Encryption](s3-encryption-client.md) 

## Examples<a name="examples"></a>
+  [Amazon CloudFront](cf-examples.md) 
+  [Signing Custom Amazon CloudSearch Domain Requests](service_cloudsearch-custom-requests.md) 
+  [Amazon CloudWatch Examples](cw-examples.md) 
+  [Amazon EC2](ec2-examples.md) 
+  [Signing an Amazon Elasticsearch Service Search Request](service_es-data-plane.md) 
+  [AWS Identity and Access Management Examples](iam-examples.md) 
+  [AWS Key Management Service](kms-examples.md) 
+  [Amazon Kinesis Examples](kinesis-examples.md) 
+  [AWS Elemental MediaConvert Examples](emc-examples.md) 
+  [Amazon S3 Examples](s3-examples.md) 
+  [Amazon Simple Email Services Examples](ses-examples.md) 
+  [Amazon SNS Examples](sns-examples.md) 
+  [Amazon SQS Examples](sqs-examples.md) 

For additional examples, see the [AWS Code Sample Catalog](https://docs.aws.amazon.com/code-samples/latest/catalog)\.

## Reference<a name="reference"></a>
+  [FAQ](faq.md) 
+  [Glossary](glossary.md) 
+  [Contributing to the SDK](https://github.com/aws/aws-sdk-php/blob/master/CONTRIBUTING.md) 
+  [Guzzle Documentation](http://guzzlephp.org) 

Submit issues on GitHub:
+  [Submit documentation issues](https://github.com/awsdocs/aws-php-developers-guide/issues) 
+  [Report a bug or request a feature](https://github.com/aws/aws-sdk-php/issues/new/choose) 

## API Documentation<a name="supported-services"></a>

Find API documentation for the SDK at [https://docs\.aws\.amazon\.com/sdk\-for\-php/latest/reference/](https://docs.aws.amazon.com/aws-sdk-php/v3/api/)\.

## Maintenance and support for SDK major versions<a name="maintenance-and-support-for-sdk-major-versions"></a>

For information about maintenance and support for SDK major versions and their underlying dependencies, see the following in the [AWS SDKs and Tools Shared Configuration and Credentials Reference Guide](https://docs.aws.amazon.com/credref/latest/refdocs/overview.html):
+  [AWS SDKs and Tools Maintenance Policy](https://docs.aws.amazon.com/credref/latest/refdocs/maint-policy.html) 
+  [AWS SDKs and Tools Version Support Matrix](https://docs.aws.amazon.com/credref/latest/refdocs/version-support-matrix.html) 