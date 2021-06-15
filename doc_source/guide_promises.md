# Promises in the AWS SDK for PHP Version 3<a name="guide_promises"></a>

The AWS SDK for PHP uses **promises** to allow for asynchronous workflows, and this asynchronicity allows HTTP requests to be sent concurrently\. The promise specification used by the SDK is [Promises/A\+](https://promisesaplus.com/)\.

## What Is a Promise?<a name="what-is-a-promise"></a>

A *promise* represents the eventual result of an asynchronous operation\. The primary way of interacting with a promise is through its `then` method\. This method registers callbacks to receive either a promise’s eventual value or the reason why the promise can’t be fulfilled\.

The AWS SDK for PHP relies on the [guzzlehttp/promises](https://github.com/guzzle/promises) Composer package for its promises implementation\. Guzzle promises support blocking and non\-blocking workflows and can be used with any non\-blocking event loop\.

**Note**  
HTTP requests are sent concurrently in the AWS SDK for PHP using a single thread, in which non\-blocking calls are used to transfer one or more HTTP requests while reacting to state changes \(e\.g\., fulfilling or rejecting promises\)\.

## Promises in the SDK<a name="promises-in-the-sdk"></a>

Promises are used throughout the SDK\. For example, promises are used in most high\-level abstractions provided by the SDK: [paginators](guide_paginators.md#async-paginators), [waiters](guide_waiters.md#async-waiters), [command pools](guide_commands.md#command-pool), [multipart uploads](s3-multipart-upload.md), [S3 directory/bucket transfers](s3-transfer.md), and so on\.

All of the clients that the SDK provides return promises when you invoke any of the `Async` suffixed methods\. For example, the following code shows how to create a promise for getting the results of an Amazon DynamoDB`DescribeTable` operation\.

```
$client = new Aws\DynamoDb\DynamoDbClient([
    'region'  => 'us-west-2',
    'version' => 'latest',
]);

// This will create a promise that will eventually contain a result
$promise = $client->describeTableAsync(['TableName' => 'mytable']);
```

Notice that you can call either `describeTable` or `describeTableAsync`\. These methods are magic `__call` methods on a client that are powered by the API model and `version` number associated with the client\. By calling methods like `describeTable` without the `Async` suffix, the client will block while it sends an HTTP request and either return an `Aws\ResultInterface` object or throw an `Aws\Exception\AwsException`\. By suffixing the operation name with `Async` \(i\.e\., `describeTableAsync`\) the client will create a promise that is eventually fulfilled with an `Aws\ResultInterface` object or rejected with an `Aws\Exception\AwsException`\.

**Important**  
When the promise is returned, the result might have already arrived \(for example, when using a mock handler\), or the HTTP request might not have been initiated\.

You can register a callback with the promise by using the `then` method\. This method accepts two callbacks, `$onFulfilled` and `$onRejected`, both of which are optional\. The `$onFulfilled` callback is invoked if the promise is fulfilled, and the `$onRejected` callback is invoked if the promise is rejected \(meaning it failed\)\.

```
$promise->then(
    function ($value) {
        echo "The promise was fulfilled with {$value}";
    },
    function ($reason) {
        echo "The promise was rejected with {$reason}";
    }
);
```

### Executing Commands Concurrently<a name="executing-commands-concurrently"></a>

Multiple promises can be composed together such that they are executed concurrently\. This can be achieved by integrating the SDK with a non\-blocking event loop, or by building up multiple promises and waiting on them to complete concurrently\.

```
use GuzzleHttp\Promise;

$sdk = new Aws\Sdk([
    'version' => 'latest',
    'region'  => 'us-west-2'
]);

$s3 = $sdk->createS3();
$ddb = $sdk->createDynamoDb();

$promises = [
    'buckets' => $s3->listBucketsAsync(),
    'tables'  => $ddb->listTablesAsync(),
];

// Wait on both promises to complete and return the results
$results = Promise\unwrap($promises);

// Notice that this method will maintain the input array keys
var_dump($results['buckets']->toArray());
var_dump($results['tables']->toArray());
```

**Note**  
The [CommandPool](guide_commands.md#command-pool) provides a more powerful mechanism for executing multiple API operations concurrently\.

## Chaining Promises<a name="chaining-promises"></a>

One of the best aspects of promises is that they are composable, allowing you to create transformation pipelines\. Promises are composed by chaining `then` callbacks with subsequent `then` callbacks\. The return value of a `then` method is a promise that is fulfilled or rejected based on the result of the provided callbacks\.

```
$promise = $client->describeTableAsync(['TableName' => 'mytable']);

$promise
    ->then(
        function ($value) {
            $value['AddedAttribute'] = 'foo';
            return $value;
        },
        function ($reason) use ($client) {
            // The call failed. You can recover from the error here and
            // return a value that will be provided to the next successful
            // then() callback. Let's retry the call.
            return $client->describeTableAsync(['TableName' => 'mytable']);
        }
    )->then(
        function ($value) {
            // This is only invoked when the previous then callback is
            // fulfilled. If the previous callback returned a promise, then
            // this callback is invoked only after that promise is
            // fulfilled.
            echo $value['AddedAttribute']; // outputs "foo"
        },
        function ($reason) {
            // The previous callback was rejected (failed).
        }
    );
```

**Note**  
The return value of a promise callback is the `$value` argument that is supplied to downstream promises\. If you want to provide a value to downstream promise chains, you must return a value in the callback function\.

### Rejection Forwarding<a name="rejection-forwarding"></a>

You can register a callback to invoke when a promise is rejected\. If an exception is thrown in any callback, the promise is rejected with the exception and the next promises in the chain are rejected with the exception\. If you return a value successfully from an `$onRejected` callback, the next promises in the promise chain is fulfilled with the return value from the `$onRejected` callback\.

## Waiting on Promises<a name="waiting-on-promises"></a>

You can synchronously force promises to complete by using a promise’s `wait` method\.

```
$promise = $client->listTablesAsync();
$result = $promise->wait();
```

If an exception is encountered while invoking the `wait` function of a promise, the promise is rejected with the exception and the exception is thrown\.

```
use Aws\Exception\AwsException;

$promise = $client->listTablesAsync();

try {
    $result = $promise->wait();
} catch (AwsException $e) {
    // Handle the error
}
```

Calling `wait` on a promise that has been fulfilled doesn’t trigger the wait function\. It simply returns the previously delivered value\.

```
$promise = $client->listTablesAsync();
$result = $promise->wait();
assert($result ### $promise->wait());
```

Calling `wait` on a promise that has been rejected throws an exception\. If the rejection reason is an instance of `\Exception` the reason is thrown\. Otherwise, a `GuzzleHttp\Promise\RejectionException` is thrown and the reason can be obtained by calling the `getReason` method of the exception\.

**Note**  
API operation calls in the AWS SDK for PHP are rejected with subclasses of the `Aws\Exception\AwsException` class\. However, it’s possible that the reason delivered to a `then` method is different because the addition of a custom middleware that alters a rejection reason\.

## Canceling Promises<a name="canceling-promises"></a>

Promises can be canceled using the `cancel()` method of a promise\. If a promise has already been resolved, calling `cancel()` will have no effect\. Canceling a promise cancels the promise and any promises that are awaiting delivery from the promise\. A canceled promise is rejected with a `GuzzleHttp\Promise\RejectionException`\.

## Combining Promises<a name="combining-promises"></a>

You can combine promises into aggregate promises to build more sophisticated workflows\. The `guzzlehttp/promise` package contains various functions that you can use to combine promises\.

You can find the API documentation for all of the promise collection functions at [namespace\-GuzzleHttp\.Promise](https://docs.aws.amazon.com/aws-sdk-php/v3/api/namespace-GuzzleHttp.Promise.html)\.

### each and each\_limit<a name="each-and-each-limit"></a>

Use the [CommandPool](guide_commands.md#command-pool) when you have a task queue of `Aws\CommandInterface` commands to perform concurrently with a fixed pool size \(the commands can be in memory or yielded by a lazy iterator\)\. The `CommandPool` ensures that a fixed number of commands are sent concurrently until the supplied iterator is exhausted\.

The `CommandPool` works only with commands that are executed by the same client\. You can use the `GuzzleHttp\Promise\each_limit` function to perform send commands of different clients concurrently using a fixed pool size\.

```
use GuzzleHttp\Promise;

$sdk = new Aws\Sdk([
    'version' => 'latest',
    'region'  => 'us-west-2'
]);

$s3 = $sdk->createS3();
$ddb = $sdk->createDynamoDb();

// Create a generator that yields promises
$promiseGenerator = function () use ($s3, $ddb) {
    yield $s3->listBucketsAsync();
    yield $ddb->listTablesAsync();
    // yield other promises as needed...
};

// Execute the tasks yielded by the generator concurrently while limiting the
// maximum number of concurrent promises to 5
$promise = Promise\each_limit($promiseGenerator(), 5);

// Waiting on an EachPromise will wait on the entire task queue to complete
$promise->wait();
```

### Promise Coroutines<a name="promise-coroutines"></a>

One of the more powerful features of the Guzzle promises library is that it allows you to use promise coroutines that make writing asynchronous workflows seem more like writing traditional synchronous workflows\. In fact, the AWS SDK for PHP uses coroutine promises in most of the high\-level abstractions\.

Imagine you wanted to create several buckets and upload a file to the bucket when the bucket becomes available, and you’d like to do this all concurrently so that it happens as fast as possible\. You can do this easily by combining multiple coroutine promises together using the `all()` promise function\.

```
use GuzzleHttp\Promise;

$uploadFn = function ($bucket) use ($s3Client) {
    return Promise\coroutine(function () use ($bucket, $s3Client) {
        // You can capture the result by yielding inside of parens
        $result = (yield $s3Client->createBucket(['Bucket' => $bucket]));
        // Wait on the bucket to be available
        $waiter = $s3Client->getWaiter('BucketExists', ['Bucket' => $bucket]);
        // Wait until the bucket exists
        yield $waiter->promise();
        // Upload a file to the bucket
        yield $s3Client->putObjectAsync([
            'Bucket' => $bucket,
            'Key'    => '_placeholder',
            'Body'   => 'Hi!'
        ]);
    });
};

// Create the following buckets
$buckets = ['foo', 'baz', 'bar'];
$promises = [];

// Build an array of promises
foreach ($buckets as $bucket) {
    $promises[] = $uploadFn($bucket);
}

// Aggregate the promises into a single "all" promise
$aggregate = Promise\all($promises);

// You can then() off of this promise or synchronously wait
$aggregate->wait();
```