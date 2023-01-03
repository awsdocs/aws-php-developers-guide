# Monitoring your sending activity using the Amazon SES API and the AWS SDK for PHP Version 3<a name="ses-send-email"></a>

Amazon Simple Email Service \(Amazon SES\) provides methods for monitoring your sending activity\. We recommend that you implement these methods so that you can keep track of important measures, such as your account’s bounce, complaint, and reject rates\. Excessively high bounce and complaint rates can jeopardize your ability to send emails using Amazon SES\.

The following examples show how to:
+ Check your sending quota using [GetSendQuota](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#getsendquota)\.
+ Monitor your sending activity using [GetSendStatistics](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-email-2010-12-01.html#getsendstatistics)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic usage](getting-started_basic-usage.md)\.

For more information about using Amazon SES, see the [Amazon SES Developer Guide](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/)\.

## Check your sending quota<a name="check-your-sending-quota"></a>

You are limited to sending only a certain amount of messages in a single 24\-hour period\. To check how many messages you are still allowed to send, use the [GetSendQuota](https://docs.aws.amazon.com/ses/latest/APIReference/API_GetSendQuota.html) operation\. For more information, see [Managing Your Amazon SES Sending Limits](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/manage-sending-limits.html)\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Ses\SesClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$SesClient = new SesClient([
    'profile' => 'default',
    'version' => '2010-12-01',
    'region' => 'us-east-1'

]);

try {
    $result = $SesClient->getSendQuota([
    ]);
    $send_limit = $result["Max24HourSend"];
    $sent = $result["SentLast24Hours"];
    $available = $send_limit - $sent;
    print("<p>You can send " . $available . " more messages in the next 24 hours.</p>");
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Monitor your sending activity<a name="monitor-your-sending-activity"></a>

To retrieve metrics for messages you’ve sent in the past two weeks, use the [GetSendStatistics](https://docs.aws.amazon.com/ses/latest/APIReference/API_GetSendStatistics.html) operation\. This example returns the number of delivery attempts, bounces, complaints, and rejected messages in 15\-minute increments\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Ses\SesClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$SesClient = new SesClient([
    'profile' => 'default',
    'version' => '2010-12-01',
    'region' => 'us-east-1'
]);

try {
    $result = $SesClient->getSendStatistics([
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```