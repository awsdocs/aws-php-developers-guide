# Using an Amazon S3 bucket as a static web host with AWS SDK for PHP Version 3<a name="s3-examples-static-web-host"></a>

You can host a static website on Amazon S3\. To learn more, see [Hosting a Static Website on Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html)\.

The following example shows how to:
+ Get the website configuration for a bucket using [GetBucketWebsite](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-s3-2006-03-01.html#getbucketwebsite)\.
+ Set the website configuration for a bucket using [PutBucketWebsite](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-s3-2006-03-01.html#putbucketwebsite)\.
+ Remove the website configuration from a bucket using [DeleteBucketWebsite](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-s3-2006-03-01.html#deletebucketwebsite)\.

All the example code for the AWS SDK for PHP Version 3 is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials\. See [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md)\.

## Get, set, and delete the website configuration for a bucket<a name="get-set-and-delete-the-website-configuration-for-a-bucket"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\S3\S3Client;  
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$s3Client = new S3Client([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2006-03-01'
]);

// Retrieving the Bucket Website Configuration
$bucket = 'my-s3-bucket';
try {
    $resp = $s3Client->getBucketWebsite([
        'Bucket' => $bucket
    ]);
    echo "Succeed in retrieving website configuration for bucket: " . $bucket . "\n";
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}

// Setting a Bucket Website Configuration
$params = [
    'Bucket' => $bucket,
    'WebsiteConfiguration' => [
        'ErrorDocument' => [
            'Key' => 'foo',
        ],
        'IndexDocument' => [
            'Suffix' => 'bar',
        ],
    ]
];

try {
    $resp = $s3Client->putBucketWebsite($params);
    echo "Succeed in setting bucket website configuration.\n";
} catch (AwsException $e) {
    // Display error message
    echo $e->getMessage();
    echo "\n";
}

// Deleting a Bucket Website Configuration
try {
    $resp = $s3Client->deleteBucketWebsite([
        'Bucket' => $bucket
    ]);
    echo "Succeed in deleting policy for bucket: " . $bucket . "\n";
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```