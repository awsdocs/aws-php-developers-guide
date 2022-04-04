# Working with Aliases Using the AWS KMS API and the AWS SDK for PHP Version 3<a name="kms-example-alias"></a>

An alias is an optional display name for an AWS Key Management Service \(AWS KMS\) [customer master key \(CMK\)](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#master_keys.html)\.

The following examples show how to:
+ Create an alias using [CreateAlias](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#createalias)\.
+ View an alias using [ListAliases](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#listaliases)\.
+ Update an alias using [UpdateAlias](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#updatealias)\.
+ Delete an alias using [DeleteAlias](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#deletealias)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting Credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic Usage](getting-started_basic-usage.md)\.

For more information about using AWS Key Management Service \(AWS KMS\), see the [AWS KMS Developer Guide](https://docs.aws.amazon.com/kms/latest/developerguide/)\.

## Create an Alias<a name="create-an-alias"></a>

To create an alias for a CMK, use the [CreateAlias](https://docs.aws.amazon.com/kms/latest/APIReference/API_CreateAlias.html) operation\. The alias must be unique in the account and AWS Region\. If you create an alias for a CMK that already has an alias, `CreateAlias` creates another alias to the same CMK\. It doesn’t replace the existing alias\.

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
$aliasName = "alias/projectKey1";

try {
    $result = $KmsClient->createAlias([
        'AliasName' => $aliasName,
        'TargetKeyId' => $keyId,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## View an Alias<a name="view-an-alias"></a>

To list all aliases, use the [ListAliases](https://docs.aws.amazon.com/kms/latest/APIReference/API_ListAliases.html) operation\. The response includes aliases that are defined by AWS services, but are not associated with a CMK\.

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

$limit = 10;

try {
    $result = $KmsClient->listAliases([
        'Limit' => $limit,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Update an Alias<a name="update-an-alias"></a>

To associate an existing alias with a different CMK, use the [UpdateAlias](https://docs.aws.amazon.com/kms/latest/APIReference/API_UpdateAlias.html) operation\.

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
$aliasName = "alias/projectKey1";


try {
    $result = $KmsClient->updateAlias([
        'AliasName' => $aliasName,
        'TargetKeyId' => $keyId,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Delete an Alias<a name="delete-an-alias"></a>

To delete an alias, use the [DeleteAlias](https://docs.aws.amazon.com/kms/latest/APIReference/API_DeleteAlias.html) operation\. Deleting an alias has no effect on the underlying CMK\.

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

$aliasName = "alias/projectKey1";

try {
    $result = $KmsClient->deleteAlias([
        'AliasName' => $aliasName,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```