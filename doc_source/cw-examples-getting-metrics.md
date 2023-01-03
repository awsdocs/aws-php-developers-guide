# Getting metrics from Amazon CloudWatch with AWS SDK for PHP Version 3<a name="cw-examples-getting-metrics"></a>

Metrics are data about the performance of your systems\. You can enable detailed monitoring of some resources, such as your Amazon EC2 instances, or of your own application metrics\.

The following examples show how to:
+ List metrics using [ListMetrics](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-monitoring-2010-08-01.html#listmetrics)\.
+ Retrieve alarms for a metric using [DescribeAlarmsForMetric](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-monitoring-2010-08-01.html#describealarmsformetric)\.
+ Get statistics for a specified metric using [GetMetricStatistics](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-monitoring-2010-08-01.html#getmetricstatistics)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic usage](getting-started_basic-usage.md)\.

## List metrics<a name="list-metrics"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudWatch\CloudWatchClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function listMetrics($cloudWatchClient)
{
    try {
        $result = $cloudWatchClient->listMetrics();

        $message = ''; 

        if (isset($result['@metadata']['effectiveUri']))
        {
            $message .= 'For the effective URI at ' . 
                $result['@metadata']['effectiveUri'] . ":\n\n";
        
            if ((isset($result['Metrics'])) and 
                (count($result['Metrics']) > 0))
            {
                $message .= "Metrics found:\n\n";

                foreach($result['Metrics'] as $metric) 
                {
                    $message .= 'For metric ' . $metric['MetricName'] . 
                        ' in namespace ' . $metric['Namespace'] . ":\n";
                    
                    if ((isset($metric['Dimensions'])) and 
                        (count($metric['Dimensions']) > 0))
                    {
                        $message .= "Dimensions:\n";

                        foreach ($metric['Dimensions'] as $dimension)
                        {
                            $message .= 'Name: ' . $dimension['Name'] . 
                                ', Value: ' . $dimension['Value'] . "\n";
                        }

                        $message .= "\n";
                    } else {
                        $message .= "No dimensions.\n\n";
                    }
                }
            } else {
                $message .= 'No metrics found.';
            }
        } else {
            $message .= 'No metrics found.';
        }

        return $message;
    } catch (AwsException $e) {
        return 'Error: ' . $e->getAwsErrorMessage();
    }
}

function listTheMetrics()
{
    $cloudWatchClient = new CloudWatchClient([
        'profile' => 'default',
        'region' => 'us-east-1',
        'version' => '2010-08-01'
    ]);

    echo listMetrics($cloudWatchClient);
}

// Uncomment the following line to run this code in an AWS account.
// listTheMetrics();
```

## Retrieve alarms for a metric<a name="retrieve-alarms-for-a-metric"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudWatch\CloudWatchClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function describeAlarmsForMetric($cloudWatchClient, $metricName, 
    $namespace, $dimensions)
{
    try {
        $result = $cloudWatchClient->describeAlarmsForMetric([
            'MetricName' => $metricName,
            'Namespace' => $namespace,
            'Dimensions' => $dimensions
        ]);

        $message = '';

        if (isset($result['@metadata']['effectiveUri']))
        {
            $message .= 'At the effective URI of ' .
                $result['@metadata']['effectiveUri'] . ":\n\n";

            if ((isset($result['MetricAlarms'])) and 
                (count($result['MetricAlarms']) > 0))
            {
                $message .= 'Matching alarms for ' . $metricName . ":\n\n";

                foreach ($result['MetricAlarms'] as $alarm)
                {
                    $message .= $alarm['AlarmName'] . "\n";
                }
            } else {
                $message .= 'No matching alarms found for ' . $metricName . '.';
            }
        } else {
            $message .= 'No matching alarms found for ' . $metricName . '.';
        }

        return $message;
    } catch (AwsException $e) {
        return 'Error: ' . $e->getAwsErrorMessage();
    }
}

function describeTheAlarmsForMetric()
{
    $metricName = 'BucketSizeBytes';
    $namespace = 'AWS/S3';
    $dimensions = [
        [
            'Name' => 'StorageType',
            'Value'=> 'StandardStorage'
        ],
        [
            'Name' => 'BucketName',
            'Value' => 'my-bucket'
        ]
    ];

    $cloudWatchClient = new CloudWatchClient([
        'profile' => 'default',
        'region' => 'us-east-1',
        'version' => '2010-08-01'
    ]);

    echo describeAlarmsForMetric($cloudWatchClient, $metricName, 
        $namespace, $dimensions);
}

// Uncomment the following line to run this code in an AWS account.
// describeTheAlarmsForMetric();
```

## Get metric statistics<a name="get-metric-statistics"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\CloudWatch\CloudWatchClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
function getMetricStatistics($cloudWatchClient, $namespace, $metricName, 
    $dimensions, $startTime, $endTime, $period, $statistics, $unit)
{
    try {
        $result = $cloudWatchClient->getMetricStatistics([
            'Namespace' => $namespace,
            'MetricName' => $metricName,
            'Dimensions' => $dimensions,
            'StartTime' => $startTime,
            'EndTime' => $endTime,
            'Period' => $period,
            'Statistics' => $statistics,
            'Unit' => $unit
        ]);
        
        $message = '';

        if (isset($result['@metadata']['effectiveUri']))
        {
            $message .= 'For the effective URI at ' . 
                $result['@metadata']['effectiveUri'] . "\n\n";
        
            if ((isset($result['Datapoints'])) and 
                (count($result['Datapoints']) > 0))
            {
                $message .= "Datapoints found:\n\n";

                foreach($result['Datapoints'] as $datapoint)
                {
                    foreach ($datapoint as $key => $value)
                    {
                        $message .= $key . ' = ' . $value . "\n";
                    }

                    $message .= "\n";
                }
            } else {
                $message .= 'No datapoints found.';
            }
        } else {
            $message .= 'No datapoints found.';
        }

        return $message;
    } catch (AwsException $e) {
        return 'Error: ' . $e->getAwsErrorMessage();
    }
}

function getTheMetricStatistics()
{
    // Average number of Amazon EC2 vCPUs every 5 minutes within 
    // the past 3 hours.
    $namespace = 'AWS/Usage';
    $metricName = 'ResourceCount';
    $dimensions = [
        [
            'Name' => 'Service',
            'Value' => 'EC2'
        ],
        [
            'Name' => 'Resource',
            'Value'=> 'vCPU'
        ],
        [
            'Name' => 'Type',
            'Value' => 'Resource'
        ],
        [
            'Name' => 'Class',
            'Value'=> 'Standard/OnDemand'
        ]
    ];
    $startTime = strtotime('-3 hours');
    $endTime = strtotime('now');
    $period = 300; // Seconds. (5 minutes = 300 seconds.)
    $statistics = array('Average');
    $unit = 'None';

    $cloudWatchClient = new CloudWatchClient([
        'profile' => 'default',
        'region' => 'us-east-1',
        'version' => '2010-08-01'
    ]);

    echo getMetricStatistics($cloudWatchClient, $namespace, $metricName, 
        $dimensions, $startTime, $endTime, $period, $statistics, $unit);

    // Another example: average number of bytes of standard storage in the 
    // specified Amazon S3 bucket each day for the past 3 days.

    /*
    $namespace = 'AWS/S3';
    $metricName = 'BucketSizeBytes';
    $dimensions = [
        [
            'Name' => 'StorageType',
            'Value'=> 'StandardStorage'
        ],
        [
            'Name' => 'BucketName',
            'Value' => 'my-bucket'
        ]
    ];
    $startTime = strtotime('-3 days');
    $endTime = strtotime('now');
    $period = 86400; // Seconds. (1 day = 86400 seconds.)
    $statistics = array('Average');
    $unit = 'Bytes';
    
    $cloudWatchClient = new CloudWatchClient([
        'profile' => 'default',
        'region' => 'us-east-1',
        'version' => '2010-08-01'
    ]);

    echo getMetricStatistics($cloudWatchClient, $namespace, $metricName, 
    $dimensions, $startTime, $endTime, $period, $statistics, $unit);
    */
}

// Uncomment the following line to run this code in an AWS account.
// getTheMetricStatistics();
```