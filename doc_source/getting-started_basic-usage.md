# Basic Usage Patterns of the AWS SDK for PHP Version 3<a name="getting-started_basic-usage"></a>

This topic focuses on basic usage patterns of the AWS SDK for PHP\.

## Prerequisites<a name="prerequisites"></a>
+  [Download and installed the SDK](getting-started_installation.md) 
+ Retrieve your [AWS access keys](https://aws.amazon.com/developers/access-keys/)\.

## Including the SDK in Your Code<a name="including-the-sdk-in-your-code"></a>

No matter which technique you used to install the SDK, you can include the SDK in your code with just a single `require` statement\. See the following table for the PHP code that best fits your installation technique\. Replace any instances of `/path/to/` with the actual path on your system\.


****  

| Installation Technique | Require Statement | 
| --- | --- | 
|  Using Composer  |   `require '/path/to/vendor/autoload.php';`   | 
|  Using the phar  |   `require '/path/to/aws.phar';`   | 
|  Using the ZIP  |   `require '/path/to/aws-autoloader.php';`   | 

In this topic, we show examples that assume the Composer installation method\. If you’re using a different installation method, you can refer back to this section to find the correct `require` code to use\.

## Usage Summary<a name="usage-summary"></a>

To use the SDK to interact with an AWS service, instantiate a **Client** object\. Client objects have methods that correspond one to one with operations in the service’s API\. To execute a particular operation, you call its corresponding method\. This method either returns an array\-like **Result** object on success, or throws an **Exception** on failure\.

## Creating a Client<a name="creating-a-client"></a>

You can create a client by passing an associative array of options to a client’s constructor\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\S3\S3Client;
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
//Create an S3Client
$s3 = new Aws\S3\S3Client([
    'version' => 'latest',
    'region' => 'us-east-2'
]);
```

Notice that we did **not** explicitly provide credentials to the client\. That’s because the SDK should detect the credentials from [environment variables](guide_credentials_environment.md) \(via `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`\), an [AWS credentials INI file](guide_credentials_profiles.md) in your HOME directory, AWS Identity and Access Management \(IAM\) [instance profile credentials](guide_credentials_assume_role.md), or [credential providers](guide_credentials_provider.md)\.

All of the general client configuration options are described in detail in the [configuration guide](guide_configuration.md)\. The array of options provided to a client can vary based on which client you’re creating\. These custom client configuration options are described in the [API documentation](https://docs.aws.amazon.com/aws-sdk-php/latest/) for each client\.

## Using the Sdk Class<a name="sdk-class"></a>

The `Aws\Sdk` class acts as a client factory and is used to manage shared configuration options across multiple clients\. The same options that can be provided to a specific client constructor can also be supplied to the `Aws\Sdk` class\. These options are then applied to each client constructor\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\S3\S3Client;
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
// The same options that can be provided to a specific client constructor can also be supplied to the Aws\Sdk class.
// Use the us-west-2 region and latest version of each client.
$sharedConfig = [
    'region' => 'us-west-2',
    'version' => 'latest'
];
// Create an SDK class used to share configuration across clients.
$sdk = new Aws\Sdk($sharedConfig);
// Create an Amazon S3 client using the shared configuration data.
$client = $sdk->createS3();
```

Options that are shared across all clients are placed in root\-level key\-value pairs\. Service\-specific configuration data can be provided in a key that is the same as the namespace of a service \(e\.g\., “S3”, “DynamoDb”, etc\.\)\.

```
$sdk = new Aws\Sdk([
    'region'   => 'us-west-2',
    'version'  => 'latest',
    'DynamoDb' => [
        'region' => 'eu-central-1'
    ]
]);

// Creating an Amazon DynamoDb client will use the "eu-central-1" AWS Region
$client = $sdk->createDynamoDb();
```

Service\-specific configuration values are a union of the service\-specific values and the root\-level values \(i\.e\., service\-specific values are shallow\-merged onto root\-level values\)\.

**Note**  
We highly recommended that you use the `Sdk` class to create clients if you’re using multiple client instances in your application\. The `Sdk` class automatically uses the same HTTP client for each SDK client, allowing SDK clients for different services to perform nonblocking HTTP requests\. If the SDK clients don’t use the same HTTP client, then HTTP requests sent by the SDK client might block promise orchestration between services\.

## Executing Service Operations<a name="executing-service-operations"></a>

You can execute a service operation by calling the method of the same name on a client object\. For example, to perform the Amazon S3 [PutObject operation](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectPUT.html), you must call the `Aws\S3\S3Client::putObject()` method\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\S3\S3Client;
```

 **Sample Code** 

```
// Use the us-east-2 region and latest version of each client.
$sharedConfig = [
    'profile' => 'default',
    'region' => 'us-east-2',
    'version' => 'latest'
];

// Create an SDK class used to share configuration across clients.
$sdk = new Aws\Sdk($sharedConfig);

// Use an Aws\Sdk class to create the S3Client object.
$s3Client = $sdk->createS3();

// Send a PutObject request and get the result object.
$result = $s3Client->putObject([
    'Bucket' => 'my-bucket',
    'Key' => 'my-key',
    'Body' => 'this is the body!'
]);

// Download the contents of the object.
$result = $s3Client->getObject([
    'Bucket' => 'my-bucket',
    'Key' => 'my-key'
]);

// Print the body of the result by indexing into the result object.
echo $result['Body'];
```

Operations available to a client and the structure of the input and output are defined at runtime based on a service description file\. When creating a client, you must provide a version \(e\.g\., *“2006\-03\-01”* or *“latest”*\)\. The SDK finds the corresponding configuration file based on the provided version\.

Operation methods like `putObject()` all accept a single argument, an associative array that represents the parameters of the operation\. The structure of this array \(and the structure of the result object\) is defined for each operation in the SDK’s API Documentation \(e\.g\., see the API docs for [putObject operation](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-s3-2006-03-01.html#putobject)\)\.

### HTTP Handler Options<a name="http-handler-options"></a>

You can also fine\-tune how the underlying HTTP handler executes the request by using the special `@http` parameter\. The options you can include in the `@http` parameter are the same as the ones you can set when you instantiate the client with the [“http” client option](guide_configuration.md#config-http)\.

```
// Send the request through a proxy
$result = $s3Client->putObject([
    'Bucket' => 'my-bucket',
    'Key'    => 'my-key',
    'Body'   => 'this is the body!',
    '@http'  => [
        'proxy' => 'http://192.168.16.1:10'
    ]
]);
```

## Asynchronous Requests<a name="asynchronous-requests"></a>

You can send commands concurrently using the asynchronous features of the SDK\. You can send requests asynchronously by suffixing an operation name with `Async`\. This initiates the request and returns a promise\. The promise is fulfilled with the result object on success or rejected with an exception on failure\. This enables you to create multiple promises and have them send HTTP requests concurrently when the underlying HTTP handler transfers the requests\.

 **Imports** 

```
require 'vendor/autoload.php';
use Aws\S3\S3Client;
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
// Create an SDK class used to share configuration across clients.
$sdk = new Aws\Sdk([
    'region'   => 'us-west-2',
    'version'  => 'latest'
]);
// Use an Aws\Sdk class to create the S3Client object.
$s3Client = $sdk->createS3();
//Listing all S3 Bucket
$CompleteSynchronously = $s3Client->listBucketsAsync();
// Block until the result is ready.
$CompleteSynchronously = $CompleteSynchronously->wait();
```

You can force a promise to complete synchronously by using the `wait` method of the promise\. Forcing the promise to complete also “unwraps” the state of the promise by default, meaning it will either return the result of the promise or throw the exception that was encountered\. When calling `wait()` on a promise, the process blocks until the HTTP request is completed and the result is populated or an exception is thrown\.

When using the SDK with an event loop library, don’t block on results\. Instead, use the `then()` method of a result to access a promise that is resolved or rejected when the operation completes\.

 **Imports** 

```
require 'vendor/autoload.php';
use Aws\S3\S3Client;
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
// Create an SDK class used to share configuration across clients.
$sdk = new Aws\Sdk([
    'region'   => 'us-west-2',
    'version'  => 'latest'
]);
// Use an Aws\Sdk class to create the S3Client object.
$s3Client = $sdk->createS3();
```

```
$promise = $s3Client->listBucketsAsync();
$promise
    ->then(function ($result) {
        echo 'Got a result: ' . var_export($result, true);
    })
    ->otherwise(function ($reason) {
        echo 'Encountered an error: ' . $reason->getMessage();
    });
}
```

## Working with Result Objects<a name="result-objects"></a>

Executing a successful operation returns an `Aws\Result` object\. Instead of returning the raw XML or JSON data of a service, the SDK coerces the response data into an associative array structure\. It normalizes some aspects of the data based on its knowledge of the specific service and the underlying response structure\.

You can access data from the AWSResult object like an associative PHP array\.

 **Imports** 

```
require 'vendor/autoload.php';
use Aws\S3\S3Client;
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
// Use the us-east-2 region and latest version of each client.
$sharedConfig = [
    'profile' => 'default',
    'region' => 'us-east-2',
    'version' => 'latest'
];

// Create an SDK class used to share configuration across clients.
$sdk = new Aws\Sdk($sharedConfig);

// Use an Aws\Sdk class to create the S3Client object.
$s3 = $sdk->createS3();
$result = $s3->listBuckets();
foreach ($result['Buckets'] as $bucket) {
    echo $bucket['Name'] . "\n";
}

// Convert the result object to a PHP array
$array = $result->toArray();
```

The contents of the result object depend on the operation that was executed and the version of a service\. The result structure of each API operation is documented in the API docs for each operation\.

The SDK is integrated with [JMESPath](http://jmespath.org/), a [DSL](http://en.wikipedia.org/wiki/Domain-specific_language) used to search and manipulate JSON data or, in our case, PHP arrays\. The result object contains a `search()` method you can use to more declaratively extract data from the result\.

 **Sample Code** 

```
$s3 = $sdk->createS3();
$result = $s3->listBuckets();
```

```
$names = $result->search('Buckets[].Name');
```

## Handling Errors<a name="handling-errors"></a>

### Synchronous Error Handling<a name="synchronous-error-handling"></a>

If an error occurs while performing an operation, an exception is thrown\. For this reason, if you need to handle errors in your code, use `try`/`catch` blocks around your operations\. The SDK throws service\-specific exceptions when an error occurs\.

The following example uses the `Aws\S3\S3Client`\. If there is an error, the exception thrown will be of the type `Aws\S3\Exception\S3Exception`\. All service\-specific exceptions that the SDK throws extend from the `Aws\Exception\AwsException` class\. This class contains useful information about the failure, including the request\-id, error code, and error type\. Note for some services which support it, response data is coerced into an associative array structure \(similar to `Aws\Result` objects\), which can be accessed like a normal PHP associative array\. The `toArray()` method will return any such data, if it exists\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\S3\S3Client;
use Aws\Exception\AwsException;
use Aws\S3\Exception\S3Exception;
```

 **Sample Code** 

```
// Create an SDK class used to share configuration across clients.
$sdk = new Aws\Sdk([
    'region'   => 'us-west-2',
    'version'  => 'latest'
]);

// Use an Aws\Sdk class to create the S3Client object.
$s3Client = $sdk->createS3();

try {
    $s3Client->createBucket(['Bucket' => 'my-bucket']);
} catch (S3Exception $e) {
    // Catch an S3 specific exception.
    echo $e->getMessage();
} catch (AwsException $e) {
    // This catches the more generic AwsException. You can grab information
    // from the exception using methods of the exception object.
    echo $e->getAwsRequestId() . "\n";
    echo $e->getAwsErrorType() . "\n";
    echo $e->getAwsErrorCode() . "\n";

    // This dumps any modeled response data, if supported by the service
    // Specific members can be accessed directly (e.g. $e['MemberName'])
    var_dump($e->toArray());
}
```

### Asynchronous Error Handling<a name="asynchronous-error-handling"></a>

Exceptions are not thrown when sending asynchronous requests\. Instead, you must use the `then()` or `otherwise()` method of the returned promise to receive the result or error\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\S3\S3Client;
use Aws\Exception\AwsException;
use Aws\S3\Exception\S3Exception;
```

 **Sample Code** 

```
//Asynchronous Error Handling
$promise = $s3Client->createBucketAsync(['Bucket' => 'my-bucket']);
$promise->otherwise(function ($reason) {
    var_dump($reason);
});

// This does the same thing as the "otherwise" function.
$promise->then(null, function ($reason) {
    var_dump($reason);
});
```

You can “unwrap” the promise and cause the exception to be thrown instead\.

 **Imports** 

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\S3\S3Client;
use Aws\Exception\AwsException;
use Aws\S3\Exception\S3Exception;
```

 **Sample Code** 

```
$promise = $s3Client->createBucketAsync(['Bucket' => 'my-bucket']);
```

```
//throw exception
try {
    $result = $promise->wait();
} catch (S3Exception $e) {
    echo $e->getMessage();
}
```