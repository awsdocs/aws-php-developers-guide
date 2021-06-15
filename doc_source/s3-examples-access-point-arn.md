# Using S3 Access Point ARNs the AWS SDK for PHP Version 3<a name="s3-examples-access-point-arn"></a>

S3 introduced access points, a new way to interact with S3 buckets\. Access Points can have unique policies and configuration applied to them instead of directly to the bucket\. The AWS SDK for PHP allows you to use access point ARNs in the bucket field for API operations instead of specifying bucket name explicitly\. More details on how S3 access points and ARNs work can be found [here](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-access-points.html)\. The following examples show how to:
+ Use [GetObject](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-s3-2006-03-01.html#getobject) with an access point ARN to fetch an object from a bucket\.
+ Use [PutObject](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-s3-2006-03-01.html#putobject) with an access point ARN to add an object to a bucket\.
+ Configure the S3 client to use the ARN region instead of the client region\.

All the example code for the AWS SDK for PHP Version 3 is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage Patterns of the AWS SDK for PHP Version 3](getting-started_basic-usage.md)\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\S3\S3Client;  
use Aws\Exception\AwsException;
```

## Get Object<a name="get-object"></a>

First create an AWS\.S3 client service that specifies the AWS region and version\. Then call the `getObject` method with your key and an S3 access point ARN in the `Bucket` field, which will fetch the object from the bucket associated with that access point\.

 **Sample Code** 

```
$s3 = new S3Client([
    'version'     => 'latest',
    'region'      => 'us-west-2',
]);
$result = $s3->getObject([
    'Bucket' => 'arn:aws:s3:us-west-2:123456789012:accesspoint:endpoint-name',
    'Key' => 'MyKey'
]);
```

## Put an Object in a Bucket<a name="put-an-object-in-a-bucket"></a>

First create an AWS\.S3 client service that specifies the AWS Region and version\. Then call the `putObject` method with the desired key, the body or source file, and an S3 access point ARN in the `Bucket` field, which will put the object in the bucket associated with that access point\.

 **Sample Code** 

```
$s3 = new S3Client([
    'version'     => 'latest',
    'region'      => 'us-west-2',
]);
$result = $s3->putObject([
    'Bucket' => 'arn:aws:s3:us-west-2:123456789012:accesspoint:endpoint-name',
    'Key' => 'MyKey',
    'Body' => 'MyBody'
]);
```

## Configure the S3 client to use the ARN region instead of the client region<a name="configure-the-s3-client-to-use-the-arn-region-instead-of-the-client-region"></a>

When using an S3 access point ARN in an S3 client operation, by default the client will make sure that the ARN region matches the client region, throwing an exception if it does not\. This behavior can be changed to accept the ARN region over the client region by setting the `use_arn_region` configuration option to `true`\. By default, the option is set to `false`\.

 **Sample Code** 

```
$s3 = new S3Client([
    'version'        => 'latest',
    'region'         => 'us-west-2',
    'use_arn_region' => true
]);
```

The client will also check an environment variable and a config file option, in the following order of priority:

1. The client option `use_arn_region`, as in the above example\.

1. The environment variable `AWS_S3_USE_ARN_REGION` 

```
export AWS_S3_USE_ARN_REGION=true
```

1. The config variable `s3_use_arn_region` in the AWS shared configuration file \(by default in `~/.aws/config`\)\.

```
[default]
s3_use_arn_region = true
```