# Enabling long polling in Amazon SQS with AWS SDK for PHP Version 3<a name="sqs-examples-enable-long-polling"></a>

Long polling reduces the number of empty responses by allowing Amazon SQS to wait a specified time for a message to become available in the queue before sending a response\. Also, long polling eliminates false empty responses by querying all of the servers instead of a sampling of servers\. To enable long polling, specify a non\-zero wait time for received messages\. To learn more, see [SQS Long Polling](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-long-polling.html)\.

The following examples show how to:
+ Set attributes on an Amazon SQS queue to enable long polling, using [SetQueueAttributes](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sqs-2012-11-05.html#setqueueattributes)\.
+ Retrieve one or more messages with long polling using [ReceiveMessage](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sqs-2012-11-05.html#receivemessage)\.
+ Create a long polling queue using [CreateQueue](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sqs-2012-11-05.html#createqueue)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic usage](getting-started_basic-usage.md)\.

## Set attributes on a queue to enable long polling<a name="set-attributes-on-a-queue-to-enable-long-polling"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Sqs\SqsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$queueUrl = "QUEUE_URL";
 

$client = new SqsClient([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2012-11-05'
]);

try {
    $result = $client->setQueueAttributes(array(
        'Attributes' => [
            'ReceiveMessageWaitTimeSeconds' => 20
        ],
        'QueueUrl' => $queueUrl, // REQUIRED
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Retrieve messages with long polling<a name="retrieve-messages-with-long-polling"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Sqs\SqsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$queueUrl = "QUEUE_URL";
 

$client = new SqsClient([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2012-11-05'
]);

try {
    $result = $client->receiveMessage(array(
        'AttributeNames' => ['SentTimestamp'],
        'MaxNumberOfMessages' => 1,
        'MessageAttributeNames' => ['All'],
        'QueueUrl' => $queueUrl, // REQUIRED
        'WaitTimeSeconds' => 20,
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Create a queue with long polling<a name="create-a-queue-with-long-polling"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Sqs\SqsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$queueName = "QUEUE_NAME";
 

$client = new SqsClient([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2012-11-05'
]);

try {
    $result = $client->createQueue(array(
        'QueueName' => $queueName,
        'Attributes' => array(
            'ReceiveMessageWaitTimeSeconds' => 20
        ),
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```