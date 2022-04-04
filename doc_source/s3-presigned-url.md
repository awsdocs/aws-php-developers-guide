# Amazon S3 Pre\-Signed URL with AWS SDK for PHP Version 3<a name="s3-presigned-url"></a>

You can authenticate certain types of requests by passing the required information as query\-string parameters instead of using the Authorization HTTP header\. This is useful for enabling direct third\-party browser access to your private Amazon S3 data, without proxying the request\. The idea is to construct a “pre\-signed” request and encode it as a URL that an end\-user’s browser can retrieve\. Additionally, you can limit a pre\-signed request by specifying an expiration time\.

The following examples show how to:
+ Create a pre\-signed URL to get an S3 object using [createPresignedRequest](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.S3.S3Client.html#_createPresignedRequest)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting Credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage](getting-started_basic-usage.md)\.

## Creating a Pre\-Signed Request<a name="creating-a-pre-signed-request"></a>

You can get the pre\-signed URL to an Amazon S3 object by using the `Aws\S3\S3Client::createPresignedRequest()` method\. This method accepts an `Aws\CommandInterface` object and expired timestamp and returns a pre\-signed `Psr\Http\Message\RequestInterface` object\. You can retrieve the pre\-signed URL of the object using the `getUri()` method of the request\.

The most common scenario is creating a pre\-signed URL to GET an object\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\S3\S3Client;  
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$s3Client = new Aws\S3\S3Client([
    'profile' => 'default',
    'region' => 'us-east-2',
    'version' => '2006-03-01',
]);

$cmd = $s3Client->getCommand('GetObject', [
    'Bucket' => 'my-bucket',
    'Key' => 'testKey'
]);

$request = $s3Client->createPresignedRequest($cmd, '+20 minutes');
```

## Creating a Pre\-Signed URL<a name="creating-a-pre-signed-url"></a>

You can create pre\-signed URLs for any Amazon S3 operation using the `getCommand` method for creating a command object, and then calling the `createPresignedRequest()` method with the command\. When ultimately sending the request, be sure to use the same method and the same headers as the returned request\.

 **Sample Code** 

```
//Creating a presigned URL
$cmd = $s3Client->getCommand('GetObject', [
    'Bucket' => 'my-bucket',
    'Key' => 'testKey'
]);

$request = $s3Client->createPresignedRequest($cmd, '+20 minutes');

// Get the actual presigned-url
$presignedUrl = (string)$request->getUri();
```

## Getting the URL to an Object<a name="getting-the-url-to-an-object"></a>

If you only need the public URL to an object stored in an Amazon S3 bucket, you can use the `Aws\S3\S3Client::getObjectUrl()` method\. This method returns an unsigned URL to the given bucket and key\.

 **Sample Code** 

```
//Getting the URL to an object
$url = $s3Client->getObjectUrl('my-bucket', 'my-key');
```

**Important**  
The URL returned by this method is not validated to ensure that the bucket or key exists, nor does this method ensure that the object allows unauthenticated access\.