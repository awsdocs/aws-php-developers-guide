# Publishing custom metrics in Amazon CloudWatch with AWS SDK for PHP Version 3<a name="cw-examples-publishing-custom-metrics"></a>

Metrics are data about the performance of your systems\. An alarm watches a single metric over a time period you specify\. It performs one or more actions based on the value of the metric, relative to a given threshold over a number of time periods\.

The following examples show how to:
+ Publish metric data using [PutMetricData](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-monitoring-2010-08-01.html#putmetricdata)\.
+ Create an alarm using [PutMetricAlarm](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-monitoring-2010-08-01.html#putmetricalarm)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic usage](getting-started_basic-usage.md)\.

## Publish metric data<a name="publish-metric-data"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudWatch\CloudWatchClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function putMetricData($cloudWatchClient, $cloudWatchRegion, $namespace, 
    $metricData)
{
    try {
        $result = $cloudWatchClient->putMetricData([
            'Namespace' => $namespace,
            'MetricData' => $metricData
        ]);
        
        if (isset($result['@metadata']['effectiveUri']))
        {
            if ($result['@metadata']['effectiveUri'] == 
                'https://monitoring.' . $cloudWatchRegion . '.amazonaws.com')
            {
                return 'Successfully published datapoint(s).';
            } else {
                return 'Could not publish datapoint(s).';
            }
        } else {
            return 'Error: Could not publish datapoint(s).';
        }
    } catch (AwsException $e) {
        return 'Error: ' . $e->getAwsErrorMessage();
    }
}

function putTheMetricData()
{
    $namespace = 'MyNamespace';
    $metricData = [
        [
            'MetricName' => 'MyMetric',
            'Timestamp' => 1589228818, // 11 May 2020, 20:26:58 UTC.
            'Dimensions' => [
                [
                    'Name' => 'MyDimension1',
                    'Value' => 'MyValue1'
                    
                ],
                [
                    'Name' => 'MyDimension2',
                    'Value' => 'MyValue2'
                ]
            ],
            'Unit' => 'Count',
            'Value' => 1
        ]
    ];

    $cloudWatchRegion = 'us-east-1';
    $cloudWatchClient = new CloudWatchClient([
        'profile' => 'default',
        'region' => $cloudWatchRegion,
        'version' => '2010-08-01'
    ]);

    echo putMetricData($cloudWatchClient, $cloudWatchRegion, $namespace, 
        $metricData);
}

// Uncomment the following line to run this code in an AWS account.
// putTheMetricData();
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