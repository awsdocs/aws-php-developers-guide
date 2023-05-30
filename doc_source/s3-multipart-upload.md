# Using Amazon S3 multipart uploads with AWS SDK for PHP Version 3<a name="s3-multipart-upload"></a>

With a single `PutObject` operation, you can upload objects up to 5 GB in size\. However, by using the multipart upload methods \(for example, `CreateMultipartUpload`, `UploadPart`, `CompleteMultipartUpload`, `AbortMultipartUpload`\), you can upload objects from 5 MB to 5 TB in size\.

The following example shows how to:
+ Upload an object to Amazon S3, using [ObjectUploader](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.S3.ObjectUploader.html)\.
+ Create a multipart upload for an Amazon S3 object using [MultipartUploader](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.S3.MultipartUploader.html)\.
+ Copy objects from one Amazon S3 location to another using [ObjectCopier](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.S3.ObjectCopier.html)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic usage](getting-started_basic-usage.md)\.

## Object uploader<a name="object-uploader"></a>

If you’re not sure whether `PutObject` or `MultipartUploader` is best for the task, use `ObjectUploader`\. `ObjectUploader` uploads a large file to Amazon S3 using either `PutObject` or `MultipartUploader`, depending on what is best based on the payload size\.

```
require 'vendor/autoload.php';

use Aws\S3\S3Client;
use Aws\Exception\AwsException;
use Aws\S3\ObjectUploader;
```

 **Sample Code** 

```
$s3Client = new S3Client([
    'profile' => 'default',
    'region' => 'us-east-2',
    'version' => '2006-03-01'
]);

$bucket = 'your-bucket';
$key = 'my-file.zip';

// Using stream instead of file path
$source = fopen('/path/to/large/file.zip', 'rb');

$uploader = new ObjectUploader(
    $s3Client,
    $bucket,
    $key,
    $source
);

do {
    try {
        $result = $uploader->upload();
        if ($result["@metadata"]["statusCode"] == '200') {
            print('<p>File successfully uploaded to ' . $result["ObjectURL"] . '.</p>');
        }
        print($result);
    } catch (MultipartUploadException $e) {
        rewind($source);
        $uploader = new MultipartUploader($s3Client, $source, [
            'state' => $e->getState(),
        ]);
    }
} while (!isset($result));

fclose($source);
```

### Configuration<a name="object-uploader-configuration"></a>

The `ObjectUploader` object constructor accepts the following arguments:

**`$client`**  
The `Aws\ClientInterface` object to use for performing the transfers\. This should be an instance of `Aws\S3\S3Client`\.

**`$bucket`**  
\(`string`, *required*\) Name of the bucket to which the object is being uploaded\.

**`$key`**  
\(`string`, *required*\) Key to use for the object being uploaded\.

**`$body`**  
\(`mixed`, *required*\) Object data to upload\. Can be a `StreamInterface`, a PHP stream resource, or a string of data to upload\.

**`$acl`**  
\(`string`\) Access control list \(ACL\) to set on the object being upload\. Objects are private by default\.

**`$options`**  
An associative array of configuration options for the multipart upload\. The following configuration options are valid:    
**`add_content_md5`**  
\(`bool`\) Set to true to automatically calculate the MD5 checksum for the upload\.  
**`mup_threshold`**  
\(`int`, *default*: `int(16777216)`\) The number of bytes for the file size\. If the file size exceeds this limit, a multipart upload is used\.  
**`before_complete`**  
\(`callable`\) Callback to invoke before the `CompleteMultipartUpload` operation\. The callback should have a function signature similar to: `function (Aws\Command $command) {...}`\.  
**`before_initiate`**  
\(`callable`\) Callback to invoke before the `CreateMultipartUpload` operation\. The callback should have a function signature similar to: `function (Aws\Command $command) {...}`\.  
**`before_upload`**  
\(`callable`\) Callback to invoke before any `PutObject` or `UploadPart` operations\. The callback should have a function signature similar to: `function (Aws\Command $command) {...}`\.  
**`concurrency`**  
\(`int`, *default*: `int(3)`\) Maximum number of concurrent `UploadPart` operations allowed during the multipart upload\.  
**`part_size`**  
\(`int`, *default*: `int(5242880)`\) Part size, in bytes, to use when doing a multipart upload\. The value must between 5 MB and 5 GB, inclusive\.  
**`state`**  
\(`Aws\Multipart\UploadState`\) An object that represents the state of the multipart upload and that is used to resume a previous upload\. When this option is provided, the `$bucket` and `$key` arguments and the `part_size` option are ignored\.

## MultipartUploader<a name="multipartuploader"></a>

Multipart uploads are designed to improve the upload experience for larger objects\. They enable you to upload objects in parts independently, in any order, and in parallel\.

Amazon S3 customers are encouraged to use multipart uploads for objects greater than 100 MB\.

## MultipartUploader object<a name="multipartuploader-object"></a>

The SDK has a special `MultipartUploader` object that simplifies the multipart upload process\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\S3\S3Client;
use Aws\Exception\AwsException;
use Aws\S3\MultipartUploader;
use Aws\Exception\MultipartUploadException;
```

 **Sample Code** 

```
$s3Client = new S3Client([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2006-03-01'
]);

// Use multipart upload
$source = '/path/to/large/file.zip';
$uploader = new MultipartUploader($s3Client, $source, [
    'bucket' => 'your-bucket',
    'key' => 'my-file.zip',
]);

try {
    $result = $uploader->upload();
    echo "Upload complete: {$result['ObjectURL']}\n";
} catch (MultipartUploadException $e) {
    echo $e->getMessage() . "\n";
}
```

The uploader creates a generator of part data, based on the provided source and configuration, and attempts to upload all parts\. If some part uploads fail, the uploader continues to upload later parts until the entire source data is read\. Afterwards, the uploader retries to upload the failed parts or throws an exception containing information about the parts that failed to upload\.

## Customizing a multipart upload<a name="customizing-a-multipart-upload"></a>

You can set custom options on the `CreateMultipartUpload`, `UploadPart`, and `CompleteMultipartUpload` operations executed by the multipart uploader via callbacks passed to its constructor\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\S3\S3Client;
use Aws\Exception\AwsException;
use Aws\S3\MultipartUploader;
use Aws\Exception\MultipartUploadException;
```

 **Sample Code** 

```
// Create an S3Client
$s3Client = new S3Client([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2006-03-01'
]);


// Customizing a multipart upload
$source = '/path/to/large/file.zip';
$uploader = new MultipartUploader($s3Client, $source, [
    'bucket' => 'your-bucket',
    'key' => 'my-file.zip',
    'before_initiate' => function (\Aws\Command $command) {
        // $command is a CreateMultipartUpload operation
        $command['CacheControl'] = 'max-age=3600';
    },
    'before_upload' => function (\Aws\Command $command) {
        // $command is an UploadPart operation
        $command['RequestPayer'] = 'requester';
    },
    'before_complete' => function (\Aws\Command $command) {
        // $command is a CompleteMultipartUpload operation
        $command['RequestPayer'] = 'requester';
    },
]);
```

### Manual garbage collection between part uploads<a name="manual-garbage-collection-between-part-uploads"></a>

If you are hitting the memory limit with large uploads, this may be due to cyclic references generated by the SDK not yet having been collected by the [PHP garbage collector](https://www.php.net/manual/en/features.gc.php) when your memory limit was hit\. Manually invoking the collection algorithm between operations may allow the cycles to be collected before hitting that limit\. The following example invokes the collection algorithm using a callback before each part upload\. Note that invoking the garbage collector does come with a performance cost, and optimal usage will depend on your use case and environment\.

```
$uploader = new MultipartUploader($client, $source, [
   'bucket' => 'your-bucket',
   'key' => 'your-key',
   'before_upload' => function(\Aws\Command $command) {
      gc_collect_cycles();
   }
]);
```

## Recovering from errors<a name="recovering-from-errors"></a>

When an error occurs during the multipart upload process, a `MultipartUploadException` is thrown\. This exception provides access to the `UploadState` object, which contains information about the multipart upload’s progress\. The `UploadState` can be used to resume an upload that failed to complete\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\S3\S3Client;
use Aws\Exception\AwsException;
use Aws\S3\MultipartUploader;
use Aws\Exception\MultipartUploadException;
```

 **Sample Code** 

```
// Create an S3Client
$s3Client = new S3Client([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2006-03-01'
]);

$source = '/path/to/large/file.zip';
$uploader = new MultipartUploader($s3Client, $source, [
    'bucket' => 'your-bucket',
    'key' => 'my-file.zip',
]);

//Recover from errors
do {
    try {
        $result = $uploader->upload();
    } catch (MultipartUploadException $e) {
        $uploader = new MultipartUploader($s3Client, $source, [
            'state' => $e->getState(),
        ]);
    }
} while (!isset($result));

//Abort a multipart upload if failed
try {
    $result = $uploader->upload();
} catch (MultipartUploadException $e) {
    // State contains the "Bucket", "Key", and "UploadId"
    $params = $e->getState()->getId();
    $result = $s3Client->abortMultipartUpload($params);
}
```

Resuming an upload from an `UploadState` attempts to upload parts that are not already uploaded\. The state object tracks the missing parts, even if they are not consecutive\. The uploader reads or seeks through the provided source file to the byte ranges that belong to the parts that still need to be uploaded\.

 `UploadState` objects are serializable, so you can also resume an upload in a different process\. You can also get the `UploadState` object, even when you’re not handling an exception, by calling `$uploader->getState()`\.

**Important**  
Streams passed in as a source to a `MultipartUploader` are not automatically rewound before uploading\. If you’re using a stream instead of a file path in a loop similar to the previous example, reset the `$source` variable inside of the `catch` block\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\S3\S3Client;
use Aws\Exception\AwsException;
use Aws\S3\MultipartUploader;
use Aws\Exception\MultipartUploadException;
```

 **Sample Code** 

```
// Create an S3Client
$s3Client = new S3Client([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2006-03-01'
]);

//Using stream instead of file path
$source = fopen('/path/to/large/file.zip', 'rb');
$uploader = new MultipartUploader($s3Client, $source, [
    'bucket' => 'your-bucket',
    'key' => 'my-file.zip',
]);

do {
    try {
        $result = $uploader->upload();
    } catch (MultipartUploadException $e) {
        rewind($source);
        $uploader = new MultipartUploader($s3Client, $source, [
            'state' => $e->getState(),
        ]);
    }
} while (!isset($result));
```

### Aborting a multipart upload<a name="aborting-a-multipart-upload"></a>

A multipart upload can be aborted by retrieving the `UploadId` contained in the `UploadState` object and passing it to `abortMultipartUpload`\.

```
try {
    $result = $uploader->upload();
} catch (MultipartUploadException $e) {
    // State contains the "Bucket", "Key", and "UploadId"
    $params = $e->getState()->getId();
    $result = $s3Client->abortMultipartUpload($params);
}
```

## Asynchronous multipart uploads<a name="asynchronous-multipart-uploads"></a>

Calling `upload()` on the `MultipartUploader` is a blocking request\. If you are working in an asynchronous context, you can get a [promise](guide_promises.md) for the multipart upload\.

```
require 'vendor/autoload.php';

use Aws\S3\S3Client;
use Aws\Exception\AwsException;
use Aws\S3\MultipartUploader;
use Aws\Exception\MultipartUploadException;
```

 **Sample Code** 

```
// Create an S3Client
$s3Client = new S3Client([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2006-03-01'
]);


$source = '/path/to/large/file.zip';
$uploader = new MultipartUploader($s3Client, $source, [
    'bucket' => 'your-bucket',
    'key' => 'my-file.zip',
]);

$promise = $uploader->promise();
```

### Configuration<a name="asynchronous-multipart-uploads-configuration"></a>

The `MultipartUploader` object constructor accepts the following arguments:

** `$client` **  
The `Aws\ClientInterface` object to use for performing the transfers\. This should be an instance of `Aws\S3\S3Client`\.

** `$source` **  
The source data being uploaded\. This can be a path or URL \(for example, `/path/to/file.jpg`\), a resource handle \(for example, `fopen('/path/to/file.jpg', 'r)`\), or an instance of a [PSR\-7 stream](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Psr.Http.Message.StreamInterface.html)\.

** `$config` **  
An associative array of configuration options for the multipart upload\.  
The following configuration options are valid:    
** `acl` **  
\(`string`\) Access control list \(ACL\) to set on the object being upload\. Objects are private by default\.  
** `before_complete` **  
\(`callable`\) Callback to invoke before the `CompleteMultipartUpload` operation\. The callback should have a function signature like `function (Aws\Command $command) {...}`\.  
** `before_initiate` **  
\(`callable`\) Callback to invoke before the `CreateMultipartUpload` operation\. The callback should have a function signature like `function (Aws\Command $command) {...}`\.  
** `before_upload` **  
\(`callable`\) Callback to invoke before any `UploadPart` operations\. The callback should have a function signature like `function (Aws\Command $command) {...}`\.  
** `bucket` **  
\(`string`, *required*\) Name of the bucket to which the object is being uploaded\.  
** `concurrency` **  
\(`int`, *default*: `int(5)`\) Maximum number of concurrent `UploadPart` operations allowed during the multipart upload\.  
** `key` **  
\(`string`, *required*\) Key to use for the object being uploaded\.  
** `part_size` **  
\(`int`, *default*: `int(5242880)`\) Part size, in bytes, to use when doing a multipart upload\. This must between 5 MB and 5 GB, inclusive\.  
** `state` **  
\(`Aws\Multipart\UploadState`\) An object that represents the state of the multipart upload and that is used to resume a previous upload\. When this option is provided, the `bucket`, `key`, and `part_size` options are ignored\.  
**`add_content_md5`**  
\(`boolean`\) Set to true to automatically calculate the MD5 checksum for the upload\.  
** `track_upload` **  
\(`boolean` \) Set true to track status in 1/8th increments for upload\.

## Multipart copies<a name="multipart-copies"></a>

The AWS SDK for PHP also includes a `MultipartCopy` object that is used in a similar way to the `MultipartUploader`, but is designed for copying objects between 5 GB and 5 TB in size within Amazon S3\.

```
require 'vendor/autoload.php';

use Aws\S3\S3Client;
use Aws\Exception\AwsException;
use Aws\S3\MultipartCopy;
use Aws\Exception\MultipartUploadException;
```

 **Sample Code** 

```
// Create an S3Client
$s3Client = new S3Client([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2006-03-01'
]);


//Copy objects within S3
$copier = new MultipartCopy($s3Client, '/bucket/key?versionId=foo', [
    'bucket' => 'your-bucket',
    'key' => 'my-file.zip',
]);

try {
    $result = $copier->copy();
    echo "Copy complete: {$result['ObjectURL']}\n";
} catch (MultipartUploadException $e) {
    echo $e->getMessage() . "\n";
}
```
