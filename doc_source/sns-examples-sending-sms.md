# Sending SMS messages in Amazon SNS with the AWS SDK for PHP Version 3<a name="sns-examples-sending-sms"></a>

You can use Amazon Simple Notification Service \(Amazon SNS\) to send text messages, or SMS messages, to SMS\-enabled devices\. You can send a message directly to a phone number, or you can send a message to multiple phone numbers at once by subscribing those phone numbers to a topic and sending your message to the topic\.

Use Amazon SNS to specify preferences for SMS messaging, such as how your deliveries are optimized \(for cost or for reliable delivery\), your monthly spending limit, how message deliveries are logged, and whether to subscribe to daily SMS usage reports\. These preferences are retrieved and set as SMS attributes for Amazon SNS\.

When you send an SMS message, specify the phone number using the E\.164 format\. E\.164 is a standard for the phone number structure used for international telecommunications\. Phone numbers that follow this format can have a maximum of 15 digits, and are prefixed with the plus character \(\+\) and the country code\. For example, a US phone number in E\.164 format would appear as \+1001XXX5550100\.

The following examples show how to:
+ Retrieve the default settings for sending SMS messages from your account using [GetSMSAttributes](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sns-2010-03-31.html#getsmsattributes)\.
+ Update the default settings for sending SMS messages from your account using [SetSMSAttributes](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sns-2010-03-31.html#setsmsattributes)\.
+ Discover if a given phone number owner has opted out of receiving SMS messages from your account using [CheckIfPhoneNumberISOptedOut](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sns-2010-03-31.html#checkifphonenumberisoptedout)\.
+ List phone numbers where the owner has opted out of receiving SMS messages from your account using [ListPhoneNumberOptedOut](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sns-2010-03-31.html#listphonenumbersoptedout)\.
+ Send a text message \(SMS message\) directly to a phone number using [Publish](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sns-2010-03-31.html#publish)\.

For more information about using Amazon SNS, see [Using Amazon SNS for User Notifications with a Mobile Phone Number as a Subscriber \(Send SMS\)](https://docs.aws.amazon.com/sns/latest/dg/sns-mobile-phone-number-as-subscriber.html)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic usage](getting-started_basic-usage.md)\.

## Get SMS attributes<a name="get-sms-attributes"></a>

To retrieve the default settings for SMS messages, use the [GetSMSAttributes](https://docs.aws.amazon.com/sns/latest/api/API_GetSMSAttributes.html) operation\.

This example gets the `DefaultSMSType` attribute\. This attribute controls whether SMS messages are sent as `Promotional`, which optimizes message delivery to incur the lowest cost, or as `Transactional`, which optimizes message delivery to achieve the highest reliability\.

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
    $result = $SnSclient->getSMSAttributes([
        'attributes' => ['DefaultSMSType'],
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Set SMS attributes<a name="set-sms-attributes"></a>

To update the default settings for SMS messages, use the [SetSMSAttributes](https://docs.aws.amazon.com/sns/latest/api/API_SetSMSAttributes.html) operation\.

This example sets the `DefaultSMSType` attribute to `Transactional`, which optimizes message delivery to achieve the highest reliability\.

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
    $result = $SnSclient->SetSMSAttributes([
        'attributes' => [
            'DefaultSMSType' => 'Transactional',
        ],
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Check if a phone number has opted out<a name="check-if-a-phone-number-has-opted-out"></a>

To determine if a given phone number owner has opted out of receiving SMS messages from your account, use the [CheckIfPhoneNumberIsOptedOut](https://docs.aws.amazon.com/sns/latest/api/API_CheckIfPhoneNumberIsOptedOut.html) operation\.

In this example, the phone number is in E\.164 format, a standard for international telecommunications\.

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

$phone = '+1XXX5550100';

try {
    $result = $SnSclient->checkIfPhoneNumberIsOptedOut([
        'phoneNumber' => $phone,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## List opted\-out phone numbers<a name="list-opted-out-phone-numbers"></a>

To retrieve a list of phone numbers where the owner has opted out of receiving SMS messages from your account, use the [ListPhoneNumbersOptedOut](https://docs.aws.amazon.com/sns/latest/api/API_ListPhoneNumbersOptedOut.html) operation\.

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
    $result = $SnSclient->listPhoneNumbersOptedOut([
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Publish to a text message \(SMS message\)<a name="publish-to-a-text-message-sms-message"></a>

To deliver a text message \(SMS message\) directly to a phone number, use the [Publish](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html) operation\.

In this example, the phone number is in E\.164 format, a standard for international telecommunications\.

SMS messages can contain up to 140 bytes\. The size limit for a single SMS publish action is 1,600 bytes\.

For more details on sending SMS messages, see [Sending an SMS Message](https://docs.aws.amazon.com/sns/latest/dg/sms_publish-to-phone.html)\.

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
$phone = '+1XXX5550100';

try {
    $result = $SnSclient->publish([
        'Message' => $message,
        'PhoneNumber' => $phone,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```