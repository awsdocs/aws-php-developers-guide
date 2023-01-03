# Configuration for the AWS SDK for PHP Version 3<a name="guide_configuration"></a>

Client constructor options can be provided in a client constructor or provided to the [https://docs.aws.amazon.com/aws-sdk-php/latest/class-Aws.Sdk.html](https://docs.aws.amazon.com/aws-sdk-php/latest/class-Aws.Sdk.html) class\. The array of options provided to a specific type of client can vary, based on which client you are creating\. These custom client configuration options are described in the [API documentation](https://docs.aws.amazon.com/aws-sdk-php/latest/) of each client\.

Note that some configuration options will check and use default values based on environment variables or an AWS configuration file\. By default, the configuration file being checked will be `.aws/config` in your home directory, commonly `~/.aws/config`\. However, you can use the environment variable `AWS_CONFIG_FILE` to set where your default config file location is\. For example, this may be useful if you are restricting file access to certain directories with `open_basedir`\.

For more information on the location and formatting of the shared AWS `config` and `credentials` files, see [Configuration](https://docs.aws.amazon.com/sdkref/latest/guide/creds-config-files.html) in the *AWS SDKs and Tools Reference Guide*\.

For details on all global configuration settings that you can set in the AWS configuration files or as environment variables, see [Configuration and authentication settings reference](https://docs.aws.amazon.com/sdkref/latest/guide/settings-reference.html) in the *AWS SDKs and Tools Reference Guide*\.

**Topics**
+ [api\_provider](#api-provider)
+ [credentials](#credentials)
+ [debug](#debug)
+ [stats](#config-stats)
+ [endpoint](#endpoint)
+ [endpoint\_provider](#endpoint-provider)
+ [endpoint\_discovery](#endpoint-discovery)
+ [handler](#handler)
+ [http](#config-http)
+ [http\_handler](#http-handler)
+ [profile](#profile)
+ [region](#cfg-region)
+ [retries](#config-retries)
+ [scheme](#scheme)
+ [service](#service)
+ [signature\_provider](#signature-provider)
+ [signature\_version](#signature-version)
+ [ua\_append](#ua-append)
+ [use\_aws\_shared\_config\_files](#use-aws-shared-config-files)
+ [validate](#validate)
+ [version](#cfg-version)

The following example shows how to pass options into an Amazon S3 client constructor\.

```
use Aws\S3\S3Client;

$options = [
    'region'            => 'us-west-2',
    'version'           => '2006-03-01',
    'signature_version' => 'v4'
];

$s3Client = new S3Client($options);
```

See the [basic usage guide](getting-started_basic-usage.md) for more information about constructing clients\.

## api\_provider<a name="api-provider"></a>

Type  
 `callable` 

A PHP callable that accepts a type, service, and version argument, and returns an array of corresponding configuration data\. The type value can be one of `api`, `waiter`, or `paginator`\.

By default, the SDK uses an instance of `Aws\Api\FileSystemApiProvider` that loads API files from the `src/data` folder of the SDK\.

## credentials<a name="credentials"></a>

Type  
 `array|Aws\CacheInterface|Aws\Credentials\CredentialsInterface|bool|callable` 

Pass an `Aws\Credentials\CredentialsInterface` object to use a specific credentials instance\.

```
$credentials = new Aws\Credentials\Credentials('key', 'secret');

$s3 = new Aws\S3\S3Client([
    'version'     => 'latest',
    'region'      => 'us-west-2',
    'credentials' => $credentials
]);
```

If you don’t provide a `credentials` option, the SDK attempts to load credentials from your environment in the following order:

1. Load credentials from [environment variables](guide_credentials_environment.md)\.

1. Load credentials from a [credentials \.ini file](guide_credentials_profiles.md)\.

1. Load credentials from an [IAM role](guide_credentials_assume_role.md)\.

Pass `false` to use null credentials and not sign requests\.

```
$s3 = new Aws\S3\S3Client([
    'version'     => 'latest',
    'region'      => 'us-west-2',
    'credentials' => false
]);
```

Pass a callable [credential provider](guide_credentials_provider.md) function to create credentials using a function\.

```
use Aws\Credentials\CredentialProvider;

// Only load credentials from environment variables
$provider = CredentialProvider::env();

$s3 = new Aws\S3\S3Client([
    'version'     => 'latest',
    'region'      => 'us-west-2',
    'credentials' => $provider
]);
```

Pass credentials cached to an instance of `Aws\CacheInterface` to cache the values returned by the default provider chain across multiple processes\.

```
use Aws\Credentials\CredentialProvider;
use Aws\DoctrineCacheAdapter;
use Doctrine\Common\Cache\ApcuCache;

$cache = new DoctrineCacheAdapter(new ApcuCache);
$provider = CredentialProvider::defaultProvider();
$cachedProvider = CredentialProvider::cache($provider, $cache);

$s3 = new Aws\S3\S3Client([
    'version' => 'latest',
    'region' => 'us-west-2',
    'credentials' => $cachedProvider
    ]);
```

You can find more information about providing credentials to a client in the [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md) guide\.

**Note**  
Credentials are loaded and validated lazily when they are used\.

## debug<a name="debug"></a>

Type  
 `bool|array` 

Outputs debug information about each transfer\. Debug information contains information about each state change of a transaction as it is prepared and sent over the wire\. Also included in the debug output is information about the specific HTTP handler used by a client \(e\.g\., debug cURL output\)\.

Set to `true` to display debug information when sending requests\.

```
$s3 = new Aws\S3\S3Client([
    'version' => 'latest',
    'region'  => 'us-west-2',
    'debug'   => true
]);

// Perform an operation to see the debug output
$s3->listBuckets();
```

Alternatively, you can provide an associative array with the following keys\.

**logfn \(callable\)**  
Function that is invoked with log messages\. By default, PHP’s `echo` function is used\.

**stream\_size \(int\)**  
When the size of a stream is greater than this number, the stream data is not logged\. Set to `0` to not log any stream data\.

**scrub\_auth \(bool\)**  
Set to `false` to disable the scrubbing of auth data from the logged messages \(meaning your AWS access key ID and signature will be passed through to the `logfn`\)\.

**http \(bool\)**  
Set to `false` to disable the “debug” feature of lower\-level HTTP handlers \(e\.g\., verbose cURL output\)\.

**auth\_headers \(array\)**  
Set to a key\-value mapping of headers you want to replace mapped to the value you want to replace them with\. These values are not used unless `scrub_auth` is set to `true`\.

**auth\_strings \(array\)**  
Set to a key\-value mapping of regular expressions to map to their replacements\. These values are used by the authentication data scrubber if `scrub_auth` is set to `true`\.

```
$s3 = new Aws\S3\S3Client([
    'version' => 'latest',
    'region'  => 'us-west-2',
    'debug'   => [
        'logfn'        => function ($msg) { echo $msg . "\n"; },
        'stream_size'  => 0,
        'scrub_auth'   => true,
        'http'         => true,
        'auth_headers' => [
            'X-My-Secret-Header' => '[REDACTED]',
        ],
        'auth_strings' => [
            '/SuperSecret=[A-Za-z0-9]{20}/i' => 'SuperSecret=[REDACTED]',
        ],
    ]
]);

// Perform an operation to see the debug output
$s3->listBuckets();
```

**Note**  
This option also outputs the underlying HTTP handler information produced by the `http` debug option\. The debug output is extremely useful when diagnosing issues in the AWS SDK for PHP\. Please provide the debug output for an isolated failure case when opening issues on the SDK\.

## stats<a name="config-stats"></a>

Type  
 `bool|array` 

Binds transfer statistics to errors and results returned by SDK operations\.

Set to `true` to gather transfer statistics on requests sent\.

```
$s3 = new Aws\S3\S3Client([
    'version' => 'latest',
    'region'  => 'us-west-2',
    'stats'   => true
]);

// Perform an operation
$result = $s3->listBuckets();
// Inspect the stats
$stats = $result['@metadata']['transferStats'];
```

Alternatively, you can provide an associative array with the following keys\.

**retries \(bool\)**  
Set to `true` to enable reporting on retries attempted\. Retry statistics are collected by default and returned\.

**http \(bool\)**  
Set to `true` to enable collecting statistics from lower\-level HTTP adapters \(e\.g\., values returned in GuzzleHttpTransferStats\)\. HTTP handlers must support an \_\_on\_transfer\_stats option for this to have an effect\. HTTP stats are returned as an indexed array of associative arrays; each associative array contains the transfer stats returned for a request by the client’s HTTP handler\. Disabled by default\.  
If a request was retried, each request’s transfer stats are returned, with `$result['@metadata']['transferStats']['http'][0]` containing the stats for the first request, `$result['@metadata']['transferStats']['http'][1]` containing the statistics for the second request, and so on\.

**timer \(bool\)**  
Set to `true` to enable a command timer that reports the total wall clock time spent on an operation in seconds\. Disabled by default\.

```
$s3 = new Aws\S3\S3Client([
    'version' => 'latest',
    'region'  => 'us-west-2',
    'stats'   => [
        'retries'      => true,
        'timer'        => false,
        'http'         => true,
    ]
]);

// Perform an operation
$result = $s3->listBuckets();
// Inspect the HTTP transfer stats
$stats = $result['@metadata']['transferStats']['http'];
// Inspect the number of retries attempted
$stats = $result['@metadata']['transferStats']['retries_attempted'];
// Inspect the total backoff delay inserted between retries
$stats = $result['@metadata']['transferStats']['total_retry_delay'];
```

## endpoint<a name="endpoint"></a>

Type  
 `string` 

The full URI of the web service\. This is required for services, such as [MediaConvert](AWS Elemental MediaConvert User Guide) , that use account\-specific endpoints\. For these services, request this endpoint using the `describeEndpoints` method\.

This is only required when connecting to a custom endpoint \(e\.g\., a local version of Amazon S3 or [Amazon DynamoDB Local](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Tools.DynamoDBLoca.html)\)\.

Here’s an example of connecting to Amazon DynamoDB Local:

```
$client = new Aws\DynamoDb\DynamoDbClient([
    'version'  => '2012-08-10',
    'region'   => 'us-east-1',
    'endpoint' => 'http://localhost:8000'
]);
```

See the [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) for a list of available AWS Regions and endpoints\.

## endpoint\_provider<a name="endpoint-provider"></a>

Type  
 `Aws\EndpointV2\EndpointProviderV2|callable` 

An optional instance of EndpointProviderV2 or PHP callable that accepts a hash of options, including a “service” and “region” key\. It returns `NULL` or a hash of endpoint data, of which the “endpoint” key is required\.

Here’s an example of how to create a minimal endpoint provider\.

```
$provider = function (array $params) {
    if ($params['service'] == 'foo') {
        return ['endpoint' => $params['region'] . '.example.com'];
    }
    // Return null when the provider cannot handle the parameters
    return null;
});
```

## endpoint\_discovery<a name="endpoint-discovery"></a>

Type  
 `array|Aws\CacheInterface|Aws\EndpointDiscovery\ConfigurationInterface|callable` 

Endpoint discovery identifies and connects to the correct endpoint for a service API that supports endpoint discovery\. For services that support but don’t require endpoint discovery, enable `endpoint_discovery` during client creation\. If a service does not support endpoint discovery this configuration is ignored\.

 `Aws\EndpointDiscovery\ConfigurationInterface` 

An optional configuration provider that enables automatic connection to the appropriate endpoint of a service API for operations the service specifies\.

The `Aws\EndpointDiscovery\Configuration` object accepts two options, including a Boolean value, “enabled”, that indicates if endpoint discovery is enabled, and an integer “cache\_limit” that indicates the maximum number of keys in the endpoint cache\.

For each client created, pass an `Aws\EndpointDiscovery\Configuration` object to use a specific configuration for endpoint discovery\.

```
use Aws\EndpointDiscovery\Configuration;
use Aws\S3\S3Client;

$enabled = true;
$cache_limit = 1000;

$config = new Aws\EndpointDiscovery\Configuration (
    $enabled,
    $cache_limit
);

$s3 = new Aws\S3\S3Client([
    'version' => 'latest',
    'region' => 'us-east-2',
    'endpoint_discovery' => $config,

]);
```

Pass an instance of `Aws\CacheInterface` to cache the values returned by endpoint discovery across multiple processes\.

```
use Aws\DoctrineCacheAdapter;
use Aws\S3\S3Client;
use Doctrine\Common\Cache\ApcuCache;

$s3 = new S3Client([
    'version'     => 'latest',
    'region'      => 'us-west-2',
    'endpoint_discovery' => new DoctrineCacheAdapter(new ApcuCache),
]);
```

Pass an array to endpoint discovery\.

```
use Aws\S3\S3Client;

$s3 = new S3Client([
    'version'     => 'latest',
    'region'      => 'us-west-2',
    'endpoint_discovery' => [
        'enabled' => true,
        'cache_limit' => 1000
    ],
]);
```

## handler<a name="handler"></a>

Type  
 `callable` 

A handler that accepts a command object and request object, and that returns a promise \(`GuzzleHttp\Promise\PromiseInterface`\) that is fulfilled with an `Aws\ResultInterface` object or rejected with an `Aws\Exception\AwsException`\. A handler does not accept a next handler as it is terminal and expected to fulfill a command\. If no handler is provided, a default Guzzle handler is used\.

You can use the `Aws\MockHandler` to return mocked results or throw mock exceptions\. You enqueue results or exceptions, and the MockHandler will dequeue them in FIFO order\.

```
use Aws\Result;
use Aws\MockHandler;
use Aws\DynamoDb\DynamoDbClient;
use Aws\CommandInterface;
use Psr\Http\Message\RequestInterface;
use Aws\Exception\AwsException;

$mock = new MockHandler();

// Return a mocked result
$mock->append(new Result(['foo' => 'bar']));

// You can provide a function to invoke; here we throw a mock exception
$mock->append(function (CommandInterface $cmd, RequestInterface $req) {
    return new AwsException('Mock exception', $cmd);
});

// Create a client with the mock handler
$client = new DynamoDbClient([
    'region'  => 'us-west-2',
    'version' => 'latest',
    'handler' => $mock
]);

// Result object response will contain ['foo' => 'bar']
$result = $client->listTables();

// This will throw the exception that was enqueued
$client->listTables();
```

## http<a name="config-http"></a>

Type  
 `array` 

Set to an array of HTTP options that are applied to HTTP requests and transfers created by the SDK\.

The SDK supports the following configuration options:

### cert<a name="http-cert"></a>

Type  
 `string|array` 

Specify the PEM formatted client side certificate\.
+ Set as a string for the path to only the certificate file\.

```
use Aws\S3\S3Client;

$client = new S3Client([
    'region'  => 'us-west-2',
    'version' => 'latest',
    'http'    => ['cert' => '/path/to/cert.pem']
]);
```
+ Set as an array containing the path and password\.

```
use Aws\S3\S3Client;

$client = new S3Client([
    'region'  => 'us-west-2',
    'version' => 'latest',
    'http'    => [
        'cert' => ['/path/to/cert.pem', 'password']
    ]
]);
```

### connect\_timeout<a name="http-connect-timeout"></a>

A float describing the number of seconds to wait while trying to connect to a server\. Use `0` to wait indefinitely \(the default behavior\)\.

```
use Aws\DynamoDb\DynamoDbClient;

// Timeout after attempting to connect for 5 seconds
$client = new DynamoDbClient([
    'region'  => 'us-west-2',
    'version' => 'latest',
    'http'    => [
        'connect_timeout' => 5
    ]
]);
```

### debug<a name="http-debug"></a>

Type  
 `bool|resource` 

Instructs the underlying HTTP handler to output debug information\. The debug information provided by different HTTP handlers will vary\.
+ Pass `true` to write debug output to STDOUT\.
+ Pass a `resource` as returned by `fopen` to write debug output to a specific PHP stream resource\.

### decode\_content<a name="http-decode-content"></a>

Type  
 `bool` 

Instructs the underlying HTTP handler to inflate the body of compressed responses\. When not enabled, compressed response bodies might be inflated with a `GuzzleHttp\Psr7\InflateStream`\.

**Note**  
Content decoding is enabled by default in the SDK’s default HTTP handler\. For backward compatibility reasons, this default cannot be changed\. If you store compressed files in Amazon S3, we recommend that you disable content decoding at the S3 client level\.  

```
use Aws\S3\S3Client;
use GuzzleHttp\Psr7\InflateStream;

$client = new S3Client([
    'region'  => 'us-west-2',
    'version' => 'latest',
    'http'    => ['decode_content' => false],
]);

$result = $client->getObject([
    'Bucket' => 'my-bucket',
    'Key'    => 'massize_gzipped_file.tgz'
]);

$compressedBody = $result['Body']; // This content is still gzipped
$inflatedBody = new InflateStream($result['Body']); // This is now readable
```

### delay<a name="http-delay"></a>

Type  
 `int` 

The number of milliseconds to delay before sending the request\. This is often used for delaying before retrying a request\.

### expect<a name="http-expect"></a>

Type  
 `bool|string` 

This option is passed through to the underlying HTTP handler\. By default, Expect: 100\-Continue header is set when the body of the request exceeds 1 MB\. `true` or `false` enables or disables the header on all requests\. If an integer is used, only requests with bodies that exceed this setting will use the header\. When used as an integer, if the body size is unknown the Expect header will be sent\.

**Warning**  
Disabling the Expect header can prevent the service from returning authentication or other errors\. This option should be configured with caution\.

### progress<a name="http-progress"></a>

Type  
 `callable` 

Defines a function to invoke when transfer progress is made\. The function accepts the following arguments:

1. The total number of bytes expected to be downloaded\.

1. The number of bytes downloaded so far\.

1. The number of bytes expected to be uploaded\.

1. The number of bytes uploaded so far\.

```
use Aws\S3\S3Client;

$client = new S3Client([
    'region'  => 'us-west-2',
    'version' => 'latest'
]);

// Apply the http option to a specific command using the "@http"
// command parameter
$result = $client->getObject([
    'Bucket' => 'my-bucket',
    'Key'    => 'large.mov',
    '@http' => [
        'progress' => function ($expectedDl, $dl, $expectedUl, $ul) {
            printf(
                "%s of %s downloaded, %s of %s uploaded.\n",
                $expectedDl,
                $dl,
                $expectedUl,
                $ul
            );
        }
    ]
]);
```

### proxy<a name="http-proxy"></a>

Type  
 `string|array` 

You can connect to an AWS service through a proxy by using the `proxy` option\.
+ Provide a string value to connect to a proxy for all types of URIs\. The proxy string value can contain a scheme, user name, and password\. For example, `"http://username:password@192.168.16.1:10"`\.
+ Provide an associative array of proxy settings where the key is the scheme of the URI, and the value is the proxy for the given URI \(i\.e\., you can give different proxies for “http” and “https” endpoints\)\.

```
use Aws\DynamoDb\DynamoDbClient;

// Send requests through a single proxy
$client = new DynamoDbClient([
    'region'  => 'us-west-2',
    'version' => 'latest',
    'http'    => [
        'proxy' => 'http://192.168.16.1:10'
    ]
]);

// Send requests through a different proxy per scheme
$client = new DynamoDbClient([
    'region'  => 'us-west-2',
    'version' => 'latest',
    'http'    => [
        'proxy' => [
            'http' => 'tcp://192.168.16.1:10',
            'https' => 'tcp://192.168.16.1:11',
        ]
    ]
]);
```

You can use the `HTTP_PROXY` environment variable to configure an “http” protocol\-specific proxy, and the `HTTPS_PROXY` environment variable to configure an “https” specific proxy\.

### sink<a name="http-sink"></a>

Type  
 `resource|string|Psr\Http\Message\StreamInterface` 

The `sink` option controls where the response data of an operation is downloaded to\.
+ Provide a `resource` as returned by `fopen` to download the response body to a PHP stream\.
+ Provide the path to a file on disk as a `string` value to download the response body to a specific file on disk\.
+ Provide a `Psr\Http\Message\StreamInterface` to download the response body to a specific PSR stream object\.

**Note**  
The SDK downloads the response body to a PHP temp stream by default\. This means that the data stays in memory until the size of the body reaches 2 MB, at which point the data is written to a temporary file on disk\.

### synchronous<a name="http-sync"></a>

Type  
 `bool` 

The `synchronous` option informs the underlying HTTP handler that you intend to block the result\.

### stream<a name="http-stream"></a>

Type  
 `bool` 

Set to `true` to tell the underlying HTTP handler that you want to stream the response body of a response from the web service, rather than download it all up front\. For example, this option is relied on in the Amazon S3 stream wrapper class to ensure that the data is streamed\.

### timeout<a name="http-timeout"></a>

Type  
 `float` 

A float describing the timeout of the request in seconds\. Use `0` to wait indefinitely \(the default behavior\)\.

```
use Aws\DynamoDb\DynamoDbClient;

// Timeout after 5 seconds
$client = new DynamoDbClient([
    'region'  => 'us-west-2',
    'version' => 'latest',
    'http'    => [
        'timeout' => 5
    ]
]);
```

### verify<a name="http-verify"></a>

Type  
 `bool|string` 

You can customize the peer SSL/TLS certificate verification behavior of the SDK using the `verify` `http` option\.
+ Set to `true` to enable SSL/TLS peer certificate verification and use the default CA bundle provided by the operating system\.
+ Set to `false` to disable peer certificate verification\. \(This is not secure\!\)
+ Set to a string to provide the path to a CA cert bundle to enable verification using a custom CA bundle\.

If the CA bundle cannot be found for your system and you receive an error, provide the path to a CA bundle to the SDK\. If you do not need a specific CA bundle, Mozilla provides a commonly used CA bundle which you can download [here](https://raw.githubusercontent.com/bagder/ca-bundle/master/ca-bundle.crt) \(this is maintained by the maintainer of cURL\)\. Once you have a CA bundle available on disk, you can set the `openssl.cafile` PHP \.ini setting to point to the path to the file, allowing you to omit the `verify` request option\. You can find much more detail on SSL certificates on the [cURL website](http://curl.haxx.se/docs/sslcerts.html)\.

```
use Aws\DynamoDb\DynamoDbClient;

// Use a custom CA bundle
$client = new DynamoDbClient([
    'region'  => 'us-west-2',
    'version' => 'latest',
    'http'    => [
        'verify' => '/path/to/my/cert.pem'
    ]
]);

// Disable SSL/TLS verification
$client = new DynamoDbClient([
    'region'  => 'us-west-2',
    'version' => 'latest',
    'http'    => [
        'verify' => false
    ]
]);
```

## http\_handler<a name="http-handler"></a>

Type  
 `callable` 

The `http_handler` option is used to integrate the SDK with other HTTP clients\. An `http_handler` option is a function that accepts a `Psr\Http\Message\RequestInterface` object and an array of `http` options applied to the command, and returns a `GuzzleHttp\Promise\PromiseInterface` object that is fulfilled with a `Psr\Http\Message\ResponseInterface` object or rejected with an array of the following exception data:
+  `exception` \- \(`\Exception`\) the exception that was encountered\.
+  `response` \- \(`Psr\Http\Message\ResponseInterface`\) the response that was received \(if any\)\.
+  `connection_error` \- \(bool\) set to `true` to mark the error as a connection error\. Setting this value to `true` also allows the SDK to automatically retry the operation, if needed\.

The SDK automatically converts the given `http_handler` into a normal `handler` option by wrapping the provided `http_handler` with a `Aws\WrappedHttpHandler` object\.

By default, the SDK uses Guzzle as its HTTP handler\. You can supply a different HTTP handler here, or provide a Guzzle client with your own custom defined options\.

 **Setting TLS version** 

One use case is to set the TLS version used by Guzzle with Curl, assuming Curl is installed in your environment\. Note the Curl [version constraints](https://curl.haxx.se/libcurl/c/CURLOPT_SSLVERSION.html) for what version of TLS is supported\. By default, the latest version is used\. If the TLS version is explicitly set, and the remote server doesn’t support this version, it will produce an error instead of using an earlier TLS version\.

You can determine the TLS version being used for a given client operation by setting the `debug` client option to true and examining the SSL connection output\. That line might look something like: `SSL connection using TLSv1.2` 

Example setting TLS 1\.2 with Guzzle 6:

```
use Aws\DynamoDb\DynamoDbClient;
use Aws\Handler\GuzzleV6\GuzzleHandler;
use GuzzleHttp\Client;

$handler = new GuzzleHandler(
    new Client([
        'curl' => [
            CURLOPT_SSLVERSION => CURL_SSLVERSION_TLSv1_2
        ]
    ])
);

$client = new DynamoDbClient([
    'region'  => 'us-west-2',
    'version' => 'latest',
    'http_handler' => $handler
]);
```

**Note**  
The `http_handler` option supersedes any provided `handler` option\.

## profile<a name="profile"></a>

Type  
 `string` 

Enables you to specify which profile to use when credentials are created from the AWS credentials file in your HOME directory\. This setting overrides the `AWS_PROFILE` environment variable\.

**Note**  
Specifying “profile” will cause the “credentials” key to be ignored\.

```
// Use the "production" profile from your credentials file
$ec2 = new Aws\Ec2\Ec2Client([
    'version' => '2014-10-01',
    'region'  => 'us-west-2',
    'profile' => 'production'
]);
```

See [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md) for more information about configuring credentials and the \.ini file format\.

## region<a name="cfg-region"></a>

Type  
 `string` 

Required  
true

AWS Region to connect to\. See the [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) for a list of available Regions\.

```
// Set the Region to the EU (Frankfurt) Region
$s3 = new Aws\S3\S3Client([
    'region'  => 'eu-central-1',
    'version' => '2006-03-01'
]);
```

## retries<a name="config-retries"></a>

Type  
 `int|array|Aws\CacheInterface|Aws\Retry\ConfigurationInterface|callable` 

Default  
 `int(3)` 

Configures the retry mode and maximum number of allowed retries for a client\. Pass `0` to disable retries\.

The three retry modes are: \* `legacy` \- the default legacy retry implementation \* `standard` \- adds a retry quota system to prevent retries that are unlikely

to succeed
+  `adaptive` \- builds on the standard mode, adding a client\-side rate limiter\. Note this mode is considered experimental\.

The configuration for retries consists of the mode and the max attempts to be used for each request\. The configuration can be set in a couple of different locations, in the following order of precedence\.

 **Order of Precedence** 

The order of precedence for retry configuration is as follows \(1 overrides 2\-3, etc\.\):

1. Client configuration option

1. Environment variables

1. AWS Shared config file

 **Environment variables** 
+  `AWS_RETRY_MODE` \- set to `legacy`, `standard`, or `adaptive`\.
+  `AWS_MAX_ATTEMPTS` \- set to an integer value for the max attempts per request

 **Shared config file keys** 
+  `retry_mode` \- set to `legacy`, `standard`, or `adaptive`\.
+  `max_attempts` \- set to an integer value for the max attempts per request

 **Client configuration** 

The following example disables retries for the Amazon DynamoDB client\.

```
// Disable retries by setting "retries" to 0
$client = new Aws\DynamoDb\DynamoDbClient([
    'version' => '2012-08-10',
    'region'  => 'us-west-2',
    'retries' => 0
]);
```

The following example passes in an integer, which will default to `legacy` mode with the passed in number of retries

```
// Disable retries by setting "retries" to 0
$client = new Aws\DynamoDb\DynamoDbClient([
    'version' => '2012-08-10',
    'region'  => 'us-west-2',
    'retries' => 6
]);
```

**The `Aws\Retry\Configuration` object accepts two parameters, the retry mode**  
and an integer for the maximum attempts per request\. This example passes in an

 `Aws\Retry\Configuration` object for retry configuration\.

```
use Aws\EndpointDiscovery\Configuration;
use Aws\S3\S3Client;

$enabled = true;
$cache_limit = 1000;

$config = new Aws\Retry\Configuration('adaptive', 10);

$s3 = new Aws\S3\S3Client([
    'version' => 'latest',
    'region' => 'us-east-2',
    'retries' => $config,
]);
```

This example passes in an array for retry configuration\.

```
use Aws\S3\S3Client;

$s3 = new S3Client([
    'version'     => 'latest',
    'region'      => 'us-west-2',
    'retries' => [
        'mode' => 'standard,
        'max_attempts' => 7
    ],
]);
```

This examples passes an instance of `Aws\CacheInterface` to cache the values returned by the default retry configuration provider\.

```
use Aws\DoctrineCacheAdapter;
use Aws\S3\S3Client;
use Doctrine\Common\Cache\ApcuCache;

$s3 = new S3Client([
    'version'     => 'latest',
    'region'      => 'us-west-2',
    'endpoint_discovery' => new DoctrineCacheAdapter(new ApcuCache),
]);
```

## scheme<a name="scheme"></a>

Type  
 `string` 

Default  
 `string(5) "https"` 

URI scheme to use when connecting\. The SDK uses “https” endpoints \(i\.e\., uses SSL/TLS connections\) by default\. You can attempt to connect to a service over an unencrypted “http” endpoint by setting `scheme` to “http”\.

```
$s3 = new Aws\S3\S3Client([
    'version' => '2006-03-01',
    'region'  => 'us-west-2',
    'scheme'  => 'http'
]);
```

See the [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) for a list of endpoints and whether a service supports the `http` scheme\.

## service<a name="service"></a>

Type  
 `string` 

Required  
true

Name of the service to use\. This value is supplied by default when using a client provided by the SDK \(i\.e\., `Aws\S3\S3Client`\)\. This option is useful when testing a service that has not yet been published in the SDK but that you have available on disk\.

## signature\_provider<a name="signature-provider"></a>

Type  
 `callable` 

A callable that accepts a signature version name \(e\.g\., `v4`\), a service name, and AWS Region and returns a `Aws\Signature\SignatureInterface` object or `NULL` if the provider is able to create a signer for the given parameters\. This provider is used to create signers used by the client\.

There are various functions provided by the SDK in the `Aws\Signature\SignatureProvider` class that can be used to create customized signature providers\.

## signature\_version<a name="signature-version"></a>

Type  
 `string` 

A string representing a custom signature version to use with a service \(e\.g\., `v4`, etc\.\)\. Per operation signature version MAY override this requested signature version, if needed\.

The following examples show how to configure an Amazon S3 client to use [signature version 4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html):

```
// Set a preferred signature version
$s3 = new Aws\S3\S3Client([
    'version'           => '2006-03-01',
    'region'            => 'us-west-2',
    'signature_version' => 'v4'
]);
```

**Note**  
The `signature_provider` used by your client MUST be able to create the `signature_version` option you provide\. The default `signature_provider` used by the SDK can create signature objects for “v4” and “anonymous” signature versions\.

## ua\_append<a name="ua-append"></a>

Type  
 `string|string[]` 

Default  
 `[]` 

A string or array of strings that are added to the user\-agent string passed to the HTTP handler\.

## use\_aws\_shared\_config\_files<a name="use-aws-shared-config-files"></a>

Type  
 `bool|array` 

Default  
 `bool(true)` 

Set to false to disable checking for shared config file in ‘\~/\.aws/config’ and ‘\~/\.aws/credentials’\. This will override the AWS\_CONFIG\_FILE environment variable\.

## validate<a name="validate"></a>

Type  
 `bool|array` 

Default  
 `bool(true)` 

Set to `false` to disable client\-side parameter validation\. You might find that turning validation off will slightly improve client performance, but the difference is negligible\.

```
// Disable client-side validation
$s3 = new Aws\S3\S3Client([
    'version'  => '2006-03-01',
    'region'   => 'eu-west-1',
    'validate' => false
]);
```

Set to an associative array of validation options to enable specific validation constraints:
+  `required` \- Validate that required parameters are present \(on by default\)\.
+  `min` \- Validate the minimum length of a value \(on by default\)\.
+  `max` \- Validate the maximum length of a value\.
+  `pattern` \- Validate that the value matches a regular expression\.

```
// Validate only that required values are present
$s3 = new Aws\S3\S3Client([
    'version'  => '2006-03-01',
    'region'   => 'eu-west-1',
    'validate' => ['required' => true]
]);
```

## version<a name="cfg-version"></a>

Type  
 `string` 

Required  
true

The version of the web service to use \(e\.g\., `2006-03-01`\)\.

A “version” configuration value is required\. Specifying a version constraint ensures that your code will not be affected by a breaking change made to the service\. For example, when using Amazon S3, you can lock your API version to `2006-03-01`\.

```
$s3 = new Aws\S3\S3Client([
    'version' => '2006-03-01',
    'region'  => 'us-east-1'
]);
```

A list of available API versions can be found on each client’s [API documentation page](https://docs.aws.amazon.com/aws-sdk-php/v3/api/index.html)\. If you are unable to load a specific API version, you might need to update your copy of the SDK\.

You can provide the string `latest` to the “version” configuration value to use the most recent available API version that your client’s API provider can find \(the default api\_provider scans the `src/data` directory of the SDK for API models\)\.

```
// Use the latest version available
$s3 = new Aws\S3\S3Client([
    'version' => 'latest',
    'region'  => 'us-east-1'
]);
```

**Warning**  
We do not recommend Using `latest` in a production application because pulling in a new minor version of the SDK that includes an API update could break your production application\.