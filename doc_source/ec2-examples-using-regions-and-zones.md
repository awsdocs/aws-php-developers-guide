# Using Regions and Availability Zones for Amazon EC2 with AWS SDK for PHP Version 3<a name="ec2-examples-using-regions-and-zones"></a>

Amazon EC2 is hosted in multiple locations worldwide\. These locations are composed of AWS Regions and Availability Zones\. Each Region is a separate geographic area, with multiple isolated locations known as Availability Zones\. Amazon EC2 provides the ability to place instances and data in multiple locations\.

The following examples show how to:
+ Describe the Availability Zones that are available to you using [DescribeAvailabilityZones](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-ec2-2016-11-15.html#describeavailabilityzones)\.
+ Describe AWS Regions that are currently available to you using [DescribeRegions](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-ec2-2016-11-15.html#describeregions)\.

All the example code for the AWS SDK for PHP Version 3 is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage Patterns of the AWS SDK for PHP Version 3](getting-started_basic-usage.md)\.

## Describe Availability Zones<a name="describe-availability-zones"></a>

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

$result = $ec2Client->describeAvailabilityZones();

var_dump($result);
```

## Describe Regions<a name="describe-regions"></a>

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

$result = $ec2Client->describeRegions();

var_dump($result);
```