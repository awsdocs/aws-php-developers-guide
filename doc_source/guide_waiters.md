# Waiters in the AWS SDK for PHP Version 3<a name="guide_waiters"></a>

Waiters help make it easier to work with *eventually consistent* systems by providing an abstracted way to wait until a resource enters into a particular state by polling the resource\. You can find a list of the waiters supported by a client by viewing the [API documentation](https://docs.aws.amazon.com/aws-sdk-php/v3/api/index.html) for a single version of a service client\. To navigate there, go to the client’s page in the API documentation and navigate to the specific version number \(represented by a date\) and scroll down to the ‘Waiters’ section\. [This link will bring you to the waiters section of S3\.](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-s3-2006-03-01.html#waiters) 

In the following example, the Amazon S3 client is used to create a bucket\. Then the waiter is used to wait until the bucket exists\.

```
// Create a bucket
$s3Client->createBucket(['Bucket' => 'my-bucket']);

// Wait until the created bucket is available
$s3Client->waitUntil('BucketExists', ['Bucket' => 'my-bucket']);
```

If the waiter has to poll the bucket too many times, it will throw a `\RuntimeException` exception\.

## Waiter Configuration<a name="waiter-configuration"></a>

Waiters are driven by an associative array of configuration options\. All of the options used by a particular waiter have default values, but they can be overridden to support different waiting strategies\.

You can modify waiter configuration options by passing an associative array of `@waiter` options to the `$args` argument of a client’s `waitUntil()` and `getWaiter()` methods\.

```
// Providing custom waiter configuration options to a waiter
$s3Client->waitUntil('BucketExists', [
    'Bucket'  => 'my-bucket',
    '@waiter' => [
        'delay'       => 3,
        'maxAttempts' => 10
    ]
]);
```

**delay \(int\)**  
Number of seconds to delay between polling attempts\. Each waiter has a default `delay` configuration value, but you might need to modify this setting for specific use cases\.

**maxAttempts \(int\)**  
Maximum number of polling attempts to issue before failing the waiter\. This option ensures that you do not wait on a resource indefinitely\. Each waiter has a default `maxAttempts` configuration value, but you might need to modify this setting for specific use cases\.

**initDelay \(int\)**  
Amount of time in seconds to wait before the first polling attempt\. This might be useful when waiting on a resource that you know will take awhile to enter into the desired state\.

**before \(callable\)**  
A PHP callable function that is invoked before each attempt\. The callable is invoked with the `Aws\CommandInterface` command that is about to be executed and the number of attempts that have been executed so far\. Uses of the `before` callable might be to modify commands before they are executed or provide progress information\.  

```
use Aws\CommandInterface;

$s3Client->waitUntil('BucketExists', [
    'Bucket'  => 'my-bucket',
    '@waiter' => [
        'before' => function (CommandInterface $command, $attempts) {
            printf(
                "About to send %s. Attempt %d\n",
                $command->getName(),
                $attempts
            );
        }
    ]
]);
```

## Waiting Asynchronously<a name="async-waiters"></a>

In addition to waiting synchronously, you can invoke a waiter to wait asynchronously while sending other requests or waiting on multiple resources at once\.

You can access a waiter promise by retrieving a waiter from a client using the client’s `getWaiter($name, array $args = [])` method\. Use the `promise()` method of a waiter to initiate the waiter\. A waiter promise is fulfilled with the last `Aws\CommandInterface` that was executed in the waiter, and rejected with a `RuntimeException` on error\.

```
use Aws\CommandInterface;

$waiterName = 'BucketExists';
$waiterOptions = ['Bucket' => 'my-bucket'];

// Create a waiter promise
$waiter = $s3Client->getWaiter($waiterName, $waiterOptions);

// Initiate the waiter and retrieve a promise
$promise = $waiter->promise();

// Call methods when the promise is resolved.
$promise
    ->then(function () {
        echo "Waiter completed\n";
    })
    ->otherwise(function (\Exception $e) {
        echo "Waiter failed: " . $e . "\n";
    });

// Block until the waiter completes or fails. Note that this might throw
// a \RuntimeException if the waiter fails.
$promise->wait();
```

Exposing a promise\-based waiters API allows for some powerful and relatively low overhead use cases\. For example, what if you wanted to wait on multiple resources, and do something with the first waiter that successfully resolved?

```
use Aws\CommandInterface;

// Create an array of waiter promises
$promises = [
    $s3Client->getWaiter('BucketExists', ['Bucket' => 'a'])->promise(),
    $s3Client->getWaiter('BucketExists', ['Bucket' => 'b'])->promise(),
    $s3Client->getWaiter('BucketExists', ['Bucket' => 'c'])->promise()
];

// Initiate a race between the waiters, fulfilling the promise with the
// first waiter to complete (or the first bucket to become available)
$any = Promise\any($promises)
    ->then(function (CommandInterface $command) {
        // This is invoked with the command that succeeded in polling the
        // resource. Here we can know which bucket won the race.
        echo "The {$command['Bucket']} waiter completed first!\n";
    });

// Force the promise to complete
$any->wait();
```