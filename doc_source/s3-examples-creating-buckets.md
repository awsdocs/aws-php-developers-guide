# Creating and using Amazon S3 buckets with the AWS SDK for PHP Version 3<a name="s3-examples-creating-buckets"></a>

The following examples show how to:
+ Return a list of buckets owned by the authenticated sender of the request using [ListBuckets](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-s3-2006-03-01.html#listbuckets)\.
+ Create a new bucket using [CreateBucket](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-s3-2006-03-01.html#createbucket)\.
+ Add an object to a bucket using [PutObject](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-s3-2006-03-01.html#putobject)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic usage](getting-started_basic-usage.md)\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\S3\S3Client;  
use Aws\Exception\AwsException;
```

## List buckets<a name="list-buckets"></a>

Create a PHP file with the following code\. First create an AWS\.S3 client service that specifies the AWS Region and version\. Then call the `listBuckets` method, which returns all Amazon S3 buckets owned by the sender of the request as an array of Bucket structures\.

 **Sample Code** 

```
$s3Client = new S3Client([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2006-03-01'
]);

//Listing all S3 Bucket
$buckets = $s3Client->listBuckets();
foreach ($buckets['Buckets'] as $bucket) {
    echo $bucket['Name'] . "\n";
}
```

## Create a bucket<a name="create-a-bucket"></a>

Create a PHP file with the following code\. First create an AWS\.S3 client service that specifies the AWS Region and version\. Then call the `createBucket` method with an array as the parameter\. The only required field is the key ‘Bucket’, with a string value for the bucket name to create\. However, you can specify the AWS Region with the ‘CreateBucketConfiguration’ field\. If successful, this method returns the ‘Location’ of the bucket\.

 **Sample Code** 

```
function createBucket($s3Client, $bucketName)
{
    try {
        $result = $s3Client->createBucket([
            'Bucket' => $bucketName,
        ]);
        return 'The bucket\'s location is: ' .
            $result['Location'] . '. ' .
            'The bucket\'s effective URI is: ' . 
            $result['@metadata']['effectiveUri'];
    } catch (AwsException $e) {
        return 'Error: ' . $e->getAwsErrorMessage();
    }
}

function createTheBucket()
{
    $s3Client = new S3Client([
        'profile' => 'default',
        'region' => 'us-east-1',
        'version' => '2006-03-01'
    ]);

    echo createBucket($s3Client, 'my-bucket');
}

// Uncomment the following line to run this code in an AWS account.
// createTheBucket();
```

## Put an object in a bucket<a name="put-an-object-in-a-bucket"></a>

To add files to your new bucket, create a PHP file with the following code\.

In your command line, execute this file and pass in the name of the bucket where you want to upload your file as a string, followed by the full file path to the file to upload\.

 **Sample Code** 

```
$USAGE = "\n" .
    "To run this example, supply the name of an S3 bucket and a file to\n" .
    "upload to it.\n" .
    "\n" .
    "Ex: php PutObject.php <bucketname> <filename>\n";

if (count($argv) <= 2) {
    echo $USAGE;
    exit();
}

$bucket = $argv[1];
$file_Path = $argv[2];
$key = basename($argv[2]);

try {
    //Create a S3Client
    $s3Client = new S3Client([
        'profile' => 'default',
        'region' => 'us-west-2',
        'version' => '2006-03-01'
    ]);
    $result = $s3Client->putObject([
        'Bucket' => $bucket,
        'Key' => $key,
        'SourceFile' => $file_Path,
    ]);
} catch (S3Exception $e) {
    echo $e->getMessage() . "\n";
}
```