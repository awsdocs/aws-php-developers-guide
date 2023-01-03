# Working with grants using the AWS KMS API and the AWS SDK for PHP version 3<a name="kms-example-grants"></a>

A grant is another mechanism for providing permissions\. It is an alternative to the key policy\. You can use grants to give long\-term access that allows AWS principals to use your AWS Key Management Service \(AWS KMS\) customer\-managed [AWS KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#kms_keys)\. For more information, see [Grants in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/grants.html) in the *AWS Key Management Service Developer Guide*\.

The following examples show how to:
+ Create a grant for a KMS key using [CreateGrant](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#creategrant)\.
+ View a grant for a KMS key using [ListGrants](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#listgrants)\.
+ Retire a grant for a KMS key using [RetireGrant](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#retiregrant)\.
+ Revoke a grant for a KMS key using [RevokeGrant](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-kms-2014-11-01.html#revokegrant)\.

All the example code for the AWS SDK for PHP is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code)\.

## Credentials<a name="credentials"></a>

Before running the example code, configure your AWS credentials, as described in [Setting credentials](guide_credentials.md)\. Then import the AWS SDK for PHP, as described in [Basic usage](getting-started_basic-usage.md)\.

For more information about using AWS Key Management Service \(AWS KMS\), see the [AWS KMS Developer Guide](https://docs.aws.amazon.com/kms/latest/developerguide/)\.

## Create a grant<a name="create-a-grant"></a>

To create a grant for an AWS KMS key, use the [CreateGrant](https://docs.aws.amazon.com/kms/latest/APIReference/API_CreateGrant.html) operation\.

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
$granteePrincipal = "arn:aws:iam::111122223333:user/Alice";
$operation = ['Encrypt', 'Decrypt']; // A list of operations that the grant allows.

try {
    $result = $KmsClient->createGrant([
        'GranteePrincipal' => $granteePrincipal,
        'KeyId' => $keyId,
        'Operations' => $operation
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## View a grant<a name="view-a-grant"></a>

To get detailed information about the grants on an AWS KMS key, use the [ListGrants](https://docs.aws.amazon.com/kms/latest/APIReference/API_ListGrants.html) operation\.

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
    $result = $KmsClient->listGrants([
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

## Retire a grant<a name="retire-a-grant"></a>

To retire a grant for an AWS KMS key, use the [RetireGrant](https://docs.aws.amazon.com/kms/latest/APIReference/API_RetireGrant.html) operation\. Retire a grant to clean up after you finish using it\.

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

$grantToken = 'Place your grant token here';


try {
    $result = $KmsClient->retireGrant([
        'GrantToken' => $grantToken,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}

//Can also identify grant to retire by a combination of the grant ID and the Amazon Resource Name (ARN) of the customer master key (CMK)
$keyId = 'arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab';
$grantId = 'Unique identifier of the grant returned during CreateGrant operation'

try {
    $result = $KmsClient->retireGrant([
        'GrantId' => $grantToken,
        'KeyId' => $keyId,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```

## Revoke a grant<a name="revoke-a-grant"></a>

To revoke a grant to an AWS KMS key, use the [RevokeGrant](https://docs.aws.amazon.com/kms/latest/APIReference/API_RevokeGrant.html) operation\. You can revoke a grant to explicitly deny operations that depend on it\.

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
$grantId = "grant1";

try {
    $result = $KmsClient->revokeGrant([
        'KeyId' => $keyId,
        'GrantId' => $grantId,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    echo $e->getMessage();
    echo "\n";
}
```