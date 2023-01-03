# Amazon S3 stream wrapper with AWS SDK for PHP Version 3<a name="s3-stream-wrapper"></a>

The Amazon S3 stream wrapper enables you to store and retrieve data from Amazon S3 using built\-in PHP functions, such as `file_get_contents`, `fopen`, `copy`, `rename`, `unlink`, `mkdir`, and `rmdir`\.

You need to register the Amazon S3 stream wrapper to use it\.

```
$client = new Aws\S3\S3Client([/** options **/]);

// Register the stream wrapper from an S3Client object
$client->registerStreamWrapper();
```

This enables you to access buckets and objects stored in Amazon S3 using the `s3://` protocol\. The Amazon S3 stream wrapper accepts strings that contain a bucket name followed by a forward slash and an optional object key or prefix: `s3://<bucket>[/<key-or-prefix>]`\.

**Note**  
The stream wrapper is designed for working with objects and buckets on which you have at least read permission\. This means that your user should have permission to execute `ListBucket` on any buckets and `GetObject` on any object with which the user needs to interact\. For use cases where you don’t have this permission level, we recommended that you use Amazon S3 client operations directly\.

## Downloading data<a name="downloading-data"></a>

You can grab the contents of an object by using `file_get_contents`\. However, be careful with this function; it loads the entire contents of the object into memory\.

```
// Download the body of the "key" object in the "bucket" bucket
$data = file_get_contents('s3://bucket/key');
```

Use `fopen()` when working with larger files or if you need to stream data from Amazon S3\.

```
// Open a stream in read-only mode
if ($stream = fopen('s3://bucket/key', 'r')) {
    // While the stream is still open
    while (!feof($stream)) {
        // Read 1,024 bytes from the stream
        echo fread($stream, 1024);
    }
    // Be sure to close the stream resource when you're done with it
    fclose($stream);
}
```

**Note**  
File write errors are only returned when a call to `fflush` is made\. These errors are not returned when an unflushed `fclose` is called\. The return value for `fclose` will be `true` if it closes the stream, regardless of any errors in response to its internal `fflush`\. These errors are also not returned when calling `file_put_contents` because of how PHP implements it\.

### Opening seekable streams<a name="opening-seekable-streams"></a>

Streams opened in “r” mode only allow data to be read from the stream, and are not seekable by default\. This is so that data can be downloaded from Amazon S3 in a truly streaming manner, where previously read bytes do not need to be buffered into memory\. If you need a stream to be seekable, you can pass `seekable` into the [stream context options](http://www.php.net/manual/en/function.stream-context-create.php) of a function\.

```
$context = stream_context_create([
    's3' => ['seekable' => true]
]);

if ($stream = fopen('s3://bucket/key', 'r', false, $context)) {
    // Read bytes from the stream
    fread($stream, 1024);
    // Seek back to the beginning of the stream
    fseek($stream, 0);
    // Read the same bytes that were previously read
    fread($stream, 1024);
    fclose($stream);
}
```

Opening seekable streams enables you to seek bytes that were previously read\. You can’t skip ahead to bytes that have not yet been read from the remote server\. To allow previously read data to recalled, data is buffered in a PHP temp stream using a stream decorator\. When the amount of cached data exceeds 2 MB, the data in the temp stream transfers from memory to disk\. Keep this in mind when downloading large files from Amazon S3 using the `seekable` stream context setting\.

## Uploading data<a name="uploading-data"></a>

You can upload data to Amazon S3 using `file_put_contents()`\.

```
file_put_contents('s3://bucket/key', 'Hello!');
```

You can upload larger files by streaming data using `fopen()` and a “w”, “x”, or “a” stream access mode\. The Amazon S3 stream wrapper does **not** support simultaneous read and write streams \(e\.g\. “r\+”, “w\+”, etc\)\. This is because the HTTP protocol doesn’t allow simultaneous reading and writing\.

```
$stream = fopen('s3://bucket/key', 'w');
fwrite($stream, 'Hello!');
fclose($stream);
```

**Note**  
Amazon S3 requires a Content\-Length header to be specified before the payload of a request is sent\. Therefore, the data to be uploaded in a `PutObject` operation is internally buffered using a PHP temp stream until the stream is flushed or closed\.

**Note**  
File write errors are returned only when a call to `fflush` is made\. These errors are not returned when an unflushed `fclose` is called\. The return value for `fclose` will be `true` if it closes the stream, regardless of any errors in response to its internal `fflush`\. These errors are also not returned when calling `file_put_contents` because of how PHP implements it\.

## fopen modes<a name="fopen-modes"></a>

PHP’s [fopen\(\)](http://php.net/manual/en/function.fopen.php) function requires that you specify a `$mode` option\. The mode option specifies whether data can be read or written to a stream, and whether the file must exist when opening a stream\. The Amazon S3 stream wrapper supports the following modes\.


****  

|  |  | 
| --- |--- |
|  r  |  A read\-only stream where the file must already exist\.  | 
|  w  |  A write\-only stream\. If the file already exists, it is overwritten\.  | 
|  a  |  A write\-only stream\. If the file already exists, it is downloaded to a temporary stream and any writes to the stream is appended to any previously uploaded data\.  | 
|  x  |  A write\-only stream\. An error is raised if the file does not already exist\.  | 

## Other object functions<a name="other-object-functions"></a>

Stream wrappers allow many different built\-in PHP functions to work with a custom system such as Amazon S3\. Here are some of the functions that the Amazon S3 stream wrapper enables you to perform with objects stored in Amazon S3\.


****  

|  |  | 
| --- |--- |
|  unlink\(\)  |  Delete an object from a bucket\. <pre>// Delete an object from a bucket<br />unlink('s3://bucket/key');</pre> You can pass in any options available to the `DeleteObject` operation to modify how the object is deleted \(e\.g\. specifying a specific object version\)\. <pre>// Delete a specific version of an object from a bucket<br />unlink('s3://bucket/key', stream_context_create([<br />    's3' => ['VersionId' => '123']<br />]);</pre>  | 
|  filesize\(\)  |  Get the size of an object\. <pre>// Get the Content-Length of an object<br />$size = filesize('s3://bucket/key', );</pre>  | 
|  is\_file\(\)  |  Checks if a URL is a file\. <pre>if (is_file('s3://bucket/key')) {<br />    echo 'It is a file!';<br />}</pre>  | 
|  file\_exists\(\)  |  Checks if an object exists\. <pre>if (file_exists('s3://bucket/key')) {<br />    echo 'It exists!';<br />}</pre>  | 
|  filetype\(\)  |  Checks if a URL maps to a file or bucket \(dir\)\.  | 
|  file\(\)  |  Load the contents of an object in an array of lines\. You can pass in any options available to the `GetObject` operation to modify how the file is downloaded\.  | 
|  filemtime\(\)  |  Get the last modified date of an object\.  | 
|  rename\(\)  |  Rename an object by copying the object then deleting the original\. You can pass in options available to the `CopyObject` and `DeleteObject` operations to the stream context parameters to modify how the object is copied and deleted\.  | 

**Note**  
Although `copy` generally works with the Amazon S3 stream wrapper, some errors might not be properly reported due to the internals of the `copy` function in PHP\. We recommend that you use an instance of [AwsS3ObjectCopier](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.S3.ObjectCopier.html) instead\.

## Working with buckets<a name="working-with-buckets"></a>

You can modify and browse Amazon S3 buckets similarly to how PHP allows the modification and traversal of directories on your file system\.

Here’s an example of creating a bucket\.

```
mkdir('s3://bucket');
```

You can pass in stream context options to the `mkdir()` method to modify how the bucket is created using the parameters available to the [CreateBucket](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.S3.S3Client.html#_createBucket) operation\.

```
// Create a bucket in the EU (Ireland) Region
mkdir('s3://bucket', stream_context_create([
    's3' => ['LocationConstraint' => 'eu-west-1']
]);
```

You can delete buckets using the `rmdir()` function\.

```
// Delete a bucket
rmdir('s3://bucket');
```

**Note**  
A bucket can only be deleted if it is empty\.

### Listing the contents of a bucket<a name="listing-the-contents-of-a-bucket"></a>

You can use the [opendir\(\)](http://www.php.net/manual/en/function.opendir.php), [readdir\(\)](http://www.php.net/manual/en/function.readdir.php), [rewinddir\(\)](http://www.php.net/manual/en/function.rewinddir.php), and [closedir\(\)](http://php.net/manual/en/function.closedir.php) PHP functions with the Amazon S3 stream wrapper to traverse the contents of a bucket\. You can pass in parameters available to the [ListObjects](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.S3.S3Client.html#_listObjects) operation as custom stream context options to the `opendir()` function to modify how objects are listed\.

```
$dir = "s3://bucket/";

if (is_dir($dir) && ($dh = opendir($dir))) {
    while (($file = readdir($dh)) !== false) {
        echo "filename: {$file} : filetype: " . filetype($dir . $file) . "\n";
    }
    closedir($dh);
}
```

You can recursively list each object and prefix in a bucket using PHP’s [RecursiveDirectoryIterator](http://php.net/manual/en/class.recursivedirectoryiterator.php)\.

```
$dir = 's3://bucket';
$iterator = new RecursiveIteratorIterator(new RecursiveDirectoryIterator($dir));

foreach ($iterator as $file) {
    echo $file->getType() . ': ' . $file . "\n";
}
```

Another way to list the contents of a bucket recursively that incurs fewer HTTP requests is to use the `Aws\recursive_dir_iterator($path, $context = null)` function\.

```
<?php
require 'vendor/autoload.php';

$iter = Aws\recursive_dir_iterator('s3://bucket/key');
foreach ($iter as $filename) {
    echo $filename . "\n";
}
```

## Stream context options<a name="id1"></a>

You can customize the client used by the stream wrapper, or the cache used to cache previously loaded information about buckets and keys, by passing in custom stream context options\.

The stream wrapper supports the following stream context options on every operation\.

** `client` **  
The `Aws\AwsClientInterface` object to use to execute commands\.

** `cache` **  
An instance of `Aws\CacheInterface` to use to cache previously obtained file stats\. By default, the stream wrapper uses an in\-memory LRU cache\.