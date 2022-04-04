# Using Dead\-Letter Queues in Amazon SQS with AWS SDK for PHP Version 3<a name="sqs-examples-dead-letter-queues"></a>

A dead\-letter queue is one that other \(source\) queues can target for messages that can’t be processed successfully\. You can set aside and isolate these messages in the dead\-letter queue to determine why their processing did not succeed\. You must individually configure each source queue that sends messages to a dead\-letter queue\. Multiple queues can target a single dead\-letter queue\.

To learn more, see [Using SQS Dead Letter Queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html)\.

The following example shows how to:
+ Enable a dead\-letter queue using [SetQueueAttributes](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sqs-2012-11-05.html#setqueueattributes)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting Credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage](getting-started_basic-usage.md)\.

## Enable a Dead\-Letter Queue<a name="enable-a-dead-letter-queue"></a>

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
    $result = $client->setQueueAttributes([
        'Attributes' => [
            'RedrivePolicy' => "{\"deadLetterTargetArn\":\"DEAD_LETTER_QUEUE_ARN\",\"maxReceiveCount\":\"10\"}"
        ],
        'QueueUrl' => $queueUrl // REQUIRED
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```