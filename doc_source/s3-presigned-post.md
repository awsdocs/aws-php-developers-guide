# Amazon S3 Pre\-Signed POSTs with AWS SDK for PHP Version 3<a name="s3-presigned-post"></a>

Much like pre\-signed URLs, pre\-signed POSTs enable you to give write access to a user without giving them AWS credentials\. Pre\-signed POST forms can be created with the help of an instance of [AwsS3PostObjectV4](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.S3.PostObjectV4.html)\.

The following examples show how to:
+ Get data for an S3 Object POST upload form using [PostObjectV4](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.S3.PostObjectV4.html)\.

All the example code for the AWS SDK for PHP Version 3 is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage Patterns of the AWS SDK for PHP Version 3](getting-started_basic-usage.md)\.

## Create PostObjectV4<a name="create-postobjectv4"></a>

To create an instance of `PostObjectV4`, you must provide the following:
+ instance of `Aws\S3\S3Client` 
+ bucket
+ associative array of form input fields
+ array of policy conditions \(see [Policy Construction](https://docs.aws.amazon.com/AmazonS3/latest/dev/HTTPPOSTForms.html) in the Amazon Simple Storage Service User Guide\)
+ expiration time string for the policy \(optional, one hour by default\)\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\S3\S3Client;  
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$client = new S3Client([
    'profile' => 'default',
    'version' => 'latest',
    'region' => 'us-west-2',
]);
$bucket = 'mybucket';

// Set some defaults for form input fields
$formInputs = ['acl' => 'public-read'];

// Construct an array of conditions for policy
$options = [
    ['acl' => 'public-read'],
    ['bucket' => $bucket],
    ['starts-with', '$key', 'user/eric/'],
];

// Optional: configure expiration time string
$expires = '+2 hours';

$postObject = new \Aws\S3\PostObjectV4(
    $client,
    $bucket,
    $formInputs,
    $options,
    $expires
);

// Get attributes to set on an HTML form, e.g., action, method, enctype
$formAttributes = $postObject->getFormAttributes();

// Get form input fields. This will include anything set as a form input in
// the constructor, the provided JSON policy, your AWS access key ID, and an
// auth signature.
$formInputs = $postObject->getFormInputs();
```