# FAQ for AWS SDK for PHP Version 3<a name="faq"></a>

## What methods are available on a client?<a name="what-methods-are-available-on-a-client"></a>

The AWS SDK for PHP uses service descriptions and dynamic [magic \_\_call\(\) methods](http://www.php.net/manual/en/language.oop5.overloading.php#object.call) to execute API operations\. You can find a full list of methods available for a web service client in the [API documentation](https://docs.aws.amazon.com/aws-sdk-php/v3/api/index.html) of the client\.

## What do I do about a cURL SSL certificate error?<a name="what-do-i-do-about-a-curl-ssl-certificate-error"></a>

This issue can occur when using an out\-of\-date CA bundle with cURL and SSL\. You can get around this issue by updating the CA bundle on your server or downloading a more up\-to\-date CA bundle from the [cURL website directly](http://curl.haxx.se/docs/caextract.html)\.

By default, the AWS SDK for PHP will use the CA bundle that is configured when PHP is compiled\. You can change the default CA bundle used by PHP by modifying the `openssl.cafile` PHP \.ini configuration setting to be set to the path of a CA file on disk\.

## What API versions are available for a client?<a name="what-api-versions-are-available-for-a-client"></a>

A `version` option is required when creating a client\. A list of available API versions can be found on each client’s API documentation page ::aws\-php\-class:<index\.html>\. If you’re unable to load a specific API version, you might need to update your copy of the AWS SDK for PHP\.

You can provide the string `latest` to the “version” configuration value to use the most recent available API version that your client’s API provider can find \(the default api\_provider will scan the `src/data` directory of the SDK for API models\)\.

**Warning**  
We don’t recommend using `latest` in a production application because pulling in a new minor version of the SDK that includes an API update could break your production application\.

## What Region versions are available for a client?<a name="what-region-versions-are-available-for-a-client"></a>

A `region` option is required when creating a client, and is specified using a string value\. For a list of available AWS Regions and endpoints, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) in the AWS General Reference\.

```
// Set the Region to the EU (Frankfurt) Region.
$s3 = new Aws\S3\S3Client([
    'region'  => 'eu-central-1',
    'version' => '2006-03-01'
]);
```

## Why can’t I upload or download files larger than 2 GB?<a name="why-can-t-i-upload-or-download-files-larger-than-2-gb"></a>

Because PHP’s integer type is signed, and many platforms use 32\-bit integers, the AWS SDK for PHP doesn’t correctly handle files larger than 2 GB on a 32\-bit stack \(where “stack” includes CPU, OS, web server, and PHP binary\)\. This is a [well\-known PHP issue](http://www.google.com/search?q=php+2gb+32-bit)\. In the case of Microsoft Windows, only builds of PHP 7 support 64\-bit integers\.

The recommended solution is to use a [64\-bit Linux stack](https://aws.amazon.com/amazon-linux-ami/), such as the 64\-bit Amazon Linux AMI, with the latest version of PHP installed\.

For more information, see [PHP filesize: Return values](http://docs.php.net/manual/en/function.filesize.php#refsect1-function.filesize-returnvalues)\.

## How can I see what data is sent over the wire?<a name="how-can-i-see-what-data-is-sent-over-the-wire"></a>

You can get debug information, including the data sent over the wire, using the `debug` option in a client constructor\. When this option is set to `true`, all of the mutations of the command being executed, the request being sent, the response being received, and the result being processed are emitted to STDOUT\. This includes the data that is sent and received over the wire\.

```
$s3Client = new Aws\S3\S3Client([
    'region'  => 'us-standard',
    'version' => '2006-03-01',
    'debug'   => true
]);
```

## How can I set arbitrary headers on a request?<a name="how-can-i-set-arbitrary-headers-on-a-request"></a>

You can add any arbitrary headers to a service operation by adding a custom middleware to the `Aws\HandlerList` of an `Aws\CommandInterface` or `Aws\ClientInterface`\. The following example shows how to add an `X-Foo-Baz` header to a specific Amazon S3`PutObject` operation using the `Aws\Middleware::mapRequest` helper method\.

See [mapRequest](guide_handlers-and-middleware.md#map-request) for more information\.

## How can I sign an arbitrary request?<a name="how-can-i-sign-an-arbitrary-request"></a>

You can sign an arbitrary :aws\-php\-class: *PSR\-7 request <class\-Psr\.Http\.Message\.RequestInterface\.html>* using the SDK’s :aws\-php\-class: *SignatureV4 class <class\-Aws\.Signature\.SignatureV4\.html>*\.

See [Signing Custom Amazon CloudSearch Domain Requests with AWS SDK for PHP Version 3](service_cloudsearch-custom-requests.md) for a full example of how to do this\.

## How can I modify a command before sending it?<a name="how-can-i-modify-a-command-before-sending-it"></a>

You can modify a command before sending it by adding a custom middleware to the `Aws\HandlerList` of an `Aws\CommandInterface` or `Aws\ClientInterface`\. The following example shows how to add custom command parameters to a command before it’s sent, essentially adding default options\. This example uses the `Aws\Middleware::mapCommand` helper method\.

See [mapCommand](guide_handlers-and-middleware.md#map-command) for more information\.

## What is a CredentialsException?<a name="what-is-a-credentialsexception"></a>

If you are seeing an `Aws\Exception\CredentialsException` while using the AWS SDK for PHP, it means that the SDK was not provided with any credentials and was unable to find credentials in the environment\.

If you instantiate a client *without* credentials, the first time that you perform a service operation the SDK will attempt to find credentials\. It first checks in some specific environment variables, then it looks for instance profile credentials, which are only available on configured Amazon EC2 instances\. If absolutely no credentials are provided or found, an `Aws\Exception\CredentialsException` is thrown\.

If you are seeing this error and you are intending to use instance profile credentials, you need to be sure that the Amazon EC2 instance that the SDK is running on is configured with an appropriate IAM role\.

If you are seeing this error and you are **not** intending to use instance profile credentials, you need to be sure that you are properly providing credentials to the SDK\.

For more information, see [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md)\.

## Does the AWS SDK for PHP work on HHVM?<a name="does-the-sdk-php-work-on-hhvm"></a>

The AWS SDK for PHP doesn’t currently run on HHVM, and won’t be able to until the [issue with the yield semantics in HHVM](https://github.com/facebook/hhvm/issues/6807) is resolved\.

## How do I disable SSL?<a name="how-do-i-disable-ssl"></a>

You can disable SSL by setting the `scheme` parameter in a client factory method to ‘http’\. It is important to note that not all services support `http` access\. See [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) in the AWS General Reference for a list of regions, endpoints, and the supported schemes\.

```
$client = new Aws\DynamoDb\DynamoDbClient([
    'version' => '2012-08-10',
    'region'  => 'us-west-2',
    'scheme'  => 'http'
]);
```

**Warning**  
Because SSL requires all data to be encrypted and requires more TCP packets to complete a connection handshake than just TCP, disabling SSL may provide a small performance improvement\. However, with SSL disabled, all data is sent over the wire unencrypted\. Before disabling SSL, you must carefully consider the security implications and the potential for eavesdropping over the network\.

## What do I do about a “Parse error”?<a name="what-do-i-do-about-a-parse-error"></a>

The PHP engine will throw parsing errors when it encounters syntax it doesn’t understand\. This is almost always encountered when attempting to run code that was written for a different version of PHP\.

If you encounter a parsing error, check your system and be sure it fulfills the SDK’s [Requirements and Recommendations for the AWS SDK for PHP Version 3](getting-started_requirements.md)\.

## Why is the Amazon S3 client decompressing gzipped files?<a name="why-is-the-s3-client-decompressing-gzipped-files"></a>

Some HTTP handlers, including the default Guzzle 6 HTTP handler, will inflate compressed response bodies by default\. You can override this behavior by setting the [decode\_content](guide_configuration.md#http-decode-content) HTTP option to `false`\. For backward\-compatibility reasons, this default cannot be changed, but we recommend that you disable content decoding at the S3 client level\.

See [decode\_content](guide_configuration.md#http-decode-content) for an example of how to disable automatic content decoding\.

## How do I disable body signing in Amazon S3?<a name="how-do-i-disable-body-signing-in-s3"></a>

You can disable body signing by setting the `ContentSHA256` parameter in the command object to `Aws\Signature\S3SignatureV4::UNSIGNED_PAYLOAD`\. Then the AWS SDK for PHP will use it as the ‘x\-amz\-content\-sha\-256’ header and the body checksum in the canonical request\.

```
$s3Client = new Aws\S3\S3Client([
    'version' => '2006-03-01',
    'region'  => 'us-standard'
]);

$params = [
    'Bucket' => 'foo',
    'Key'    => 'baz',
    'ContentSHA256' => Aws\Signature\S3SignatureV4::UNSIGNED_PAYLOAD
];

// Using operation methods creates command implicitly
$result = $s3Client->putObject($params);

// Using commands explicitly.
$command = $s3Client->getCommand('PutObject', $params);
$result = $s3Client->execute($command);
```

## How is retry scheme handled in the AWS SDK for PHP?<a name="how-is-retry-scheme-handled-in-the-sdk-php"></a>

The AWS SDK for PHP has a `RetryMiddleware` that handles retry behavior\. In terms of 5xx HTTP status codes for server errors, the SDK retries on 500, 502, 503 and 504\.

Throttling exceptions, including `RequestLimitExceeded`, `Throttling`, `ProvisionedThroughputExceededException`, `ThrottlingException`, `RequestThrottled` and `BandwidthLimitExceeded`, are also handled with retries\.

The AWS SDK for PHP also integrates exponential delay with a backoff and jitter algorithm in the retry scheme\. Furthermore, default retry behavior is configured as `3` for all services except Amazon DynamoDB, which is `10`\.

## How do I handle exceptions with error codes?<a name="how-do-i-handle-exceptions-with-error-codes"></a>

Besides AWS SDK for PHP\-customized `Exception` classes, each AWS service client has its own exception class that inherits from [AwsExceptionAwsException](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.Exception.AwsException.html)\. You can determine more specific error types to catch with the API\-specific errors listed under the `Errors` section of each method\.

Error code information is available with [getAwsErrorCode\(\)](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.Exception.AwsException.html#_getAwsErrorCode) from `Aws\Exception\AwsException`\.

```
$sns = new \Aws\Sns\SnsClient([
    'region' => 'us-west-2',
    'version' => 'latest',
]);

try {
    $sns->publish([
        // parameters
        ...
    ]);
    // Do something
} catch (SnsException $e) {
    switch ($e->getAwsErrorCode()) {
        case 'EndpointDisabled':
        case 'NotFound':
            // Do something
            break;
    }
}
```