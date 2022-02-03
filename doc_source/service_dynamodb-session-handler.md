# Using the DynamoDB Session Handler with AWS SDK for PHP Version 3<a name="service_dynamodb-session-handler"></a>

The DynamoDB Session Handler is a custom session handler for PHP that enables developers to use Amazon DynamoDB as a session store\. Using DynamoDB for session storage alleviates issues that occur with session handling in a distributed web application by moving sessions off of the local file system and into a shared location\. DynamoDB is fast, scalable, easy to set up, and handles replication of your data automatically\.

The DynamoDB Session Handler uses the `session_set_save_handler()` function to hook DynamoDB operations into PHP’s [native session functions](http://www.php.net/manual/en/ref.session.php) to allow for a true drop in replacement\. This includes support for features such as session locking and garbage collection, which are a part of PHP’s default session handler\.

For more information about the DynamoDB service, see the [Amazon DynamoDB homepage](https://aws.amazon.com/dynamodb/)\.

## Basic Usage<a name="basic-usage"></a>

### Step 1: Register the Handler<a name="step-1-register-the-handler"></a>

First, instantiate and register the session handler\.

```
use Aws\DynamoDb\SessionHandler;

$sessionHandler = SessionHandler::fromClient($dynamoDb, [
    'table_name' => 'sessions'
]);

$sessionHandler->register();
```

### Step 2\. Create a Table to Store Your Sessions<a name="create-a-table-for-storing-your-sessions"></a>

Before you can actually use the session handler, you need to create a table in which to store the sessions\. You can do this ahead of time by using the [AWS Console for Amazon DynamoDB](https://console.aws.amazon.com/dynamodb/home), or by using the AWS SDK for PHP\.

When creating this table use ‘id’ as the name of the primary key\. Also it is recommended to setup a [Time To Live attribute](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/TTL.html) using the ‘expires’ attribute to benefit from automatic garbage collection of sessions\.

### Step 3\. Use PHP Sessions as You Normally Would<a name="step-3-use-php-sessions-as-you-normally-would"></a>

Once the session handler is registered and the table exists, you can write to and read from the session using the `$_SESSION` superglobal, just like you normally do with PHP’s default session handler\. The DynamoDB Session Handler encapsulates and abstracts the interactions with DynamoDB and enables you to simply use PHP’s native session functions and interface\.

```
// Start the session
session_start();

// Alter the session data
$_SESSION['user.name'] = 'jeremy';
$_SESSION['user.role'] = 'admin';

// Close the session (optional, but recommended)
session_write_close();
```

## Configuration<a name="configuration"></a>

You can configure the behavior of the session handler using the following options\. All options are optional, but be sure to understand what the defaults are\.

** `table_name` **  
The name of the DynamoDB table in which to store the sessions\. This defaults to `'sessions'`\.

** `hash_key` **  
The name of the hash key in the DynamoDB sessions table\. This defaults to `'id'`\.

** `data_attribute` **  
The name of the attribute in the DynamoDB sessions table in which the session data is stored\. This defaults to `'data'`\.

** `data_attribute_type` **  
The type of the attribute in the DynamoDB sessions table in which the session data is stored\. This defaults to `'string'`, but can optionally be set to `'binary'`\.

** `session_lifetime` **  
The lifetime of an inactive session before it should be garbage collected\. If it isn’t provided, the actual lifetime value that will be used is `ini_get('session.gc_maxlifetime')`\.

** `session_lifetime_attribute` **  
The name of the attribute in the DynamoDB sessions table in which the session expiration time is stored\. This defaults to `'expires'`\.

** `consistent_read` **  
Whether the session handler should use consistent reads for the `GetItem` operation\. The default is `true`\.

** `locking` **  
Whether to use session locking\. The default is `false`\.

** `batch_config` **  
Configuration used to batch deletes during garbage collection\. These options are passed directly into [DynamoDB WriteRequestBatch](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.DynamoDb.WriteRequestBatch.html) objects\. Manually trigger garbage collection via `SessionHandler::garbageCollect()`\.

** `max_lock_wait_time` **  
Maximum time \(in seconds\) that the session handler should wait to acquire a lock before giving up\. The default to is `10` and is only used with session locking\.

** `min_lock_retry_microtime` **  
Minimum time \(in microseconds\) that the session handler should wait between attempts to acquire a lock\. The default is `10000` and is only used with session locking\.

** `max_lock_retry_microtime` **  
Maximum time \(in microseconds\) that the session handler should wait between attempts to acquire a lock\. The default is `50000` and is only used with session locking\.

To configure the Session Handler, specify the configuration options when you instantiate the handler\. The following code is an example with all of the configuration options specified\.

```
$sessionHandler = SessionHandler::fromClient($dynamoDb, [
    'table_name'                    => 'sessions',
    'hash_key'                      => 'id',
    'data_attribute'                => 'data',
    'data_attribute_type'           => 'string',
    'session_lifetime'              => 3600,
    'session_lifetime_attribute'    => 'expires',
    'consistent_read'               => true,
    'locking'                       => false,
    'batch_config'                  => [],
    'max_lock_wait_time'            => 10,
    'min_lock_retry_microtime'      => 5000,
    'max_lock_retry_microtime'      => 50000,
]);
```

## Pricing<a name="pricing"></a>

Aside from data storage and data transfer fees, the costs associated with using DynamoDB are calculated based on the provisioned throughput capacity of your table \(see the [Amazon DynamoDB pricing details](https://aws.amazon.com/dynamodb/pricing/)\)\. Throughput is measured in units of write capacity and read capacity\. The Amazon DynamoDB homepage says:

A unit of read capacity represents one strongly consistent read per second \(or two eventually consistent reads per second\) for items as large as 4 KB\. A unit of write capacity represents one write per second for items as large as 1 KB\.

Ultimately, the throughput and the costs required for your sessions table will correlate with your expected traffic and session size\. The following table explains the amount of read and write operations that are performed on your DynamoDB table for each of the session functions\.


****  

|  |  | 
| --- |--- |
|  Read via `session_start()`   |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/service_dynamodb-session-handler.html)  | 
|  Read via `session_start()` \(Using session locking\)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/service_dynamodb-session-handler.html)  | 
|  Write via `session_write_close()`   |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/service_dynamodb-session-handler.html)  | 
|  Delete via `session_destroy()`   |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/service_dynamodb-session-handler.html)  | 
|  Garbage Collection  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/service_dynamodb-session-handler.html)  | 

## Session Locking<a name="ddbsh-session-locking"></a>

The DynamoDB Session Handler supports pessimistic session locking to mimic the behavior of PHP’s default session handler\. By default, the DynamoDB Session Handler has this feature *turned off* because it can become a performance bottleneck and drive up costs, especially when an application accesses the session when using Ajax requests or iframes\. Carefully consider whether your application requires session locking before enabling it\.

To enable session locking, set the `'locking'` option to `true` when you instantiate the `SessionHandler`\.

```
$sessionHandler = SessionHandler::fromClient($dynamoDb, [
    'table_name' => 'sessions',
    'locking'    => true,
]);
```

## Garbage Collection<a name="ddbsh-garbage-collection"></a>

Setup a TTL attribute in your DynamoDB table, using the attribute ‘expires’\. This will automatically garbage collect your sessions and avoid the need to garbage collect them yourself\.

Alternatively, the DynamoDB Session Handler supports session garbage collection by using a series of `Scan` and `BatchWriteItem` operations\. Due to the nature of how the `Scan` operation works, and to find all of the expired sessions and delete them, the garbage collection process can require a lot of provisioned throughput\.

For this reason, we do not support automated garbage collection\. A better practice is to schedule the garbage collection to occur during an off\-peak time when a burst of consumed throughput will not disrupt the rest of the application\. For example, you could have a nightly cron job trigger a script to run the garbage collection\. This script would need to do something like the following\.

```
$sessionHandler = SessionHandler::fromClient($dynamoDb, [
    'table_name'   => 'sessions',
    'batch_config' => [
        'batch_size' => 25,
        'before' => function ($command) {
            echo "About to delete a batch of expired sessions.\n";
        }
    ]
]);

$sessionHandler->garbageCollect();
```

You can also use the `'before'` option within `'batch_config'` to introduce delays on the `BatchWriteItem` operations that are performed by the garbage collection process\. This will increase the amount of time it takes the garbage collection to complete, but it can help you spread out the requests made by the DynamoDB Session Handler to help you stay close to or within your provisioned throughput capacity during garbage collection\.

```
$sessionHandler = SessionHandler::fromClient($dynamoDb, [
    'table_name'   => 'sessions',
    'batch_config' => [
        'before' => function ($command) {
            $command['@http']['delay'] = 5000;
        }
    ]
]);

$sessionHandler->garbageCollect();
```

## Best Practices<a name="best-practices"></a>

1. Create your sessions table in an AWS Region that is geographically closest to or in the same Region as your application servers\. This ensures the lowest latency between your application and DynamoDB database\.

1. Choose the provisioned throughput capacity of your sessions table carefully\. Take into account the expected traffic to your application and the expected size of your sessions\. Alternatively use the ‘On Demand’ Read/Write capacity mode for your table\.

1. Monitor your consumed throughput through the AWS Management Console or with Amazon CloudWatch, and adjust your throughput settings as needed to meet the demands of your application\.

1. Keep the size of your sessions small \(ideally less than 1 KB\)\. Small sessions perform better and require less provisioned throughput capacity\.

1. Do not use session locking unless your application requires it\.

1. Instead of using PHP’s built\-in session garbage collection triggers, schedule your garbage collection via a cron job, or another scheduling mechanism, to run during off\-peak hours\. Use the `'batch_config'` option to your advantage\.

## Required IAM Permissions<a name="required-iam-permissions"></a>

To use the DynamoDB SessionHhandler, your [configured credentials](guide_credentials.md) must have permission to use the DynamoDB table that [you created in a previous step](#create-a-table-for-storing-your-sessions)\. The following IAM policy contains the minimum permissions that you need\. To use this policy, replace the Resource value with the Amazon Resource Name \(ARN\) of the table that you created previously\. For more information about creating and attaching IAM policies, see [Managing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage.html) in the IAM User Guide\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "dynamodb:GetItem",
        "dynamodb:UpdateItem",
        "dynamodb:DeleteItem",
        "dynamodb:Scan",
        "dynamodb:BatchWriteItem"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:dynamodb:<region>:<account-id>:table/<table-name>"
    }
  ]
}
```