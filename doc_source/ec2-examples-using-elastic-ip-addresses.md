# Using Elastic IP Addresses with Amazon EC2 with AWS SDK for PHP Version 3<a name="ec2-examples-using-elastic-ip-addresses"></a>

An Elastic IP address is a static IP address designed for dynamic cloud computing\. An Elastic IP address is associated with your AWS account\. It’s a public IP address, which is reachable from the internet\. If your instance does not have a public IP address, you can associate an Elastic IP address with your instance to enable communication with the internet\.

The following examples show how to:
+ Describe one or more of your instances using [DescribeInstances](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-ec2-2016-11-15.html#describeinstances)\.
+ Acquire an Elastic IP address using [AllocateAddress](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-ec2-2016-11-15.html#allocateaddress)\.
+ Associate an Elastic IP address with an instance using [AssociateAddress](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-ec2-2016-11-15.html#associateaddress)\.
+ Release an Elastic IP address using [ReleaseAddress](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-ec2-2016-11-15.html#releaseaddress)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting Credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage](getting-started_basic-usage.md)\.

## Describe an Instance<a name="describe-an-instance"></a>

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

var_dump($result);
```

## Allocate and Associate an Address<a name="allocate-and-associate-an-address"></a>

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

$instanceId = 'InstanceID';

$allocation = $ec2Client->allocateAddress(array(
    'DryRun' => false,
    'Domain' => 'vpc',
));

$result = $ec2Client->associateAddress(array(
    'DryRun' => false,
    'InstanceId' => $instanceId,
    'AllocationId' => $allocation->get('AllocationId')
));

var_dump($result);
```

## Release an Address<a name="release-an-address"></a>

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

$associationID = 'AssociationID';

$allocationID = 'AllocationID';

$result = $ec2Client->disassociateAddress(array(
    'AssociationId' => $associationID,
));

$result = $ec2Client->releaseAddress(array(
    'AllocationId' => $allocationID,
));

var_dump($result);
```