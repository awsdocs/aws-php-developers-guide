# Sending events to Amazon CloudWatch Events with AWS SDK for PHP Version 3<a name="cw-examples-sending-events"></a>

CloudWatch Events delivers a near real\-time stream of system events that describe changes in Amazon Web Services \(AWS\) resources to any of various targets\. Using simple rules, you can match events and route them to one or more target functions or streams\.

The following examples show how to:
+ Create a rule using [PutRule](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-events-2015-10-07.html#putrule)\.
+ Add targets to a rule using [PutTargets](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-events-2015-10-07.html#puttargets)\.
+ Send custom events to CloudWatch Events using [PutEvents](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-events-2015-10-07.html#putevents)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic usage](getting-started_basic-usage.md)\.

## Create a rule<a name="create-a-rule"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudWatchEvents\CloudWatchEventsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$client = new Aws\cloudwatchevents\cloudwatcheventsClient([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2015-10-07'
]);

try {
    $result = $client->putRule(array(
        'Name' => 'DEMO_EVENT', // REQUIRED
        'RoleArn' => 'IAM_ROLE_ARN',
        'ScheduleExpression' => 'rate(5 minutes)',
        'State' => 'ENABLED',
    ));
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Add targets to a rule<a name="add-targets-to-a-rule"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudWatchEvents\CloudWatchEventsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$client = new Aws\cloudwatchevents\cloudwatcheventsClient([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2015-10-07'
]);

try {
    $result = $client->putTargets([
        'Rule' => 'DEMO_EVENT', // REQUIRED
        'Targets' => [ // REQUIRED
            [
                'Arn' => 'LAMBDA_FUNCTION_ARN', // REQUIRED
                'Id' => 'myCloudWatchEventsTarget' // REQUIRED
            ],
        ],
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```

## Send custom events<a name="send-custom-events"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudWatchEvents\CloudWatchEventsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$client = new Aws\cloudwatchevents\cloudwatcheventsClient([
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => '2015-10-07'
]);

try {
    $result = $client->putEvents([
        'Entries' => [ // REQUIRED
            [
                'Detail' => '<string>',
                'DetailType' => '<string>',
                'Resources' => ['<string>'],
                'Source' => '<string>',
                'Time' => time()
            ],
        ],
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```