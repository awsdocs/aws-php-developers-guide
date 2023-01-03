# Working with security groups in Amazon EC2 with AWS SDK for PHP Version 3<a name="ec2-examples-using-security-groups"></a>

An Amazon EC2 security group acts as a virtual firewall that controls the traffic for one or more instances\. You add rules to each security group to allow traffic to or from its associated instances\. You can modify the rules for a security group at any time\. The new rules are automatically applied to all instances that are associated with the security group\.

The following examples show how to:
+ Describe one or more of your security groups using [DescribeSecurityGroups](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-ec2-2016-11-15.html#describesecuritygroups)\.
+ Add an ingress rule to a security group using [AuthorizeSecurityGroupIngress](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-ec2-2016-11-15.html#authorizesecuritygroupingress)\.
+ Create a security group using [CreateSecurityGroup](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-ec2-2016-11-15.html#createsecuritygroup)\.
+ Delete a security group using [DeleteSecurityGroup](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-ec2-2016-11-15.html#deletesecuritygroup)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic usage](getting-started_basic-usage.md)\.

## Describe security groups<a name="describe-security-groups"></a>

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

$result = $ec2Client->describeSecurityGroups();

var_dump($result);
```

## Add an ingress rule<a name="add-an-ingress-rule"></a>

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

$result = $ec2Client->authorizeSecurityGroupIngress(array(
    'GroupName' => 'string',
    'SourceSecurityGroupName' => 'string'
));

var_dump($result);
```

## Create a security group<a name="create-a-security-group"></a>

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

// Create the security group
$securityGroupName = 'my-security-group';
$result = $ec2Client->createSecurityGroup(array(
    'GroupId' => $securityGroupName,

));

// Get the security group ID (optional)
$securityGroupId = $result->get('GroupId');

echo "Security Group ID: " . $securityGroupId . '\n';
```

## Delete a security group<a name="delete-a-security-group"></a>

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

$securityGroupId = 'my-security-group-id';

$result = $ec2Client->deleteSecurityGroup(array(
    'GroupId' => $securityGroupId
));

var_dump($result);
```