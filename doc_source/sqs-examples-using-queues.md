# Using queues in Amazon SQS with AWS SDK for PHP Version 3<a name="sqs-examples-using-queues"></a>

To learn about Amazon SQS queues, see [How SQS Queues Work](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-how-it-works.html)\.

The following examples show how to:
+ Return a list of your queues using [ListQueues](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sqs-2012-11-05.html#listqueues)\.
+ Create a new queue using [CreateQueue](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sqs-2012-11-05.html#createqueue)\.
+ Return the URL of an existing queue using [GetQueueUrl](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sqs-2012-11-05.html#getqueueurl)\.
+ Delete a specified queue using [DeleteQueue](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sqs-2012-11-05.html#deletequeue)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic usage](getting-started_basic-usage.md)\.

## Return a list of queues<a name="return-a-list-of-queues"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Sqs\SqsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$client = new SqsClient([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2012-11-05'
]);

try {
    $result = $client->listQueues();
    foreach ($result->get('QueueUrls') as $queueUrl) {
        echo "$queueUrl\n";
    }
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Create a queue<a name="create-a-queue"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Sqs\SqsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$queueName = "SQS_QUEUE_NAME";
 
$client = new SqsClient([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2012-11-05'
]);

try {
    $result = $client->createQueue(array(
        'QueueName' => $queueName,
        'Attributes' => array(
            'DelaySeconds' => 5,
            'MaximumMessageSize' => 4096, // 4 KB
        ),
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Return the URL of a queue<a name="return-the-url-of-a-queue"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Sqs\SqsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$queueName = "SQS_QUEUE_NAME";
 
$client = new SqsClient([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2012-11-05'
]);

try {
    $result = $client->getQueueUrl([
        'QueueName' => $queueName // REQUIRED
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Delete a queue<a name="delete-a-queue"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Sqs\SqsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$queueUrl = "SQS_QUEUE_URL";
 
$client = new SqsClient([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2012-11-05'
]);

try {
    $result = $client->deleteQueue([
        'QueueUrl' => $queueUrl // REQUIRED
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```