# Working with AWS KMS Key Policies Using the AWS SDK for PHP Version 3<a name="kms-example-key-policy"></a>

When you create an AWS Key Management Service \(AWS KMS\) [customer master key \(CMK\)](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#master_keys.html), you determine who can use and manage that CMK\. These permissions are contained in a document called the key policy\. You can use the key policy to add, remove, or modify permissions at any time for a customer\-managed CMK, but you cannot edit the key policy for a CMK that’s managed by AWS\. For more information, see [Authentication and Access Control for AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/control-access.html)\.

The following examples show how to:
+ List the names of key policies using [ListKeyPolicies](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#listkeypolicies)\.
+ Get a key policy using [GetKeyPolicy](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#getkeypolicy)\.
+ Set a key policy using [PutKeyPolicy](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#putkeypolicy)\.

All the example code for the AWS SDK for PHP Version 3 is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Credentials for the AWS SDK for PHP Version 3](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage Patterns of the AWS SDK for PHP Version 3](getting-started_basic-usage.md)\.

For more information about using AWS Key Management Service \(AWS KMS\), see the [AWS KMS Developer Guide](https://docs.aws.amazon.com/kms/latest/developerguide/)\.

## List All Key Policies<a name="list-all-key-policies"></a>

To get the names of key policies for a CMK, use the `ListKeyPolicies` operation\. The only key policy name it returns is the default name\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Kms\KmsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$KmsClient = new Aws\Kms\KmsClient([
    'profile' => 'default',
    'version' => '2014-11-01',
    'region' => 'us-east-2'
]);

$keyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab';
$limit = 10;

try {
    $result = $KmsClient->listKeyPolicies([
        'KeyId' => $keyId,
        'Limit' => $limit,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Retrieve a Key Policy<a name="retrieve-a-key-policy"></a>

To get the key policy for a CMK, use the `GetKeyPolicy` operation\.

 `GetKeyPolicy` requires a policy name\. Unless you created a key policy when you created the CMK, the only valid policy name is the default\. Learn more about the [default key policy](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-default.html)\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Kms\KmsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$KmsClient = new Aws\Kms\KmsClient([
    'profile' => 'default',
    'version' => '2014-11-01',
    'region' => 'us-east-2'
]);

$keyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab';
$policyName = "default";


try {
    $result = $KmsClient->getKeyPolicy([
        'KeyId' => $keyId,
        'PolicyName' => $policyName
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Set a Key Policy<a name="set-a-key-policy"></a>

To establish or change a key policy for a CMK, use the `PutKeyPolicy` operation\.

 `PutKeyPolicy` requires a policy name\. Unless you created a Key Policy when you created the CMK, the only valid policy name is the default\. Learn more about the [Default Key Policy](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-default.html)\.

 **Imports** 

```
require 'vendor/autoload.php';

use Aws\Kms\KmsClient; 
use Aws\Exception\AwsException;
```

 **Sample Code** 

```
$KmsClient = new Aws\Kms\KmsClient([
    'profile' => 'default',
    'version' => '2014-11-01',
    'region' => 'us-east-2'
]);

$keyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab';
$policyName = "default";

try {
    $result = $KmsClient->putKeyPolicy([
        'KeyId' => $keyId,
        'PolicyName' => $policyName,
        'Policy' => '{ 
            "Version": "2012-10-17", 
            "Id": "custom-policy-2016-12-07", 
            "Statement": [ 
                { "Sid": "Enable IAM User Permissions", 
                "Effect": "Allow", 
                "Principal": 
                   { "AWS": "arn:aws:iam::111122223333:user/root" }, 
                "Action": [ "kms:*" ], 
                "Resource": "*" }, 
                { "Sid": "Enable IAM User Permissions", 
                "Effect": "Allow", 
                "Principal":                 
                   { "AWS": "arn:aws:iam::111122223333:user/ExampleUser" }, 
                "Action": [
                    "kms:Encrypt*",
                    "kms:GenerateDataKey*",
                    "kms:Decrypt*",
                    "kms:DescribeKey*",
                    "kms:ReEncrypt*"
                ], 
                "Resource": "*" }                 
            ]            
        } '
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```