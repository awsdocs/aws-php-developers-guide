# Managing Subscriptions in Amazon SNS with AWS SDK for PHP Version 3<a name="sns-examples-subscribing-unsubscribing-topics"></a>

Use Amazon Simple Notification Service \(Amazon SNS\) topics to send notifications to Amazon Simple Queue Service \(Amazon SQS\), HTTP/HTTPS, email addresses, AWS Server Migration Service \(AWS SMS\), or AWS Lambda\.

Subscriptions are attached to a topic that manages sending messages to subscribers\. Learn more about creating topics in [Managing Topics in Amazon SNS with the AWS SDK for PHP Version 3](sns-examples-managing-topics.md)\.

The following examples show how to:
+ Subscribe to an existing topic using [Subscribe](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sns-2010-03-31.html#subscribe)\.
+ Verify a subscription using [ConfirmSubscription](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sns-2010-03-31.html#confirmsubscription)\.
+ List existing subscriptions using [ListSubscriptionsByTopic](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sns-2010-03-31.html#listsubscriptionsbytopic)\.
+ Delete a subscription using [Unsubscribe](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sns-2010-03-31.html#unsubscribe)\.
+ Send a message to all subscribers of a topic using [Publish](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sns-2010-03-31.html#publish)\.

For more information about using Amazon SNS, see [Using Amazon SNS for System\-to\-System Messaging](https://docs.aws.amazon.com/sns/latest/dg/sns-system-to-system-messaging.html)\.

All the example code for the AWS SDK for PHP Version 3 is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage Patterns of the AWS SDK for PHP Version 3](getting-started_basic-usage.md)\.

## Subscribe an Email Address to a Topic<a name="subscribe-an-email-address-to-a-topic"></a>

To initiate a subscription to an email address, use the [Subscribe](https://docs.aws.amazon.com/sns/latest/api/API_API_Subscribe.html) operation\.

You can use the subscribe method to subscribe several different endpoints to an Amazon SNS topic, depending on the values used for parameters passed\. This is shown in other examples in this topic\.

In this example, the endpoint is an email address\. A confirmation token is sent to this email\. Verify the subscription with this confirmation token within three days of receipt\.

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

$protocol = 'email';
$endpoint = 'sample@example.com';
$topic = 'arn:aws:sns:us-east-1:111122223333:MyTopic';

try {
    $result = $SnSclient->subscribe([
        'Protocol' => $protocol,
        'Endpoint' => $endpoint,
        'ReturnSubscriptionArn' => true,
        'TopicArn' => $topic,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Subscribe an Application Endpoint to a Topic<a name="subscribe-an-application-endpoint-to-a-topic"></a>

To initiate a subscription to a web app, use the [Subscribe](https://docs.aws.amazon.com/sns/latest/api/API_API_Subscribe.html) operation\.

You can use the subscribe method to subscribe several different endpoints to an Amazon SNS topic, depending on the values used for parameters passed\. This is shown in other examples in this topic\.

In this example, the endpoint is a URL\. A confirmation token is sent to this web address\. Verify the subscription with this confirmation token within three days of receipt\.

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

$protocol = 'https';
$endpoint = 'https://';
$topic = 'arn:aws:sns:us-east-1:111122223333:MyTopic';

try {
    $result = $SnSclient->subscribe([
        'Protocol' => $protocol,
        'Endpoint' => $endpoint,
        'ReturnSubscriptionArn' => true,
        'TopicArn' => $topic,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Subscribe a Lambda Function to a Topic<a name="subscribe-a-lam-function-to-a-topic"></a>

To initiate a subscription to a Lambda function, use the [Subscribe](https://docs.aws.amazon.com/sns/latest/api/API_API_Subscribe.html) operation\.

You can use the subscribe method to subscribe several different endpoints to an Amazon SNS topic, depending on the values used for parameters passed\. This is shown in other examples in this topic\.

In this example, the endpoint is a Lambda function\. A confirmation token is sent to this Lambda function\. Verify the subscription with this confirmation token within three days of receipt\.

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

$protocol = 'lambda';
$endpoint = 'arn:aws:lambda:us-east-1:123456789023:function:messageStore';
$topic = 'arn:aws:sns:us-east-1:111122223333:MyTopic';

try {
    $result = $SnSclient->subscribe([
        'Protocol' => $protocol,
        'Endpoint' => $endpoint,
        'ReturnSubscriptionArn' => true,
        'TopicArn' => $topic,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Subscribe a Text SMS to a Topic<a name="subscribe-a-text-sms-to-a-topic"></a>

To send SMS messages to multiple phone numbers at the same time, subscribe each number to a topic\.

To initiate a subscription to a phone number, use the [Subscribe](https://docs.aws.amazon.com/sns/latest/api/API_API_Subscribe.html) operation\.

You can use the subscribe method to subscribe several different endpoints to an Amazon SNS topic, depending on the values used for parameters passed\. This is shown in other examples in this topic\.

In this example, the endpoint is a phone number in E\.164 format, a standard for international telecommunications\.

A confirmation token is sent to this phone number\. Verify the subscription with this confirmation token within three days of receipt\.

For an alternative way to send SMS messages with Amazon SNS, see [Sending SMS Messages in Amazon SNS with the AWS SDK for PHP Version 3](sns-examples-sending-sms.md)\.

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

$protocol = 'sms';
$endpoint = '+1XXX5550100';
$topic = 'arn:aws:sns:us-east-1:111122223333:MyTopic';

try {
    $result = $SnSclient->subscribe([
        'Protocol' => $protocol,
        'Endpoint' => $endpoint,
        'ReturnSubscriptionArn' => true,
        'TopicArn' => $topic,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Confirm Subscription to a Topic<a name="confirm-subscription-to-a-topic"></a>

To actually create a subscription, the endpoint owner must acknowledge intent to receive messages from the topic using a token sent when a subscription is established initially, as described earlier\. Confirmation tokens are valid for three days\. After three days, you can resend a token by creating a new subscription\.

To confirm a subscription, use the [ConfirmSubscription](https://docs.aws.amazon.com/sns/latest/api/API_API_ConfirmSubscription.html) operation\.

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

$subscription_token = 'arn:aws:sns:us-east-1:111122223333:MyTopic:123456-abcd-12ab-1234-12ba3dc1234a';
$topic = 'arn:aws:sns:us-east-1:111122223333:MyTopic';

try {
    $result = $SnSclient->subscribe([
        'Token' => $subscription_token,
        'TopicArn' => $topic,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## List Subscriptions to a Topic<a name="list-subscriptions-to-a-topic"></a>

To list up to 100 existing subscriptions in a given AWS Region, use the [ListSubscriptions](https://docs.aws.amazon.com/sns/latest/api/API_API_ListSubscriptions.html) operation\.

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
    $result = $SnSclient->listSubscriptions([
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Unsubscribe from a Topic<a name="unsubscribe-from-a-topic"></a>

To remove an endpoint subscribed to a topic, use the [Unsubscribe](https://docs.aws.amazon.com/sns/latest/api/API_API_Unsubscribe.html) operation\.

If the subscription requires authentication for deletion, only the owner of the subscription or the topic’s owner can unsubscribe, and an AWS signature is required\. If the unsubscribe call doesn’t require authentication and the requester isn’t the subscription owner, a final cancellation message is delivered to the endpoint\.

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

$subscription = 'arn:aws:sns:us-east-1:111122223333:MySubscription';

try {
    $result = $SnSclient->unsubscribe([
        'SubscriptionArn' => $subscription,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Publish a Message to an Amazon SNS Topic<a name="publish-a-message-to-an-sns-topic"></a>

To deliver a message to each endpoint that’s subscribed to an Amazon SNS topic, use the [Publish](https://docs.aws.amazon.com/sns/latest/api/API_API_Publish.html) operation\.

Create an object that contains the parameters for publishing a message, including the message text and the Amazon Resource Name \(ARN\) of the Amazon SNS topic\.

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

$message = 'This message is sent from a Amazon SNS code sample.';
$topic = 'arn:aws:sns:us-east-1:111122223333:MyTopic';

try {
    $result = $SnSclient->publish([
        'Message' => $message,
        'TopicArn' => $topic,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```