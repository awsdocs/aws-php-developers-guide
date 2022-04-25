# Managing Amazon EC2 Instances Using the AWS SDK for PHP Version 3<a name="ec2-examples-managing-instances"></a>

The following examples show how to:
+ Describe Amazon EC2 instances using [DescribeInstances](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-ec2-2016-11-15.html#describeinstances)\.
+ Enable detailed monitoring for a running instance using [MonitorInstances](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-ec2-2016-11-15.html#monitorinstances)\.
+ Disable monitoring for a running instance using [UnmonitorInstances](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-ec2-2016-11-15.html#unmonitorinstances)\.
+ Start an Amazon EBS\-backed AMI that you’ve previously stopped, using [StartInstances](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-ec2-2016-11-15.html#startinstances)\.
+ Stop an Amazon EBS\-backed instance using [StopInstances](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-ec2-2016-11-15.html#stopinstances)\.
+ Request a reboot of one or more instances using [RebootInstances](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-ec2-2016-11-15.html#rebootinstances)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting Credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage](getting-started_basic-usage.md)\.

## Describe Instances<a name="describe-instances"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Ec2\Ec2Client;
```

 **Sample Code** 

```
$ec2Client = new Aws\Ec2\Ec2Client([
    'region' => 'us-west-2',
    'version' => '2016-11-15',
    'profile' => 'default'
]);
$result = $ec2Client->describeInstances();
echo "Instances: \n";
foreach ($result['Reservations'] as $reservation) {
    foreach ($reservation['Instances'] as $instance) {
        echo "InstanceId: {$instance['InstanceId']} - {$instance['State']['Name']} \n";
    }
}
```

## Enable and Disable Monitoring<a name="enable-and-disable-monitoring"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Ec2\Ec2Client;
```

 **Sample Code** 

```
$ec2Client = new Aws\Ec2\Ec2Client([
    'region' => 'us-west-2',
    'version' => '2016-11-15',
    'profile' => 'default'
]);

$instanceIds = array('InstanceID1', 'InstanceID2');

$monitorInstance = 'ON';

if ($monitorInstance == 'ON') {
    $result = $ec2Client->monitorInstances(array(
        'InstanceIds' => $instanceIds
    ));
} else {
    $result = $ec2Client->unmonitorInstances(array(
        'InstanceIds' => $instanceIds
    ));
}

var_dump($result);
```

## Start and Stop an Instance<a name="start-and-stop-an-instance"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Ec2\Ec2Client;
```

 **Sample Code** 

```
$ec2Client = new Aws\Ec2\Ec2Client([
    'region' => 'us-west-2',
    'version' => '2016-11-15',
    'profile' => 'default'
]);

$action = 'START';

$instanceIds = array('InstanceID1', 'InstanceID2');

if ($action == 'START') {
    $result = $ec2Client->startInstances(array(
        'InstanceIds' => $instanceIds,
    ));
} else {
    $result = $ec2Client->stopInstances(array(
        'InstanceIds' => $instanceIds,
    ));
}

var_dump($result);
```

## Reboot an Instance<a name="reboot-an-instance"></a>

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Ec2\Ec2Client;
```

 **Sample Code** 

```
$ec2Client = new Aws\Ec2\Ec2Client([
    'region' => 'us-west-2',
    'version' => '2016-11-15',
    'profile' => 'default'
]);

$instanceIds = array('InstanceID1', 'InstanceID2');

$result = $ec2Client->rebootInstances(array(
    'InstanceIds' => $instanceIds
));

var_dump($result);
```