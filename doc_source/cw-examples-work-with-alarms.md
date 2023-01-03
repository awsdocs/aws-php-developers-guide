# Working with Amazon CloudWatch alarms with AWS SDK for PHP Version 3<a name="cw-examples-work-with-alarms"></a>

An Amazon CloudWatch alarm watches a single metric over a time period you specify\. It performs one or more actions based on the value of the metric relative to a given threshold over a number of time periods\.

The following examples show how to:
+ Describe an alarm using [DescribeAlarms](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-monitoring-2010-08-01.html#describealarms)\.
+ Create an alarm using [PutMetricAlarm](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-monitoring-2010-08-01.html#putmetricalarm)\.
+ Delete an alarm using [DeleteAlarms](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-monitoring-2010-08-01.html#deletealarms)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic usage](getting-started_basic-usage.md)\.

## Describe alarms<a name="describe-alarms"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudWatch\CloudWatchClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function describeAlarms($cloudWatchClient)
{
    try {
        $result = $cloudWatchClient->describeAlarms();

        $message = '';

        if (isset($result['@metadata']['effectiveUri']))
        {
            $message .= 'Alarms at the effective URI of ' . 
                $result['@metadata']['effectiveUri'] . "\n\n";

            if (isset($result['CompositeAlarms']))
            {
                $message .= "Composite alarms:\n";

                foreach ($result['CompositeAlarms'] as $alarm) {
                    $message .= $alarm['AlarmName'] . "\n";
                }
            } else {
                $message .= "No composite alarms found.\n";
            }
            
            if (isset($result['MetricAlarms']))
            {
                $message .= "Metric alarms:\n";

                foreach ($result['MetricAlarms'] as $alarm) {
                    $message .= $alarm['AlarmName'] . "\n";
                }
            } else {
                $message .= 'No metric alarms found.';
            }
        } else {
            $message .= 'No alarms found.';
        }
        
        return $message;
    } catch (AwsException $e) {
        return 'Error: ' . $e->getAwsErrorMessage();
    }
}

function describeTheAlarms()
{
    $cloudWatchClient = new CloudWatchClient([
        'profile' => 'default',
        'region' => 'us-east-1',
        'version' => '2010-08-01'
    ]);

    echo describeAlarms($cloudWatchClient);
}

// Uncomment the following line to run this code in an AWS account.
// describeTheAlarms();
```

## Create an alarm<a name="create-an-alarm"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudWatch\CloudWatchClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function putMetricAlarm($cloudWatchClient, $cloudWatchRegion, 
    $alarmName, $namespace, $metricName, 
    $dimensions, $statistic, $period, $comparison, $threshold, 
    $evaluationPeriods)
{
    try {
        $result = $cloudWatchClient->putMetricAlarm([
            'AlarmName' => $alarmName,
            'Namespace' => $namespace,
            'MetricName' => $metricName,
            'Dimensions' => $dimensions,
            'Statistic' => $statistic,
            'Period' => $period,
            'ComparisonOperator' => $comparison,
            'Threshold' => $threshold,
            'EvaluationPeriods' => $evaluationPeriods
        ]);
        
        if (isset($result['@metadata']['effectiveUri']))
        {
            if ($result['@metadata']['effectiveUri'] == 
                'https://monitoring.' . $cloudWatchRegion . '.amazonaws.com')
            {
                return 'Successfully created or updated specified alarm.';
            } else {
                return 'Could not create or update specified alarm.';
            }
        } else {
            return 'Could not create or update specified alarm.';
        }
    } catch (AwsException $e) {
        return 'Error: ' . $e->getAwsErrorMessage();
    }
}

function putTheMetricAlarm()
{
    $alarmName = 'my-ec2-resources';
    $namespace = 'AWS/Usage';
    $metricName = 'ResourceCount';
    $dimensions = [
        [
            'Name' => 'Type',
            'Value' => 'Resource'
        ],
        [
            'Name' => 'Resource',
            'Value' => 'vCPU'
        ],
        [
            'Name' => 'Service',
            'Value' => 'EC2'
        ],
        [
            'Name' => 'Class',
            'Value' => 'Standard/OnDemand'
        ]
    ];
    $statistic = 'Average';
    $period = 300;
    $comparison = 'GreaterThanThreshold';
    $threshold = 1;
    $evaluationPeriods = 1;

    $cloudWatchRegion = 'us-east-1';
    $cloudWatchClient = new CloudWatchClient([
        'profile' => 'default',
        'region' => $cloudWatchRegion,
        'version' => '2010-08-01'
    ]);

    echo putMetricAlarm($cloudWatchClient, $cloudWatchRegion, 
        $alarmName, $namespace, $metricName, 
        $dimensions, $statistic, $period, $comparison, $threshold, 
        $evaluationPeriods);
}

// Uncomment the following line to run this code in an AWS account.
// putTheMetricAlarm();
```

## Delete alarms<a name="delete-alarms"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudWatch\CloudWatchClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function deleteAlarms($cloudWatchClient, $alarmNames)
{
    try {
        $result = $cloudWatchClient->deleteAlarms([
            'AlarmNames' => $alarmNames
        ]);

        return 'The specified alarms at the following effective URI have ' . 
            'been deleted or do not currently exist: ' . 
            $result['@metadata']['effectiveUri'];

    } catch (AwsException $e) {
        return 'Error: ' . $e->getAwsErrorMessage();
    }
}

function deleteTheAlarms()
{
    $alarmNames = array('my-alarm');
    
    $cloudWatchClient = new CloudWatchClient([
        'profile' => 'default',
        'region' => 'us-east-1',
        'version' => '2010-08-01'
    ]);

    echo deleteAlarms($cloudWatchClient, $alarmNames);
}

// Uncomment the following line to run this code in an AWS account.
// deleteTheAlarms();
```