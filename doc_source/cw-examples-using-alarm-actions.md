# Using Alarm Actions with Amazon CloudWatch Alarms with AWS SDK for PHP Version 3<a name="cw-examples-using-alarm-actions"></a>

Use alarm actions to create alarms that automatically stop, terminate, reboot, or recover your Amazon EC2 instances\. You can use the stop or terminate actions when you no longer need an instance to be running\. You can use the reboot and recover actions to automatically reboot those instances\.

The following examples show how to:
+ Enable actions for specified alarms using [EnableAlarmActions](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-monitoring-2010-08-01.html#enablealarmactions)\.
+ Disable actions for specified alarms using [DisableAlarmActions](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-monitoring-2010-08-01.html#disablealarmactions)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting Credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage](getting-started_basic-usage.md)\.

## Enable Alarm Actions<a name="enable-alarm-actions"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudWatch\CloudWatchClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function enableAlarmActions($cloudWatchClient, $alarmNames)
{
    try {
        $result = $cloudWatchClient->enableAlarmActions([
            'AlarmNames' => $alarmNames
        ]);
        
        if (isset($result['@metadata']['effectiveUri']))
        {
            return 'At the effective URI of ' . 
                $result['@metadata']['effectiveUri'] . 
                ', actions for any matching alarms have been enabled.';
        } else {
            return'Actions for some matching alarms ' . 
                'might not have been enabled.';
        }

    } catch (AwsException $e) {
        return 'Error: ' . $e->getAwsErrorMessage();
    }
}

function enableTheAlarmActions()
{
    $alarmNames = array('my-alarm');

    $cloudWatchClient = new CloudWatchClient([
        'profile' => 'default',
        'region' => 'us-east-1',
        'version' => '2010-08-01'
    ]);

    echo enableAlarmActions($cloudWatchClient, $alarmNames);
}

// Uncomment the following line to run this code in an AWS account.
// enableTheAlarmActions();
```

## Disable Alarm Actions<a name="disable-alarm-actions"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudWatch\CloudWatchClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function disableAlarmActions($cloudWatchClient, $alarmNames)
{
    try {
        $result = $cloudWatchClient->disableAlarmActions([
            'AlarmNames' => $alarmNames
        ]);

        if (isset($result['@metadata']['effectiveUri']))
        {
            return 'At the effective URI of ' . 
                $result['@metadata']['effectiveUri'] . 
                ', actions for any matching alarms have been disabled.';
        } else {
            return 'Actions for some matching alarms ' . 
                'might not have been disabled.';
        }

    } catch (AwsException $e) {
        return 'Error: ' . $e->getAwsErrorMessage();
    }
}

function disableTheAlarmActions()
{
    $alarmNames = array('my-alarm');
 
    $cloudWatchClient = new CloudWatchClient([
        'profile' => 'default',
        'region' => 'us-east-1',
        'version' => '2010-08-01'
    ]);

    echo disableAlarmActions($cloudWatchClient, $alarmNames);
}

// Uncomment the following line to run this code in an AWS account.
// disableTheAlarmActions();
```