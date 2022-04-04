# Managing Topics in Amazon SNS with the AWS SDK for PHP Version 3<a name="sns-examples-managing-topics"></a>

To send notifications to Amazon Simple Queue Service \(Amazon SQS\), HTTP/HTTPS URLs, email, AWS SMS, or AWS Lambda, you must first create a topic that manages the delivery of messages to any subscribers of that topic\.

In terms of the observer design pattern, a topic is like the subject\. After a topic is created, you add subscribers that are notified automatically when a message is published to the topic\.

Learn more about subscribing to topics in [Managing Subscriptions in Amazon SNS with AWS SDK for PHP Version 3](sns-examples-subscribing-unsubscribing-topics.md)\.

The following examples show how to:
+ Create a topic to publish notifications to using [CreateTopic](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sns-2010-03-31.html#createtopic)\.
+ Return a list of the requester’s topics using [ListTopics](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sns-2010-03-31.html#listtopic)\.
+ Delete a topic and all of its subscriptions using [DeleteTopic](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sns-2010-03-31.html#deletetopic)\.
+ Return all of the properties of a topic using [GetTopicAttributes](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sns-2010-03-31.html#gettopicattributes)\.
+ Allow a topic owner to set an attribute of the topic to a new value using [SetTopicAttributes](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sns-2010-03-31.html#settopicattributes)\.

For more information about using Amazon SNS, see [Amazon SNS Topic Attributes for Message Delivery Status](https://docs.aws.amazon.com/sns/latest/dg/sns-topic-attributes.html)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting Credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage](getting-started_basic-usage.md)\.

## Create a Topic<a name="create-a-topic"></a>

To create a topic, use the [CreateTopic](https://docs.aws.amazon.com/sns/latest/api/API_API_CreateTopic.html) operation\.

Each topic name in your AWS account must be unique\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Sns\SnsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$SnSclient = new SnsClient([
    'profile' => 'default',
    'region' => 'us-east-1',
    'version' => '2010-03-31'
]);

$topicname = 'myTopic';

try {
    $result = $SnSclient->createTopic([
        'Name' => $topicname,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## List Your Topics<a name="list-your-topics"></a>

To list up to 100 existing topics in the current AWS Region, use the [ListTopics](https://docs.aws.amazon.com/sns/latest/api/API_API_ListTopics.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Sns\SnsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$SnSclient = new SnsClient([
    'profile' => 'default',
    'region' => 'us-east-1',
    'version' => '2010-03-31'
]);

try {
    $result = $SnSclient->listTopics([
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Delete a Topic<a name="delete-a-topic"></a>

To remove an existing topic and all of its subscriptions, use the [DeleteTopic](https://docs.aws.amazon.com/sns/latest/api/API_API_DeleteTopic.html) operation\.

Any messages that have not been delivered yet to subscribers will also be deleted\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Sns\SnsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$SnSclient = new SnsClient([
    'profile' => 'default',
    'region' => 'us-east-1',
    'version' => '2010-03-31'
]);

$topic = 'arn:aws:sns:us-east-1:111122223333:MyTopic';

try {
    $result = $SnSclient->deleteTopic([
        'TopicArn' => $topic,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Get Topic Attributes<a name="get-topic-attributes"></a>

To retrieve properties of a single existing topic, use the [GetTopicAttributes](https://docs.aws.amazon.com/sns/latest/api/API_API_GetTopicAttributes.html) operation\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Sns\SnsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$SnSclient = new SnsClient([
    'profile' => 'default',
    'region' => 'us-east-1',
    'version' => '2010-03-31'
]);

$topic = 'arn:aws:sns:us-east-1:111122223333:MyTopic';

try {
    $result = $SnSclient->getTopicAttributes([
        'TopicArn' => $topic,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Set Topic Attributes<a name="set-topic-attributes"></a>

To update properties of a single existing topic, use the [SetTopicAttributes](https://docs.aws.amazon.com/sns/latest/api/API_API_SetTopicAttributes.html) operation\.

You can set only the `Policy`, `DisplayName`, and `DeliveryPolicy` attributes\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Sns\SnsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$SnSclient = new SnsClient([
    'profile' => 'default',
    'region' => 'us-east-1',
    'version' => '2010-03-31'
]);
$attribute = 'Policy | DisplayName | DeliveryPolicy';
$value = 'First Topic';
$topic = 'arn:aws:sns:us-east-1:111122223333:MyTopic';

try {
    $result = $SnSclient->setTopicAttributes([
        'AttributeName' => $attribute,
        'AttributeValue' => $value,
        'TopicArn' => $topic,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```