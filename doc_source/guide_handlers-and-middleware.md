# Handlers and Middleware in the AWS SDK for PHP Version 3<a name="guide_handlers-and-middleware"></a>

The primary mechanism for extending the AWS SDK for PHP is through **handlers** and **middleware**\. Each SDK client class owns an `Aws\HandlerList` instance that is accessible through the `getHandlerList()` method of a client\. You can retrieve a client’s `HandlerList` and modify it to add or remove client behavior\.

## Handlers<a name="handlers"></a>

A handler is a function that performs the actual transformation of a command and request into a result\. A handler typically sends HTTP requests\. Handlers can be composed with middleware to augment their behavior\. A handler is a function that accepts an `Aws\CommandInterface` and a `Psr\Http\Message\RequestInterface` and returns a promise that is fulfilled with an `Aws\ResultInterface` or rejected with an `Aws\Exception\AwsException` reason\.

Here’s a handler that returns the same mock result for each call\.

```
use Aws\CommandInterface;
use Aws\Result;
use Psr\Http\Message\RequestInterface;
use GuzzleHttp\Promise;

$myHandler = function (CommandInterface $cmd, RequestInterface $request) {
    $result = new Result(['foo' => 'bar']);
    return Promise\promise_for($result);
};
```

You can then use this handler with an SDK client by providing a `handler` option in the constructor of a client\.

```
// Set the handler of the client in the constructor
$s3 = new Aws\S3\S3Client([
    'region'  => 'us-east-1',
    'version' => '2006-03-01',
    'handler' => $myHandler
]);
```

You can also change the handler of a client after it is constructed using the `setHandler` method of an `Aws\ClientInterface`\.

```
// Set the handler of the client after it is constructed
$s3->getHandlerList()->setHandler($myHandler);
```

### Mock Handler<a name="mock-handler"></a>

We recommend using the `MockHandler` when writing tests that use the SDK\. You can use the `Aws\MockHandler` to return mocked results or throw mock exceptions\. You enqueue results or exceptions, and the MockHandler dequeues them in FIFO order\.

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

## Middleware<a name="middleware"></a>

Middleware is a special type of high\-level function that augments the behavior of transferring a command, and delegates to a “next” handler\. Middleware functions accept an `Aws\CommandInterface` and a `Psr\Http\Message\RequestInterface` and return a promise that is fulfilled with an `Aws\ResultInterface` or rejected with an `Aws\Exception\AwsException` reason\.

A middleware is a higher\-order function that modifies a command, request, or result as it passes through the middleware\. A middleware takes the following form\.

```
use Aws\CommandInterface;
use Psr\Http\Message\RequestInterface;

$middleware = function () {
    return function (callable $handler) use ($fn) {
        return function (
            CommandInterface $command,
            RequestInterface $request = null
        ) use ($handler, $fn) {
            // Do something before calling the next handler
            // ...
            $promise = $fn($command, $request);
            // Do something in the promise after calling the next handler
            // ...
            return $promise;
        };
    };
};
```

A middleware receives a command to execute and an optional request object\. The middleware can choose to augment the request and command or leave them as\-is\. A middleware then invokes the next handle in the chain or can choose to short\-circuit the next handler and return a promise\. The promise that is created by invoking the next handler can then be augmented using the `then` method of the promise to modify the eventual result or error before returning the promise back up the stack of middleware\.

### HandlerList<a name="handlerlist"></a>

The SDK uses an `Aws\HandlerList` to manage the middleware and handlers used when executing a command\. Each SDK client owns a `HandlerList`, and this `HandlerList` is cloned and added to each command that a client creates\. You can attach a middleware and default handler to use for each command created by a client by adding a middleware to the client’s `HandlerList`\. You can add and remove middleware from specific commands by modifying the `HandlerList` owned by a specific command\.

A `HandlerList` represents a stack of middleware that are used to wrap a **handler**\. To help manage the list of middleware and the order in which they wrap a handler, the `HandlerList` breaks the middleware stack into named steps that represents part of the lifecycle of transferring a command:

1.  `init` \- Add default parameters

1.  `validate` \- Validate required parameters

1.  `build` \- Serialize an HTTP request for sending

1.  `sign` \- Sign the serialized HTTP request

1. <handler> \(not a step, but performs the actual transfer\)

**init**  
This lifecycle step represents the initialization of a command, and a request has not yet been serialized\. This step is typically used to add default parameters to a command\.  
You can add a middleware to the `init` step using the `appendInit` and `prependInit` methods, where `appendInit` adds the middleware to the end of the `prepend` list while `prependInit` adds the middleware to the front of the `prepend` list\.  

```
use Aws\Middleware;

$middleware = Middleware::tap(function ($cmd, $req) {
    // Observe the step
});

// Append to the end of the step with a custom name
$client->getHandlerList()->appendInit($middleware, 'custom-name');
// Prepend to the beginning of the step
$client->getHandlerList()->prependInit($middleware, 'custom-name');
```

**validate**  
This lifecycle step is used for validating the input parameters of a command\.  
You can add a middleware to the `validate` step using the `appendValidate` and `prependValidate` methods, where `appendValidate` adds the middleware to the end of the `validate` list while `prependValidate` adds the middleware to the front of the `validate` list\.  

```
use Aws\Middleware;

$middleware = Middleware::tap(function ($cmd, $req) {
    // Observe the step
});

// Append to the end of the step with a custom name
$client->getHandlerList()->appendValidate($middleware, 'custom-name');
// Prepend to the beginning of the step
$client->getHandlerList()->prependValidate($middleware, 'custom-name');
```

**build**  
This lifecycle step is used to serialize an HTTP request for the command being executed\. Downstream lifecycle events will receive a command and PSR\-7 HTTP request\.  
You can add a middleware to the `build` step using the `appendBuild` and `prependBuild` methods, where `appendBuild` adds the middleware to the end of the `build` list while `prependBuild` adds the middleware to the front of the `build` list\.  

```
use Aws\Middleware;

$middleware = Middleware::tap(function ($cmd, $req) {
    // Observe the step
});

// Append to the end of the step with a custom name
$client->getHandlerList()->appendBuild($middleware, 'custom-name');
// Prepend to the beginning of the step
$client->getHandlerList()->prependBuild($middleware, 'custom-name');
```

**sign**  
This lifecycle step is typically used to sign HTTP requests before they are sent over the wire\. You should typically refrain from mutating an HTTP request after it is signed to avoid signature errors\.  
This it the last step in the `HandlerList` before the HTTP request is transferred by a handler\.  
You can add a middleware to the `sign` step using the `appendSign` and `prependSign` methods, where `appendSign` adds the middleware to the end of the `sign` list while `prependSign` adds the middleware to the front of the `sign` list\.  

```
use Aws\Middleware;

$middleware = Middleware::tap(function ($cmd, $req) {
    // Observe the step
});

// Append to the end of the step with a custom name
$client->getHandlerList()->appendSign($middleware, 'custom-name');
// Prepend to the beginning of the step
$client->getHandlerList()->prependSign($middleware, 'custom-name');
```

### Available Middleware<a name="available-middleware"></a>

The SDK provides several middleware that you can use to augment the behavior of a client or to observe the execution of a command\.

#### mapCommand<a name="map-command"></a>

The `Aws\Middleware::mapCommand` middleware is useful when you need to modify a command before the command is serialized as an HTTP request\. For example, `mapCommand` could be used to perform validation or add default parameters\. The `mapCommand` function accepts a callable that accepts an `Aws\CommandInterface` object and returns an `Aws\CommandInterface` object\.

```
use Aws\Middleware;
use Aws\CommandInterface;

// Here we've omitted the require Bucket parameter. We'll add it in the
// custom middleware.
$command = $s3Client->getCommand('HeadObject', ['Key' => 'test']);

// Apply a custom middleware named "add-param" to the "init" lifecycle step
$command->getHandlerList()->appendInit(
    Middleware::mapCommand(function (CommandInterface $command) {
        $command['Bucket'] = 'mybucket';
        // Be sure to return the command!
        return $command;
    }),
    'add-param'
);
```

#### mapRequest<a name="map-request"></a>

The `Aws\Middleware::mapRequest` middleware is useful when you need to modify a request after it is serialized but before it is sent\. For example, this can be used to add custom HTTP headers to a request\. The `mapRequest` function accepts a callable that accepts a `Psr\Http\Message\RequestInterface` argument and returns a `Psr\Http\Message\RequestInterface` object\.

```
use Aws\Middleware;
use Psr\Http\Message\RequestInterface;

// Create a command so that we can access the handler list
$command = $s3Client->getCommand('HeadObject', [
    'Key'    => 'test',
    'Bucket' => 'mybucket'
]);

// Apply a custom middleware named "add-header" to the "build" lifecycle step
$command->getHandlerList()->appendBuild(
    Middleware::mapRequest(function (RequestInterface $request) {
        // Return a new request with the added header
        return $request->withHeader('X-Foo-Baz', 'Bar');
    }),
    'add-header'
);
```

Now when the command is executed, it is sent with the custom header\.

**Important**  
Notice that the middleware was appended to the handler list at the end of `build` step\. This is to ensure that a request has been built before this middleware is invoked\.

#### mapResult<a name="mapresult"></a>

The `Aws\Middleware::mapResult` middleware is useful when you need to modify the result of a command execution\. The `mapResult` function accepts a callable that accepts an `Aws\ResultInterface` argument and returns an `Aws\ResultInterface` object\.

```
use Aws\Middleware;
use Aws\ResultInterface;

$command = $s3Client->getCommand('HeadObject', [
    'Key'    => 'test',
    'Bucket' => 'mybucket'
]);

$command->getHandlerList()->appendSign(
    Middleware::mapResult(function (ResultInterface $result) {
        // Add a custom value to the result
        $result['foo'] = 'bar';
        return $result;
    })
);
```

Now when the command is executed, the returned result will contain a `foo` attribute\.

#### history<a name="history"></a>

The `history` middleware is useful for testing that the SDK executed the commands you expected, sent the HTTP requests you expected, and received the results you expected\. It’s essentially a middleware that acts similarly to the history of a web browser\.

```
use Aws\History;
use Aws\Middleware;

$ddb = new Aws\DynamoDb\DynamoDbClient([
    'version' => 'latest',
    'region'  => 'us-west-2'
]);

// Create a history container to store the history data
$history = new History();

// Add the history middleware that uses the history container
$ddb->getHandlerList()->appendSign(Middleware::history($history));
```

An `Aws\History` history container stores 10 entries by default before purging entries\. You can customize the number of entries by passing in the number of entries to persist to the constructor\.

```
// Create a history container that stores 20 entries
$history = new History(20);
```

You can inspect the history container after executing requests that pass the history middleware\.

```
// The object is countable, returning the number of entries in the container
count($history);

// The object is iterable, yielding each entry in the container
foreach ($history as $entry) {
    // You can access the command that was executed
    var_dump($entry['command']);
    // The request that was serialized and sent
    var_dump($entry['request']);
    // The result that was received (if successful)
    var_dump($entry['result']);
    // The exception that was received (if a failure occurred)
    var_dump($entry['exception']);
}

// You can get the last Aws\CommandInterface that was executed. This method
// will throw an exception if no commands have been executed.
$command = $history->getLastCommand();

// You can get the last request that was serialized. This method will throw an exception
// if no requests have been serialized.
$request = $history->getLastRequest();

// You can get the last return value (an Aws\ResultInterface or Exception).
// The method will throw an exception if no value has been returned for the last
// executed operation (e.g., an async request has not completed).
$result = $history->getLastReturn();

// You can clear out the entries using clear
$history->clear();
```

#### tap<a name="tap"></a>

The `tap` middleware is used as an observer\. You can use this middleware to invoke functions when sending commands through the chain of middleware\. The `tap` function accepts a callable that accepts the `Aws\CommandInterface` and an optional `Psr\Http\Message\RequestInterface` that is being executed\.

```
use Aws\Middleware;

$s3 = new Aws\S3\S3Client([
    'region'  => 'us-east-1',
    'version' => '2006-03-01'
]);

$handlerList = $s3->getHandlerList();

// Create a tap middleware that observes the command at a specific step
$handlerList->appendInit(
    Middleware::tap(function (CommandInterface $cmd, RequestInterface $req = null) {
        echo 'About to send: ' . $cmd->getName() . "\n";
        if ($req) {
            echo 'HTTP method: ' . $request->getMethod() . "\n";
        }
    }
);
```

## Creating Custom handlers<a name="creating-custom-handlers"></a>

A handler is simply a function that accepts an `Aws\CommandInterface` object and `Psr\Http\Message\RequestInterface` object, and returns a `GuzzleHttp\Promise\PromiseInterface` that is fulfilled with an `Aws\ResultInterface` or rejected with an `Aws\Exception\AwsException`\.

Although the SDK has several `@http` options, a handler only needs to know how to use the following options:
+  [connect\_timeout](guide_configuration.md#http-connect-timeout) 
+  [debug](guide_configuration.md#http-debug) 
+  [decode\_content](guide_configuration.md#http-decode-content) \(optional\)
+  [delay](guide_configuration.md#http-delay) 
+  [progress](guide_configuration.md#http-progress) \(optional\)
+  [proxy](guide_configuration.md#http-proxy) 
+  [sink](guide_configuration.md#http-sink) 
+  [synchronous](guide_configuration.md#http-sync) \(optional\)
+  [stream](guide_configuration.md#http-stream) \(optional\)
+  [timeout](guide_configuration.md#http-timeout) 
+  [verify](guide_configuration.md#http-verify) 
+ http\_stats\_receiver \(optional\) \- A function to invoke with an associative array of HTTP transfer statistics if requested using the [stats](guide_configuration.md#config-stats) configuration parameter\.

Unless the option is specified as optional, a handler MUST be able to handle the option or it MUST return a rejected promise\.

In addition to handling specific `@http` options, a handler MUST add a `User-Agent` header that takes the following form, where “3\.X” can be replaced with `Aws\Sdk::VERSION` and “HandlerSpecificData/version …” should be replaced with your handler\-specific User\-Agent string\.

 `User-Agent: aws-sdk-php/3.X HandlerSpecificData/version ...` 