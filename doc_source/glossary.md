# Glossary<a name="glossary"></a>

**API Version**  
Services have one or more API versions, and which version you are using dictates which operations and parameters are valid\. API versions are formatted like a date\. For example, the latest API version for Amazon S3 is `2006-03-01`\. [Specify a version](guide_configuration.md#cfg-version) when you configure a client object\.

**Client**  
Client objects are used to execute operations for a service\. Each service that is supported in the SDK has a corresponding client object\. Client objects have methods that correspond one\-to\-one with the service operations\. See the [basic usage guide](getting-started_basic-usage.md) for details on how to create and use client objects\.

**Command**  
Command objects encapsulate the execution of an operation\. When following the [basic usage patterns](getting-started_basic-usage.md) of the SDK, you will not deal directly with command objects\. Command objects can be accessed using the `getCommand()` method of a client, in order to use advanced features of the SDK like concurrent requests and batching\. See the [Command Objects in the AWS SDK for PHP Version 3](guide_commands.md) guide for more details\.

**Credentials**  
To interact with AWS services, authenticate with the service using your credentials, or [AWS access keys](https://aws.amazon.com/developers/access-keys/)\. Your access keys consist of two parts: your access key ID, which identifies your account, and your secret access, which is used to create **signatures** when executing operations\. [Provide credentials](guide_credentials.md) when you configure a client object\.

**Handler**  
A handler is a function that performs the actual transformation of a command and request into a result\. A handler typically sends HTTP requests\. Handlers can be composed with middleware to augment their behavior\. A handler is a function that accepts an `Aws\CommandInterface` and a `Psr\Http\Message\RequestInterface` and returns a promise that is fulfilled with an `Aws\ResultInterface` or rejected with an `Aws\Exception\AwsException` reason\.

**JMESPath**  
 [JMESPath](http://jmespath.org/) is a query language for JSON\-like data\. The AWS SDK for PHP uses JMESPath expressions to query PHP data structures\. JMESPath expressions can be used directly on `Aws\Result` and `Aws\ResultPaginator` objects via the `search($expression)` method\.

**Middleware**  
Middleware is a special type of high\-level function that augments the behavior of transferring a command and delegating to a “next” handler\. Middleware functions accept an `Aws\CommandInterface` and a `Psr\Http\Message\RequestInterface` and return a promise that is fulfilled with an `Aws\ResultInterface` or rejected with an `Aws\Exception\AwsException` reason\.

**Operation**  
Refers to a single operation within a service’s API \(e\.g\., `CreateTable` for DynamoDB, `RunInstances` for Amazon EC2\)\. In the SDK, operations are executed by calling a method of the same name on the corresponding service’s client object\. Executing an operation involves preparing and sending an HTTP request to the service and parsing the response\. This process of executing an operation is abstracted by the SDK via **command** objects\.

**Paginator**  
Some AWS service operations are paginated and respond with truncated results\. For example, Amazon S3’s `ListObjects` operation only returns up to 1000 objects at a time\. Operations like these require making subsequent requests with token \(or marker\) parameters to retrieve the entire set of results\. paginators are a feature of the SDK that act as an abstraction over this process to make it easier for developers to use paginated APIs\. They are accessed via the `getPaginator()` method of the client\. See the [Paginators in the AWS SDK for PHP Version 3](guide_paginators.md) guide for more details\.

**Promise**  
A promise represents the eventual result of an asynchronous operation\. The primary way of interacting with a promise is through its then method, which registers callbacks to receive either a promise’s eventual value or the reason why the promise cannot be fulfilled\.

**Region**  
Services are supported in [one or more geographical regions](https://docs.aws.amazon.com/general/latest/gr/rande.html)\. Services may have different endpoints/URLs in each region, which exist to reduce data latency in your applications\. [Provide a region](guide_configuration.md#cfg-region) when you configure a client object, so that the SDK can determine which endpoint to use with the service\.

**SDK**  
The term “SDK” can refer to the AWS SDK for PHP library as a whole, but also refers to the `Aws\Sdk` class [\(docs\)](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.Sdk.html), which acts as a factory for the client objects for each **service**\. The `Sdk` class also let’s you provide a set of [global configuration values](guide_configuration.md) that are applied to all client objects that it creates\.

**Service**  
A general way to refer to any of the AWS services \(e\.g\., Amazon S3, Amazon DynamoDB, AWS OpsWorks, etc\.\)\. Each service has a corresponding **client** object in the SDK that supports one or more **API versions**\. Each service also has one or more **operations** that make up its API\. Services are supported in one or more **regions**\.

**Signature**  
When executing operations, the SDK uses your credentials to create a digital signature of your request\. The service then verifies the signature before processing your request\. The signing process is encapsulated by the SDK, and happens automatically using the credentials you configure for the client\.

**Waiter**  
Waiters are a feature of the SDK that make it easier to work with operations that change the state of a resource and that are *eventually consistent* or *asynchronous* in nature\. For example, the Amazon DynamoDB`CreateTable` operation sends a response back immediately, but the table may not be ready to access for several seconds\. Executing a waiter allows you to wait until a resource enters into a particular state by sleeping and polling the resource’s status\. Waiters are accessed using the `waitUntil()` method of the client\. See the [Waiters in the AWS SDK for PHP Version 3](guide_waiters.md) guide for more details\.

For the latest AWS terminology, see the [AWS Glossary](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html) in the AWS General Reference\.